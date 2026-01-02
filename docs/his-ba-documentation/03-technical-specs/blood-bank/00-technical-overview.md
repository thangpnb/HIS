# Tổng quan Kỹ thuật: Ngân hàng Máu (Blood Bank)

## 1. Giới thiệu
Module **Ngân hàng Máu (Blood Bank)** chịu trách nhiệm quản lý toàn bộ vòng đời của đơn vị máu trong bệnh viện, bao gồm:
1.  **Nhập kho (Blood Import)**: Từ Viện Huyết học hoặc Hiến máu nhân đạo.
2.  **Lưu trữ & Bảo quản (Inventory)**: Theo dõi nhiệt độ, hạn sử dụng, vị trí.
3.  **Cấp phát (Dispensing)**: Thực hiện phản ứng chéo, định nhóm máu và xuất kho cho lâm sàng.
4.  **Truyền máu (Transfusion)**: Ghi nhận quá trình truyền máu và theo dõi phản ứng tại khoa lâm sàng.

Mục tiêu cốt lõi là đảm bảo **An toàn Truyền máu** (Patient Safety) thông qua quy trình kiểm soát nghiêm ngặt "3 tra 5 đối".

## 2. Kiến trúc Hệ thống (Architecture)
Module được xây dựng trên nền tảng **Smart Client** (Desktop Application) của HIS, tuân thủ mô hình 3 lớp:

### 2.1. Client Library (UI & Logic)
Các tính năng được đóng gói thành các **Plugins** độc lập, tích hợp vào `DesktopRootExtensionPoint`:
*   `HIS.Desktop.Plugins.ImportBlood`: Nhập kho máu.
*   `HIS.Desktop.Plugins.BloodTypeInStock`: Quản lý tồn kho.
*   `HIS.Desktop.Plugins.ConfirmPresBlood`: Duyệt dự trù và xuất kho.
*   `HIS.Desktop.Plugins.HisCheckBeforeTransfusionBlood`: Định nhóm máu và phản ứng chéo.
*   `HIS.Desktop.Plugins.BloodTransfusion`: Theo dõi truyền máu (tại khoa lâm sàng).

### 2.2. API & Data Access
Giao tiếp với Database thông qua hệ thống **MOS API** (RESTful):
*   `api/HisImpMest/*`: Tạo phiếu nhập.
*   `api/HisExpMest/*`: Tạo phiếu xuất, cập nhật kết quả xét nghiệm hòa hợp.
*   `api/HisBlood/*`: Quản lý thông tin túi máu.
*   `api/HisServiceReq/*`: Xử lý y lệnh dự trù.

### 2.3. Database (Oracle)
Các bảng dữ liệu chính:
*   **Inventory**: `HIS_IMP_MEST`, `HIS_EXP_MEST` (Kế thừa từ Dược/Vật tư).
*   **Blood Specific**: `HIS_BLOOD` (Chi tiết túi máu), `HIS_BLOOD_TYPE` (Danh mục chế phẩm).
*   **Transfusion**: `HIS_TRANSFUSION` (Hồ sơ truyền máu), `HIS_SERVICE_REQ` (Y lệnh).

## 3. Phân hệ Chức năng
### 3.1. Quản lý Kho Máu
*   Sử dụng cơ chế `ImpMest` (Phiếu nhập) và `ExpMest` (Phiếu xuất) tương tự như Dược, nhưng có thêm bảng mở rộng `HIS_BLOOD` để quản lý đặc tính riêng của máu (ABO, Rh, Mã túi duy nhất).

### 3.2. An toàn Truyền máu
*   **Cross-matching**: Logic kiểm tra hòa hợp miễn dịch được thực hiện tại Plugin `HisCheckBeforeTransfusionBlood`.
*   **Bedside Check**: Module `BloodTransfusion` hỗ trợ điều dưỡng kiểm tra lại thông tin tại giường bệnh trước khi truyền.

## 4. Công nghệ & Tích hợp
*   **Barcode/QR Scanning**: Hỗ trợ tối đa việc quét mã túi máu để giảm thiểu sai sót nhập liệu.
*   **Integration**:
    *   **LIS**: Kết nối máy xét nghiệm để lấy kết quả định nhóm máu tự động (nếu có).
    *   **HIS Clinical**: Tích hợp chặt chẽ với Hồ sơ bệnh án điện tử (EMR) để hiển thị lịch sử truyền máu.
