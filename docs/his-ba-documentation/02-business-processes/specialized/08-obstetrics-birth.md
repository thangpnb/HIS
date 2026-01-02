# Quy trình Sản khoa & Đỡ đẻ (Obstetrics & Birth)

## 1. Tổng quan
Quy trình quản lý các hoạt động tiếp nhận thai phụ, theo dõi chuyển dạ, thực hiện đỡ đẻ/phẫu thuật lấy thai và ghi nhận thông tin trẻ sơ sinh chào đời (Chứng sinh).

## 2. Lưu đồ Quy trình

```mermaid
graph TB
    Start((Bắt đầu)) --> TiepDon[1. Tiếp đón & Nhập viện]
    TiepDon --> TheoDoi[2. Theo dõi Chuyển dạ (Partogram)]
    TheoDoi --> ChiDinh[3. Chỉ định Sanh/Mổ]
    
    ChiDinh -- Sanh Thường --> DoDe[4a. Đỡ đẻ]
    ChiDinh -- Mổ lấy thai --> Mo[4b. Phẫu thuật]
    
    DoDe & Mo --> GhiNhan[5. Ghi nhận Thông tin Sinh (InfantInformation)]
    GhiNhan --> ChungSinh[6. Cấp Giấy Chứng sinh]
    ChungSinh --> End((Kết thúc))
```

## 3. Chi tiết Các bước & Mapping Plugin

### 3.1. Theo dõi Chuyển dạ
Sử dụng bệnh án điện tử để ghi nhận diễn biến chuyển dạ (cơn co, tim thai, độ mở cổ tử cung).
*   **Plugin chính**:
    *   `HIS.Desktop.Plugins.TreatmentFinish`: Hồ sơ bệnh án sản khoa.
    *   `HIS.Desktop.Plugins.SurgeryConfig`: Quản lý kíp đỡ đẻ/phẫu thuật (Ekip).

### 3.2. Ghi nhận Thông tin Sinh (Birth Recording)
Ngay sau khi trẻ chào đời, nữ hộ sinh/bác sĩ ghi nhận thông tin chi tiết của cuộc đẻ và thông tin trẻ sơ sinh.
*   **Plugin chính**:
    *   `HIS.Desktop.Plugins.InfantInformation`: Màn hình quản lý thông tin trẻ sơ sinh.
        *   *Thông tin Mẹ*: Tiền sử sản khoa, quá trình chuyển dạ.
        *   *Thông tin Con*: Giới tính, Cân nặng, Chiều cao, Chỉ số Apgar, Tình trạng sau sinh.
        *   *Kết quả Sinh*: Sống/Chết, Dị tật (nếu có).
    *   `HIS.Desktop.Plugins.HisBornPosition`: Danh mục tư thế sinh (Ngôi chỏm, Ngôi mông...).
    *   `HIS.Desktop.Plugins.HisBornResult`: Danh mục kết quả sinh.

### 3.3. Cấp Giấy Chứng sinh
Hệ thống## 1. Tình trạng Hiện tại
*   **Mã nguồn**:
    *   **Quản lý Sổ sinh**: Có plugin `HIS.Desktop.Plugins.HisBirthCertBook` để cấp giấy chứng sinh.
    *   **Khám Sản khoa**: Tích hợp trong module Khám bệnh chung (`HIS.Desktop.Plugins.HisExamServiceTemp`), sử dụng trường dữ liệu `PART_EXAM_OBSTETRIC` (Khám chuyên khoa Sản) trong phiếu khám.
    *   **Quản lý Chuyển dạ/Sinh**: Chưa có module chuyên sâu quản lý biểu đồ chuyển dạ (Partogram) hay diễn biến cuộc sinh.
*   **Tài liệu**: Đã sơ lược quy trình, cần bổ sung chi tiết kỹ thuật. tự động trích xuất dữ liệu từ `InfantInformation` để in Giấy chứng sinh theo mẫu quy định của Bộ Y tế (Thông tư 17/2012/TT-BYT).
*   **Dữ liệu**: Đồng bộ thông tin Họ tên mẹ, Nơi sinh, Thời gian sinh chính xác từng phút.

## 4. Dữ liệu Đầu ra
*   **Giấy Chứng sinh**: Chứng từ pháp lý quan trọng nhất để làm khai sinh cho trẻ.
*   **Hồ sơ bệnh án Sản khoa**: Đầy đủ phiếu phẫu thuật/thủ thuật, phiếu theo dõi chức năng sống.

## 5. Liên kết Tài liệu
*   [Quy trình Nội trú](../clinical/04-daily-treatment.md).
*   [Quy trình Chăm sóc Sơ sinh](./09-newborn-care.md).
