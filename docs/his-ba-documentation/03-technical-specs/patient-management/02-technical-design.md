# Thiết kế Kỹ thuật: Patient Call & Queue

## 1. Tổng quan Architecture
Module Gọi bệnh nhân sử dụng họ các plugin `HIS.Desktop.Plugins.CallPatient*` để quản lý hàng đợi và giao tiếp với hệ thống hiển thị (Display).

## 2. Danh sách Plugin & Namespace

Hệ thống duy trì nhiều phiên bản plugin để phục vụ các nhu cầu khác nhau:

| Tên Plugin | Phiên bản/Loại | Số file | Mục đích |
|:---|:---|:---|:---|
| `CallPatientV6` | Stable | 26 | Phiên bản chuẩn cho phòng khám ngoại trú. |
| `CallPatientVer7` | Newest | 25 | UI hiện đại, logic ưu tiên nâng cao. |
| `CallPatientCLS` | Specialized | ~30 | Dành riêng cho khu vực lấy mẫu/xét nghiệm. |
| `CallPatientCashier` | Specialized | ~30 | Dành cho quầy thu ngân. |
| `CallPatientTypeAlter`| Utility | 52 | Chuyển đổi đối tượng bệnh nhân (VD: Thường -> Cấp cứu). |

## 3. Chi tiết Thiết kế

### 3.1. Kiến trúc Plugin (MVP Pattern)
Tất cả các phiên bản `CallPatient` đều tuân thủ mô hình MVP:
*   **Entry**: Đăng ký plugin với Core.
*   **Processor**: Xử lý logic nghiệp vụ (Gọi số, Bỏ qua, Gọi lại).
*   **Behavior**: Xử lý sự kiện UI, tương tác người dùng.
*   **ADO**: Mapping dữ liệu hàng đợi từ DB/API.

> **Sơ đồ Class:**
> ```mermaid
> graph LR
>   Plugin --> Processor
>   Processor --> Behavior
>   Behavior --> FormUI
>   Processor --> ADO
>   ADO --> ApiConsumer
> ```

### 3.2. Cơ chế Gọi và Hiển thị (PubSub)
Hệ thống sử dụng mô hình Publish-Subscribe để đồng bộ trạng thái giữa máy trạm của bác sĩ và màn hình hiển thị bên ngoài.

1.  **Bác sĩ nhấn "Gọi"**: 
    *   Gọi API `CallNextPatient`.
    *   Publish sự kiện `PatientCalledEvent` qua `LocalStorage.PubSub`.
2.  **Màn hình Hiển thị (Subscriber)**:
    *   Nhận sự kiện `PatientCalledEvent`.
    *   Cập nhật UI (Số phiếu, Tên bệnh nhân).
    *   Phát âm thanh (Audio Notification).

**Cấu trúc bản tin sự kiện (Event Message):**
*   `PatientName`: Tên bệnh nhân (đã che giấu một phần).
*   `QueueNumber`: Số thứ tự.
*   `RoomName`: Tên phòng/quầy.
*   `Priority`: Mức ưu tiên.

### 3.3. Tích hợp Backend
*   **Lưu trữ**: Trạng thái hàng đợi được lưu trữ persistent trong Database (`HIS_SERVICE_REQ`, `HIS_QUEUE`).
*   **Caching**: `HIS.Desktop.LocalStorage.BackendData` cache danh sách chờ để giảm tải cho Server và hỗ trợ offline tạm thời.
*   **API**: `HIS.Desktop.ApiConsumer` cung cấp các endpoint `/api/queue/*`.

## 4. Database Schema (Tham chiếu)
*   `HIS_SERVICE_REQ`: Yêu cầu dịch vụ (đại diện cho một lượt chờ).
*   `HIS_PATIENT_TYPE_ALTER`: Lịch sử chuyển đổi đối tượng.
*   `V_HIS_ROOM_COUNTER`: View tổng hợp số lượng đang chờ theo phòng.

## 5. Cấu hình (Config)
Các key cấu hình trong `App.config` hoặc `SysConfig`:
*   `QUEUE_AUTO_CALL_INTERVAL`: Thời gian tự động gọi số tiếp theo.
*   `CALL_PATIENT_VERSION`: Chỉ định phiên bản plugin sử dụng (V6 hay Ver7).
