# Quản lý Thông tin Hộ gia đình (Family Information)

## 1. Tổng quan
Module **Thông tin Hộ gia đình** (`FamilyInformation`) là một phân hệ mở rộng của Quản lý Hệ thống ID Y tế (Health ID - HID), được tích hợp chặt chẽ với quy trình tiếp đón và quản lý hồ sơ người bệnh. Module này cho phép quản lý mối quan hệ giữa các thành viên trong cùng một gia đình/hộ khẩu, hỗ trợ việc theo dõi tiền sử bệnh gia đình và quản lý biến động dân số cho Y tế cơ sở.

## 2. Truy cập & Tích hợp
Module này thường không chạy độc lập mà được gọi từ các phân hệ khác:

*   **Từ Danh sách Người bệnh (`PatientList`)**:
    *   Tại giao diện danh sách bệnh nhân, người dùng (Điều dưỡng/Tiếp đón) chọn bệnh nhân cần cập nhật.
    *   Kích hoạt chức năng "Cập nhật thông tin gia đình" (thường qua menu chuột phải hoặc nút chức năng mở rộng).
*   **Tham số đầu vào**:
    *   `ModuleData`: Thông tin cấu hình module.
    *   `PersonCode`: Mã định danh cá nhân của người bệnh đang chọn.

## 3. Chức năng Chính (Dự kiến)

Dựa trên cấu trúc plugin `HID.Desktop.Plugins.FamilyInformation`, các chức năng chính bao gồm:

### 3.1. Quản lý Thành viên Hộ gia đình
*   **Xác định Chủ hộ**: Gán người đại diện cho hộ gia đình.
*   **Thêm thành viên**: Thêm mới hoặc liên kết các mã bệnh nhân (`PatientID`) đã có trong hệ thống vào cùng một hộ.
*   **Thiết lập Mối quan hệ**:
    *   Định nghĩa quan hệ với chủ hộ (Vợ, Chồng, Con, Bố, Mẹ...).
    *   Dữ liệu này rất quan trọng để xác định người giám hộ hoặc người chịu trách nhiệm chi trả.

### 3.2. Quản lý Địa chỉ & Thông tin Hành chính
*   Đồng bộ địa chỉ thường trú/tạm trú cho tất cả thành viên trong cùng hộ.
*   Cập nhật thông tin BHYT hộ gia đình (nếu có tính năng liên thông).

### 3.3. Theo dõi Y tế Gia đình (Potential Feature)
*   Ghi nhận tiền sử bệnh di truyền.
*   Lên kế hoạch thăm hỏi sức khỏe cho cả hộ (đối với Y tế xã/phường).

## 4. Mapping Plugin Source Code

| Chức năng | Plugin / ClassName | Mô tả |
| :--- | :--- | :--- |
| **Giao diện chính** | `HID.Desktop.Plugins.FamilyInformation.frmFamilyInformation` | Form hiển thị và xử lý thông tin hộ gia đình. |
| **Xử lý Logic** | `HID.Desktop.Plugins.FamilyInformation.FamilyInformationBehavior` | Lớp xử lý nghiệp vụ, tải dữ liệu người bệnh và người thân. |
| **Tích hợp** | `HIS.Desktop.Plugins.Patient` | Module gọi (`CallModule`) đến plugin FamilyInformation từ danh sách người bệnh. |

## 5. Liên kết Tài liệu
*   [Quản lý Hồ sơ Người bệnh](./02-patient-records.md)
*   [Y tế Cơ sở & Dân số](../commune-health/01-business-overview.md)
