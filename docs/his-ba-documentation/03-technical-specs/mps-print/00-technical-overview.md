# Technical Overview: MPS Print System

## 1. Mục đích

**MPS (Medical Printing System)** là phân hệ quản lý in ấn tập trung của hệ thống HIS. Thay vì mỗi màn hình tự xử lý logic in, hệ thống sử dụng kiến trúc Processor để chuẩn hóa việc trích xuất dữ liệu và fill vào biểu mẫu.

## 2. Kiến trúc Module

Hệ thống in ấn được chia thành 2 phần chính:

*   **MPS.ProcessorBase**: Thư viện lõi, định nghĩa các lớp cơ sở (`BaseProcessor`), Interface và các tiện ích dùng chung (`PrintConfig`, `FileUtils`) để xử lý việc in.
*   **MPS (Implementations)**: Chứa hàng ngàn class Processor cụ thể (VD: `MPS.Processor.Mps000500`), mỗi class tương ứng với một mẫu phiếu in hoặc báo cáo cụ thể.

## 3. Luồng xử lý (Workflow)

1.  **Request**: Người dùng bấm nút In trên giao diện.
2.  **Mapping**: Hệ thống xác định `PrintTypeCode` (Mã phiếu in) và tìm Processor tương ứng trong `MPS.dll`.
3.  **Data Fetching**: Processor thực thi query để lấy dữ liệu từ Database.
4.  **Rendering**: Dữ liệu được merge vào Template (Excel/Word/PDF) thông qua các thư viện như `FlexCel`.
5.  **Output**: File kết quả được gửi ra máy in hoặc hiển thị lên màn hình.

## 4. Tài liệu chi tiết

Hiện tại phân hệ này có số lượng Processor rất lớn (~4000), do đó tài liệu này tập trung vào kiến trúc lõi. Chi tiết từng mẫu in cụ thể cần tham khảo trực tiếp mã nguồn của từng Processor.
