# Technical Overview: Internal Monitoring (Giám sát Nội bộ)

## 1. Tổng quan Kiến trúc

Phân hệ `internal-monitoring` bao gồm các công cụ kỹ thuật giúp giám sát và quản lý trạng thái hoạt động của Client (Desktop App) và đồng bộ dữ liệu Cache giữa Client-Server.

### Các thành phần chính
1.  **Cache Monitor**: Quản lý việc làm mới (Reload) các dữ liệu danh mục Cache tại máy trạm.
2.  **RAM Monitor**: (Legacy) Quản lý việc tối ưu hóa bộ nhớ RAM thông qua cấu hình XML.

## 2. Kiến trúc Module

| Module | Namespace | Vai trò |
|:---|:---|:---|
| **XmlCacheMonitor** | `HIS.Desktop.XmlCacheMonitor` | Thư viện lõi xử lý việc đọc/ghi cấu hình Cache Keys từ file XML cục bộ. |
| **XmlRamMonitor** | `HIS.Desktop.XmlRamMonitor` | Thư viện lõi xử lý cấu hình giám sát RAM (thường dùng chung cấu trúc với CacheMonitor). |
| **Plugin UI** | `HIS.Desktop.Plugins.HisCacheMonitor` | Giao diện quản trị (GUI) cho phép Admin cấu hình key nào cần reload. |

## 3. Danh sách Tài liệu

| File Tài liệu | Hạng mục | Mô tả ngắn |
|:---|:---|:---|
| `01-cache-monitor.md` | **Cache Monitor System** | Cơ chế đồng bộ Cache phân tán qua Database để đảm bảo tính nhất quán dữ liệu. |
| `02-ram-monitor.md` | **RAM Monitor** | Cơ chế giám sát và giải phóng tài nguyên. |
