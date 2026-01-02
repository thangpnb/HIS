# Quản lý Bệnh Truyền nhiễm & Bệnh xã hội (Infectious Diseases)

## 1. Tổng quan
Phân hệ Quản lý Bệnh Truyền nhiễm trong HIS tập trung vào việc quản lý điều trị chuyên sâu cho các bệnh nhân mắc bệnh xã hội (HIV/AIDS, Lao) tại cơ sở khám chữa bệnh (Bệnh viện), khác biệt với quy trình quản lý cộng đồng và dự phòng tại Trạm Y tế (TYT).

Hệ thống hỗ trợ theo dõi xuyên suốt quá trình điều trị, quản lý phác đồ phức tạp và báo cáo số liệu chuyên môn theo quy định của các chương trình mục tiêu quốc gia.

## 2. Quản lý Điều trị HIV/AIDS (`HisHivTreatment`)

### 2.1. Quy trình Nghiệp vụ
Quản lý hồ sơ bệnh án ngoại trú (OPD) dành cho bệnh nhân nhiễm HIV đang điều trị ARV tại phòng khám OPC của bệnh viện.

**Các bước chính:**
1.  **Tiếp nhận & Đăng ký**: Bệnh nhân đến khám định kỳ, đăng ký vào phòng khám OPC.
2.  **Cập nhật Thông tin HIV**:
    *   Phân loại đối tượng (Người lớn, Trẻ em, Phụ nữ mang thai).
    *   Trạng thái bệnh nhân (Mới, Chuyển đến, Tái điều trị...).
    *   Giai đoạn lâm sàng (I, II, III, IV).
3.  **Quản lý Phác đồ ARV**:
    *   Ghi nhận phác đồ khởi điểm (`BeginRegimen`).
    *   Ghi nhận và thay đổi phác đồ hiện tại (`CurrentRegimen`).
    *   Lý do thay đổi phác đồ (Tác dụng phụ, Thất bại điều trị...).
4.  **Theo dõi Xét nghiệm Tải lượng**:
    *   Ghi nhận kết quả xét nghiệm PCR / Tải lượng virus (Viral Load).
    *   Theo dõi diễn biến miễn dịch (CD4).
5.  **Cấp thuốc ARV**: Kết nối với quy trình kê đơn ngoại trú để xuất thuốc ARV (nguồn BHYT hoặc Dự án).

### 2.2. Mapping Plugin & Dữ liệu
*   **Plugin quản lý hồ sơ**: `HIS.Desktop.Plugins.HisHivTreatment`.
*   **Plugin quản lý danh mục phác đồ**: `HIS.Desktop.Plugins.HisRegimenHiv`.

**Các trường dữ liệu quan trọng (`HIS_HIV_TREATMENT`):**
*   `HIV_PATIENT_TYPE`: Phân loại bệnh nhân.
*   `REGIMEN_HIV_CODE`: Mã phác đồ hiện tại.
*   `ARV_PATIENT_BEGIN`: Ngày bắt đầu điều trị ARV.
*   `TEST_PCR_RESULT`: Kết quả xét nghiệm tải lượng virus.
*   `TUBERCULOSIS_TREATMENT_BEGIN`: Ngày bắt đầu điều trị Lao (đồng nhiễm).

## 3. Quản lý Điều trị Lao (`HisTuberculosisTreatment`)

### 3.1. Quy trình Nghiệp vụ
Hỗ trợ quản lý bệnh nhân lao theo chiến lược DOTS tại bệnh viện, bao gồm cả lao phổi và lao ngoài phổi.

**Các bước chính:**
1.  **Chẩn đoán & Phân loại**:
    *   Phân loại bệnh lao: Lao phổi (AFB +/-), Lao ngoài phổi.
    *   Tiền sử điều trị: Mới, Tái phát, Thất bại, Bỏ trị.
2.  **Lập Phác đồ Điều trị**:
    *   Chỉ định phác đồ theo chương trình Chống lao (VD: 2RHZE/4RH).
    *   Xác định ngày bắt đầu và dự kiến kết thúc.
3.  **Theo dõi Điều trị**:
    *   Quản lý cấp thuốc định kỳ.
    *   Theo dõi các mốc xét nghiệm đờm (tháng 2, 5, 7).
4.  **Kết thúc Điều trị**:
    *   Đánh giá kết quả: Khỏi, Hoàn thành, Thất bại, Chuyển đi, Tử vong.

### 3.2. Mapping Plugin & Dữ liệu
*   **Plugin quản lý hồ sơ**: `HIS.Desktop.Plugins.HisTuberclusisTreatment`.
*   **Plugin quản lý danh mục phác đồ**: `HIS.Desktop.Plugins.HisRegimenTuberclusis`.

**Các trường dữ liệu quan trọng (`HIS_TUBERCULOSIS_TREAT`):**
*   `TUBERCULOSIS_CLASSIFY_LOCATION`: Phân loại vị trí (Phổi/Ngoài phổi).
*   `TUBERCULOSIS_CLASSIFY_VK`: Kết quả vi khuẩn học (AFB).
*   `REGIMEN_TUBERCULOSIS_CODE`: Phác đồ điều trị.
*   `TUBERCULOSIS_TREATMEN_RESULT`: Kết quả điều trị.

## 4. Liên kết Dữ liệu (Đồng nhiễm HIV-Lao)
Hệ thống thiết kế sự liên kết chặt chẽ giữa hai phân hệ này để quản lý các ca đồng nhiễm (TB/HIV):
*   Trong hồ sơ HIV có các trường theo dõi điều trị Lao (`TuberculosisTreatmentBegin`, `TuberculosisRegimen`).
*   Trong hồ sơ Lao cập nhật trạng thái HIV và ngày bắt đầu ARV (`HivKdDate`, `ArvBeginTime`).

## 5. Báo cáo
Hệ thống cung cấp dữ liệu đầu ra cho các biểu mẫu báo cáo chương trình mục tiêu:
*   Báo cáo tình hình nhiễm HIV/AIDS.
*   Báo cáo thu nhận điều trị ARV.
*   Báo cáo Chương trình Chống lao (Thu nhận, Kết quả điều trị...).
