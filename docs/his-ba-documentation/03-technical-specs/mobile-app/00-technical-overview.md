# Technical Overview: Mobile App (Moba)

## 1. Introduction
Module Mobile App (Moba) cung cấp các chức năng HIS trên nền tảng di động (Tablet/Smartphone) hoặc các thiết bị chuyên dụng (Handheld Scanner). Module này tập trung vào tính tiện lợi, xử lý tại chỗ (Point of Care/Point of Sale) và tối ưu hóa quy trình kho vận.

## 2. Architecture & Components
Mobile App trong HIS hoạt động như một Client Extension của hệ thống Desktop, sử dụng chung cơ chế Plugin và API Backend.

### 2.1. Key Plugins
*   **Inventory (Kho dược)**:
    *   `HIS.Desktop.Plugins.MobaImpMestList`: Quản lý danh sách nhập/xuất/kiểm kê.
    *   `HIS.Desktop.Plugins.MobaImpMestCreate`: Tạo phiếu nhập/kiểm kê mới.
*   **Point of Sale (Bán hàng)**:
    *   `HIS.Desktop.Plugins.MobaSaleCreate`: Bán thuốc lẻ, bán tại quầy di động.
*   **Clinical (Lâm sàng)**:
    *   `HIS.Desktop.Plugins.MobaExamPresCreate`: Khám bệnh, kê đơn tại giường.
    *   `HIS.Desktop.Plugins.MobaPrescriptionCreate`: Giao diện kê đơn chuyên biệt.
*   **Services (Cận lâm sàng)**:
    *   `HIS.Desktop.Plugins.MobaBloodCreate`: Xác thực lấy mẫu máu tại giường.

### 2.2. Common Pattern
Các plugin mobile thường tuân theo mẫu thiết kế sau:
1.  **Simplified UI**: Giao diện tối giản, nút lệnh lớn, hỗ trợ touch.
2.  **Barcode Integrated**: Tích hợp sự kiện quét mã vạch (Scanner Driver hoặc KeyListener) để điền dữ liệu nhanh.
3.  **Direct API Calls**: Gọi trực tiếp các API HIS (ví dụ `api/HisImpMest`, `api/HisServiceReq`) thay vì qua các lớp xử lý phức tạp của bản Desktop đầy đủ, nhằm giảm độ trễ.

## 3. Database Schema Overview
Module Mobile không có database riêng biệt mà thao tác trực tiếp trên các bảng Core của HIS.

*   **Inventory**: `HIS_IMP_MEST`, `HIS_IMP_MEST_DETAIL`, `HIS_EXP_MEST`, `HIS_EXP_MEST_DETAIL`.
*   **Clinical**: `HIS_SERVICE_REQ`, `HIS_GUEST` (Bệnh nhân vãng lai), `HIS_TREATMENT`.
*   **Catalog**: `HIS_MEDICINE_TYPE`, `HIS_MATERIAL_TYPE` (Cache cục bộ để tra cứu nhanh).

## 4. Integration
*   **Printer**: Kết nối máy in nhiệt Bluetooth hoặc Wifi để in phiếu ngay tại chỗ.
*   **Scanner**: Tích hợp driver máy quét mã vạch chuyên dụng (Zebra, Honeywell) hoặc Camera thiết bị.
