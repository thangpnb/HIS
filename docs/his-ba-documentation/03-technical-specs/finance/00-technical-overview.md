# Technical Overview: Phân hệ Tài chính (Finance)

## 1. Tổng quan Kiến trúc
Phân hệ Tài chính trong HIS chịu trách nhiệm quản lý toàn bộ luồng tiền và công nợ của bệnh viện. Nó hoạt động chặt chẽ với các phân hệ Lâm sàng (Clinical) để tính toán chi phí dựa trên y lệnh và Dược (Pharmacy) để quản lý kho thuốc/vật tư tiêu hao.

## 2. Danh sách Modules & Plugins
Các plugin chính trong phân hệ Tài chính, nằm trong namespace `HIS.Desktop.Plugins.*`:

| STT | Tên Plugin | Mô tả Chức năng | Loại |
| :-- | :--- | :--- | :--- |
| 1 | `TransactionBill` | Thu ngân, thanh toán chi phí ngoại trú/nội trú | Form |
| 2 | `TransactionDeposit` | Thu tiền tạm ứng | Form |
| 3 | `TransactionBillKiosk` | Thanh toán trên Kiosk tự phục vụ | Form |
| 4 | `TransactionBillCancel` | Hủy phiếu thu, hoàn tiền (Repay) | Form |
| 5 | `CheckInfoBHYT` | Kiểm tra thông tin thẻ BHYT online | Form |
| 6 | `InsuranceExpertise` | Giám định BHYT, khóa số liệu | Form |
| 7 | `ExportXmlQD130` | Xuất dữ liệu XML BHYT (Chuẩn 130) | Form |
| 8 | `DebtManager` | Quản lý công nợ bệnh nhân | Form |

## 3. Cấu trúc Dữ liệu Quan trọng (Common Database Schema)
Các bảng dữ liệu lõi được sử dụng chung cho toàn phân hệ:

### 3.1. HIS_TRANSACTION
Lưu trữ header của mọi giao dịch tài chính (Thu tiền, Chi tiền, Tạm ứng, Hoàn ứng).
*   `ID`: Primary Key.
*   `TRANSACTION_CODE`: Mã giao dịch (Generated).
*   `TREATMENT_ID`: ID đợt điều trị của bệnh nhân.
*   `AMOUNT`: Tổng số tiền của giao dịch.
*   `TRANSACTION_TYPE_ID`: Loại giao dịch (1=Thanh toán, 2=Tạm ứng, 3=Hoàn ứng...).
*   `PAY_FORM_ID`: Hình thức thanh toán (Tiền mặt, Chuyển khoản, Thẻ).
*   `CASHIER_ROOM_ID`: ID phòng thu ngân thực hiện.

### 3.2. HIS_SERE_SERV_BILL
Bảng chi tiết, liên kết giữa Dịch vụ (`HIS_SERE_SERV`) và Giao dịch (`HIS_TRANSACTION`).
*   `BILL_ID`: FK tới `HIS_TRANSACTION`.
*   `SERE_SERV_ID`: FK tới `HIS_SERE_SERV`.
*   `PRICE`: Giá áp dụng tại thời điểm thanh toán.
*   `AMOUNT`: Số lượng thanh toán.

### 3.3. HIS_DEPOSIT_REQ
Lưu yêu cầu tạm ứng từ khoa lâm sàng.
*   `AMOUNT`: Số tiền yêu cầu.
*   `REQUEST_ROOM_ID`: Phòng yêu cầu.
*   `DEPOSIT_REQ_STT_ID`: Trạng thái (Đã nộp/Chưa nộp).

## 4. Các Thư viện Chung (Common Libraries)
*   **`HIS.Desktop.ADO`**: Chứa các lớp truy cập dữ liệu (Data Access Object) như `TransactionBillADO`.
*   **`Inventec.Common.Logging`**: Ghi log lỗi và hành vi người dùng.
*   **`MosConsumer`**: Library client để gọi API backend.

## 5. Nguyên lý Tính giá (Pricing Logic)
Hệ thống tính giá dịch vụ dựa trên:
*   **Đối tượng bệnh nhân**: BHYT, Viện phí, Dịch vụ.
*   **Giá dịch vụ**: Được cấu hình trong `HIS_SERVICE_PATY` (Service Patient Type).
*   **Chính sách miễn giảm**: Discount theo chương trình ưu đãi hoặc chỉ định của lãnh đạo.
*   **Quy định BHYT**: Đúng tuyến/Trái tuyến, Trần kỹ thuật cao, Tỷ lệ thanh toán (80%, 95%, 100%).
