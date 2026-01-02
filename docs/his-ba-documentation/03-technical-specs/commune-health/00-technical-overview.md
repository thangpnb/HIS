# Technical Overview: Y Tế Xã/Phường (Commune Health)

## 1. Introduction
Module **Y Tế Xã/Phường (Commune Health)** được thiết kế đặc thù cho tuyến y tế cơ sở (Trạm Y tế xã/phường/thị trấn), tập trung vào các Chương trình Mục tiêu Quốc gia (National Health Programs), Quản lý Sức khỏe Sinh sản (Reproductive Health), và Quản lý Bệnh không lây nhiễm (NCDs).

Khác với các module bệnh viện (HIS Core), module này hoạt động chủ yếu dựa trên các danh mục và quy trình báo cáo của hệ thống Y tế Dự phòng.

## 2. Architecture & Tech Stack

### 2.1. Plugin Architecture
Hệ thống sử dụng kiến trúc Plugin của HIS Desktop. Các plugin của Y tế xã thường có prefix `TYT` (viết tắt của Trạm Y Tế) hoặc `SDA.Commune`.
*   **Namespace chính**: `TYT.Desktop.Plugins.*`, `HIS.Desktop.Plugins.Commune*`.
*   **Base Classes**: Thường kế thừa từ `HIS.Desktop.Plugins.BasePlugin`.

### 2.2. Database Schema
Dữ liệu của Y tế xã được lưu trữ phân tách hoặc chung với HIS Core tùy vào loại dữ liệu:
*   **Riêng biệt (TYT Prefix)**: Các bảng đặc thù cho chương trình (VD: `TYT_FETUS_ABORTION`, `TYT_HIV`, `TYT_TUBERCULOSIS`).
*   **Chia sẻ (HIS Core)**: Dữ liệu hành chính bệnh nhân (`HIS_PATIENT`), bác sĩ (`HIS_EMPLOYEE`), danh mục (`HIS_SERVICE`).

## 3. Core Modules (Domain Breakdown)
Hệ thống được chia thành các phân hệ chính (được chi tiết hóa trong các file specs riêng):

### 3.1. Chương trình Mục tiêu (National Programs)
*   **Mục tiêu**: Quản lý bệnh truyền nhiễm (HIV, Lao, Sốt rét) và không lây nhiễm (Tăng huyết áp, Đái tháo đường).
*   **Docs**: [01-national-programs.md](./01-national-programs.md)

### 3.2. Sức khỏe Sinh sản (Reproductive Health)
*   **Mục tiêu**: Quản lý thai sản, KHHGĐ, Tiêm chủng mở rộng.
*   **Docs**: [02-reproductive-health.md](./02-reproductive-health.md)

## 4. Common Dependencies
*   **Patient Management**: Sử dụng chung module Quản lý Bệnh nhân (MPI) để định danh.
*   **Reporting**: Tích hợp chặt chẽ với hệ thống báo cáo thống kê (HIS.Desktop.Plugins.Report).

