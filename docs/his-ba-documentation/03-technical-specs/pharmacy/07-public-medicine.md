# Technical Spec: Thuốc Tủ Trực (Public Medicine)

## 1. Business Mapping
*   **Ref**: [Quy trình Quản lý Tủ trực](../../02-business-processes/pharmacy/07-public-medicine.md)
*   **Scope**: Quản lý cơ số thuốc tại khoa lâm sàng, lĩnh bù và lĩnh theo đợt.
*   **Key Plugins**:
    *   `HIS.Desktop.Plugins.PublicMedicineGeneral`: Quản lý chung.
    *   `HIS.Desktop.Plugins.PublicMedicineByDate`: Lĩnh hàng ngày.
    *   `HIS.Desktop.Plugins.PublicMedicineByPhased`: Lĩnh theo đợt.

## 2. Core Components

### 2.1. Plugin Main Structure
*   **PublicMedicineGeneral**:
    *   Giao diện Dashboard hiển thị danh sách tủ trực của khoa.
    *   Quản lý danh mục thuốc được phép tồn tại tủ trực ("Danh mục tủ trực").
*   **PublicMedicineByDate**:
    *   Tự động tổng hợp y lệnh đã thực hiện trong ngày để tạo phiếu lĩnh bù.
    *   Logic: `GetMedicinesUsedToday()` -> `Group By Medicine` -> `Create ImpMest request`.

### 2.2. Logic "Cơ số Tủ trực"
*   Hệ thống cho phép định nghĩa **Cơ số quy định** cho mỗi đầu thuốc tại tủ trực khoa.
*   **Lĩnh bù**: Số lượng lĩnh = Cơ số quy định - Số lượng tồn thực tế.

## 3. Database Schema

### 3.1. HIS_MEDI_STOCK (Kho tủ trực)
*   Khoa lâm sàng cũng được định nghĩa là một kho (`HIS_MEDI_STOCK`) nhưng có cờ `IS_CABINET = 1` (Tủ trực).
*   Quản lý tồn kho tương tự kho dược chính.

### 3.2. HIS_IMP_MEST (Loại lĩnh bù)
*   Sử dụng `IMP_MEST_TYPE_ID` đặc thù (ví dụ: Lĩnh bù tủ trực, Lĩnh cơ số).

## 4. Integration Points
*   **Y lệnh (Medical Record)**: Khi điều dưỡng thực hiện y lệnh (cho bệnh nhân uống thuốc), kho tủ trực sẽ bị trừ tồn kho (nếu cấu hình trừ tủ trực).
*   **Kho Dược**: Nhận phiếu yêu cầu từ tủ trực và thực hiện xuất chuyển kho (`ExpMestDepaCreate`).

## 5. Common Config
*   `ALLOW_AUTO_COMPENSATION`: Cho phép tự động tạo phiếu lĩnh bù cuối ngày.
