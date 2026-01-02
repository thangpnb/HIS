# Action Controls (Các control thao tác)

## 1. PNSimpleButton

`PNSimpleButton` là một Button tùy biến hoàn toàn (Custom Painted) để hỗ trợ giao diện bo tròn và style phẳng (Flat Design).

### Kiến trúc
*   **Base Class**: `System.Windows.Forms.Button` (Lưu ý: Kế thừa Button chuẩn của .NET, không phải DevExpress SimpleButton, mặc dù có references DevExpress).
*   **Cơ chế vẽ**: Override `OnPaint` để tự vẽ hình dáng (Region) của button dựa trên `GraphicsPath` chứa các cung tròn (Arc).

### Thuộc tính Mở rộng (Custom Properties)
Thuộc tính trong nhóm "PN Code Advance":

| Tên thuộc tính | Kiểu dữ liệu | Mô tả | Mặc định |
|:---|:---|:---|:---|
| `BorderRadius` | `int` | Độ bo tròn 4 góc. | 40 |
| `BorderSize` | `int` | Độ dày viền. | 5 |
| `BorderColor` | `Color` | Màu viền. | `Color.Blue` |
| `BackGroundColor`| `Color` | Màu nền nút. | `Color.Blue` |
| `TextColor` | `Color` | Màu chữ. | `Color.White` |

### Logic Vẽ (Rendering Logic)
1.  **AntiAlias**: Luôn bật khử răng cưa (`SmoothingMode.AntiAlias`) để đường cong mịn.
2.  **Trường hợp `BorderRadius > 2`**:
    *   Tạo `GraphicsPath` hình chữ nhật bo góc.
    *   Set `this.Region` theo path này để cắt phần thừa (click ra ngoài vùng bo tròn sẽ không nhận sự kiện).
    *   Vẽ viền (`DrawPath`) bằng `Pen`.
3.  **Trường hợp Button thường**: Vẽ hình chữ nhật vuông vức nếu không set Radius.

### Hướng dẫn Sử dụng
Thường dùng cho các nút to, quan trọng trên màn hình cảm ứng hoặc Kiosk:
```csharp
PNSimpleButton btnSave = new PNSimpleButton();
btnSave.Text = "LƯU TRẠNG THÁI";
btnSave.BackGroundColor = Color.ForestGreen;
btnSave.BorderRadius = 40; // Bo tròn mạnh tạo hình viên thuốc (Pill shape)
btnSave.Click += BtnSave_Click;
```
