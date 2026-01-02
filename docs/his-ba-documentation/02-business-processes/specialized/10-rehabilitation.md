# Quy trình Phục hồi Chức năng (Rehabilitation)

## 1. Tổng quan
Phân hệ Phục hồi Chức năng (PHCN) hỗ trợ quản lý toàn diện quy trình điều trị Vật lý trị liệu và Phục hồi chức năng cho người bệnh, từ khâu chỉ định, tiếp nhận đến thực hiện các kỹ thuật tập luyện và đánh giá kết quả.

## 2. Lưu đồ Quy trình

```mermaid
graph TB
    Start((Bắt đầu)) --> ChiDinh[1. Chỉ định Kỹ thuật PHCN]
    ChiDinh --> TiepNhan[2. Tiếp nhận tại Khoa PHCN]
    TiepNhan --> ThucHien[3. Thực hiện Kỹ thuật/Bài tập]
    
    subgraph "Thực hiện Kỹ thuật"
        ThucHien --Ghi nhận--> DienBien[Diễn biến Trước/Sau tập]
        ThucHien --Ghi nhận--> SinhHieu[Dấu hiệu sinh tồn]
        ThucHien --Quản lý--> DotTap[Các đợt tập (RehaTrain)]
    end
    
    DienBien --> KetThuc[4. Kết thúc & Trả kết quả]
    KetThuc --> End((Kết thúc))
```

## 3. Chi tiết Các bước & Mapping Plugin

### 3.1. Chỉ định (Indication)
Bác sĩ tại phòng khám hoặc khoa điều trị chỉ định các dịch vụ PHCN (VD: Tập vận động, Siêu âm trị liệu, Kéo giãn cột sống...).
*   **Thao tác**: Thực hiện trên Form Chỉ định dịch vụ chung (`AssignService`).

### 3.2. Thực hiện Kỹ thuật (Execution)
Tại khoa PHCN, Kỹ thuật viên/Bác sĩ tiếp nhận yêu cầu và thực hiện kỹ thuật.
*   **Plugin chính**: `HIS.Desktop.Plugins.RehaServiceReqExecute` - Màn hình thực hiện kỹ thuật PHCN.
*   **Chức năng chi tiết**:
    *   **Ghi nhận tình trạng**:
        *   *Triệu chứng cơ năng (Symptom)*: Trước và Sau khi tập.
        *   *Tình trạng hô hấp (Respiratory)*: Trước và Sau khi tập.
        *   *Điện tim (ECG)*: Trước và Sau khi tập (nếu cần).
        *   *Lời dặn (Advise)*: Hướng dẫn bệnh nhân tự tập luyện.
    *   **Quản lý Đợt tập (RehaTrain)**:
        *   Một chỉ định (ServiceReq) có thể bao gồm nhiều buổi tập/đợt tập.
        *   Hệ thống cho phép ghi nhận chi tiết thời gian và kết quả của từng đợt tập.
    *   **Dấu hiệu sinh tồn**: Ghi nhận mạch, nhiệt độ, huyết áp trong quá trình tập (`VitalSigns`).

### 3.3. Dữ liệu Danh mục
Để vận hành quy trình, hệ thống sử dụng các danh mục cấu hình chuyên biệt:
*   **Plugin cấu hình**:
    *   `HIS.Desktop.Plugins.HisRehaServiceTypeList`: Định nghĩa các loại hình dịch vụ PHCN.
    *   `HIS.Desktop.Plugins.HisRehaTrainUnit`: Quản lý danh mục đơn vị luyện tập.

## 4. Dữ liệu Đầu ra
*   **Phiếu kết quả PHCN**: In phiếu ghi nhận quá trình tập luyện, diễn biến bệnh và kết luận của bác sĩ/KTV.
*   **Dữ liệu BHYT**: Tự động tổng hợp chi phí và đẩy lên cổng giám định BHYT theo quy định.

## 5. Liên kết Tài liệu
*   [Quy trình Chỉ định Dịch vụ](../clinical/02-service-indication.md)
