# Quy trình Quản lý Tử vong (Death Management)

## 1. Tổng quan
Quy trình này hướng dẫn các bước xử lý khi có bệnh nhân tử vong tại bệnh viện, bao gồm ghi nhận y khoa, lập hồ sơ tử vong, cấp giấy báo tử và bàn giao thi hài.

## 2. Lưu đồ Quy trình

```mermaid
graph TB
    Start((Bắt đầu)) --> XacNhan[1. Xác nhận Tử vong (Bác sĩ)]
    XacNhan --> HoSo[2. Hoàn thiện Bệnh án Tử vong]
    
    HoSo --> GhiNhan[3. Ghi nhận Thông tin (DeathInformation)]
    GhiNhan --> KiemThao[4. Kiểm thảo Tử vong]
    
    KiemThao --> BaoTu[5. Cấp Giấy Báo tử]
    BaoTu --> NhaDaiThe[6. Chuyển Nhà Đại thể]
    NhaDaiThe --> BanGiao[7. Bàn giao cho Gia đình]
    BanGiao --> End((Kết thúc))
```

## 3. Chi tiết Các bước & Mapping Plugin

### 3.1. Ghi nhận Thông tin Tử vong
Sau khi bác sĩ lâm sàng xác nhận bệnh nhân đã tử vong, cần cập nhật trạng thái hồ sơ và ghi nhận nguyên nhân.
*   **Plugin chính**:
    *   `HIS.Desktop.Plugins.DeathInformationList`: Quản lý danh sách bệnh nhân tử vong.
        *   *Dữ liệu*: Thời gian tử vong, Nguyên nhân chính (ICD-10), Nguyên nhân kèm theo, Có khám nghiệm tử thi không?
    *   `HIS.Desktop.Plugins.HisDeathCertBook`: Quản lý sổ cấp giấy báo tử.

### 3.2. Kiểm thảo Tử vong (Death Review)
Hội đồng chuyên môn thực hiện kiểm thảo để rút kinh nghiệm.
*   **Plugin chính**:
    *   `HIS.Desktop.Plugins.TreatmentFinish`: Chức năng tổng kết bệnh án tử vong.
    *   `HIS.Desktop.Plugins.EmrDocument`: Lưu trữ biên bản kiểm thảo tử vong (số hóa).

### 3.3. Cấp Giấy Báo tử
In giấy báo tử từ hệ thống để gia đình làm thủ tục mai táng và khai tử.
*   **Chức năng**:
    *   Tự động điền thông tin hành chính, nguyên nhân tử vong.
    *   Quản lý số seri giấy báo tử để tránh thất thoát.

## 4. Dữ liệu Đầu ra
*   **Giấy Báo tử**: Theo mẫu quy định của Bộ Tư pháp/Bộ Y tế.
*   **Trích lục tử vong**: Phục vụ báo cáo thống kê (A6).

## 5. Liên kết Tài liệu
*   [Quy trình Nội trú](../clinical/04-daily-treatment.md).
