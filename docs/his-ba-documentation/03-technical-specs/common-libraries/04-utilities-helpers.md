# Utilities & Helpers (Tiện ích Bổ trợ)

## 1. Xử lý Dữ liệu Cơ bản

### String Utility (`Inventec.Common.String`)
*   Các hàm xử lý chuỗi tiếng Việt (bỏ dấu, chuyển đổi encoding).
*   Cắt chuỗi, format chuỗi template.

### DateTime Utility (`Inventec.Common.DateTime`)
*   Chuyển đổi các định dạng ngày tháng đặc thù y tế (VD: "ngày ... tháng ... năm ...").
*   Tính toán tuổi (theo tháng, theo ngày cho trẻ sơ sinh).

### Number Utility (`Inventec.Common.Number`)
*   Đọc số thành chữ (dùng cho tổng tiền trên hóa đơn/phiếu thu).
*   Làm tròn số tiền theo quy tắc cấu hình.

## 2. Bảo mật & Mã hóa

### Checksum (`Inventec.Common.Checksum`)
*   Tính MD5/SHA cho file hoặc nội dung text để verify toàn vẹn dữ liệu.

### TripleDES (`Inventec.Common.TripleDes`)
*   Mã hóa đối xứng (Symmetric Encryption). Dùng để mã hóa các thông tin nhạy cảm trong file config hoặc local storage.

## 3. Document Processing

### Word Content (`Inventec.Common.WordContent`)
*   Xử lý file Word (OpenXML).
*   Ứng dụng: Ghi nội dung khám bệnh vào template bệnh án Word.

### PDF & Viewer (`Inventec.Common.DocumentViewer`)
*   User Control để hiển thị file PDF/Image ngay trên giao diện phần mềm mà không cần mở phần mềm ngoài.

### Excel Import (`Inventec.Common.ExcelImport`)
*   Đọc dữ liệu từ file Excel upload lên (VD: Import danh mục thuốc thầu, Import danh sách nhân viên).

## 4. Local Storage & Registry

### SQLite (`Inventec.Common.Sqlite`, `Inventec.Common.SQLiteHelper`)
*   Quản lý cơ sở dữ liệu nhỏ tại máy trạm.
*   Dùng để lưu cache danh mục, cấu hình riêng của máy, hoặc dữ liệu offline tạm thời.

### Registry (`Inventec.Common.RegistryUtil`)
*   Lưu các cấu hình user-specific vào Windows Registry (VD: Màu giao diện, Máy in mặc định, Vị trí cửa sổ).
