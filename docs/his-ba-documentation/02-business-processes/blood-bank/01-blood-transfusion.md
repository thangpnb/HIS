# Quy trình Quản lý Máu & Truyền máu (Blood Bank & Transfusion)

## 1. Tổng quan
Quy trình mô tả việc tiếp nhận máu từ trung tâm huyết học, lưu trữ tại kho máu bệnh viện và cấp phát cho các khoa lâm sàng để truyền cho bệnh nhân.

## 2. Lưu đồ Quy trình

```mermaid
graph TB
    Start((Bắt đầu)) --> NhapKho[1. Nhập kho Máu]
    NhapKho --> LuuTru[2. Lưu trữ & Bảo quản]
    
    LuuTru --> DuTru[3. Khoa Lâm sàng Dự trù]
    DuTru --> PhatMau[4. Khoa XN Cấp phát]
    PhatMau --> NhanMau[5. Khoa LS Nhận máu]
    
    NhanMau --> DinhNhom[6. Định nhóm & Phản ứng chéo (Tại giường)]
    DinhNhom --> TruyenMau[7. Truyền máu & Theo dõi]
    TruyenMau --> HoanTra[8. Hoàn trả Vỏ túi/Máu hỏng]
    HoanTra --> End((Kết thúc))
```

## 3. Chi tiết Các bước & Mapping Plugin

### 3.1. Nhập và Quản lý Kho Máu (Blood Inventory)
Quy trình nhập và quản lý tồn kho máu chi tiết, đảm bảo an toàn truyền máu.
*   **Plugin chính**:
    *   `HIS.Desktop.Plugins.HisBloodType`: Quản lý danh mục nhóm máu (ABO, Rh) và chế phẩm (Hồng cầu khối, Huyết tương...).
    *   `HIS.Desktop.Plugins.ImportBlood`: Nhập kho máu từ Viện Huyết học (Hỗ trợ nhập tay hoặc Import file).
    *   `HIS.Desktop.Plugins.BloodTypeInStock`: Theo dõi tồn kho máu chi tiết theo từng túi (Mã túi, Hạn sử dụng, Vị trí tủ bảo quản).
    *   `HIS.Desktop.Plugins.ExportBlood`: Xuất hủy máu hỏng, hết hạn hoặc xuất trả vỏ túi.
    *   `HIS.Desktop.Plugins.ExpMestChmsCreate`: Các nghiệp vụ nhập xuất điều chỉnh khác.

### 3.2. Dự trù và Định nhóm máu (Ordering & Typing)
Bác sĩ lâm sàng chỉ định truyền máu. Khoa Xét nghiệm thực hiện định nhóm và phản ứng chéo.
*   **Plugin chính**:
    *   `HIS.Desktop.Plugins.AssignPrescription`: Chỉ định truyền máu (Y lệnh).
    *   `HIS.Desktop.Plugins.ConfirmPresBlood`: Xác nhận và duyệt dự trù máu.
    *   `HIS.Desktop.Plugins.ServiceReqResult`: Trả kết quả định nhóm máu.

### 3.3. Cấp phát và Truyền máu (Dispensing & Transfusion)
Xuất kho máu cho khoa lâm sàng. Tại khoa, thực hiện quy trình "3 tra 5 đối" và phản ứng chéo tại giường trước khi truyền.
*   **Plugin chính**:
    *   `HIS.Desktop.Plugins.ExpMestChmsCreate`: Xuất kho máu (Phiếu xuất).
    *   `HIS.Desktop.Plugins.BloodTransfusionCheck`: Ghi nhận kết quả kiểm tra an toàn truyền máu tại giường (Bedside Check).

## 4. Dữ liệu Đầu ra
*   **Phiếu Dự trù và Cấp phát máu**: Kèm mã túi máu cụ thể.
*   **Phiếu Theo dõi Truyền máu**: Ghi nhận sinh hiệu và các phản ứng (nếu có).

## 5. Liên kết Tài liệu
*   [Quy trình Điều trị Nội trú](../clinical/04-daily-treatment.md).
