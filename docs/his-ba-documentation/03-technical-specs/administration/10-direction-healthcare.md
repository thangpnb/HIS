# Technical Spec: Chỉ đạo Tuyến (Direction of Healthcare Activities)

## 1. Business Mapping
*   **Ref**: [Chỉ đạo Tuyến & Đào tạo](../../02-business-processes/administration/10-direction-healthcare.md)
*   **Scope**: Quản lý các hoạt động hỗ trợ y tế tuyến dưới, chuyển giao kỹ thuật, đào tạo.
*   **Key Plugin**: Chưa xác định (Gap).

## 2. Core Components (Proposed)
*   **Module**: `HIS.Desktop.Plugins.DirectionHealthcare`.
*   **Chức năng**:
    *   Quản lý hợp đồng chuyển giao kỹ thuật.
    *   Quản lý lớp đào tạo (Học viên, Giảng viên).
    *   Thống kê báo cáo chỉ đạo tuyến (1816).

## 3. Database Schema
### 3.1. HIS_TRAINING_COURSE
*   `ID`: PK.
*   `COURSE_NAME`: Tên khóa đào tạo.
*   `START_DATE`, `END_DATE`.

### 3.2. HIS_TECH_TRANSFER
*   `TECH_ID`: Kỹ thuật chuyển giao.
*   `RECEIVE_UNIT`: Đơn vị tiếp nhận.

## 4. Automation
*   **Auto Report**: Tự động tổng hợp số liệu khám chữa bệnh của các bác sĩ đi luân phiên để làm báo cáo về Sở.
