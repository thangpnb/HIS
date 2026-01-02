# Ngữ cảnh Hệ thống (System Context)

## 1. Mục đích và Phạm vi

Tài liệu này định nghĩa phạm vi hoạt động của **Hệ thống Thông tin Bệnh viện (HIS) HisNguonMo**. Đây không chỉ là một phần mềm quản lý đơn thuần mà là một hệ sinh thái toàn diện phục vụ cho việc vận hành bệnh viện, từ quản lý hành chính, khám chữa bệnh đến các nghiệp vụ chuyên sâu như xét nghiệm và in ấn y tế.

Hệ thống được thiết kế để xử lý quy mô lớn với:
- **956 Plugin**: Các mô-đun chức năng nghiệp vụ riêng biệt.
- **790+ Print Processor**: Hệ thống in ấn chuyên dụng.
- **131 User Control**: Các thành phần giao diện tái sử dụng (trong thư viện Core).

## 2. Sơ đồ Ngữ cảnh (System Context Diagram)

Sơ đồ dưới đây mô tả vị trí của HIS trong bức tranh tổng thể, tương tác với các đối tượng người dùng và hệ thống bên ngoài.

```mermaid
C4Context
    title Biểu đồ Ngữ cảnh Hệ thống HIS (Software System Context)

    Person(patient, "Bệnh nhân", "Người đến khám, chữa bệnh tại bệnh viện")
    Person(staff, "Nhân viên Y tế", "Bác sĩ, Y tá, Dược sĩ, Nhân viên thu ngân sử dụng hệ thống")
    Person(admin, "Quản trị viên", "Người cấu hình và bảo trì hệ thống")

    System_Boundary(his_boundary, "Hệ thống HIS HisNguonMo") {
        System(his_app, "Ứng dụng HIS Desktop", "Cung cấp giao diện chính cho mọi hoạt động nghiệp vụ")
        System(mps_app, "Hệ thống In ấn (MPS)", "Xử lý việc tạo và in các biểu mẫu y tế phức tạp")
    }

    System_Ext(social_insurance, "Cổng Bảo hiểm Xã hội", "Gửi/Nhận dữ liệu giám định BHYT")
    System_Ext(payment_gateway, "Cổng Thanh toán", "Xử lý thanh toán viện phí (QR Code, Thẻ)")
    System_Ext(lis_device, "Máy Xét nghiệm (LIS)", "Gửi kết quả xét nghiệm tự động vào HIS")
    System_Ext(pacs, "Hệ thống PACS", "Lưu trữ và truyền hình ảnh y tế (X-Quang, MRI)")

    Rel(patient, staff, "Tương tác trực tiếp")
    Rel(staff, his_app, "Sử dụng để khám bệnh, kê đơn, viện phí")
    Rel(admin, his_app, "Quản lý người dùng, phân quyền")
    
    Rel(his_app, social_insurance, "Đẩy dữ liệu giám định (XML 4210)")
    Rel(his_app, payment_gateway, "Yêu cầu thanh toán")
    Rel(his_app, mps_app, "Gửi yêu cầu in ấn")
    Rel(his_app, pacs, "Truy xuất hình ảnh (DicomViewer)")
    
    Rel(lis_device, his_app, "Trả kết quả xét nghiệm")
```

## 3. Các Bên liên quan chính (Stakeholders)

| Hạng mục | Đối tượng | Vai trò trong hệ thống |
|----------|-----------|------------------------|
| **Người dùng cuối** | Bác sĩ, Y tá | Sử dụng các plugin nghiệp vụ (Khám bệnh, Đơn thuốc, Bệnh án điện tử) để thực hiện công việc hàng ngày. |
| | Thu ngân, Kế toán | Sử dụng các plugin Thanh toán, Viện phí để xử lý tài chính. |
| | Dược sĩ | Sử dụng các plugin Kho dược để quản lý nhập xuất thuốc. |
| **Hệ thống vệ tinh** | LIS, PACS | Các hệ thống chuyên môn kết nối chặt chẽ với HIS để trao đổi dữ liệu cận lâm sàng. |
| **Cơ quan quản lý** | BHXH, Bộ Y tế | Nhận báo cáo và dữ liệu giám định từ hệ thống HIS. |
