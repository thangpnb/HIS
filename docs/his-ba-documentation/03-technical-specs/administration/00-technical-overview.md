# Technical Overview: Phân hệ Quản trị (Administration)

## 1. Tổng quan Kiến trúc
Phân hệ **Administration** đóng vai trò xương sống của hệ thống HIS, cung cấp các dịch vụ nền tảng (Foundation Services) và các quy trình quản lý hành chính bệnh viện.
Namespace chính: `Global.ACS.*` (Access Control), `HIS.Desktop.Plugins.System.*` (System Data), và các module nghiệp vụ hành chính khác.

## 2. Danh sách Module & Mapping
Bảng dưới đây ánh xạ các tài liệu kỹ thuật với quy trình nghiệp vụ tương ứng:

| Tech Spec | Business Process | Plugin Chính (Key Plugin) | Môt tả ngắn |
|:---|:---|:---|:---|
| `01-access-control.md` | `01-access-control-business.md` | `Global.ACS.Desktop.Plugins.AcsUser` | Quản lý người dùng, phân quyền, bảo mật. |
| `02-death-management.md` | `02-death-management.md` | `HIS.Desktop.Plugins.DeathMngt` | Quản lý báo tử, nhà đại thể. |
| `03-medical-record-storage.md`| `03-medical-record-storage.md` | `HIS.Desktop.Plugins.MedicalRecordStorage`| Lưu trữ hồ sơ bệnh án, mượn/trả hồ sơ. |
| `03-1-system-data.md` | `03-system-data-business.md` | `HIS.Desktop.Plugins.Sda*` | Quản lý danh mục dùng chung (ICD, Dân tộc...).|
| `04-reporting-system.md` | `04-reporting-system.md` | `HIS.Desktop.Plugins.SarReport` | Hệ thống báo cáo tổng hợp. |
| `05-queue-management.md` | `05-queue-management.md` | `HIS.Desktop.Plugins.Queue*` | Quản lý hàng đợi, QMS. |
| `06-voice-assistant.md` | `06-voice-assistant.md` | `HIS.Desktop.Plugins.Voice` | Trợ lý giọng nói (Speech-to-Text). |
| `07-shift-handover.md` | `07-shift-handover.md` | `HIS.Desktop.Plugins.ShiftHandover` | Bàn giao ca trực. |
| `08-incident-reporting.md` | `08-incident-reporting.md` | `HIS.Desktop.Plugins.Incident` | Báo cáo sự cố y khoa. |
| `09-infection-control.md` | `09-infection-control.md` | *(Out of Scope)* | Kiểm soát nhiễm khuẩn. |
| `13-duty-rostering.md` | `13-duty-rostering.md` | `HIS.Desktop.Plugins.DutyRostering` | Phân lịch trực. |

## 3. Kiến trúc Dữ liệu Chung
Các bảng dữ liệu nền tảng thường được sử dụng chéo giữa các module:
*   **ACS_USER / ACS_ROLE**: Quản lý định danh.
*   **HIS_DEPARTMENT / HIS_ROOM**: Cơ cấu tổ chức.
*   **HIS_EMPLOYEE**: Hồ sơ nhân sự (liên kết với ACS_USER).
*   **HIS_BRANCH**: Chi nhánh bệnh viện.

## 4. Các Điểm Tích hợp Chính (Integration Points)
*   **Authentication Service**: Cung cấp Token và Context cho toàn bộ Client.
*   **Logging Service**: `HIS.Desktop.Plugins.Log` ghi nhận mọi vết thao tác.
*   **Configuration Service**: `HIS.Desktop.Plugins.Config` quản lý cấu hình động.
