# Quy trình Xuất Dữ liệu Giám định BHYT (XML Export)

## 1. Tổng quan
Quy trình xuất dữ liệu giám định bảo hiểm y tế quản lý việc trích xuất, kiểm tra, và chuẩn hóa dữ liệu hồ sơ khám chữa bệnh để gửi lên Cổng Giám định BHYT (theo quyết định 4210/QĐ-BYT và 130/QĐ-BYT). Đây là bước quyết định để bệnh viện được thanh toán chi phí KCB BHYT.

## 2. Lưu đồ Quy trình

```mermaid
graph TB
    Start((Bắt đầu)) --> TongHop[1. Tổng hợp Hồ sơ Đã khóa]
    TongHop --> KiemTra[2. Kiểm tra Lỗi (Validation)]
    
    subgraph "Xử lý Lỗi"
        KiemTra -- Có Lỗi --> ThongBao[Thông báo Khoa/Phòng sửa]
        ThongBao --> SuaLoi[Sửa đổi Dữ liệu]
        SuaLoi --> KiemTra
    end
    
    KiemTra -- Hợp lệ --> XuatXML[3. Xuất file XML (130/4210)]
    XuatXML --> KySo[4. Ký số (Digital Signature)]
    KySo --> DayCong[5. Đẩy lên Cổng Giám định]
    DayCong --> End((Kết thúc))
```

## 3. Chi tiết Các bước & Mapping Plugin

### 3.1. Tổng hợp & Kiểm tra Lỗi (Validation)
Hệ thống quét toàn bộ các hồ sơ bệnh án đã kết thúc điều trị và thanh toán xong trong khoảng thời gian chọn lọc. So sánh dữ liệu với các quy tắc (Rule) của Bộ Y tế.
*   **Plugin chính**:
    *   `HIS.Desktop.Plugins.ExportXmlQD130`: Giao diện chính xử lý theo chuẩn 130 (Mới).
        *   *Chức năng*: Liệt kê danh sách hồ sơ, chạy validator kiểm tra các trường thiếu/sai định dạng (Mã bệnh, Mã thuốc, Ngày giờ...).
    *   `HIS.Desktop.Plugins.ExportXmlQD4210`: Giao diện xử lý theo chuẩn 4210 (Cũ).

### 3.2. Xuất dữ liệu & Ký số (Export & Sign)
Sau khi dữ liệu sạch, nhân viên BHYT thực hiện xuất file XML.
*   **Có chế hoạt động**:
    *   Xuất ra 5 file (XML 1-5) hoặc 12 bảng (theo QĐ 130).
    *   Tự động mã hóa tên file theo cấu trúc quy định.
    *   Tích hợp ký số (Token) để xác thực tính pháp lý.

### 3.3. Đẩy cổng & Phản hồi
Gửi dữ liệu lên cổng BHXH và nhận kết quả giám định (Chấp nhận/Từ chối).
*   **Plugin chính**:
    *   `HIS.Desktop.Plugins.EmrConnector`: hoặc các module kết nối cổng (nếu tích hợp trực tiếp).

## 4. Các Chuẩn Dữ liệu
*   **Quyết định 4210/QĐ-BYT**: Chuẩn dữ liệu cũ (đang thay thế dần).
*   **Quyết định 130/QĐ-BYT**: Chuẩn dữ liệu mới nhất (áp dụng từ 2023/2024), yêu cầu chi tiết hơn về bảng kê thuốc, dịch vụ kỹ thuật và kết quả cận lâm sàng.

## 5. Dữ liệu Đầu ra
*   **File XML**: Bộ file dữ liệu XML được mã hóa.
*   **Biên bản Gửi dữ liệu**: Log ghi nhận thời gian và người gửi.
*   **Báo cáo Tổng hợp**: Danh sách các hồ sơ đã gửi và trạng thái giám định.

## 6. Liên kết Tài liệu
*   [Quy trình Thanh toán Viện phí](./01-cashier-payment.md).
*   [Quy trình Bảo hiểm Y tế](./03-health-insurance.md).
