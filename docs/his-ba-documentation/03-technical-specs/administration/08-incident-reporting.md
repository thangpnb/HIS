# Technical Spec: Báo cáo Sự cố Y khoa (Incident Reporting)

## 1. Business Mapping
*   **Ref**: [Báo cáo Sự cố Y khoa](../../02-business-processes/administration/08-incident-reporting.md)
*   **Scope**: Ghi nhận sự cố y khoa (sai sót thuốc, té ngã...), phân tích nguyên nhân và báo cáo về Sở/Bộ.
*   **Key Plugin**: `HIS.Desktop.Plugins.Incident` (Proposed/Gap).

## 2. Core Components (Proposed)
Hiện tại chưa tìm thấy plugin chuyên biệt trong codebase. Kiến trúc đề xuất:

### 2.1. Plugin Main Structure
*   **Plugin Name**: `HIS.Desktop.Plugins.Incident`.
*   **Extension Point**: `DesktopRootExtensionPoint`.

## 3. Database Schema (Legacy/Proposed)
### 3.1. HIS_INCIDENT_REPORT
*   `ID`: PK.
*   `INCIDENT_TIME`: Thời gian xảy ra.
*   `SEVERITY_LEVEL`: Mức độ (Nặng/Nhẹ/Suýt bị).
*   `DESCRIPTION`: Mô tả sự cố.
*   `REPORTER`: Người báo cáo (Ẩn danh/Có danh).

## 4. Integration Points
*   **EMR**: Link sự cố với hồ sơ bệnh án (nếu sự cố xảy ra trên bệnh nhân cụ thể).
*   **Mobile App**: Cho phép báo cáo nhanh qua Mobile.
