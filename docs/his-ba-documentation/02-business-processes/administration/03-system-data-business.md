# Quản trị Dữ liệu Hệ thống (SDA) - Business Overview

## 1. Mục đích và Phạm vi
Phân hệ **SDA (System Data Administration)** đóng vai trò là "xương sống" dữ liệu của toàn bộ hệ thống HIS.
*   **Mục đích**: Quản lý tập trung hàng trăm danh mục dùng chung (Master Data) và các tham số cấu hình hệ thống.
*   **Phạm vi**: Cung cấp dữ liệu chuẩn cho tất cả các phân hệ khác (Tiếp đón, Lâm sàng, Dược, Viện phí...) để đảm bảo sự nhất quán.

## 2. Chiến lược Quản lý Danh mục (Master Data Strategy)
Với số lượng lớn module quản lý danh mục (hơn 100+ plugin `His*`), hệ thống không xây dựng các màn hình rời rạc mà áp dụng chiến lược quản lý tập trung:

### 2.1. Phân loại Dữ liệu
Dữ liệu hệ thống được chia thành các nhóm chính:
*   **Hành chính (Administrative)**: Đơn vị hành chính, Dân tộc, Nghề nghiệp, Quốc tịch...
*   **Chuyên môn (Clinical)**: ICD-10, ICD-9 (Phẫu thuật thủ thuật), Danh mục chỉ số xét nghiệm, Ghi chú điều dưỡng...
*   **Tài chính (Financial)**: Dải giá dịch vụ, Tỷ lệ chi trả BHYT, Nguồn ngân sách...
*   **Cấu hình (Configuration)**: Lý do hủy, Lý do nhập/xuất kho, Các tham số vận hành (System Configs).

### 2.2. Nguyên tắc Quản trị
*   **Chuẩn hóa**: Tất cả các danh mục đều tuân theo chuẩn mã hóa của Bộ Y tế và Bảo hiểm Xã hội (QĐ 4210, QĐ 130).
*   **Phân quyền**: Quyền thêm/sửa/xóa danh mục được phân cấp chặt chẽ (Ví dụ: Chỉ phòng KHTH mới sửa được giá dịch vụ).
*   **Đồng bộ**: Dữ liệu danh mục được cache tại máy trạm (Client Side Caching) để đảm bảo tốc độ truy xuất tức thời trong quá trình khám chữa bệnh.

## 3. Các Nhóm Danh mục Chính

### 3.1. Danh mục Hành chính
Quản lý phân cấp hành chính chuẩn theo Tổng cục Thống kê:
*   **Tỉnh/Thành phố (Province)** - **Quận/Huyện (District)** - **Xã/Phường (Commune)**: Hỗ trợ tự động điền và kiểm tra tính hợp lệ của địa chỉ bệnh nhân.
*   **Dân tộc & Tôn giáo**: Phục vụ báo cáo thống kê.

### 3.2. Cấu hình Hệ thống (System Configs)
Cho phép quản trị viên (Admin) can thiệp vào hành vi của phần mềm mà không cần sửa code:
*   **System Fields**: Định nghĩa trường nào bắt buộc nhập, kiểm tra định dạng dữ liệu.
*   **System Parameters**: Bật/tắt các tính năng (Ví dụ: "Cho phép kê đơn khi hết tồn kho" - Có/Không).

### 3.3. Công cụ Hỗ trợ (Admin Tools)
*   **SQL Runner**: Công cụ dành cho đội IT để thực thi truy vấn nóng (Cần cẩn trọng và phân quyền cao nhất).
*   **Log Viewer**: Truy vết lịch sử thay đổi cấu hình.

## 4. Ghi chú Rà soát Phạm vi (Scope Review Notes)
Trong quá trình vận hành và triển khai, cần lưu ý rà soát các phân hệ đặc thù sau để đảm bảo đầy đủ danh mục:

> [!WARNING]
> **Rà soát Phạm vi Ung bướu (Oncology)**
> Cần xác nhận quy trình quản lý **Hóa chất & Xạ trị**.
> *   Hiện tại hệ thống dùng chung quy trình Kê đơn/Điều trị nội trú.
> *   Cần rà soát xem có yêu cầu module riêng cho việc **Pha chế hóa chất** (Mixing) và **Phác đồ điều trị ung thư** chuyên biệt hay không.

> [!WARNING]
> **Rà soát Phạm vi Kiểm soát Nhiễm khuẩn (Infection Control)**
> Cần xác nhận quy trình **Tiệt khuẩn & Giám sát nhiễm khuẩn**.
> *   Hiện tại chưa thấy plugin chuyên biệt cho quy trình này (Quản lý đồ vải, Dụng cụ vô khuẩn, Giám sát ca nhiễm khuẩn bệnh viện).
> *   Cần đánh giá nhu cầu tích hợp hoặc bổ sung module mới.

## 5. Liên kết Tài liệu
*   [Thiết kế Kỹ thuật SDA](../../../03-technical-specs/administration/04-system-data-technical.md).
