# Technical Overview: HIS Desktop (`HIS.Desktop`)

## 1. Tổng quan Kiến trúc

`HIS.Desktop` là ứng dụng client chính của hệ thống quản lý bệnh viện (HIS), được xây dựng trên nền tảng **.NET Framework (Windows Forms)**. Ứng dụng này đóng vai trò là "Container" hoặc "Shell" để tải và chạy các module nghiệp vụ (Plugins).

### Đặc điểm chính
*   **Modular Monolith**: Ứng dụng chính (EXE) rất nhẹ, hầu hết các màn hình nghiệp vụ đều được tách thành các thư viện DLL (Plugin) và được tải động (Lazy Loading) khi cần thiết.
*   **Service-Oriented Client**: Giao tiếp với Backend thông qua hệ thống API Consumer (RESTful API), không kết nối trực tiếp vào Database (trừ trường hợp báo cáo trực tiếp hoặc công cụ tiện ích).
*   **DevExpress UI**: Sử dụng bộ control của DevExpress để xây dựng giao diện hiện đại (Ribbon, Tab, Grid, Chart).

## 2. Các Thành phần Cốt lõi

| Thành phần | Namespace | Vai trò |
|:---|:---|:---|
| **Core Framework** | `HIS.Desktop` | Project khởi chạy (Startup), quản lý vòng đời ứng dụng, menu chính, và loading plugin. |
| **Local Storage** | `HIS.Desktop.LocalStorage` | Hệ thống cache dữ liệu danh mục tại máy trạm để giảm tải cho Server và tăng tốc độ thao tác. |
| **Utilities** | `HIS.Desktop.Utility` | Bộ thư viện tiện ích dùng chung (Xử lý chuỗi, ngày tháng, form base, helper). |

## 3. Danh sách Tài liệu

| File Tài liệu | Hạng mục | Mô tả kỹ thuật |
|:---|:---|:---|
| `01-core-framework.md` | **Core Framework** | Phân tích quy trình khởi động (`Program.cs`), Main Form, và cơ chế quản lý Plugin/Session. |
| `02-local-storage.md` | **Local Storage** | Kiến trúc lưu trữ và đồng bộ Cache Client-side. |
| `03-utility-libraries.md`| **Utility Libraries** | Các lớp tiện ích và biến toàn cục (`GlobalVariables`) quan trọng. |
