# Core Infrastructure (Hạ tầng Cốt lõi)

## 1. WebApiClient (`Inventec.Common.WebApiClient`)

Đây là thành phần quan trọng nhất, chịu trách nhiệm cho mọi giao tiếp giữa Client và Server.

### Kiến trúc & Design Pattern
*   **Wrapper Pattern**: Bao đóng `System.Net.Http.HttpClient` để đơn giản hóa việc gọi API.
*   **Token Injection**: Tự động chèn `TokenCode` vào Header/QueryParam của request để xác thực với hệ thống ACS.
*   **Serialization**: Sử dụng `Newtonsoft.Json` để tự động chuyển đổi giữa JSON và C# Objects.

### Chi tiết Kỹ thuật
*   **Namespace**: `Inventec.Common.WebApiClient`
*   **Class Chính**: `ApiConsumer`
*   **Critical Method**:
    *   `Get<T>()`: Gọi GET request, tự động deserialize về kiểu `T`.
    *   `Post<T>()`: Gọi POST request với data body, tự động deserialize response về kiểu `T`.
    *   `GetFile()`: Tải file từ server (trả về `MemoryStream`).

### Lưu ý cho Developer
> [!WARNING]
> **Socket Exhaustion Risk**: Implementation hiện tại đang tạo mới `HttpClient` trong khối `using` mỗi lần gọi request. Trong môi trường tải cao, điều này có thể dẫn đến lỗi cạn kiệt cổng kết nối (Socket Exhaustion). Cần cân nhắc refactor sang sử dụng `IHttpClientFactory` hoặc static client nếu gặp vấn đề hiệu năng.

---

## 2. Logging System (`Inventec.Common.Logging`)

Hệ thống ghi nhật ký hoạt động tập trung, giúp dev trace lỗi và monitor hệ thống.

### Công nghệ
*   **Library**: `Log4Net`
*   **Config**: `XmlConfigurator` đọc cấu hình từ file `.config` (App.config/Web.config).

### Các Mức độ Log (Log Levels)
1.  **DEBUG**: Thông tin chi tiết dùng để dev debug (VD: "Start calling API...", "Received response...").
2.  **INFO**: Thông tin luồng nghiệp vụ chính (VD: "User logged in", "Created Patient Profile").
3.  **WARN**: Các vấn đề tiềm ẩn nhưng chưa gây lỗi (VD: "Config file missing, using default").
4.  **ERROR**: Lỗi nghiệp vụ hoặc Runtime Exception (VD: "NullReferenceException", "API 500 Error").
5.  **FATAL**: Lỗi nghiêm trọng khiến ứng dụng crash.

### Code Snippet Chuẩn
```csharp
try 
{
    Inventec.Common.Logging.LogSystem.Info("Bắt đầu xử lý nghiệp vụ X");
    // ... logic code
} 
catch (Exception ex) 
{
    Inventec.Common.Logging.LogSystem.Error("Lỗi xử lý nghiệp vụ X", ex);
    throw; // Rethrow nếu cần UI xử lý tiếp
}
```

---

## 3. Repository Pattern (`Inventec.Common.Repository`)

Cung cấp interface chuẩn cho việc truy cập dữ liệu, giúp tách biệt logic nghiệp vụ khỏi logic truy vấn database.

*   **Mục đích**: Được sử dụng chủ yếu ở Backend hoặc các module làm việc với Local SQLite.
*   **Lợi ích**: Giúp code dễ test hơn (Mocking repository) và dễ thay đổi nguồn dữ liệu.
