# Quy trình Viện phí & Thu ngân (Cashier & Payment)

## 1. Tổng quan
Quy trình mô tả các hoạt động liên quan đến thu chi viện phí, bao gồm tạm ứng, thanh toán chi phí khám chữa bệnh, và hoàn ứng cho bệnh nhân.

## 2. Lưu đồ Quy trình

```mermaid
graph TB
    Start((Bắt đầu)) --> ChiDinh[1. Y lệnh/Chỉ định đã thực hiện]
    ChiDinh --> TinhTien[2. Tính tiền (Hệ thống)]
    TinhTien --> TamUng{Đã Tạm ứng đủ?}
    
    TamUng --Chưa đủ--> ThuTamUng[3. Thu Tạm ứng]
    ThuTamUng --> Duyet
    TamUng --Đủ--> Duyet[4. Duyệt & Trừ tiền]
    
    Duyet --> QuyetToan{Kết thúc Đợt?}
    QuyetToan --Chưa--> End((Tiếp tục ĐT))
    QuyetToan --Rồi--> ThanhToan[5. Thanh toán ra viện]
    ThanhToan --> XuatHD[6. Xuất Hóa đơn]
    XuatHD --> End((Kết thúc))
```

## 3. Chi tiết Các bước & Mapping Plugin

### 3.1. Thu Tạm ứng (Deposit Collection)
Bệnh nhân đóng tiền tạm ứng trước khi nhập viện hoặc khi chi phí điều trị vượt quá số dư hiện có.
*   **Plugin chính**:
    *   `HIS.Desktop.Plugins.TransactionDeposit`: Màn hình thu tiền tạm ứng.
    *   `HIS.Desktop.Plugins.RequestDeposit`: Giấy đề nghị tạm ứng (in từ khoa lâm sàng).
    *   `HIS.Desktop.Plugins.DepositService`: Quản lý các khoản tạm ứng theo dịch vụ (khi cần tạm ứng cho gói kỹ thuật cao).
    *   **Quy trình**:
        1.  Khoa lâm sàng tạo yêu cầu tạm ứng (`RequestDeposit`).
        2.  Thu ngân thu tiền và xuất phiếu tạm ứng (`TransactionDeposit`).
        3.  Khoản tiền này được hệ thống tự động cấn trừ khi tính viện phí.
        4.  Nếu thừa sẽ hoàn lại (`TransactionRepay`), nếu thiếu sẽ phải đóng thêm.

### 3.2. Thanh toán Chi phí (Payment & Settlement)
Thực hiện thu tiền các dịch vụ ngoại trú hoặc quyết toán ra viện cho bệnh nhân nội trú.
*   **Plugin chính**:
    *   `HIS.Desktop.Plugins.TransactionBill`: Màn hình thanh toán viện phí tổng hợp.
    *   `HIS.Desktop.Plugins.TransactionCancel`: Hủy giao dịch thanh toán (Hoàn tiền).

### 3.2a. Quản lý Công nợ (Debt Management)
Trường hợp bệnh nhân chưa có khả năng thanh toán ngay, hệ thống ghi nhận công nợ để thu hồi sau.
*   **Plugin chính**:
    *   `HIS.Desktop.Plugins.DebtManager`: Quản lý danh sách bệnh nhân còn nợ viện phí.
    *   `HIS.Desktop.Plugins.TransactionDebtCollect`: Thực hiện thu hồi công nợ (Thanh toán nợ).
*   **Quy trình**:
    1.  Xác nhận nợ: Bệnh nhân ký xác nhận nợ (bảo lãnh hoặc cam kết).
    2.  Ghi sổ nợ: Hệ thống ghi nhận trạng thái thanh toán là "Nợ".
    3.  Thu nợ: Khi bệnh nhân quay lại thanh toán, thu ngân truy xuất khoản nợ và thực hiện thu.

### 3.3. Các Nghiệp vụ Khác
*   **Thanh toán tại Kiosk**: `HIS.Desktop.Plugins.TransactionBillKiosk`.
*   **Duyệt viện phí**: Khoa Dược/Lâm sàng duyệt chi phí trước khi bệnh nhân xuống thanh toán (tránh sai sót).
*   **Hoàn ứng**: Trả lại tiền thừa cho bệnh nhân khi ra viện (`TransactionRepay`).

## 4. Phương thức Thanh toán
Hệ thống hỗ trợ đa dạng phương thức:
*   Tiền mặt.
*   Chuyển khoản (QR Code động).
*   Thẻ ngân hàng (POS).

## 5. Dữ liệu Đầu ra
*   **Phiếu thu**: Xác nhận đã đóng tiền (Mẫu C38-BB).
*   **Bảng kê chi phí**: Chi tiết các dịch vụ đã sử dụng.
*   **Hóa đơn GTGT (E-Invoice)**: Hóa đơn điện tử gửi cơ quan thuế.

## 6. Liên kết Tài liệu
*   [Quy trình Khám ngoại trú](../clinical/01-outpatient-examination.md).
*   [Quy trình Xuất viện](../clinical/05-discharge-transfer.md).
