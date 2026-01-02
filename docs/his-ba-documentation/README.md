# Hệ thống Thông tin Bệnh viện HIS - Tài liệu Nghiệp vụ (BA)

> Tài liệu phân tích nghiệp vụ (Business Analysis) dành cho hệ thống HisNguonMo.

> [!IMPORTANT]
> **Lưu ý về Phạm vi Source Code**: Repository này chỉ chứa mã nguồn phía **Client (Desktop App)**. Phần Source Code Backend (API, Database, Stored Procedures) **KHÔNG** có trong repository này. Mọi tài liệu về Backend và Database đều được suy diễn từ mã nguồn Client và API Consumer. Xem chi tiết tại [Hạ tầng Backend](./03-technical-specs/backend-infrastructure/00-technical-overview.md).

## 📚 Mục lục

### 1. Kiến trúc Hệ thống
- [Tổng quan kiến trúc](./01-architecture/overview.md) - Phân tích 4 module cốt lõi: HIS, MPS, UC, Common.
- [Kiến trúc Plugin](./01-architecture/plugin-system/01-overview.md) - Chi tiết về cơ chế mở rộng với hệ thống hơn 950 plugin.

### 2. Chi tiết các Module

#### HIS Desktop (Ứng dụng chính)
- [Core Framework](./03-technical-specs/his-desktop/core.md) - Điểm khởi đầu (Entry point) và vòng đời (lifecycle) ứng dụng.
- [Business Plugins](./03-technical-specs/his-desktop/business-plugins.md) - Tập hợp các plugin xử lý nghiệp vụ.
- [API Consumer](./03-technical-specs/his-desktop/api-consumer.md) - Cách thức giao tiếp và tương tác với các dịch vụ backend.

#### UC Controls (Thành phần giao diện)
- [Form Type Controls](./03-technical-specs/uc-controls/form-type-controls.md) - Thư viện gồm 329 thành phần (components) cho các biểu mẫu.
- [Service Room Controls](./03-technical-specs/uc-controls/service-room-controls.md) - Các thành phần quản lý phòng dịch vụ.

#### Thư viện dùng chung (Common Libraries)
- [Libraries (Tổng quan)](./03-technical-specs/common-libraries/00-technical-overview.md) - Hệ thống 46 thư viện dùng chung (Logging, Cache, Device, Util...).
- [Core Infrastructure](./03-technical-specs/common-libraries/01-core-infrastructure.md) - Các thành phần lõi.
- [External Integrations](./03-technical-specs/common-libraries/02-external-integrations.md) - Tích hợp bên thứ 3.

#### Các Module Hạ tầng khác
- [Internal Monitoring](./03-technical-specs/internal-monitoring/00-technical-overview.md) - Giám sát Cache và RAM nội bộ.
- [Custom Controls](./03-technical-specs/custom-controls/00-technical-overview.md) - Bộ control tùy biến (Legacy).
- [Financial Integrations](./03-technical-specs/financial-integrations/00-technical-overview.md) - Tích hợp thanh toán QR, Hóa đơn điện tử.
- [MPS Print](./03-technical-specs/mps-print/00-technical-overview.md) - Hệ thống in ấn tập trung.

### 3. Nghiệp vụ theo Lĩnh vực (Business Domains)

#### Quản lý Bệnh nhân
- [Hiển thị & Gọi bệnh nhân](./02-business-processes/patient-management/01-business-overview.md) - Quy trình tiếp đón và điều phối bệnh nhân.
- [Quản lý Buồng/Giường](./02-business-processes/clinical/07-bed-management.md) - Điều phối giường và theo dõi công suất phòng bệnh.
- [Ứng dụng Di động](./02-business-processes/mobile-app/01-business-overview.md) - Các quy trình trên thiết bị di động (Kho, Bán hàng, Chăm sóc).

#### Xét nghiệm (LIS)
- [LIS Plugins](./02-business-processes/laboratory/01-business-overview.md) - Hệ thống 12 plugin chuyên dụng cho quản lý xét nghiệm.

#### Dược & Vật tư y tế
- [Thuốc & Vật tư](./02-business-processes/pharmacy/01-business-overview.md) - Quản lý danh mục thuốc, vật tư và kho dược.
- [Dược Lâm sàng](./02-business-processes/pharmacy/06-clinical-pharmacy.md) - Cảnh báo tương tác thuốc và báo cáo ADR.

#### Quản trị Hệ thống
- [Phân quyền (ACS)](./02-business-processes/administration/01-access-control-business.md) - Cơ chế kiểm soát truy cập với 13 plugin hỗ trợ.
- [Dữ liệu hệ thống (SDA)](./02-business-processes/administration/03-system-data-business.md) - Quản lý dữ liệu nền tảng với 14 plugin.
- [Báo cáo Sự cố Y khoa](./02-business-processes/administration/08-incident-reporting.md) - Quy trình báo cáo và xử lý sự cố (NC).

### 4. Tích hợp (Integrations)
- [Notifications & Events](./04-integrations/notifications-events.md) - Cơ chế Pub/Sub và kiến trúc hướng sự kiện (event-driven).
- [Helper Plugins](./04-integrations/helper-plugins.md) - Danh sách 36 plugin hỗ trợ (library plugins).
 
 ### 5. Cơ sở dữ liệu (Database)
 ### 5. Cơ sở dữ liệu (Database)
 - [Architecture Constraint](./03-technical-specs/backend-infrastructure/00-technical-overview.md) - **QUAN TRỌNG**: Cảnh báo về việc thiếu source code Backend & DB.
 - [Data Dictionary](./05-database/01-data-dictionary.md) - Từ điển dữ liệu và cấu trúc bảng (Suy diễn).


### 6. Vận hành & Bảo trì (Operations)
 - [Deployment Guide](./06-operations/01-deployment-guide.md) - Hướng dẫn triển khai Client.
 - [Configuration Guide](./06-operations/02-configuration-guide.md) - Cấu hình hệ thống.
 - [Troubleshooting](./06-operations/03-troubleshooting.md) - Xử lý sự cố thường gặp.
 
 ### 7. Hướng dẫn Sử dụng (User Guides)
 - [Getting Started](./07-user-guides/01-getting-started.md) - Bắt đầu sử dụng.
 - [Common Workflows](./07-user-guides/02-common-workflows.md) - Các thao tác thường quy.
 
 ### 8. Kiểm thử (Testing)
 - [Test Strategy](./08-testing/01-test-strategy.md) - Chiến lược kiểm thử.
 - [UAT Checklist](./08-testing/02-uat-checklist.md) - Ví dụ UAT cho phân hệ Tiếp đón.
 
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
