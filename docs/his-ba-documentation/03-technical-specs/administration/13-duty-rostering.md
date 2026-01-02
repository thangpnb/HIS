# Technical Spec: Phân lịch trực (Duty Rostering)

## 1. Business Mapping
*   **Ref**: [Phân lịch trực & Chấm công](../../02-business-processes/administration/13-duty-rostering.md)
*   **Scope**: Lập kế hoạch trực, quản lý đổi trực, tích hợp chấm công.
*   **Key Plugin**: `HIS.Desktop.Plugins.HisWorkingShift` (Thực hiện ca trực).
*   **Gap**: Module lập kế hoạch (`DutyRostering`) chưa được tìm thấy, hiện tại quản lý chủ yếu là *thực hiện* ca trực.

## 2. Core Components
### 2.1. Plugin Main Structure
*   **Execution**: `HIS.Desktop.Plugins.HisWorkingShift`.
*   **Chức năng**: Ghi nhận nhân sự thực tế tham gia kíp trực (Check-in/Check-out).

### 2.2. Logic Nghiệp vụ (Proposed for Rostering)
*   **Rule**:
    *   Không trực 2 ca liên tiếp quá 24h.
    *   Đảm bảo cơ cấu (1 Lãnh đạo, 1 Bác sĩ, 2 Điều dưỡng).
*   **Rotation**: Hỗ trợ xoay ca tự động.

## 3. Database Schema
### 3.1. HIS_DUTY_ROSTER (Proposed)
*   `ID`: PK.
*   `ROSTER_DATE`: Ngày trực.
*   `DEPARTMENT_ID`: Khoa.
*   `MEMBER_LIST`: JSON list nhân viên dự kiến.

## 4. Integration Points
*   **HRM**: Kết nối hệ thống nhân sự để tính lương/phụ cấp trực.
*   **Shift Handover**: Dữ liệu lịch trực được load tự động sang module Bàn giao ca (`07-shift-handover.md`).
