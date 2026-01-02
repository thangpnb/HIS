# Thiết kế Kỹ thuật: Phân hệ SDA

## 1. Tổng quan Architecture
Phân hệ SDA bao gồm 14 plugin trong namespace `Global.SDA.Desktop.Plugins.*`.
Các plugin này quản lý Master Data (Dữ liệu danh mục) và System Configuration.

## 2. Danh mục Plugin

| Nhóm | Plugin | Số file | Chức năng |
|:---|:---|:---|:---|
| **Địa lý** | `SdaNational` | 38 | Quản lý Quốc gia. |
| | `SdaProvince` | 25 | Quản lý Tỉnh/TP. |
| | `SdaDistrict` | 25 | Quản lý Quận/Huyện. |
| | `SdaCommune` | 25 | Quản lý Xã/Phường. |
| **Cấu hình** | `SdaField` | 25 | Metadata trường dữ liệu (`SDA_FIELD`). |
| | `SdaHideControl`| 24 | Logic ẩn hiện control động. |
| **Tools** | `SdaExecuteSql` | 26 | SQL Editor & Executor. |

## 3. Thiết kế Chi tiết

### 3.1. Mô hình Dữ liệu Danh mục
Dữ liệu danh mục (Geographic, Demographic) được thiết kế để:
*   **Read-Heavy**: Đọc rất nhiều, ít khi thay đổi.
*   **Global Access**: Có thể truy cập từ bất kỳ module nào.

Do đó, chiến lược là **Load-Once, Cache-Always**.

### 3.2. Quy trình Load & Cache
1.  **Startup**: Khi ứng dụng khởi động, các danh mục nhỏ (Dân tộc, Tôn giáo) được tải ngay vào `BackendData`.
2.  **Lazy Load**: Các danh mục lớn (Xã/Phường - hơn 10.000 bản ghi) được tải khi người dùng lần đầu mở form có dropdown địa chỉ.
3.  **Local Storage**: Sử dụng `HIS.Desktop.LocalStorage` để lưu trữ đối tượng `List<DTO>`.

### 3.3. SdaExecuteSql Security
Plugin thực thi SQL tiềm ẩn rủi ro cao. Cơ chế bảo vệ:
*   Chỉ user có quyền `EXECUTE_SQL` đặc biệt mới mở được.
*   Backend API lọc các từ khóa nguy hiểm (`DROP`, `TRUNCATE`) đối với user thường (nếu có cấu hình).
*   Log toàn bộ câu lệnh SQL đã chạy vào bảng Audit.

## 4. API & Database
*   **APIs**: Các Controller CRUD tiêu chuẩn `/api/Sda[Entity]/[Action]`.
*   **Tables**: Các bảng tiền tố `SDA_` (`SDA_NATIONAL`, `SDA_PROVINCE`, `SDA_ETHNIC`...).

## 5. Tích hợp Form Động
Plugin `SdaField` kết hợp với `HIS.UC.FormType` cho phép render form động:
*   Lấy cấu hình trường từ `SDA_FIELD`.
*   Build UI Control tương ứng (TextBox, DatePicker) tại thời điểm runtime.
