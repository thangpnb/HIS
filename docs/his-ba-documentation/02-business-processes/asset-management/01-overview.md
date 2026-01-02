# Quản lý Tài sản & Trang thiết bị (Asset Management)

## 1. Tổng quan
Phân hệ Quản lý Tài sản tập trung vào việc quản lý vòng đời của trang thiết bị y tế (TTBYT), vật tư kỹ thuật và công cụ dụng cụ trong bệnh viện, từ khâu tiếp nhận, bàn giao, sử dụng cho đến khi thanh lý.

## 2. Các Quy trình Chính

### 2.1. Quản lý Hồ sơ Tài sản (Asset Profile)
Mỗi thiết bị được quản lý như một thẻ tài sản riêng biệt với các thông tin chi tiết:
*   Mã tài sản, Tên, Model, Serial Number.
*   Nước sản xuất, Năm sản xuất, Hạn sử dụng.
*   Thông số kỹ thuật.
*   **Plugin chính**: 
    *   `HIS.Desktop.Plugins.HisMachine`: Quản lý danh mục thiết bị.
    *   `HIS.Desktop.Plugins.EquipmentSet`: Quản lý bộ dụng cụ.
    *   `HIS.Desktop.Plugins.MaterialType`: Định nghĩa loại vật tư/thiết bị.

### 2.2. Điều chuyển & Bàn giao (Transfer)
Theo dõi quá trình luân chuyển tài sản giữa các khoa phòng.
*   **Quy trình**: Kho Vật tư -> Khoa Lâm sàng -> Khoa vật tư (Sửa chữa).
*   **Plugin chính**:
    *   `HIS.Desktop.Plugins.ImpMestCreate`: Tạo phiếu nhập.
    *   `HIS.Desktop.Plugins.ExpMestDepaCreate`: Xuất cấp cho khoa phòng.
    *   `HIS.Desktop.Plugins.EquipmentSet`: Quản lý bộ dụng cụ phẫu thuật.

### 2.3. Kiểm kê Tài sản (Inventory)
Định kỳ kiểm kê số lượng và tình trạng sử dụng của tài sản tại các khoa phòng.
*   **Plugin chính**:
    *   `HIS.Desktop.Plugins.MediStockInventory`: Chức năng kiểm kê (dùng chung với Dược).

## 3. Phân loại Tài sản
1.  **Thiết bị Y tế**: Máy X-Quang, Máy xét nghiệm, Máy thở...
2.  **Công cụ dụng cụ (CCDC)**: Kéo, Panh, Giường bệnh, Tủ đầu giường.
3.  **Vật tư kỹ thuật**: Bóng đèn, linh kiện thay thế.

## 4. Liên kết Tài liệu
*   [Quy trình Bảo trì & Sửa chữa](./02-maintenance-cycle.md)
