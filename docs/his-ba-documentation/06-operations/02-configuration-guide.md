# Hướng dẫn Cấu hình (Configuration Guide)

Tài liệu này giải thích các tham số trong file `HIS.Desktop.exe.config`.

## 1. Chuỗi Kết nối (Connection Strings)
Các tham số kết nối tới Database Oracle/SQL Server.

```xml
<appSettings>
    <!-- Địa chỉ API Server -->
    <add key="ApiBaseUrl" value="http://192.168.1.100:5000/api/" />
    
    <!-- Timeout kết nối (ms) -->
    <add key="RequestTimeout" value="30000" />
</appSettings>
```

## 2. Cấu hình Máy trạm (Workstation Config)
Lưu tại `HIS.Desktop.LocalStorage.ConfigApplication` (User-specific).

| Key | Mô tả | Giá trị Mặc định |
|:---|:---|:---|
| `PRINTER_LABEL_NAME` | Tên máy in tem/nhãn. | `Zebra_GK420t` |
| `PRINTER_A4_NAME` | Tên máy in A4 mặc định. | `Canon_LBP2900` |
| `REMEMBER_LOGIN` | Ghi nhớ tên đăng nhập. | `true` |
| `THEME_COLOR` | Màu giao diện. | `Blue` |

## 3. Cấu hình Debug/Log
Để bật chế độ ghi log chi tiết khi gặp lỗi:

```xml
<add key="EnableDebugLog" value="true" />
<add key="LogFilePath" value="D:\HIS.Logs\Error.log" />
```

> **Lưu ý**: Chỉ bật `EnableDebugLog` trên máy của IT hoặc khi cần điều tra lỗi, vì file log có thể tăng kích thước rất nhanh.
