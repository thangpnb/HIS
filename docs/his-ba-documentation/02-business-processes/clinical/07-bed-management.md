# Quản lý Buồng/Giường (Bed Management)

## 1. Tổng quan
Quy trình Quản lý Buồng/Giường cung cấp công cụ trực quan để điều phối vị trí nằm của người bệnh, theo dõi công suất sử dụng giường và xử lý các nghiệp vụ chuyển khoa/chuyển giường.

Mục tiêu:
*   Đảm bảo mỗi bệnh nhân nội trú đều được gán đúng giường thực tế.
*   Tối ưu hóa công suất sử dụng giường (Giường đơn/Giường ghép).
*   Tính toán chính xác tiền giường (liên kết với Bảo hiểm Y tế).

## 2. Lưu đồ Quy trình

```mermaid
graph TB
    Start((Bắt đầu)) --> TiepNhan[1. Tiếp nhận vào Khoa]
    TiepNhan --> GanGiuong[2. Gán Giường (Bed Assign)]
    
    GanGiuong --> TheoDoi[3. Theo dõi Buồng bệnh]
    
    TheoDoi -- Chuyển giường --> DoiGiuong[3a. Đổi giường/Buồng]
    TheoDoi -- Ra viện --> TraGiuong[3b. Trả giường]
    
    TraGiuong --> End((Kết thúc))
```

## 3. Các Chức năng & Plugin

### 3.1. Bản đồ Giường bệnh (`BedMapView`)
Giao diện trực quan hiển thị sơ đồ buồng/giường của khoa.
*   **Plugin**: `HIS.Desktop.Plugins.BedMapView`.
*   **Chức năng**:
    *   **Danh sách Buồng**: Bên trái màn hình. Màu sắc thể hiện trạng thái:
        *   <span style="color:green">**Màu Xanh**</span>: Buồng còn trống giường (`PATIENT_COUNT < BED_COUNT`).
        *   <span style="color:red">**Màu Đỏ**</span>: Buồng đã đầy (`PATIENT_COUNT >= BED_COUNT`).
    *   **Sơ đồ Giường**: Bên phải, hiển thị các giường trong buồng được chọn.
        *   **Icon Giường**: Hiển thị tên giường.
        *   **Trạng thái**: Hiển thị tên bệnh nhân đang nằm.
        *   **Loại nằm**: Phân biệt "Nằm đơn" (1 BN/giường) hoặc "Nằm ghép" (>= 2 BN/giường).

### 3.2. Gán Giường & Dịch vụ (`BedAssign`)
Chức năng gán một bệnh nhân vào một giường cụ thể và chỉ định loại dịch vụ giường tương ứng (để tính tiền).
*   **Plugin**: `HIS.Desktop.Plugins.BedAssign`.
*   **Logic xử lý**:
    1.  **Chọn Giường (`cboBed`)**: Hệ thống tải danh sách giường khả dụng trong buồng.
        *   *Cảnh báo*: Nếu chọn giường đã có người nằm (IsKey = 1), hệ thống cảnh báo "Giường đã có bệnh nhân nằm".
        *   *Chặn*: Nếu số lượng nằm ghép vượt quá giới hạn (`MAX_CAPACITY`), hệ thống báo "Giường vượt quá số lượng".
    2.  **Chọn Loại Dịch vụ Giường (`cboBedServiceType`)**:
        *   Hệ thống tự động lọc các **Dịch vụ Giường (Ngày giường)** được cấu hình cho buồng đó.
        *   **Gợi ý thông minh**: Nếu bệnh nhân vừa phẫu thuật trong vòng 10 ngày, hệ thống tự động gợi ý các loại giường Hậu phẫu phù hợp với nhóm phẫu thuật.
    3.  **Thời gian**:
        *   `Từ ngày`: Mặc định là thời điểm hiện tại hoặc thời điểm vào khoa.
        *   `Đến ngày`: Mở (để trống) cho đến khi bệnh nhân chuyển đi hoặc ra viện.

### 3.3. Chuyển Khoa & Trả Giường
Khi bệnh nhân làm thủ tục chuyển khoa hoặc ra viện, hệ thống tự động:
*   Kết thúc thời gian nằm tại giường cũ (`FinishTime`).
*   Tính toán tổng số ngày giường để đẩy sang chi phí điều trị.

## 4. Cấu hình Liên quan
*   **Danh mục Buồng (`HIS_BED_ROOM`)**: Định nghĩa tên buồng, thuộc khoa nào.
*   **Danh mục Giường (`HIS_BED`)**: Định nghĩa tên giường, sức chứa tối đa (`MAX_CAPACITY`), và tọa độ hiển thị trên bản đồ (`X`, `Y`).
*   **Cấu hình Giá**: Giá dịch vụ ngày giường (BHYT/Viện phí) được map vào từng loại giường.

## 5. Dữ liệu Đầu ra
*   **Lịch sử Giường (`HIS_BED_LOG`)**: Lưu vết quá trình nằm giường của bệnh nhân (Từ giờ... Đến giờ..., Tại giường nào).
*   **Chi phí Giường**: Dòng chi phí trong bảng kê viện phí.
