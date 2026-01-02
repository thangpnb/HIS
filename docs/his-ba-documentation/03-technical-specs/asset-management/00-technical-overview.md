# Tổng quan Kỹ thuật: Quản lý Tài sản (Asset Management)

## 1. Giới thiệu
Tài liệu này cung cấp cái nhìn tổng quan về kiến trúc kỹ thuật của phân hệ Quản lý Tài sản trong hệ thống HIS. Phân hệ này chịu trách nhiệm quản lý vòng đời của trang thiết bị y tế, từ lúc nhập kho, phân bổ sử dụng, theo dõi tình trạng đến khi thanh lý.

## 2. Phạm vi
*   **Quản lý Hồ sơ Tài sản**: Thông tin chi tiết về thiết bị (Model, Serial, Năm sản xuất, Hãng SX).
*   **Quản lý Vị trí**: Theo dõi thiết bị đang được đặt tại khoa/phòng nào.
*   **Quản lý Bộ dụng cụ**: Định nghĩa các gói/set thiết bị (Equipment Sets).

## 3. Kiến trúc Module (Plugins)
Các plugin chính trong `HIS.Desktop.Plugins` tham gia vào quy trình này:

| Plugin Name | Chức năng Chính | Mapping Business Process |
| :--- | :--- | :--- |
| **`HIS.Desktop.Plugins.HisMachine`** | Quản lý danh sách và hồ sơ chi tiết của máy/thiết bị. Cập nhật trạng thái và vị trí. | `01-overview.md` |
| **`HIS.Desktop.Plugins.EquipmentSet`** | Quản lý các bộ dụng cụ (Set), gom nhóm vật tư/thiết bị. | `01-overview.md` |
| **`HIS.Desktop.Plugins.MaterialType`** | Quản lý danh mục vật tư, linh kiện thay thế (được tham chiếu bởi thiết bị). | N/A |
| **`HIS.Desktop.Plugins.Room`** | (Core) Quản lý danh sách phòng ban, vị trí đặt thiết bị. | N/A |

## 4. Cơ sở Dữ liệu (Database Schema)
Các bảng dữ liệu chính liên quan (Dự kiến dựa trên kiến trúc DTO):

### 4.1. HIS_MACHINE
Lưu trữ thông tin định danh của thiết bị.
*   `ID`: Khóa chính.
*   `MACHINE_CODE`: Mã quản lý tài sản.
*   `MACHINE_NAME`: Tên thiết bị.
*   `SERIAL_NUMBER`: Số Serial.
*   `MANUFACTURED_YEAR`: Năm sản xuất.
*   `USED_YEAR`: Năm bắt đầu sử dụng.
*   `ROOM_ID`: ID phòng hiện tại (FK -> `HIS_ROOM`).
*   `MACHINE_GROUP_CODE`: Phân nhóm thiết bị.
*   `SOURCE_CODE`: Nguồn gốc (Ngân sách, Viện trợ...).
*   `IS_ACTIVE`: Trạng thái hoạt động.

### 4.2. HIS_EQUIPMENT_SET
Lưu trữ định nghĩa các bộ dụng cụ.
*   `ID`: Khóa chính.
*   `EQUIPMENT_SET_CODE`: Mã bộ.
*   `EQUIPMENT_SET_NAME`: Tên bộ.

## 5. Giao tiếp Hệ thống (System Comm.)
*   **API Consumer**: Module sử dụng `MosConsumer` để giao tiếp với Backend API (các service `HisMachine`, `HisRoom`...).
*   **Validation**: Sử dụng `DXErrorProvider` để kiểm tra dữ liệu nhập liệu trên Form (ví dụ: `HIS.Desktop.Plugins.HisMachine.Validation`).

## 6. Lưu ý Kỹ thuật & Hạn chế
*   **Quy trình Bảo trì**: Hiện tại chưa có module chuyên biệt (`HisMaintenance`) trên Desktop Client để quản lý lịch bảo trì định kỳ chi tiết. Việc này hiện được xử lý thông qua việc cập nhật thông tin/trạng thái trực tiếp trên `HisMachine` hoặc quản lý ngoài hệ thống.
*   **Tích hợp**: `HisMachine` có tham chiếu chặt chẽ đến `HisRoom` để xác định vị trí tài sản.
