# Technical Spec: Công tác Xã hội (Social Work)

## 1. Business Mapping
*   **Ref**: [Công tác Xã hội & Chăm sóc Khách hàng](../../02-business-processes/patient-management/03-social-work.md)
*   **Scope**: Quản lý yêu cầu hỗ trợ, duyệt chi từ quỹ từ thiện và chăm sóc khách hàng.
*   **Key Plugins**: `HIS.Desktop.Plugins.CustomerRequest`, `HIS.Desktop.Plugins.HisFund`.

## 2. Core Components (Codebase Mapping)

### 2.1. Plugin: CustomerRequest
*   **Purpose**: Ghi nhận yêu cầu từ người bệnh hoặc khoa lâm sàng.
*   **Implementation**:
    *   `CustomerRequestProcessor.cs`: Xử lý nghiệp vụ CRUD yêu cầu.
    *   `ICustomerRequest.cs`: Interface định nghĩa behaviour.
    *   `CustomerRequestBehavior.cs`: Logic xử lý sự kiện UI.
    *   `UCCustomerRequest.cs`: User Control nhúng vào các form khác.

### 2.2. Plugin: HisFund (Quỹ Từ thiện)
*   **Purpose**: Quản lý nguồn tiền và phiếu chi hỗ trợ.
*   **Implementation**:
    *   `HisFundProcessor.cs`: Quản lý danh mục quỹ, số dư.
    *   `TransactionProcessor`: Xử lý giao dịch thu/chi (liên kết module Tài chính).

## 3. Process Flow (Technical Deep Dive)

### 3.1. Luồng Duyệt Chi Hỗ trợ (Funding Approval)
```mermaid
sequenceDiagram
    participant Staff as CTXH Viên
    participant Req as CustomerRequestPlugin
    participant Fund as HisFundPlugin
    participant DB as Database

    Staff->>Req: 1. Tạo yêu cầu hỗ trợ (SupportReq)
    Req->>DB: Insert HIS_CUSTOMER_REQUEST (Status: New)
    
    note right of Staff: Ban giám đốc duyệt
    Staff->>Req: 2. Approve Request
    Req->>DB: Update Status = APPROVED
    
    Staff->>Fund: 3. Lập phiếu chi (Sponsorship Payment)
    Fund->>DB: Check Fund Balance (Tồn quỹ)
    
    alt Enough Balance
        Fund->>DB: Insert HIS_TRANSACTION (Loại: Chi từ thiện)
        Fund->>DB: Update Fund Balance
        DB-->>Fund: Success
    else Not Enough
        Fund-->>Staff: Error "Quỹ không đủ tiền"
    end
```

## 4. Database Schema

### 4.1. HIS_CUSTOMER_REQUEST
Bảng lưu yêu cầu hỗ trợ.
*   `ID`: PK.
*   `CUSTOMER_NAME`, `PHONE`: Thông tin người yêu cầu.
*   `REQUEST_TYPE_ID`: Loại yêu cầu (1: Hỗ trợ tài chính, 2: Tư vấn, 3: Phản ánh).
*   `CONTENTS`: Nội dung chi tiết.
*   `PROCESS_STATUS`: Trạng thái xử lý.

### 4.2. HIS_FUND
Danh mục quỹ.
*   `ID`: PK.
*   `FUND_CODE`, `FUND_NAME`: Tên quỹ (VD: Quỹ Bếp ăn tình thương).
*   `TOTAL_AMOUNT`: Tổng tiền hiện có.
*   `IS_ACTIVE`: Trạng thái hoạt động.

### 4.3. HIS_PATIENT_PROGRAM
Bảng liên kết bệnh nhân vào chương trình hỗ trợ.
*   `PATIENT_ID`: FK.
*   `PROGRAM_ID`: Chương trình (VD: Mổ mắt miễn phí).
*   `FROM_TIME`, `TO_TIME`: Thời gian hiệu lực.

## 5. Integration Points
*   **Module Viện phí (`Fee`)**: Khi phiếu chi từ thiện được duyệt, hệ thống viện phí cần ghi nhận khoản thanh toán này cho bệnh nhân (giảm trừ số tiền phải đóng).
*   **Module SMS Brandname (`ChanelManager`)**: Gửi SMS thông báo kết quả duyệt hỗ trợ cho bệnh nhân.
