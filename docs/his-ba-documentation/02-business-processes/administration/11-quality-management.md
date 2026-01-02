# Quản lý Chất lượng Bệnh viện (Quality Management)

> **LƯU Ý**: Module này hiện mới chỉ đáp ứng **MỘT PHẦN** (Partial Implementation) liên quan đến kiểm chuẩn xét nghiệm. Các tính năng quản lý chất lượng tổng thể chưa hoàn thiện.

## 1. Tình trạng Hiện tại
*   **Đã có**:
    *   **Kiểm chuẩn Xét nghiệm (Laboratory QC)**: Quản lý nội kiểm, ngoại kiểm xét nghiệm. Xem chi tiết tại: [Quy trình Kiểm chuẩn Xét nghiệm](../laboratory/04-quality-control.md).
    *   **Báo cáo Sự cố Y khoa (Incident Reporting)**: Ghi nhận sự cố rủi ro. Xem chi tiết tại: [Quy trình Báo cáo Sự cố](../administration/08-incident-reporting.md).

*   **Chưa có** (Missing):
    *   Quản lý 83 tiêu chí chất lượng bệnh viện (theo QĐ 6858/QĐ-BYT).
    *   Khảo sát hài lòng người bệnh/nhân viên y tế (đang thực hiện lẻ tẻ, chưa có module tập trung).
    *   Cải tiến chất lượng (PDCA).

## 2. Mô tả Chức năng Còn thiếu
Hệ thống QLCL tổng thể cần có:
1.  **Quản lý Tiêu chí Chất lượng**:
    *   Cho phép tự đánh giá theo bộ 83 tiêu chí.
    *   Lưu trữ minh chứng (văn bản, hình ảnh).
2.  **Khảo sát Hài lòng**:
    *   Tạo mẫu phiếu khảo sát điện tử (trên Kiosk hoặc App).
    *   Thống kê và phân tích kết quả hài lòng.
3.  **Hồ sơ Cải tiến**:
    *   Theo dõi các đề án cải tiến chất lượng tại các khoa phòng.

## 3. Khuyến nghị
*   Đây là phân hệ quản trị cấp cao. Trong giai đoạn đầu có thể sử dụng các công cụ văn phòng hoặc phần mềm rời để quản lý, sau đó tích hợp dần vào HIS để lấy số liệu tự động cho các tiêu chí (VD: Thời gian chờ khám, Tỷ lệ sử dụng thuốc...).
