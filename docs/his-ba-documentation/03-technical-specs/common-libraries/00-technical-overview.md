# Technical Overview: Common Libraries (Thư viện Dùng chung)

## 1. Tổng quan Kiến trúc

`Inventec.Common` là bộ thư viện nền tảng (Foundation Framework) đóng vai trò xương sống cho toàn bộ hệ sinh thái HIS (Desktop, API, Services). Nó cung cấp các lớp tiện ích được chuẩn hóa để đảm bảo tính nhất quán, bảo mật và khả năng tái sử dụng mã nguồn trên toàn hệ thống.

### Phạm vi & Vai trò
*   **Core Infrastructure**: Cung cấp các wrapper cho giao tiếp mạng (HTTP Client), ghi log, và truy cập dữ liệu.
*   **Integration Layer**: Cầu nối tích hợp với các hệ thống bên ngoài (Hóa đơn điện tử, Thanh toán, BHYT).
*   **Hardware Abstraction**: Lớp trừu tượng hóa việc kết nối thiết bị y tế và thiết bị ngoại vi.
*   **Utilities**: Các hàm bổ trợ xử lý dữ liệu cơ bản.

## 2. Cấu trúc Tài liệu Kỹ thuật

Tài liệu kỹ thuật cho Common Libraries được chia nhỏ theo nhóm chức năng để dễ tra cứu:

| File Tài liệu | Nhóm Chức năng | Mô tả ngắn |
|:---|:---|:---|
| `01-core-infrastructure.md` | **Hạ tầng Cốt lõi** | WebApiClient, Logging, Cấu hình hệ thống. |
| `02-external-integrations.md`| **Tích hợp Bên ngoài**| Hóa đơn điện tử (EBill), Redis Cache, Facebook Wit.ai. |
| `03-hardware-integration.md` | **Tích hợp Phần cứng** | Máy in, Barcode, Serial Port, kết nối thiết bị y tế. |
| `04-utilities-helpers.md` | **Tiện ích Bổ trợ** | Xử lý ngày tháng, chuỗi, ma hóa, XML/Excel/PDF. |

## 3. Nguyên tắc Sử dụng

*   **Không phát minh lại bánh xe (Don't Reinvent the Wheel)**: Luôn kiểm tra `Inventec.Common` trước khi viết mới một tiện ích.
*   **Thống nhất Giao tiếp**: Mọi giao tiếp với Backend **BẮT BUỘC** phải thông qua `Inventec.Common.WebApiClient` (được wrap bởi `HIS.Desktop.ApiConsumer`) để đảm bảo Token được truyền đúng cách.
*   **Thống nhất Logging**: Mọi lỗi (Exception) **PHẢI** được ghi lại bằng `Inventec.Common.Logging.LogSystem`.
