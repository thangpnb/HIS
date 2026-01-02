# Hệ thống Thông tin Bệnh viện HIS - Tài liệu Nghiệp vụ (BA)

> Tài liệu phân tích nghiệp vụ (Business Analysis) dành cho hệ thống HisNguonMo.

## 📚 Mục lục

### 1. Kiến trúc Hệ thống
- [Tổng quan kiến trúc](./01-architecture/overview.md) - Phân tích 4 module cốt lõi: HIS, MPS, UC, Common.
- [Kiến trúc Plugin](./01-architecture/plugin-system/01-overview.md) - Chi tiết về cơ chế mở rộng với hệ thống hơn 950 plugin.

### 2. Chi tiết các Module

#### HIS Desktop (Ứng dụng chính)
- [Core Framework](./02-modules/his-desktop/core.md) - Điểm khởi đầu (Entry point) và vòng đời (lifecycle) ứng dụng.
- [Business Plugins](./02-modules/his-desktop/business-plugins.md) - Tập hợp các plugin xử lý nghiệp vụ.
- [API Consumer](./02-modules/his-desktop/api-consumer.md) - Cách thức giao tiếp và tương tác với các dịch vụ backend.

#### UC Controls (Thành phần giao diện)
- [Form Type Controls](./02-modules/uc-controls/form-type-controls.md) - Thư viện gồm 329 thành phần (components) cho các biểu mẫu.
- [Service Room Controls](./02-modules/uc-controls/service-room-controls.md) - Các thành phần quản lý phòng dịch vụ.

#### Thư viện dùng chung (Common Libraries)
- [Libraries](./02-modules/common-libraries/libraries.md) - Danh mục 46 thư viện dùng chung trong toàn hệ thống.

### 3. Nghiệp vụ theo Lĩnh vực (Business Domains)

#### Quản lý Bệnh nhân
- [Hiển thị & Gọi bệnh nhân](./03-business-domains/patient-management/patient-call-display.md) - Quy trình tiếp đón và điều phối bệnh nhân.

#### Xét nghiệm (LIS)
- [LIS Plugins](./03-business-domains/laboratory/lis-plugins.md) - Hệ thống 12 plugin chuyên dụng cho quản lý xét nghiệm.

#### Dược & Vật tư y tế
- [Thuốc & Vật tư](./03-business-domains/pharmacy/medicine-material.md) - Quản lý danh mục thuốc, vật tư và kho dược.

#### Quản trị Hệ thống
- [Phân quyền (ACS)](./03-business-domains/administration/access-control.md) - Cơ chế kiểm soát truy cập với 13 plugin hỗ trợ.
- [Dữ liệu hệ thống (SDA)](./03-business-domains/administration/system-data.md) - Quản lý dữ liệu nền tảng với 14 plugin.

### 4. Tích hợp (Integrations)
- [Notifications & Events](./04-integrations/notifications-events.md) - Cơ chế Pub/Sub và kiến trúc hướng sự kiện (event-driven).
- [Helper Plugins](./04-integrations/helper-plugins.md) - Danh sách 36 plugin hỗ trợ (library plugins).

### 99. Dành cho Lập trình viên
- [Thiết lập môi trường Build](./99-development/build-setup.md)
- [Các lệnh Build](./99-development/build-commands.md)
- [Build một dự án cụ thể](./99-development/build-specific-project.md)
- [Dọn dẹp thư mục dự án](./99-development/cleanup-folders.md)

---

## 📊 Thống kê Hệ thống

| Module | Số lượng thành phần | Mô tả |
|--------|---------------------|-------|
| **HIS** | 956 plugins | Ứng dụng Desktop chính đảm nhận nghiệp vụ bệnh viện. |
| **MPS** | 790+ processors | Hệ thống xử lý và in ấn biểu mẫu y tế. |
| **UC** | 131 controls | Các thành phần giao diện người dùng dùng chung. |
| **Common** | 46 projects | Các thư viện tiện ích và logic dùng chung. |

---

*Tài liệu được biên soạn tự động từ [DeepWiki](https://deepwiki.com/thangpnb/HIS) và được tổ chức lại để phù hợp với quy trình phân tích nghiệp vụ (BA).*
