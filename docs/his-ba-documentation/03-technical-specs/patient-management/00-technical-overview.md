# Technical Spec: Tổng quan Quản lý Người bệnh (Patient Management)

## 1. Tổng quan Kiến trúc
Domain **Patient Management** chịu trách nhiệm quản lý toàn bộ vòng đời kỹ thuật của thông tin bệnh nhân, từ lúc lấy số xếp hàng, đăng ký thông tin nhân khẩu học (MPI), đến các hoạt động công tác xã hội và y tế gia đình.

Hệ thống được thiết kế theo mô hình **Service-Oriented** với các module chính:
1.  **Queue System (Hàng đợi)**: Quản lý luồng vào của bệnh nhân, sử dụng cơ chế Pub/Sub để đồng bộ real-time.
2.  **Master Patient Index (MPI)**: Định danh duy nhất người bệnh toàn hệ thống.
3.  **Social Service**: Tích hợp quản lý quỹ và yêu cầu hỗ trợ.

## 2. Bản đồ Module & Plugin

| Phân hệ Nghiệp vụ | Mã Plugin Chính | Namespaces / Dự án | Vai trò Kỹ thuật |
| :--- | :--- | :--- | :--- |
| **Quản lý Hàng đợi** | `HIS.Desktop.Plugins.CallPatient*` | `HIS.Desktop.Plugin.CallPatient` | Xử lý logic xếp hàng, gọi số, hiển thị LCD/LED. |
| **Hồ sơ MPI** | `HIS.Desktop.Plugins.PatientInfo` | `HIS.Desktop.Plugins.PatientInfo` | CRUD thông tin hành chính, merge hồ sơ, quản lý thẻ BHYT. |
| **Công tác Xã hội** | `HIS.Desktop.Plugins.CustomerRequest` | `HIS.Desktop.Plugins.CustomerRequest` | Quản lý yêu cầu, đánh giá mức hỗ trợ. |
| **Quỹ Từ thiện** | `HIS.Desktop.Plugins.HisFund` | `HIS.Desktop.Plugins.HisFund` | Quản lý nguồn quỹ, phiếu thu/chi từ thiện. |
| **Thông tin Hộ gia đình** | `HID.Desktop.Plugins.FamilyInformation` | `HID.Desktop.Plugins.FamilyInformation` | Quản lý mối quan hệ hộ gia đình (Extension). |

## 3. Patterns & Technologies (Đặc thù Domain)
*   **Pub/Sub Messaging**: Sử dụng cho hệ thống Gọi số để đảm bảo độ trễ thấp (Low Latency) giữa máy bác sĩ và màn hình hiển thị.
*   **Data Validation Rules**: Áp dụng chặt chẽ cho thông tin thẻ BHYT (Check tổng 13, check thẻ hợp lệ với cổng giám định).
*   **External Integration**: Tích hợp với Cổng Giám định BHYT Quốc gia để lấy thông tin bệnh nhân.

## 4. Database Core Tables
*   `HIS_PATIENT`: Bảng chính lưu thông tin MPI.
*   `HIS_SERVICE_REQ`: Lưu thông tin lượt xếp hàng/khám.
*   `HIS_PATIENT_PROGRAM`: Chương trình hỗ trợ người bệnh.
*   `HIS_FUND`: Quỹ từ thiện.
