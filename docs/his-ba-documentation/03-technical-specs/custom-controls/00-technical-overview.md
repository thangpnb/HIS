# Technical Overview: Custom Controls (`Inventec.CustomControls`)

## 1. Tổng quan

`Inventec.CustomControls` là bộ thư viện giao diện người dùng (UI Library) bổ trợ, cung cấp các control được tùy biến giao diện (Customized Styling) đặc thù mà bộ thư viện chuẩn `DevExpress` hoặc `.NET WinForms` mặc định chưa đáp ứng được.

Mục tiêu chính của thư viện này là tạo ra các thành phần giao diện có **tính thẩm mỹ cao hơn**, cụ thể là hỗ trợ **Rounded Corners (Bo tròn góc)** và các hiệu ứng visual hiện đại, giúp ứng dụng HIS trông mềm mại và thân thiện hơn (User-Friendly).

## 2. Đặc điểm Kỹ thuật

*   **Custom Painting**: Sử dụng GDI+ (`System.Drawing.Drawing2D`) và override phương thức `OnPaint` để vẽ lại border, background với các đường cong (Arc).
*   **Wrapper Pattern**: Bao đóng (Wrap) các control chuẩn của DevExpress (như `TextEdit`) bên trong một `UserControl` để thêm lớp vỏ (Shell) đồ họa mới.
*   **Properties mở rộng**: Cung cấp các thuộc tính Design-time để dễ dàng cấu hình độ bo góc (`BorderRadius`), độ dày viền (`BorderSize`), và màu sắc (`BorderColor`).

## 3. Cấu trúc Tài liệu

| Tên File | Thành phần | Mô tả |
|:---|:---|:---|
| `01-input-controls.md` | **Input Controls** | Các control nhập liệu (Textbox) có bo góc. |
| `02-action-controls.md` | **Action Controls** | Các nút bấm (Button) có bo góc và style phẳng. |
