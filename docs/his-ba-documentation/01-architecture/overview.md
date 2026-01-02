# Tổng quan Kiến trúc HisNguonMo

Chào mừng đến với tài liệu kỹ thuật của hệ thống HisNguonMo. Để giúp độc giả dễ dàng tiếp cận hệ thống phức tạp với hơn 956 plugin này, tài liệu kiến trúc đã được chia nhỏ thành 5 phần chuyên biệt.


Vui lòng chọn tài liệu phù hợp với vai trò của bạn:

```mermaid
graph TD
    User((Người đọc))
    
    Index[("Tổng quan (Overview)")]
    
    subgraph "Các góc nhìn Kiến trúc"
        Context["1. Ngữ cảnh Hệ thống<br/>(System Context)"]
        Func["2. Kiến trúc Nghiệp vụ<br/>(Functional)"]
        Tech["3. Kiến trúc Kỹ thuật<br/>(Technical)"]
        Deploy["4. Kiến trúc Triển khai<br/>(Deployment)"]
        Data["5. Kiến trúc Dữ liệu<br/>(Data)"]
    end
    
    User --> Index
    Index --> Context
    Index --> Func
    Index --> Tech
    Index --> Deploy
    Index --> Data
    
    Context -.->|"Chi tiết hóa"| Func
    Func -.->|"Hiện thực hóa"| Tech
    Tech -.->|"Vận hành trên"| Deploy
    Tech -.->|"Lưu trữ trong"| Data
    
    style Index fill:#f9f,stroke:#333
    style Context fill:#bbf,stroke:#333
    style Func fill:#bfb,stroke:#333
    style Tech fill:#fbf,stroke:#333
    style Deploy fill:#fbb,stroke:#333
    style Data fill:#bff,stroke:#333
```

## Mục lục Tài liệu Kiến trúc

### 1. [Ngữ cảnh Hệ thống (System Context)](01-system-context.md)
> **Dành cho:** Tất cả mọi người (BA, Dev, PM, Stakeholders)
> *   Tổng quan về mục đích hệ thống HIS.
> *   Sơ đồ ngữ cảnh (Context Diagram): Ai sử dụng hệ thống? Hệ thống kết nối với bên nào (Bảo hiểm, LIS, PACS)?

### 2. [Kiến trúc Nghiệp vụ (Functional Architecture)](02-functional-architecture.md)
> **Dành cho:** Business Analyst (BA), Product Owner
> *   Phân tích 4 module lớn: HIS, MPS, UC, Common.
> *   Bản đồ các phân hệ chức năng: Tiếp đón, Khám bệnh, Dược, Viện phí, v.v.
> *   Mối quan hệ giữa Module kỹ thuật và Nghiệp vụ bệnh viện.

### 3. [Kiến trúc Kỹ thuật (Technical Architecture)](03-technical-architecture.md)
> **Dành cho:** Developers, Tech Leads
> *   Công nghệ sử dụng (Tech Stack): .NET 4.5, DevExpress.
> *   Kiến trúc phân tầng (Layered Architecture).
> *   Các mẫu giao tiếp plugin (Sync/Async, PubSub/Delegate).

### 4. [Kiến trúc Triển khai (Deployment Architecture)](04-deployment-architecture.md)
> **Dành cho:** DevOps, System Admin
> *   Tổ chức mã nguồn và Solution.
> *   Các phụ thuộc (Dependencies) và thư viện ngoài.
> *   Mô hình triển khai vật lý (Client-Server, Load Balancing).

### 5. [Kiến trúc Dữ liệu (Data Architecture)](05-data-architecture.md)
> **Dành cho:** Database Developers, Backend Devs
> *   Luồng dữ liệu (Data Flow) và chiến lược Caching (BackendData).
> *   Các đối tượng dữ liệu (ADO, PDO).
> *   Cơ chế xử lý dữ liệu in ấn.

---

## Tài liệu liên quan khác
*   [Kiến trúc Plugin](plugin-system/01-overview.md): Cách chúng tôi quản lý 956 module.