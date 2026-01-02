# Quản lý Hồ sơ & Lịch sử Người bệnh (MPI Details)

## 1. Tổng quan
Hệ thống sử dụng Master Patient Index (MPI) để định danh duy nhất người bệnh trên toàn hệ thống, bất kể họ khám ở khoa nào hay thời điểm nào. Đồng thời cung cấp công cụ xử lý các trường hợp trùng lặp hồ sơ.

## 2. Các Quy trình Chính

### 2.1. Quản lý Thông tin Hành chính (Patient Info)
Cập nhật các thông tin nhân khẩu học (Họ tên, Ngày sinh, Địa chỉ, Nghề nghiệp...) và thông tin thẻ BHYT.
*   **Plugin chính**: 
    *   `HIS.Desktop.Plugins.PatientInfo`: Xem và sửa thông tin hành chính.
    *   `HIS.Desktop.Plugins.PatientUpdate`: Cập nhật thông tin khi bệnh nhân quay lại khám.
    *   `HIS.Desktop.Plugins.CheckInfoBHYT`: Đồng bộ thông tin từ cổng giám định BHYT.

### 2.2. Xử lý Trùng lặp (Merge Patient)
Trong trường hợp một bệnh nhân có nhiều mã (do quên thẻ, đăng ký nhầm...), hệ thống cho phép gộp các hồ sơ này lại.
*   **Quy tắc gộp**:
    *   Hồ sơ A (Mã A) gộp vào Hồ sơ B (Mã B).
    *   Tất cả lịch sử khám, đơn thuốc, cận lâm sàng của A sẽ được chuyển sang B.
    *   Mã A sẽ bị hủy (Vô hiệu hóa).
*   **Plugin chính**:
    *   `HIS.Desktop.Plugins.PatientList`: Tìm kiếm bệnh nhân trùng.
    *   `HIS.Desktop.Plugins.MergePatient` (hoặc chức năng trong Admin): Thực hiện lệnh gộp.

### 2.3. Tra cứu Lịch sử Khám (Patient History)
Xem lại toàn bộ lịch sử khám chữa bệnh của bệnh nhân theo trình tự thời gian.
*   **Plugin chính**:
    *   `HIS.Desktop.Plugins.TreatmentHistory`: Lịch sử các đợt điều trị (Nội trú/Ngoại trú).
    *   `HIS.Desktop.Plugins.PrescriptionList`: Lịch sử dùng thuốc.
    *   `HIS.Desktop.Plugins.TestHistory`: Lịch sử xét nghiệm.

## 3. Quản lý Nhóm Bệnh nhân Đặc thù
Hệ thống hỗ trợ phân loại bệnh nhân để áp dụng các chính sách ưu đãi hoặc quản lý riêng.
*   **Plugin chính**:
    *   `HIS.Desktop.Plugins.PatientClassify`: Phân loại (Ví dụ: VIP, Người nước ngoài, Cán bộ...).
    *   `HIS.Desktop.Plugins.HisHivGroupPatient`: Nhóm bệnh nhân HIV/ARV.
    *   `HIS.Desktop.Plugins.HisBhytBlacklist`: Danh sách thẻ BHYT bị chặn/cảnh báo.

## 4. Liên kết Tài liệu
*   [Quy trình Tiếp đón Ban đầu](../clinical/01-outpatient-examination.md#31-tiếp-đón--đăng-ký-reception)
