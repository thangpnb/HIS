# Danh sách Kiểm thử Chấp nhận (UAT Checklist)

**Module**: Tiếp đón Bệnh nhân (Patient Registration)
**Phiên bản**: 1.0
**Môi trường**: Staging

| ID | Test Case (Trường hợp kiểm thử) | Các bước thực hiện | Kết quả Mong đợi | Trạng thái (P/F) | Ghi chú |
|:---|:---|:---|:---|:---|:---|
| **TC01** | **Tiếp đón bệnh nhân mới (BHYT)** | | | | |
| 1.1 | Check thẻ BHYT hợp lệ | 1. Nhập mã thẻ BHYT đúng tuyến.<br>2. Nhấn "Check thẻ". | Hệ thống tự động điền Họ tên, Ngày sinh, Giới tính từ cổng BHYT. | | |
| 1.2 | Tính toán quyền lợi | 1. Chọn mức hưởng.<br>2. Chọn đối tượng ưu tiên. | Đúng tỷ lệ % (80%, 95%, 100%). | | |
| 1.3 | Lưu hồ sơ | 1. Nhập đủ thông tin bắt buộc.<br>2. Nhấn Lưu. | Thông báo "Thêm mới thành công". Mã BN được sinh tự động. | | |
| **TC02** | **Tiếp đón bệnh nhân Dịch vụ** | | | | |
| 2.1 | Tiếp đón không thẻ | 1. Bỏ trống mã thẻ.<br>2. Nhập tay hành chính. | Cho phép lưu. Đối tượng tự động là "Viện phí". | | |
| 2.2 | In phiếu khám | 1. Lưu xong.<br>2. Nhấn "In phiếu". | Phiếu in ra đúng mẫu khổ A5. Hiển thị đúng số thứ tự. | | |
| **TC03** | **Kiểm tra Ràng buộc (Validation)** | | | | |
| 3.1 | Bỏ trống Tên | 1. Để trống ô Họ tên.<br>2. Nhấn Lưu. | Báo lỗi "Vui lòng nhập họ tên". | | |
| 3.2 | Sai định dạng ngày sinh | 1. Nhập ngày sinh tương lai.<br>2. Nhấn Lưu. | Báo lỗi "Ngày sinh không hợp lệ". | | |
| **TC04** | **Tìm kiếm & Sửa đổi** | | | | |
| 4.1 | Tìm lại BN cũ | 1. Nhập mã BN vừa tạo.<br>2. Nhấn Tìm. | Hiển thị lại đầy đủ thông tin cũ. | | |
| 4.2 | Sửa thông tin hành chính | 1. Sửa địa chỉ.<br>2. Nhấn Lưu. | Dữ liệu được cập nhật. Log lịch sử ghi nhận người sửa. | | |

**Kết luận UAT**:
- [ ] Chấp nhận (Pass)
- [ ] Chấp nhận có điều kiện (Pass with minor bugs)
- [ ] Từ chối (Fail)

**Người kiểm duyệt**: ____________________ **Ngày**: ____________________
