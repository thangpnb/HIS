# Khám Phá và Vòng Đời Plugin

## Cơ chế Khám phá (Plugin Discovery)

Ứng dụng HIS Desktop sử dụng cơ chế khám phá động để tự động nhận diện và tải các plugin khi khởi động. Quá trình này đảm bảo tính linh hoạt, cho phép thêm hoặc bớt tính năng bằng cách đơn giản là thêm hoặc xóa file plugin mà không cần biên dịch lại ứng dụng chính.

### Quy trình Tải

```mermaid
graph TB
    Entry["Khởi chạy ứng dụng HIS.Desktop"]
    Scanner["Trình quét Plugin (Scanner)<br/>(Inventec.Desktop.Core)"]
    Registry["Đăng ký Plugin (Registry)"]
    Loader["Trình tải Plugin (Loader)"]
    Cache["CacheClient<br/>(HIS.Desktop.Library.CacheClient)"]
    
    Entry --> Scanner
    Scanner --> |"Quét thư mục HIS/Plugins/"| Registry
    Registry --> |"Lấy Metadata"| Cache
    Registry --> Loader
    Loader --> |"Khởi tạo"| PluginInstance["Thực thể Plugin"]
    
    subgraph "Các Không gian tên (Namespace)"
        HIS["HIS.Desktop.Plugins.*"]
        ACS["ACS.Desktop.Plugins.*"]
        Other["Các Namespace khác (LIS, EMR, v.v.)"]
    end
    
    Loader --> HIS
    Loader --> ACS
    Loader --> Other
```

**Thành phần tham gia:**
*   **Plugin Scanner**: Quét thư mục cài đặt để tìm các file `.dll` tuân thủ quy ước plugin.
*   **Plugin Registry**: Duy trì danh sách các plugin khả dụng và metadata của chúng.
*   **CacheClient**: Lưu trữ metadata để tăng tốc độ khởi động trong các lần chạy sau.

## Vòng Đời Plugin (Lifecycle)

Mỗi plugin tuân theo một chu trình sống nghiêm ngặt được quản lý bởi `Inventec.Desktop.Core`. Điều này đảm bảo tài nguyên hệ thống được sử dụng hiệu quả và tránh rò rỉ bộ nhớ.

### Các Giai đoạn

```mermaid
stateDiagram-v2
    [*] --> Discovery: Khởi động App
    Discovery --> Registration: Tìm thấy
    Registration --> Instantiation: Người dùng kích hoạt
    Instantiation --> Initialization: Constructor
    Initialization --> Execution: Gọi Run()
    Execution --> Active: Hiển thị UI
    Active --> Disposal: Đóng chức năng
    Disposal --> [*]
    
    note right of Instantiation
        Dependency Injection
        Khởi tạo biến
    end note
    
    note right of Active
        Xử lý nghiệp vụ
        Tương tác người dùng
    end note
```

### Interface Module

Mọi plugin phải triển khai interface chuẩn của module để Core có thể điều khiển:

| Giai đoạn | Phương thức | Mô tả |
|-----------|-------------|-------|
| **Khởi tạo** | `Constructor` | Tạo thực thể, nhận các dependency cần thiết. |
| **Thực thi** | `Run()` | Điểm bắt đầu logic chính, thường là khởi tạo và hiển thị Form. |
| **Kết thúc** | `Dispose()` | Dọn dẹp tài nguyên, hủy đăng ký sự kiện, đóng kết nối. |

### Luồng Dữ liệu Khởi chạy

```mermaid
sequenceDiagram
    participant User as "Người dùng"
    participant Core as "Plugin Core"
    participant Plugin as "Instance Plugin"
    participant Form as "Giao diện (Form)"
    
    User->>Core: Chọn chức năng từ Menu
    Core->>Plugin: new Plugin(metadata)
    Core->>Plugin: Run()
    Plugin->>Form: new Form()
    Plugin->>Form: Show()
    Form-->>User: Hiển thị giao diện
    User->>Form: Tương tác / Đóng
    Form->>Plugin: Signal Close
    Plugin->>Core: Dispose()
```

---
*Xem tiếp: [Cấu trúc & Tổ chức Plugin](03-structure-organization.md)*
