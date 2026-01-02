# Hardware Integration (Tích hợp Phần cứng)

## 1. Kết nối Thiết bị (`Inventec.Common.ConnectDevice`)

Framework đa năng để giao tiếp với các thiết bị y tế và ngoại vi.

*   **Giao thức hỗ trợ**:
    *   **RS232 (Serial Port)**: Kết nối các máy xét nghiệm đời cũ, máy đọc thẻ BHYT cũ.
    *   **TCP/IP**: Kết nối máy xét nghiệm hiện đại, máy in mạng.
*   **Ứng dụng**:
    *   Kết nối Máy xét nghiệm (LIS): Nhận kết quả từ máy trả về (ASTM Protocol).
    *   Kết nối Bảng hiển thị LED (QMS): Gửi số thứ tự lên bảng LED.

## 2. Barcode & QR Code

### Barcode (`Inventec.Common.BarcodeLib`)
*   **Chức năng**: Tạo mã vạch 1D (Code 128, Code 39).
*   **Ứng dụng**: In mã vạch trên nhãn ống nghiệm, vòng đeo tay bệnh nhân, mã hồ sơ.

### QR Code (`Inventec.Common.QRCoder`)
*   **Chức năng**: Tạo mã 2D (QR Code).
*   **Ứng dụng**: Mã QR trên phiếu chỉ định, Hóa đơn điện tử, Thẻ BHYT.

### Bank QR (`Inventec.Common.BankQrCode`)
*   **Chức năng**: Tạo mã VietQR chuẩn (bao gồm thông tin TK ngân hàng, số tiền, nội dung).
*   **Ứng dụng**: In mã QR thanh toán trên Phiếu thu viện phí để bệnh nhân quét và chuyển khoản.

## 3. In ấn (Printing Integration)

### FlexCel (`Inventec.Common.FlexCelPrint`)
*   **Công nghệ**: Sử dụng `TMS FlexCel`.
*   **Ưu điểm**: Không cần cài đặt Microsoft Excel trên máy client. Tốc độ rất nhanh.
*   **Cách dùng**: Dùng file Excel (.xls/.xlsx) làm template, map dữ liệu vào các ô (Merge Field) và in.

### MS Office (`Inventec.Common.MSOfficePrint`)
*   **Công nghệ**: COM Interop.
*   **Nhược điểm**: Yêu cầu máy client phải cài Excel. Chậm và dễ lỗi version.
*   **Sử dụng**: Chỉ dùng cho các báo cáo cực kỳ phức tạp mà FlexCel không xử lý được.

### Dialog Máy in (`Inventec.Common.ChoosePrinter`)
*   **Chức năng**: Hiển thị hộp thoại chọn máy in, lưu lại máy in mặc định cho từng loại phiếu (Ví dụ: Phiếu thu in máy A, Vòng tay in máy B).
