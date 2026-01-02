# Quy trình Kiểm soát Nhiễm khuẩn (Infection Control)

> **LƯU Ý QUAN TRỌNG**: Module này hiện nằm **NGOÀI PHẠM VI (OUT OF SCOPE)** của dự án hiện tại. Tài liệu này được giữ lại để tham khảo cho các giai đoạn phát triển sau.

## 1. Tình trạng Hiện tại
*   **Mã nguồn**: Không tìm thấy plugin chuyên biệt cho kiểm soát nhiễm khuẩn (VD: `InfectionControl`, `Sterilization`, `WasteManagement`...).
*   **Tài liệu**: Chưa có tài liệu nghiệp vụ chi tiết.

## 2. Yêu cầu Nghiệp vụ (Dự kiến)
Theo quy định của Bộ Y tế (Thông tư 16/2018/TT-BYT), hệ thống cần đáp ứng:

1.  **Giám sát ca bệnh**:
    *   Phát hiện và theo dõi ca nhiễm khuẩn bệnh viện.
    *   Cảnh báo các trường hợp đa kháng thuốc.
2.  **Quản lý Tiệt khuẩn/Khử khuẩn**:
    *   Quản lý vòng đời dụng cụ y tế (Thu gom -> Làm sạch -> Đóng gói -> Tiệt khuẩn -> Lưu kho -> Cấp phát).
    *   Liên kết với các khoa lâm sàng để truy vết dụng cụ sử dụng cho bệnh nhân nào.
3.  **Quản lý Chất thải Y tế**:
    *   Theo dõi lượng chất thải phát sinh.
    *   Bàn giao cho đơn vị xử lý.

## 3. Khuyến nghị
*   Cần xây dựng module mới hoặc tích hợp với các phần mềm chuyên dụng khác.
*   Trước mắt có thể sử dụng các module quản lý vật tư (`MedicalStore`, `MediStock`) để quản lý hóa chất, dụng cụ kiểm soát nhiễm khuẩn, nhưng chưa đủ để đáp ứng quy trình chuyên môn.
