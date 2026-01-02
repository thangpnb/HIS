# External Integrations (Tích hợp Hệ thống Ngoài)

## 1. Electronic Bill (`Inventec.Common.ElectronicBill`)

Module cốt lõi để tích hợp với các hệ thống Hóa đơn điện tử (HĐĐT) của các nhà cung cấp (Provider) tại Việt Nam.

### Các Provider được hỗ trợ
*   **Viettel S-Invoice**: `Inventec.Common.ElectronicBillViettel`
*   **Misa MeInvoice**: `Inventec.Common.ElectronicBill.Misa`
*   **VNPT**: (Tích hợp trong core hoặc module riêng tùy version)
*   **SoftDreams (EasyInvoice)**: `Inventec.Common.EBillSoftDreams`
*   **ThaiSon**: `Inventec.Common.EHoaDon`

### Chức năng Chính
*   **Phát hành hóa đơn**: Gửi dữ liệu hóa đơn (người mua, chi tiết dịch vụ, tiền) lên hệ thống của Provider để lấy về Mã CQT, Số hóa đơn.
*   **Ký số**: Tích hợp ký số (USB Token hoặc HSM) trước khi gửi.
*   **Tra cứu**: Lấy trạng thái hóa đơn, link tải PDF/XML.
*   **Hủy/Điều chỉnh**: Gửi yêu cầu hủy hoặc điều chỉnh thông tin hóa đơn.

---

## 2. Redis Cache (`Inventec.Common.RedisCache`)

Client kết nối tới Redis Server để lưu trữ dữ liệu tạm thời (Caching) nhằm tăng tốc độ truy xuất.

*   **Mục đích**: Giảm tải cho Database chính (Oracle/SQL) bằng cách cache các dữ liệu ít thay đổi (Danh mục dùng chung, Cấu hình hệ thống).
*   **Implementation**: Wrapper xung quanh `StackExchange.Redis`.
*   **Dữ liệu thường cache**:
    *   Danh mục Tỉnh/Huyện/Xã (`SdaProvince`...).
    *   Danh mục Dịch vụ Kỹ thuật (`HisServiceType`...).
    *   Cấu hình hệ thống (`SdaConfig`).

---

## 3. Facebook Wit.ai (`Inventec.Common.WitAI`)

Module tích hợp AI xử lý ngôn ngữ tự nhiên (NLP) của Facebook.

*   **Ứng dụng**: Nhận diện giọng nói hoặc văn bản để điều khiển phần mềm (Voice Command).
*   **Luồng xử lý**:
    1.  Ghi âm giọng nói (Audio).
    2.  Gửi Audio lên Wit.ai API.
    3.  Nhận về Intent (ý định) và Entities (thực thể).
    4.  Mapping Intent sang lệnh trên HIS (Ví dụ: "Mở bệnh án" -> Open Treatment Form).

---

## 4. WebSocket (`Inventec.Common.WSPubSub`)

Client kết nối WebSocket để nhận thông báo thời gian thực (Real-time Notification).

*   **Ứng dụng**:
    *   Thông báo có bệnh nhân mới đăng ký.
    *   Cập nhật trạng thái hàng đợi (QMS).
    *   Chat nội bộ / Gửi tin nhắn khẩn.
