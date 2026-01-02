# Quy trình Tiêm chủng & Y tế Dự phòng (Preventive Medicine)

## 1. Tổng quan
Quy trình mô tả hoạt động quản lý tiêm chủng mở rộng/dịch vụ tại bệnh viện và các hoạt động y tế dự phòng (quản lý bệnh truyền nhiễm, sức khỏe cộng đồng).

## 2. Lưu đồ Quy trình

```mermaid
graph TB
    Start((Bắt đầu)) --> TiepDon[1. Tiếp đón & Sàng lọc]
    TiepDon --> KhamSangLoc[2. Khám Sàng lọc & Chỉ định]
    KhamSangLoc --> ThanhToan[3. Thanh toán (Nếu vắc-xin DV)]
    ThanhToan --> TiemChung[4. Thực hiện Tiêm]
    TiemChung --> TheoDoi[5. Theo dõi sau Tiêm (30p)]
    TheoDoi --> CapGiay[6. Cấp Giấy chứng nhận]
    CapGiay --> End((Kết thúc))
```

## 3. Chi tiết Các bước & Mapping Plugin

### 3.1. Tiếp đón và Khám sàng lọc (Screening)
Bác sĩ khám sàng lọc để đảm bảo đủ điều kiện tiêm chủng (không sốt, không dị ứng...).
*   **Plugin chính**:
    *   `HIS.Desktop.Plugins.RegisterVaccination`: Đăng ký tiêm chủng.
    *   `HIS.Desktop.Plugins.UpdateVaccinationExam`: Ghi nhận kết quả khám sàng lọc.
    *   `HIS.Desktop.Plugins.Vaccination`: Quản lý chung về đợt tiêm.

### 3.2. Chỉ định Vắc-xin (Indication)
Chọn loại vắc-xin phù hợp. Hệ thống quản lý kho vắc-xin theo lô, hạn dùng nghiêm ngặt.
*   **Plugin chính**:
    *   `HIS.Desktop.Plugins.MedicineVaccinBill`: Tính tiền vắc-xin dịch vụ.
    *   `HIS.Desktop.Plugins.InventoryVaccine`: Quản lý xuất nhập tồn vắc-xin.

### 3.3. Thực hiện Tiêm và Theo dõi (Injection & Monitoring)
Điều dưỡng thực hiện tiêm và ghi nhận thông tin lô vắc-xin, vị trí tiêm.
*   **Plugin chính**:
    *   `HIS.Desktop.Plugins.VaccinationExecute`: Màn hình thực hiện tiêm.
    *   `HIS.Desktop.Plugins.VaccinationReaction`: Ghi nhận phản ứng sau tiêm (nếu có).

### 3.4. Báo cáo Y tế Dự phòng (Preventive Reports)
Kết xuất dữ liệu để báo cáo lên hệ thống Tiêm chủng Quốc gia.
*   **Plugin chính**:
    *   `HIS.Desktop.Plugins.ExportVaccination`: Xuất dữ liệu XML/Excel báo cáo.
    *   `TYT.Desktop.Plugins.TytHiv`: Quản lý chương trình HIV/AIDS (Y tế xã/dự phòng).

## 4. Dữ liệu Đầu ra
*   **Giấy Chứng nhận Tiêm chủng**: In trực tiếp từ phần mềm.
*   **Sổ Tiêm chủng Điện tử**: Đồng bộ lịch sử tiêm.

## 5. Liên kết Tài liệu
*   [Quy trình Khám ngoại trú](../clinical/01-outpatient-examination.md).
