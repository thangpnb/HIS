# HIS Hospital Information System - BA Documentation

> Tài liệu nghiệp vụ (Business Analysis) cho hệ thống HisNguonMo

## 📚 Mục lục

### 1. Kiến trúc hệ thống
- [Tổng quan kiến trúc](./01-architecture/overview.md) - 4 module chính: HIS, MPS, UC, Common
- [Kiến trúc Plugin](./01-architecture/plugin-system.md) - Hệ thống 956 plugins

### 2. Chi tiết các Module

#### HIS Desktop (Ứng dụng chính)
- [Core Framework](./02-modules/his-desktop/core.md) - Entry point, lifecycle
- [Business Plugins](./02-modules/his-desktop/business-plugins.md) - Các plugin nghiệp vụ
- [API Consumer](./02-modules/his-desktop/api-consumer.md) - Tương tác backend

#### UC Controls (Giao diện người dùng)
- [Form Type Controls](./02-modules/uc-controls/form-type-controls.md) - 329 form components
- [Service Room Controls](./02-modules/uc-controls/service-room-controls.md) - Quản lý phòng dịch vụ

#### Common Libraries
- [Libraries](./02-modules/common-libraries/libraries.md) - 46 thư viện dùng chung

### 3. Nghiệp vụ theo Domain

#### Quản lý Bệnh nhân
- [Hiển thị & Gọi bệnh nhân](./03-business-domains/patient-management/patient-call-display.md)

#### Xét nghiệm (LIS)
- [LIS Plugins](./03-business-domains/laboratory/lis-plugins.md) - 12 plugins xét nghiệm

#### Dược
- [Thuốc & Vật tư](./03-business-domains/pharmacy/medicine-material.md)

#### Quản trị hệ thống
- [Phân quyền (ACS)](./03-business-domains/administration/access-control.md) - 13 plugins
- [Dữ liệu hệ thống (SDA)](./03-business-domains/administration/system-data.md) - 14 plugins

### 4. Tích hợp
- [Notifications & Events](./04-integrations/notifications-events.md) - Pub/Sub, event-driven
- [Helper Plugins](./04-integrations/helper-plugins.md) - 36 library plugins

### 99. Dành cho Developer
- [Build Setup](./99-development/build-setup.md)
- [Build Commands](./99-development/build-commands.md)
- [Build Specific Project](./99-development/build-specific-project.md)
- [Cleanup Folders](./99-development/cleanup-folders.md)

---

## 📊 Thống kê hệ thống

| Module | Components | Mô tả |
|--------|------------|-------|
| HIS | 956 plugins | Ứng dụng desktop chính |
| MPS | 790+ processors | Hệ thống in ấn y tế |
| UC | 131 controls | Giao diện người dùng |
| Common | 46 projects | Thư viện dùng chung |

---

*Được tạo tự động từ [DeepWiki](https://deepwiki.com/thangpnb/HIS) và tổ chức lại cho mục đích BA.*
