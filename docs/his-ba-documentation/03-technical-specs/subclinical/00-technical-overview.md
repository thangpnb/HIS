# Technical Overview: Phân hệ Cận lâm sàng (Subclinical)

## 1. Tổng quan Kiến trúc
Phân hệ Cận lâm sàng (CLS) bao gồm các module quản lý Chẩn đoán hình ảnh (X-Quang, MRI, CT), Thăm dò chức năng (Nội soi, Điện tim) và Giải phẫu bệnh.
Kiến trúc chung đều xoay quanh việc xử lý `HIS_SERVICE_REQ` (Phiếu yêu cầu) và `HIS_SERE_SERV` (Dịch vụ chỉ định).

### 1.1. Common Workflow patterns
Hầu hết các module CLS đều tuân theo mô hình **Execute - Report**:
1.  **Tiếp nhận & Thực hiện (Execute)**: Đánh dấu bệnh nhân đã vào phòng, đã chụp/soi.
    *   Plugin chính: `HIS.Desktop.Plugins.ExecuteRoom`.
2.  **Trả kết quả (Report)**: Nhập mô tả, kết luận và in phiếu.
    *   Plugin chính: `HIS.Desktop.Plugins.ExamServiceReqResult`.

## 2. Core Components

### 2.1. Framework Nhập Kết quả (ExamServiceReqResult)
Đây là trái tim của phân hệ CLS, cho phép cấu hình động các giao diện nhập liệu.
*   **Plugin Name**: `HIS.Desktop.Plugins.ExamServiceReqResult`.
*   **Namespace**: `HIS.Desktop.Plugins.ExamServiceReqResult`.
*   **Chức năng**:
    *   Load Template mẫu mô tả theo `ServiceType`.
    *   Hỗ trợ Editor soạn thảo kết quả (bên trái) và danh sách dịch vụ (bên phải).
    *   Tích hợp Module `PacsApiConsumer` để xem ảnh DICOM.

### 2.2. Sắp xếp hàng đợi (Queue Management)
*   **Plugin Name**: `HIS.Desktop.Plugins.HisServiceNumOrderExecute`.
*   **Chức năng**: Quản lý số thứ tự thực hiện tại các phòng chức năng.
*   **Logic**:
    *   Hiển thị danh sách bệnh nhân đang chờ.
    *   Gọi tên bệnh nhân (Text-to-Speech).
    *   Cập nhật trạng thái `IN_ROOM`.

### 3. Database Schema Overview

#### 3.1. HIS_SERVICE_REQ (Header)
Chứa thông tin phiếu yêu cầu từ phòng khám/khoa lâm sàng.
*   `ID`: PK.
*   `REQUEST_ROOM_ID`: Phòng yêu cầu.
*   `EXECUTE_ROOM_ID`: Phòng thực hiện.
*   `INTRUCTION_TIME`: Thời gian chỉ định.

#### 3.2. HIS_SERE_SERV (Detail/Service)
Chứa thông tin từng dịch vụ kỹ thuật.
*   `ID`: PK.
*   `SERVICE_REQ_ID`: FK.
*   `T_SERVICE_ID`: FK to HIS_SERVICE.
*   `AMOUNT`: Số lượng.

#### 3.3. HIS_SERE_SERV_EXT (Extension)
Lưu trữ kết quả mô tả chi tiết (CLOB/Text).
*   `DESCRIPTION`: Mô tả kỹ thuật/Tổn thương.
*   `CONCLUDE`: Kết luận.
*   `JSON_PRINT_ID`: ID mẫu in (nếu có custom json).

## 4. Đặc thù từng Module
| Module | Plugin Chính nhận/trả KQ | Đặc điểm |
|:---|:---|:---|
| **Chẩn đoán hình ảnh** | `ExamServiceReqResult` | Tích hợp PACS View, in phim. |
| **Nội soi/TĐCN** | `ExamServiceReqResult` | Kết nối Camera bắt hình, in 2-4 ảnh màu. |
| **Giải phẫu bệnh** | `SamplePathologyReq` | Quy trình quản lý mẫu (Block) phức tạp hơn. |

## 5. Integration Points
*   **PACS System**: Giao tiếp qua `HIS.Desktop.Plugins.PacsApiConsumer` (HTTP REST/DICOM Web).
*   **QMS**: Hệ thống gọi số xếp hàng thông minh.
