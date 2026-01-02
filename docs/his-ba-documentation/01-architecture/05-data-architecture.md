# Kiến trúc Dữ liệu (Data Architecture)

## 1. Luồng Dữ liệu Tổng quát (General Data Flow)

Hệ thống HIS phải xử lý lượng dữ liệu khổng lồ với yêu cầu tốc độ cao. Do đó, luồng dữ liệu được thiết kế tối ưu hóa qua nhiều lớp đệm (cache).

```mermaid
sequenceDiagram
    participant User as "Người Dùng (UI)"
    participant Plugin as "Plugin Logic"
    participant Cache as "BackendData (Cache)"
    participant API as "ApiConsumer"
    participant Server as "HIS Server"

    User->>Plugin: Yêu cầu dữ liệu (Ví dụ: Danh sách thuốc)
    Plugin->>API: GetMedicineList()
    API->>Cache: Kiểm tra có trong Cache không?
    
    alt Có trong Cache (Cache Hit)
        Cache-->>API: Trả về dữ liệu ngay lập tức (1ms)
        API-->>Plugin: Dữ liệu thuốc
    else Không có trong Cache (Cache Miss)
        API->>Server: Gọi REST API qua Internet (200ms)
        Server-->>API: Trả về JSON
        API->>Cache: Lưu vào Cache để dùng lần sau
        API-->>Plugin: Dữ liệu thuốc mới nhất
    end
    
    Plugin->>User: Hiển thị lên lưới
```

## 2. Các Thành phần Dữ liệu Cốt lõi

### 2.1. BackendData (Hệ thống Cache)
*   **Vị trí**: `HIS.Desktop.LocalStorage.BackendData` (69 file)
*   **Vai trò**: Là trái tim của hệ thống lưu trữ phía Client. Nó lưu trữ các danh mục ít thay đổi (Danh mục thuốc, danh mục bệnh, danh sách phòng ban) xuống đĩa cứng (SQLite/File).
*   **Lợi ích**: Giúp ứng dụng khởi động nhanh, hoạt động mượt mà ngay cả khi mất mạng tạm thời, và giảm tải đáng kể cho Server.

### 2.2. ADO (Application Data Objects)
*   **Vị trí**: `HIS.Desktop.ADO` (74 file)
*   **Vai trò**: Định nghĩa khuôn mẫu dữ liệu (Data Schema) để trao đổi giữa Client và Server.
*   **Đặc điểm**: Đây là các lớp C# thuần túy (POCO), ánh xạ 1-1 với cấu trúc JSON trả về từ API.

### 2.3. Cấu hình Ứng dụng
*   `ConfigApplication`: Lưu các cài đặt của người dùng (vị trí cửa sổ, máy in mặc định).
*   `HisConfig`, `SdaConfigKey`: Lưu các cấu hình hệ thống quan trọng được tải về từ Server.

## 3. Kiến trúc Dữ liệu In ấn (MPS Data)

Hệ thống in (MPS) sử dụng một cấu trúc dữ liệu riêng biệt gọi là **PDO (Print Data Objects)** để đảm bảo tính độc lập với nghiệp vụ HIS.

```mermaid
graph LR
    HIS_Data["Dữ liệu HIS<br/>(ADO)"]
    Converter["Bộ chuyển đổi<br/>(Mapper)"]
    PDO["Dữ liệu In<br/>(PDO)"]
    Template["Mẫu báo cáo<br/>(Excel/PDF)"]
    Output["File kết quả"]
    
    HIS_Data --> Converter
    Converter --> PDO
    PDO --> Output
    Template --> Output
```

*   **Nguyên lý**: Plugin HIS không gửi trực tiếp dữ liệu thô cho máy in. Nó phải chuyển đổi (Map) dữ liệu đó sang một đối tượng PDO chuẩn.
*   **Lợi ích**: Nếu cấu trúc Database thay đổi, ta chỉ cần sửa `Converter`, không cần sửa lại hàng trăm mẫu in đang hoạt động ổn định.
