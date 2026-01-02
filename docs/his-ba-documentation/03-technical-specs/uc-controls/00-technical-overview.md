# Technical Overview: UC Controls (`Inventec.UC`)

## 1. Mục đích

`Inventec.UC` (User Controls) là thư viện chứa các thành phần giao diện người dùng (UI Components) được sử dụng xuyên suốt trong hệ thống HIS. Thư viện này nằm trong thư mục `HIS/Inventec.UC` và chứa hàng ngàn UserControl tái sử dụng.

## 2. Kiến trúc Thư viện

Thư viện được tổ chức theo các nhóm chức năng chính:

*   **Action Controls**: Các nút bấm (Button), thanh công cụ (Toolbar).
*   **Input Controls**: Các ô nhập liệu (TextEdit), combobox (Lookup).
*   **Form Type Controls**: Các màn hình chức năng mẫu (Template Forms).
*   **Service Room Controls**: Các màn hình nghiệp vụ đặc thù cho từng loại phòng (Khám, Dược, Viện phí).

## 3. Danh sách Tài liệu

| File Tài liệu | Hạng mục | Mô tả kỹ thuật |
|:---|:---|:---|
| `01-form-type-controls.md` | **Form Type Controls** | Các control base cho từng loại màn hình (List, Detail, Report). |
| `02-service-room-controls.md` | **Service Room Controls** | Các control base cho từng loại phòng làm việc. |
