# Thiết kế Kỹ thuật: Phân hệ Đặc thù (Specialized)

## 1. Mục đích
Tài liệu này cung cấp cái nhìn tổng quan về kiến trúc kỹ thuật cho các phân hệ chuyên khoa đặc thù trong hệ thống HIS. Các phân hệ này thường có quy trình nghiệp vụ riêng biệt, sử dụng các Plugin chuyên dụng hoặc các Form mở rộng, nhưng vẫn tuân thủ kiến trúc lõi của HIS Desktop (xử lý `HIS_SERVICE_REQ`, `HIS_SERE_SERV`).

## 2. Danh sách Module & Mapping
Dưới đây là danh sách 12 phân hệ chuyên khoa đã được tài liệu hóa chi tiết:

| STT | Tài liệu | Plugin Chính (Namespace: `HIS.Desktop.Plugins.*`) | Ghi chú |
| :-- | :--- | :--- | :--- |
| 01 | [Phẫu thuật - Thủ thuật](./01-surgery-management.md) | `SurgServiceReqExecute`, `HisEkip`, `SurgAssignAndCopy` | Quản lý lịch mổ, ekip, tường trình |
| 02 | [Dinh dưỡng](./02-nutrition-management.md) | `AssignNutrition`, `RationSchedule`, `RationTime` | Quản lý suất ăn, thực đơn |
| 03 | [Thận nhân tạo](./03-hemodialysis.md) | `Hemodialysis` | Quản lý chạy thận, tủ thuốc |
| 04 | [Sàng lọc](./04-screening.md) | *(Gap Analysis)* | Hiện chưa có Plugin chuyên biệt |
| 05 | [Y học cổ truyền](./05-traditional-medicine.md) | `AssignPrescriptionYHCT`, `TraditionalMedicine*` | Kê đơn thang, sắc thuốc |
| 06 | [Hợp đồng KSK](./06-health-checkup-contracts.md) | `HisKskContract`, `HealthCheckup*` | Khám đoàn, khám lái xe |
| 07 | [Mắt - Khúc xạ](./07-optometry.md) | `Optometrist` | Đo khúc xạ, kê đơn kính |
| 08 | [Sản khoa](./08-obstetrics-birth.md) | `InfantInformation` (Shared), `HisBirthCertBook` | Quản lý sinh, thủ thuật sản |
| 09 | [Sơ sinh](./09-newborn-care.md) | `InfantInformation`, `HisBaby` | Chăm sóc bé, tiêm chủng |
| 10 | [Phục hồi chức năng](./10-rehabilitation.md) | `AssignService` (Shared Config) | Vật lý trị liệu (dùng chung core) |
| 11 | [Ung bướu](./11-oncology.md) | *(Out of Scope)* | Chưa triển khai chuyên sâu |
| 12 | [Truyền nhiễm](./12-infectious-diseases.md) | `HisHivTreatment`, `HisTuberclusisTreatment` | Quản lý HIV/Lao (Chương trình) |

## 3. Kiến trúc Dữ liệu Chung (Shared Schema)

### 3.1. Service Request Pattern
Hầu hết các module chuyên khoa đều xoay quanh các bảng lõi sau:
*   **`HIS_SERVICE_REQ`**: Yêu cầu dịch vụ (Phiếu chỉ định/Phiếu mổ/Phiếu ăn...).
    *   `SERVICE_REQ_TYPE_ID`: Phân loại phiếu (VD: Phẫu thuật, Suất ăn, Thủ thuật...).
*   **`HIS_SERE_SERV`**: Chi tiết dịch vụ thực hiện.
    *   `SERVICE_ID`: Dịch vụ kỷ thuật/Thuốc.
    *   `HEIN_LIMIT_PRICE`: Quản lý trần BHYT đặc thù.
*   **`HIS_SERE_SERV_EXT`**: Bảng mở rộng lưu trữ các trường dữ liệu đặc thù của chuyên khoa (VD: Độ cận/viễn thị, thông tin thai nhi...) mà không cần sửa cấu trúc bảng chính.

### 3.2. Program & Management
Đối với các bệnh mãn tính hoặc chương trình y tế (HIV, Lao, Thận nhân tạo):
*   **`HIS_PROGRAM`**: Quản lý việc đăng ký tham gia chương trình.
*   **`HIS_TREATMENT_END_TYPE`**: Kết thúc điều trị với các lý do đặc thù (Chuyển đơn vị quản lý, Tử vong do bệnh chuyên khoa...).

## 4. Giao tiếp & Tích hợp
*   **Core Integration**: Sử dụng `IDesktopRoot` để giao tiếp với module Tiếp đón và Viện phí.
*   **External Device**: Một số chuyên khoa (Mắt, Xét nghiệm) có thể tích hợp máy đo qua LIS/PACS hoặc nhập thủ công kết quả.
*   **Reporting**: Dữ liệu chuyên khoa thường yêu cầu các mẫu báo cáo đặc thù (Sổ Mê, Sổ Đẻ, Báo cáo HIV/Lao) được xử lý qua `HIS.Desktop.Plugins.Report`.
