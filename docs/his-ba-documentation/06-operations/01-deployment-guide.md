# Hướng dẫn Triển khai (Deployment Guide)

Tài liệu này hướng dẫn cách cài đặt và triển khai ứng dụng HIS Desktop cho người dùng cuối (Bác sĩ, Điều dưỡng, Thu ngân).

## 1. Yêu cầu Hệ thống (Client)
*   **OS**: Windows 7 SP1 trở lên (Khuyến nghị Windows 10/11).
*   **Framework**: .NET Framework 4.5.2 trở lên.
*   **RAM**: Tối thiểu 4GB (Khuyến nghị 8GB).
*   **Màn hình**: Độ phân giải tối thiểu 1366x768.

## 2. Quy trình Cài đặt

### Cách 1: Copy-Paste (Thủ công)
Do ứng dụng HIS Desktop là Portable (không cần cài đặt registry phức tạp), quy trình đơn giản nhất là:
1.  Tải gói `HIS.Desktop.Client.zip` từ máy chủ triển khai.
2.  Giải nén vào thư mục `D:\HIS.Desktop` (Tránh cài vào ổ C để hạn chế lỗi quyền Admin).
3.  Tạo Shortcut cho file `HIS.Desktop.exe` ra Desktop.

### Cách 2: Sử dụng Launcher (Tự động cập nhật)
Sử dụng `HIS.Updater.exe` để tự động tải phiên bản mới nhất.
1.  Cấu hình đường dẫn Update Server trong `Updater.config`.
2.  Chạy `HIS.Updater.exe`.
3.  Chương trình sẽ tự động tải các DLL mới nhất về thư mục local và khởi chạy ứng dụng chính.

## 3. Cấu trúc Thư mục Client
Sau khi cài đặt, cấu trúc thư mục sẽ như sau:
```
D:\HIS.Desktop\
├── HIS.Desktop.exe        # File chạy chính
├── HIS.Desktop.exe.config # File cấu hình kết nối
├── Plugins\               # Chứa ~950 DLL nghiệp vụ
├── ReportTemplates\       # (Tùy chọn) Mẫu báo cáo lưu offline (cache)
└── Logs\                  # Log lỗi client
```

## 4. Kiểm tra Sau cài đặt
1.  Mở ứng dụng, kiểm tra màn hình đăng nhập hiện version mới nhất.
2.  Kiểm tra kết nối tới Server (nếu mất mạng sẽ báo lỗi ngay góc dưới).
3.  Đăng nhập thử bằng tài khoản Test.
4.  Mở một chức năng in ấn để kiểm tra driver máy in (FlexCell/DevExpress).
