# Tổng Quan Hệ Thống Plugin

## Mục đích

Tài liệu này cung cấp cái nhìn tổng quan về kiến trúc dựa trên plugin (plugin-based architecture) của hệ thống HIS. Đây là cơ chế cốt lõi cho phép hệ thống mở rộng, bảo trì và quản lý sự phức tạp của một ứng dụng bệnh viện quy mô lớn.

Hệ thống được thiết kế để hỗ trợ khả năng modun hóa cao, cho phép các nhóm phát triển làm việc độc lập trên các tính năng khác nhau mà không ảnh hưởng đến toàn bộ ứng dụng.

## Thống kê Hệ thống

Hệ thống hiện tại bao gồm một số lượng lớn các plugin, phản ánh độ phức tạp và quy mô của các quy trình nghiệp vụ bệnh viện:

-   **Tổng số Plugin**: 956 plugin
-   **Không gian tên (Namespace)**: 7 không gian tên chính
-   **Phạm vi**: Bao gồm tất cả các khía cạnh từ lâm sàng, cận lâm sàng, dược, thanh toán đến quản trị hệ thống.

## Phạm vi Tài liệu

Bộ tài liệu này được chia nhỏ để cung cấp thông tin chi tiết về từng khía cạnh của hệ thống plugin:

1.  **[Quy trình Khám phá & Vòng đời](02-discovery-lifecycle.md)**: Cách ứng dụng tìm kiếm, tải và quản lý vòng đời của plugin.
2.  **[Cấu trúc & Tổ chức](03-structure-organization.md)**: Cách tổ chức thư mục và cấu trúc file chuẩn của một plugin.
3.  **[Cơ chế Giao tiếp](04-communication.md)**: Cách các plugin giao tiếp với nhau (đồng bộ và bất đồng bộ).
4.  **[Hướng dẫn Phát triển](05-development-guide.md)**: Hướng dẫn cho nhà phát triển để tạo và duy trì plugin.
5.  **[Danh mục Plugin](06-catalog.md)**: Danh sách chi tiết các nhóm plugin theo phân hệ chức năng.

## Điểm Tích hợp Chính

Hệ thống plugin không hoạt động độc lập mà tích hợp chặt chẽ với các thành phần lõi khác:

-   **Core Framework** (`Inventec.Desktop.Core`): Cung cấp nền tảng runtime.
-   **Local Storage**: Quản lý trạng thái và cấu hình cục bộ.
-   **API Consumer**: Xử lý giao tiếp với Backend.

---
*Xem tiếp: [Quy trình Khám phá & Vòng đời Plugin](02-discovery-lifecycle.md)*
