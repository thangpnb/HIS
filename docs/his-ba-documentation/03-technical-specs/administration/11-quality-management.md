# Technical Spec: Quản lý Chất lượng Bệnh viện (Quality Management)

## 1. Business Mapping
*   **Ref**: [Quản lý Chất lượng Bệnh viện](../../02-business-processes/administration/11-quality-management.md)
*   **Scope**: Đánh giá 83 tiêu chí chất lượng, khảo sát hài lòng người bệnh.
*   **Key Plugin**: Chưa xác định (Gap).

## 2. Core Components (Proposed)
*   **Module**: `HIS.Desktop.Plugins.QualityManagement`.
*   **Chức năng**:
    *   Chấm điểm 83 tiêu chí.
    *   Quản lý phiếu khảo sát.

## 3. Database Schema
### 3.1. HIS_QUALITY_CRITERIA
*   `ID`: Tiêu chí (VD: A1.1).
*   `SCORE`: Điểm tự chấm.
*   `EVIDENCE`: Đường dẫn file minh chứng.

## 4. Integration
*   **Patient Portal/Kiosk**: Tích hợp khảo sát hài lòng người bệnh ngay sau khi khám/xuất viện.
