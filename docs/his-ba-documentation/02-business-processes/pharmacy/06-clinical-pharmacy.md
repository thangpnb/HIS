# Dược Lâm Sàng (Clinical Pharmacy)

## 1. Tổng quan
Phân hệ Dược Lâm Sàng hỗ trợ các dược sĩ và bác sĩ trong việc đảm bảo sử dụng thuốc an toàn, hợp lý và hiệu quả. Hệ thống cung cấp các công cụ cảnh báo tương tác thuốc và quy trình báo cáo phản ứng có hại (ADR).

## 2. Quy trình Nghiệp vụ

### 2.1. Cảnh báo Tương tác Thuốc (Drug Interactions)
Hệ thống tự động kiểm tra tương tác khi bác sĩ kê đơn thuốc.
*   **Cơ sở dữ liệu**: Dựa trên danh mục hoạt chất (`Active Ingredient`) và cặp tương tác được cấu hình sẵn.
*   **Mức độ cảnh báo**:
    *   **Chống chỉ định (Mức 1)**: Ngăn chặn kê đơn.
    *   **Thận trọng (Mức 2)**: Hiển thị cảnh báo nhưng cho phép kê đơn kèm lý do.
*   **Plugin chính**:
    *   `HIS.Desktop.Plugins.ActiveIngredientAndConflict`: Quản lý danh mục tương tác.
    *   `HIS.Desktop.Plugins.HisContraindication`: Quản lý chống chỉ định.

### 2.2. Báo cáo ADR (Adverse Drug Reaction)
Quy trình ghi nhận và báo cáo phản ứng có hại của thuốc xảy ra trên bệnh nhân.
1.  **Phát hiện**: Bác sĩ/Điều dưỡng phát hiện dấu hiệu bất thường sau khi dùng thuốc.
2.  **Ghi nhận**: Nhập thông tin phản ứng vào phiếu báo cáo ADR trên hệ thống (`HisAdr`).
    *   Thông tin bệnh nhân, thuốc nghi ngờ, biểu hiện lâm sàng.
    *   Mức độ nghiêm trọng.
3.  **Thẩm định**: Dược lâm sàng xem xét và đánh giá mối liên quan.
4.  **Báo cáo**: Kết xuất báo cáo gửi Trung tâm DI & ADR Quốc gia.
*   **Plugin chính**: `HIS.Desktop.Plugins.HisAdr`.

### 2.3. Duyệt thuốc Kháng sinh (Antibiotic Stewardship)
Quy trình kiểm soát sử dụng kháng sinh hạn chế/ưu tiên quản lý.
*   Bác sĩ kê đơn kháng sinh thuộc danh mục hạn chế.
*   Hệ thống yêu cầu điền "Phiếu hội chẩn/duyệt kháng sinh" (`AntibioticRequest`).
*   Dược sĩ lâm sàng/Lãnh đạo khoa duyệt phiếu.
*   Kho dược mới được phép cấp phát thuốc.

## 3. Liên kết Tài liệu
*   [Quy trình Kê đơn Nội trú](../../clinical/04-daily-treatment.md)
*   [Danh mục Dược](../pharmacy/01-business-overview.md)
