# Utility Libraries & Global Variables

## 1. Global Variables (`GlobalVariables.cs`)

Lớp `HIS.Desktop.LocalStorage.LocalData.GlobalVariables` đóng vai trò là "Kho chứa trạng thái" toàn cục của ứng dụng trong phiên chạy (Session).

### Các biến quan trọng
*   **AcsAuthorizeSDO**: Chứa thông tin quyền hạn của user hiện tại (Role, quyền truy cập form/control).
*   **CurrentRoomTypeId / CurrentRoomTypeCode**: Loại phòng làm việc hiện tại (Ví dụ: Phòng khám, Kho dược, Thu ngân). Logic nghiệp vụ thường dựa vào biến này để ẩn hiện chức năng.
*   **ListImage**: Cache danh sách icon/hình ảnh dùng chung để tránh load lại từ ổ cứng nhiều lần.
*   **DicConfig**: Dictionary chứa các cấu hình động key-val.

> [!WARNING]
> Việc sử dụng biến toàn cục (`static`) giúp truy cập nhanh nhưng tiềm ẩn rủi ro về Race Condition trong môi trường đa luồng (Multi-threading). Cần chú ý dùng `lock (syncRoot)` khi ghi dữ liệu.

## 2. Tiện ích dùng chung (`HIS.Desktop.Utility`)

Namespace này cung cấp các lớp Helper hỗ trợ lập trình nhanh.

### 2.1. Form & Control Helpers
*   **FormBase**: Lớp cha của hầu hết các Form trong hệ thống, xử lý đồng nhất việc đóng/mở, phím tắt (ESC để thoát), và giao diện chung.
*   **ChangeFontControl**: Tự động điều chỉnh font chữ (`ApplicationFontWorker`) cho toàn bộ control trên form khi người dùng thay đổi cấu hình font hệ thống.
*   **WaitFormManager / SplashScreenManager**: Hiển thị màn hình chờ khi thực hiện tác vụ nặng.

### 2.2. Data Helpers
*   **StringUtil**: Xử lý chuỗi (Cắt chuỗi, chuẩn hóa tên, convert Unsign).
*   **DateTimeHelper**: Xử lý ngày tháng (Chuyển đổi long <-> string <-> DateTime).
*   **AgeHelper**: Tính toán tuổi bệnh nhân chính xác theo quy tắc y tế (Tháng/Ngày cho trẻ sơ sinh).

### 2.3. System Helpers
*   **RegistrySettingWorker**: Đọc/Ghi Registry Windows (Lưu cấu hình máy in, vị trí form).
*   **MemoryProcessor**: Giám sát và giải phóng bộ nhớ RAM thủ công (GC Collect) khi đóng các form nặng.
