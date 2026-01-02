# HIS Desktop Core Framework

## 1. Quy trình Khởi động (Startup Flow)

File `Program.cs` là điểm bắt đầu của ứng dụng. Quy trình khởi chạy diễn ra theo trình tự sau:

1.  **Init Environment**: Cấu hình Log (`log4net`), Font chữ (`ApplicationFontWorker`), và Skin giao diện (`SkinManager`).
2.  **Load System Config**: Hiển thị form `frmLoadConfigSystem` để tải các cấu hình hệ thống cơ bản từ local file hoặc registry.
3.  **Auto Update (AUP)**: Kiểm tra phiên bản mới từ Server:
    *   Nếu có bản mới -> Gọi `Inventec.AutoUpdater.exe`.
    *   Nếu không -> Tiếp tục.
4.  **Application Run**: Khởi chạy `MyApplicationContext` hoặc `frmMain` (Main Form).

## 2. Main Form (`frmMain`)

Đây là cửa sổ chính của ứng dụng, hoạt động như một Container chứa các module khác.

### 2.1. Cấu trúc Giao diện
*   **Ribbon Bar**: Chứa menu chức năng được sinh động (`Base.Menu.GenerateMenu`).
*   **Tab Control**: Khu vực hiển thị nội dung chính. Mỗi khi người dùng mở một chức năng, nó sẽ được mở trong một Tab mới (`XtraTabControl`).

### 2.2. Login & Session (Phiên làm việc)
*   **Token Management**: Class `ClientTokenManager` (trong `Inventec.UC.Login`) chịu trách nhiệm lưu trữ Token xác thực sau khi đăng nhập thành công.
*   **Phòng làm việc (Workplace)**: Người dùng phải chọn Phòng làm việc (`HIS_USER_ROOM`) để xác định ngữ cảnh xử lý.

## 3. Cơ chế Module & Plugin

Hệ thống sử dụng kiến trúc Plugin-based để mở rộng chức năng mà không cần build lại core.

*   **PluginManager**: Quản lý việc tải các file DLL từ thư mục `Plugins/`.
*   **Lazy Loading**: Các Plugin nghiệp vụ (Ví dụ: `HIS.Desktop.Plugins.Reception`, `HIS.Desktop.Plugins.Examination`) chỉ được tải vào RAM khi người dùng click vào menu tương ứng, giúp giảm thời gian khởi động app.

## 4. Dịch vụ Nền (Background Services)

`frmMain` khởi chạy và duy trì nhiều tác vụ ngầm:
*   `RunCheckTokenTimeout`: Kiểm tra token hết hạn để logout tự động.
*   `RunSyncBackendDataToLocal`: Đồng bộ cache danh mục (Xem thêm `02-local-storage.md`).
*   `RunCheckConnectServer`: Giám sát kết nối mạng tới Server.
*   `RunPubSub`: Lắng nghe thông báo thời gian thực (Real-time Notification) qua WebSocket.
