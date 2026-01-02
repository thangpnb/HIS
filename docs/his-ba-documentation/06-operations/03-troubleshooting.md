# Xử lý Sự cố & Bảo trì (Troubleshooting)

## 1. Các lỗi thường gặp (Common Issues)

### 1.1. Lỗi "Kết nối máy chủ thất bại"
*   **Triệu chứng**: Khi mở phần mềm hoặc khi đăng nhập, báo "Không thể kết nối đến server".
*   **Nguyên nhân**:
    *   Mất mạng LAN.
    *   API Server bị tắt hoặc treo.
    *   Sự cố tường lửa chặn port 5000.
*   **Khắc phục**:
    1.  Ping IP máy chủ (VD: `ping 192.168.1.100`).
    2.  Kiểm tra truy cập link API trên trình duyệt.
    3.  Tạm tắt Windows Firewall trên máy trạm.

### 1.2. Lỗi "Thiếu file DLL" hoặc "Plugin not found"
*   **Triệu chứng**: Vào một chức năng cụ thể bị văng hoặc báo lỗi ClassNotFound.
*   **Nguyên nhân**: Quá trình update bị lỗi, copy thiếu file trong thư mục `Plugins`.
*   **Khắc phục**: Chạy lại `HIS.Updater.exe` để tải lại các file thiếu.

### 1.3. Lỗi In ấn (Font chữ, Lệch lề)
*   **Triệu chứng**: Phiếu in ra bị ô vuông (lỗi font) hoặc mất dòng cuối.
*   **Khắc phục**:
    *   Cài đặt đủ bộ Font VNI và Unicode (Arial, Times New Roman).
    *   Kiểm tra khổ giấy mặc định của máy in (Letter vs A4).

## 2. Quy trình Bảo trì Định kỳ
### Hàng ngày
*   Kiểm tra log lỗi tập trung của Server.
*   Backup Database vào 12h trưa và 12h đêm.

### Hàng tuần
*   Xóa file Temp/Cache trên máy trạm nếu ổ cứng đầy.
*   Khởi động lại API Server để giải phóng RAM.

## 3. Công cụ Hỗ trợ
*   **TeamViewer / UltraView**: Để remote hỗ trợ từ xa.
*   **SdaExecuteSql**: Để fix lỗi dữ liệu (Dành cho Admin).
*   **Log Viewer**: Công cụ xem file log của HIS.
