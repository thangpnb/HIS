# Input Controls (Các control nhập liệu)

## 1. PNTextEdit

`PNTextEdit` là một `UserControl` được thiết kế để thay thế cho `DevExpress.XtraEditors.TextEdit` khi cần giao diện bo tròn.

### Kiến trúc
*   **Base Class**: `System.Windows.Forms.UserControl`.
*   **Core Component**: Nhúng bên trong một `DevExpress.XtraEditors.TextEdit` (biến `textEdit1`).
*   **Mục đích**: `TextEdit` chuẩn của DevExpress có hình chữ nhật sắc cạnh. `PNTextEdit` vẽ một container bo tròn (`BorderRadius`) bao quanh nó để tạo cảm giác mềm mại.

### Thuộc tính Mở rộng (Custom Properties)
Các thuộc tính này xuất hiện trong nhóm "PN Code Inventec" trên Property Grid của Visual Studio:

| Tên thuộc tính | Kiểu dữ liệu | Mô tả | Mặc định |
|:---|:---|:---|:---|
| `BorderRadius` | `int` | Độ bo tròn của 4 góc (Pixel). | 40 |
| `BorderSize` | `int` | Độ dày của đường viền. | 5 |
| `BorderColor` | `Color` | Màu sắc đường viền. | `Color.Blue` |
| `BackgroundColor`| `Color` | Màu nền của control. | `Color.White` |
| `Texts` | `string` | Wrapper cho `textEdit1.Text`. | |
| `EditMaskPn` | `string` | Wrapper cho `Properties.Mask.EditMask` (Format nhập liệu).| |
| `MaskTypes` | `MaskType` | Kiểu mask (Numeric, DateTime, RegEx...). | |
| `TextHintNull` | `string` | Placeholder text hiện khi ô trống. | |

### Sự kiện (Events)
Control này forward (chuyển tiếp) các sự kiện quan trọng từ `textEdit1` ra ngoài:
*   `_TextChanged`: Khi nội dung văn bản thay đổi.
*   `_EditValueChanged`: Khi giá trị bind thay đổi.

### Hướng dẫn Sử dụng
```csharp
// Khởi tạo và sử dụng như TextEdit thông thường
PNTextEdit txtName = new PNTextEdit();
txtName.BorderRadius = 20; // Bo nhẹ
txtName.BorderColor = Color.LightGray;
txtName.TextHintNull = "Nhập họ tên bệnh nhân...";
this.Controls.Add(txtName);

// Lấy giá trị
string name = txtName.Texts; 
```
