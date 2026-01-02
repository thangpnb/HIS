# Quy trình Chẩn đoán Hình ảnh (Diagnostic Imaging)

## 1. Tổng quan
Quy trình mô tả các hoạt động thực hiện kỹ thuật chẩn đoán hình ảnh (X-Quang, CT Scanner, MRI, Siêu âm) và kết nối với hệ thống lưu trữ và truyền tải hình ảnh (PACS).

## 2. Lưu đồ Quy trình

```mermaid
graph TB
    Start((Bắt đầu)) --> ChiDinh[1. Chỉ định CĐHA]
    ChiDinh --> TiepDon[2. Tiếp đón tại Khoa CĐHA]
    TiepDon --> ThucHien[3. Thực hiện Chụp/Chiếu]
    
    subgraph "PACS Integration"
        ThucHien --Gửi chỉ định--> MayChup[Máy Chụp (Modality)]
        MayChup --Gửi ảnh DICOM--> PACS[Hệ thống PACS]
        PACS --Trả Link xem ảnh--> HIS
    end
    
    ThucHien --> DocKQ[4. Bác sĩ Đọc & Trả kết quả]
    DocKQ --> InTra[5. In Phim/Đĩa & Trả KQ]
```

## 3. Chi tiết Các bước & Mapping Plugin

### 3.1. Tiếp đón & Sắp xếp phòng (Reception)
Bệnh nhân sau khi được chỉ định sẽ đến khoa CĐHA. Nhân viên tiếp đón xác nhận và xếp vào phòng chụp.
*   **Plugin chính**:
    *   `HIS.Desktop.Plugins.DiimRoom`: Quản lý danh sách chờ tại phòng chụp X-Quang/Siêu âm.
    *   `HIS.Desktop.Plugins.CallPatientCLS`: Gọi loa mời bệnh nhân vào phòng chụp.

### 3.2. Thực hiện Kỹ thuật (Execution)
Kỹ thuật viên (KTV) thực hiện chụp chiếu cho bệnh nhân.
*   **Plugin chính**:
    *   `HIS.Desktop.Plugins.ExecuteRoom`: Màn hình thực hiện kỹ thuật.
    *   `HIS.Desktop.Plugins.VatTuTieuHao`: Ghi nhận phim, thuốc cản quang tiêu hao cho ca chụp.

### 3.3. Kết nối PACS (PACS Integration)
Hệ thống tích hợp sâu với PACS để tự động hóa quy trình.
*   **Plugin chính**:
    *   `HIS.Desktop.Plugins.PacsApiConsumer`: Module giao tiếp API với PACS System.
    *   `HIS.Desktop.Plugins.ImageRequestADO`: Gửi thông tin bệnh nhân (Worklist) xuống máy chụp.
    *   `HIS.Desktop.Plugins.ViewImage`: Tích hợp trình xem ảnh DICOM (Web Viewer) ngay trên HIS.

### 3.4. Đọc và Trả kết quả (Reporting)
Bác sĩ CĐHA xem ảnh trên PACS và nhập kết quả mô tả trên HIS.
*   **Plugin chính**:
    *   `HIS.Desktop.Plugins.ServiceReqResult`: Nhập kết quả đọc phim.
    *   `HIS.Desktop.Plugins.DiimServiceType`: Cấu hình mẫu mô tả kết quả (Template) cho từng loại kỹ thuật (X-Quang ngực, CT Sọ não...).

## 4. Dữ liệu Đầu ra
*   **Phiếu kết quả CĐHA**: Có QR Code hoặc Link để xem ảnh trực tuyến.
*   **Phim/Đĩa CD**: (Tùy chọn) Nếu bệnh nhân yêu cầu in.

## 5. Liên kết Tài liệu
*   [Quy trình Chỉ định CLS](../clinical/02-service-indication.md).
