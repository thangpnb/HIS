# Technical Spec: Kiểm soát Nhiễm khuẩn (Infection Control)

## 1. Business Mapping
*   **Ref**: [Quy trình Kiểm soát Nhiễm khuẩn](../../02-business-processes/administration/09-infection-control.md)
*   **Scope**: Module này hiện tại được đánh dấu là **OUT OF SCOPE** trong giai đoạn hiện tại.
*   **Key Plugin**: Chưa xác định (Dự kiến: `HIS.Desktop.Plugins.InfectionControl`).

## 2. Core Components (Proposed)
Do chưa có plugin thực tế, dưới đây là kiến trúc đề xuất:

### 2.1. Plugin Main Structure
*   **Plugin Name**: `HIS.Desktop.Plugins.InfectionControl`
*   **Extension Point**: `DesktopRootExtensionPoint`.

## 3. Database Schema (Legacy Support)
Mặc dù chưa có module chuyên biệt, các bảng sau có thể liên quan trong hệ thống hiện tại (Quản lý Vật tư tiêu hao):

### 3.1. HIS_MEDI_STOCK
Lưu trữ kho vật tư/hóa chất.
*   `ID`: PK.
*   `MEDI_STOCK_NAME`: Tên kho (Ví dụ: Kho Hóa chất, Kho chống khuẩn).

### 3.2. HIS_MATERIAL_TYPE
Danh mục vật tư.
*   `MATERIAL_TYPE_NAME`: Tên vật tư (Cồn, Bông, Gạc...).

## 4. Future Integration
*   **Clinical**: Tích hợp với hồ sơ bệnh án để đánh dấu bệnh nhân nhiễm khuẩn (Isolation).
*   **MediStock**: Phiếu lĩnh/xuất hóa chất khử khuẩn.
