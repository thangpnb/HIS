# RAM Monitor System

## 1. Giới thiệu

`HIS.Desktop.XmlRamMonitor` là một module tiện ích nhỏ được thiết kế để quản lý cấu hình liên quan đến việc giám sát bộ nhớ (RAM) của ứng dụng Client.

> [!NOTE]
> Đây là một module mang tính chất Legacy (kế thừa), có cấu trúc mã nguồn gần như tương đồng hoàn toàn với `XmlCacheMonitor` nhưng phục vụ mục đích khác là giám sát tài nguyên phần cứng thay vì dữ liệu nghiệp vụ.

## 2. Cấu trúc Kỹ thuật

### 2.1. File Cấu hình
Module này đọc cấu hình từ một file XML riêng (thường là `RamMonitorKey.xml`). Cấu trúc file này định nghĩa các ngưỡng cảnh báo hoặc các tham số giám sát bộ nhớ.

### 2.2. Các Class Chính
(Tương tự như Cache Monitor do sử dụng chung mẫu thiết kế):
*   `CacheMonitorKeyStore.cs`: Mặc dù tên class giống CacheMonitor, nhưng trong namespace `HIS.Desktop.XmlRamMonitor`, nó được dùng để xử lý các Key liên quan đến RAM.
*   `ApplicationStoreLocation.cs`: Xác định vị trí lưu trữ file cấu hình trên ổ cứng máy trạm.

## 3. Ứng dụng Thực tế

Trong phiên bản HIS hiện tại, module này ít được sử dụng trực tiếp bởi người dùng cuối mà chủ yếu chạy ngầm (nếu được kích hoạt) để:
*   Ghi log cảnh báo khi RAM sử dụng vượt quá ngưỡng cho phép (tránh lỗi `OutOfMemoryException` trên các máy trạm RAM yếu < 4GB).
*   Gửi thông tin thống kê về Server để đội kỹ thuật theo dõi hiệu năng ứng dụng.
