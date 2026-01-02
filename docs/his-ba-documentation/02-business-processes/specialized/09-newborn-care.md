# Quy trình Chăm sóc Sơ sinh (Newborn Care)

## 1. Tổng quan
Quy trình chăm sóc sơ sinh bao gồm việc theo dõi sức khỏe trẻ ngay sau sinh, thực hiện tiêm chủng (Lao, Viêm gan B) và bàn giao trẻ cho gia đình hoặc chuyển tuyến điều trị (đối với trẻ bệnh lý).

## 2. Lưu đồ Quy trình

```mermaid
graph TB
    Start((Bắt đầu)) --> TiepNhan[1. Tiếp nhận Trẻ (Từ phòng sinh)]
    TiepNhan --> TheoDoi[2. Theo dõi & Chăm sóc hàng ngày]
    
    TheoDoi --> TiemChung[3. Tiêm chủng (VGB/Lao)]
    
    subgraph "Tình trạng Sức khỏe"
        TheoDoi -- Ổn định --> BanGiao[4a. Bàn giao Mẹ/Gia đình]
        TheoDoi -- Bệnh lý --> ChuyenKhoa[4b. Chuyển Khoa Nhi/Sơ sinh]
    end
    
    BanGiao --> XuatVien[5. Xuất viện]
    ChuyenKhoa --> End((Kết thúc))
```

## 3. Chi tiết Các bước & Mapping Plugin

### 3.1. Quản lý Hồ sơ Sơ sinh
Trẻ sơ sinh thường được tạo một hồ sơ riêng (mã bệnh nhân mới) hoặc quản lý kèm hồ sơ mẹ (tùy cấu hình).
*   **Plugin chính**:
    *   `HIS.Desktop.Plugins.InfantInformation`: Cập nhật thông tin hàng ngày (cân nặng, bú mẹ, vệ sinh).
    *   `HIS.Desktop.Plugins.TreatmentFinish`: Sử dụng để kê đơn thuốc, vật tư tiêu hao cho trẻ.

### 3.2. Tiêm chủng Sơ sinh (Vaccination)
Thực hiện tiêm mũi Viêm gan B và Lao trong vòng 24h đầu sau sinh.
*   **Plugin chính**:
    *   `HIS.Desktop.Plugins.VaccinationExecute` hoặc `HIS.Desktop.Plugins.RegisterVaccination`:
        *   Ghi nhận mã lô vắc-xin, thời gian tiêm, người tiêm.
        *   Sàng lọc trước tiêm (Nhiệt độ, Tim phổi).

### 3.3. Bàn giao & Xuất viện
Khi trẻ và mẹ ổn định, thực hiện thủ tục xuất viện.
*   **Plugin chính**:
    *   `HIS.Desktop.Plugins.InfantInformationList`: Theo dõi danh sách trẻ sơ sinh tại khoa.
    *   `HIS.Desktop.Plugins.TreatmentFinish`: Kết thúc điều trị cho trẻ (nếu có bệnh án riêng).

## 4. Dữ liệu Đầu ra
*   **Phiếu tiêm chủng**: Lưu lịch sử tiêm vào hệ thống quốc gia.
*   **Giấy chứng sinh**: (Như đã đề cập ở quy trình Sinh).
*   **Tóm tắt hồ sơ bệnh án**: Phục vụ chuyển tuyến nếu cần.

## 5. Liên kết Tài liệu
*   [Quy trình Sản khoa & Đỡ đẻ](./08-obstetrics-birth.md).
*   [Quy trình Tiêm chủng](../preventive-medicine/01-vaccination.md).
