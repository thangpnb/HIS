# Technical Overview: Y tế Dự phòng (Preventive Medicine)

## 1. Giới thiệu (Introduction)
Module Y tế Dự phòng quản lý các hoạt động tiêm chủng và phòng chống dịch bệnh. Module này tích hợp chặt chẽ với Quy trình Khám ngoại trú để sàng lọc và Dược để quản lý kho vắc-xin.

## 2. Kiến trúc Module (Module Architecture)
### 2.1. Các thành phần chính (Core Components)
*   **Vaccination Check-up (Khám sàng lọc)**:
    *   Thực hiện tại phòng khám.
    *   Bác sĩ đánh giá tình trạng sức khỏe trước khi tiêm.
    *   Plugin: `HIS.Desktop.Plugins.UpdateVaccinationExam`.
*   **Vaccination Management (Quản lý Tiêm chủng)**:
    *   Quản lý danh sách đối tượng tiêm, lịch sử tiêm.
    *   Thực hiện tiêm và ghi nhận phản ứng sau tiêm.
    *   Plugin: `HIS.Desktop.Plugins.Vaccination`.
*   **Nacional Data Integration (Liên thông CSDL Quốc gia)**:
    *   Xuất dữ liệu XML/Excel theo chuẩn của Cục Y tế Dự phòng.

### 2.2. Sơ đồ dữ liệu (Database Schema Overview)
Các bảng chính trong schema `HIS`:
*   `HIS_VACCINATION_EXAM`: Lưu kết quả khám sàng lọc (Huyết áp, nhiệt độ, kết luận đủ điều kiện tiêm).
*   `HIS_VACCINATION`: Lưu thông tin mũi tiêm (Loại vắc-xin, lô sản xuất, người tiêm, vị trí tiêm).
*   `HIS_VACCINATION_RESULT`: Danh mục kết quả (Đã tiêm, Hoãn tiêm...).
*   `HIS_VACCINATION_REACT`: Ghi nhận phản ứng sau tiêm (Sốt, sưng đau...).

## 3. Điểm tích hợp (Integration Points)
*   **Kho Dược (Pharmacy)**: Trừ tồn kho vắc-xin thông qua phiếu xuất (`HIS_EXP_MEST`).
*   **Thu Ngân (Cashier)**: Thanh toán chi phí vắc-xin dịch vụ (nếu có).
*   **Khám bệnh (Examination)**: Kế thừa thông tin bệnh nhân từ tiếp đón.

## 4. Công nghệ & Thư viện (Tech Stack)
*   Form/UserControl: DevExpress.
*   Backend Communication: MOS API (SDO Mapping).
*   Reports: Crystal Reports (cho phiếu khám sàng lọc và giấy chứng nhận).
