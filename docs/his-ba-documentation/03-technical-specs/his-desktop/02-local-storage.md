# Local Storage & Caching Architecture

## 1. Mục đích

Thay vì query vào Database (Server) mỗi khi cần lấy danh sách danh mục (VD: 10,000 loại thuốc, 500 khoa phòng), HIS Desktop sử dụng chiến lược **Aggressive Client-side Caching**. Toàn bộ dữ liệu danh mục được tải về máy trạm và lưu trữ dưới dạng Cache.

## 2. Cấu trúc Thư viện

Namespace `HIS.Desktop.LocalStorage` chứa nhiều project con, mỗi project phụ trách caching cho một nhóm dữ liệu:

*   **LocalStorage.BackendData**: Chứa dữ liệu danh mục nghiệp vụ chính (Thuốc, Vật tư, Dịch vụ, Khoa phòng...).
*   **LocalStorage.HisConfig**: Chứa cấu hình tham số hệ thống (`HIS_CONFIG`).
*   **LocalStorage.ConfigSystem**: Chứa cấu hình máy trạm local (File config XML).
*   **LocalStorage.Dao**: (Data Access Object) Lớp vật lý đọc ghi file cache.

## 3. Cơ chế Hoạt động

1.  **Khởi động**: Khi App start, `StoreLoader` sẽ kiểm tra file cache cục bộ (`.xml`, `.dat` hoặc SQLite).
2.  **Đồng bộ (Sync)**:
    *   Gọi API `Get` lên Server với tham số `LastModifiedTime`.
    *   Server trả về các bản ghi thay đổi (Delta).
    *   Client cập nhật Delta vào Cache cục bộ.
3.  **Sử dụng**: Code nghiệp vụ **KHÔNG** gọi API để lấy danh sách thuốc. Thay vào đó gọi:
    ```csharp
    var listMedicine = BackendDataWorker.Get<V_HIS_MEDICINE_TYPE>();
    ```
    Hàm này trả về dữ liệu tức thì từ RAM/Local Disk.

## 4. Quản lý Cache (Cache Monitor)

Để đảm bảo dữ liệu không bị cũ (Stale):
*   Hệ thống **Cache Monitor** (Xem thêm `internal-monitoring`) chạy background.
*   Khi phát hiện có thay đổi trên Server, nó kích hoạt quy trình tải lại (Reload) cho riêng danh mục đó.
