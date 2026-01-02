# Quy trình Quản lý Phẫu thuật - Thủ thuật (Surgery & Procedure)

## 1. Tổng quan
Quy trình quản lý toàn bộ vòng đời của một ca phẫu thuật/thủ thuật, từ lúc đăng ký lịch mổ, duyệt mổ, thực hiện trong phòng mổ đến khi kết thúc và tính toán thù lao cho kíp mổ.

## 2. Lưu đồ Quy trình

```mermaid
graph TB
    Start((Bắt đầu)) --> DangKy[1. Đăng ký Lịch mổ/Dự trù]
    DangKy --> Duyet[2. Duyệt Lịch mổ & Ekip]
    Duyet --> ChuanBi[3. Chuẩn bị (Bệnh nhân & VTYT)]
    
    subgraph "Tại Phòng mổ"
        ChuanBi --> GayMe[4. Khám tiền mê & Gây mê]
        GayMe --> ThucHien[5. Thực hiện PTTT]
        ThucHien --> TuongTrinh[6. Viết Tường trình PTTT]
        TuongTrinh --> KetThuc[7. Kết thúc ca mổ]
    end
    
    KetThuc --> HoiTinh[8. Hồi tỉnh & Chăm sóc sau mổ]
    KetThuc --> ThuLao[9. Tính Thù lao Phẫu thuật]
```

## 3. Chi tiết Các bước & Mapping Plugin

### 3.1. Đăng ký & Duyệt Lịch mổ (Registration & Approval)
Bác sĩ lâm sàng đăng ký lịch mổ phiên hoặc mổ cấp cứu. Lãnh đạo khoa/viện duyệt lịch.
*   **Plugin chính**:
    *   `HIS.Desktop.Plugins.SurgServiceReqExecute`: Màn hình quản lý chính của hệ phẫu thuật.
    *   `HIS.Desktop.Plugins.SurgAssignAndCopy`: Chỉ định kỹ thuật mổ.
    *   `HIS.Desktop.Plugins.PtttTemp`: Quản lý các mẫu/gói phẫu thuật.

### 3.2. Quản lý Ekip & Phương pháp (Team & Method)
Xác định các thành viên tham gia kíp mổ (Phẫu thuật viên chính, phụ, gây mê, dụng cụ...) và phương pháp vô cảm.
*   **Plugin chính**:
    *   `HIS.Desktop.Plugins.HisEkip`: Quản lý danh sách thành viên kíp.
    *   `HIS.Desktop.Plugins.PtttMethod`: Danh mục phương pháp phẫu thuật (Nội soi, Mổ hở...).
    *   `HIS.Desktop.Plugins.HisEmotionlessMethod`: Phương pháp vô cảm (Gây mê nội khí quản, Tê tủy sống...).

### 3.3. Tường trình Phẫu thuật (Operation Report)
Sau khi mổ, bác sĩ phẫu thuật chính viết tường trình chi tiết các bước thực hiện.
*   **Plugin chính**:
    *   `HIS.Desktop.Plugins.SurgServiceReqExecute` (Tab Tường trình): Soạn thảo tường trình (Text editor).
    *   `HIS.Desktop.Plugins.ViewImage`: Đính kèm ảnh chụp/video trong quá trình mổ (nếu có).
    *   `HIS.Desktop.Plugins.SkinSurgeryDesADO`: Mô tả vùng da/vị trí phẫu thuật.

### 3.4. Quản lý Thù lao (Remuneration)
Tính toán chế độ bồi dưỡng phẫu thuật thủ thuật theo quy định (QĐ 73/2011/QĐ-TTg).
*   **Plugin chính**:
    *   `HIS.Desktop.Plugins.HisSurgRemuneration`: Tính toán tiền bồi dưỡng cho từng thành viên kíp dựa trên vai trò và loại phẫu thuật (Đặc biệt, I, II, III).
    *   `HIS.Desktop.Plugins.Remuneration`: Báo cáo tổng hợp thù lao.

## 4. Dữ liệu Đầu ra
*   **Phiếu Tường trình Phẫu thuật**: Lưu hồ sơ bệnh án.
*   **Phiếu Cam đoan PTTT**: Đã ký bởi bệnh nhân/người nhà.
*   **Danh sách vật tư tiêu hao**: Đã sử dụng trong ca mổ (để trừ kho và thanh toán).

## 5. Liên kết Tài liệu
*   [Quy trình Điều trị Nội trú](../clinical/04-daily-treatment.md).
