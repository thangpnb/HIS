# Cache Monitor System

## 1. Giới thiệu

Trong hệ thống HIS, để tối ưu hiệu năng, hàng trăm danh mục (Thuốc, Dịch vụ kỹ thuật, Khoa phòng...) được tải và lưu trữ (Cache) tại RAM của máy trạm (Client Computer). Tuy nhiên, khi dữ liệu danh mục thay đổi ở Server (ví dụ: thêm loại thuốc mới), các máy trạm cần biết để tải lại (Reload). 

**Cache Monitor System** ra đời để giải quyết bài toán đồng bộ dữ liệu Cache phân tán này.

## 2. Luồng hoạt động (Workflow)

1.  **Trigger thay đổi**: Khi Admin sửa danh mục (ví dụ trên trang Quản trị), hệ thống sẽ đánh dấu (Flag) vào bảng `HIS_CACHE_MONITOR` trong Database rằng "Key X cần được reload".
2.  **Client Polling**: Tại mỗi máy trạm, `BackendDataWorker` chạy ngầm định kỳ (Background Worker) sẽ gọi API `api/HisCacheMonitor/Get` để kiểm tra xem có Key nào bị thay đổi không.
3.  **Reload**: Nếu phát hiện Key X có cờ reload = true, Client sẽ xóa cache cũ của Key X và tải lại dữ liệu mới từ Server.

## 3. Cấu trúc Kỹ thuật

### 3.1. Entity `HIS_CACHE_MONITOR`
Bảng Database lưu trạng thái Cache.

| Field Name | Data Type | Description |
|:---|:---|:---|
| `ID` | `long` | Primary Key. |
| `DATA_NAME` | `string` | Tên định danh của Cache Key (VD: `HIS.Desktop.LocalStorage.SdaProvince`). |
| `IS_RELOAD` | `number` | 1: Cần reload, 0: Đã mới nhất. |
| `MODIFY_TIME`| `long` | Thời điểm cập nhật cuối cùng. |

### 3.2. Library `HIS.Desktop.XmlCacheMonitor`
Thư viện này chịu trách nhiệm quản lý file cấu hình cục bộ `CacheMonitorKey.xml` (thường nằm trong thư mục cài đặt). File này định nghĩa danh sách các Key mà Client đang theo dõi.

*   `CacheMonitorKeyStore.cs`: Class đọc/ghi danh sách Key từ XML.
*   `XmlCacheMonitorConfig.cs`: Đọc đường dẫn file cấu hình từ App.config.

### 3.3. Management UI (`HIS.Desktop.Plugins.HisCacheMonitor`)
Plugin giao diện cho phép Admin can thiệp thủ công:
*   Xem danh sách các Cache Key đang được giám sát.
*   Bắt buộc yêu cầu Reload toàn hệ thống (Set `IS_RELOAD = 1` cho các Key được chọn).
*   Khóa/Mở khóa các Key.

## 4. API Endpoint (`MosConsumer`)

Hệ thống sử dụng MOS Service (Medical Operation System) để giao tiếp:

*   `api/HisCacheMonitor/Get`: Lấy trạng thái các Cache Key.
*   `api/HisCacheMonitor/ChangeLock`: Khóa/Mở khóa Key.
*   `api/HisCacheMonitor/Delete`: Xóa cấu hình Key.
