# Quản lý Báo cáo Sự cố Y khoa (Medical Incident Reporting)

## 1. Tổng quan
Quy trình báo cáo sự cố y khoa (Medical Incident) là một phần quan trọng của quản lý chất lượng bệnh viện (theo Thông tư 43/2018/TT-BYT). Trong hệ thống HIS, quy trình này được tích hợp vào phân hệ Báo cáo dùng chung (SAR), cho phép nhân viên y tế báo cáo các sự cố (nhầm lẫn thuốc, té ngã, lỗi thiết bị...) để cấp quản lý theo dõi và xử lý.

## 2. Lưu đồ Quy trình

```mermaid
graph TB
    Start((Bắt đầu)) --> PhatHien[1. Phát hiện Sự cố]
    PhatHien --> LapBaoCao[2. Lập Báo cáo (Tại Khoa)]
    
    LapBaoCao --> GuiBaoCao[3. Gửi Phòng QLCL]
    
    subgraph "Phòng Quản lý Chất lượng"
        GuiBaoCao --> TiepNhan[4. Tiếp nhận & Phân loại]
        TiepNhan --> PhanTich[5. Phân tích Nguyên nhân (RCA)]
        PhanTich --> KetLuan[6. Kết luận & Khuyến nghị]
    end
    
    KetLuan --> End((Kết thúc))
```

## 3. Cấu hình & Phân quyền (`SarUserReportTypeList`)

Vì báo cáo sự cố không phải là một module cứng (hard-coded) mà là một loại báo cáo động, việc phân quyền truy cập được thực hiện thông qua module quản lý loại báo cáo người dùng.

*   **Plugin**: `HIS.Desktop.Plugins.SarUserReportTypeList`.
*   **Chức năng**:
    *   **Danh sách Báo cáo (`ListReportType`)**: Nơi định nghĩa báo cáo "Báo cáo sự cố y khoa".
    *   **Danh sách Người dùng (`ListUser`)**: Danh sách nhân viên toàn viện.
    *   **Phân quyền (Mapping)**: Quản trị viên tích chọn để gán quyền báo cáo sự cố cho từng nhân viên hoặc nhóm người dùng (ví dụ: Điều dưỡng trưởng, Bác sĩ trưởng khoa).

## 4. Thực hiện Báo cáo

Nhân viên y tế thực hiện báo cáo thông qua giao diện Báo cáo tổng hợp (`SarReport`).

### 4.1. Nhập liệu
*   **Mẫu báo cáo**: Chọn mẫu "Phiếu Báo cáo sự cố y khoa (Tự nguyện/Bắt buộc)".
*   **Thông tin chính**:
    *   Thời gian, địa điểm xảy ra.
    *   Mô tả diễn biến sự cố.
    *   Phân loại sự cố (NC0, NC1, NC2, NC3...).
    *   Hậu quả đối với người bệnh.

### 4.2. Trích xuất & Thống kê
Phòng Quản lý Chất lượng sử dụng cùng hệ thống để trích xuất dữ liệu:
*   Tổng hợp số lượng sự cố theo tháng/quý.
*   Phân tích theo khoa phòng hoặc loại sự cố.

## 5. Dữ liệu Đầu ra
*   **Phiếu Báo cáo sự cố**: In ra giấy để lưu hồ sơ hoặc ký trình.
*   **Sổ tay Sự cố**: Dữ liệu điện tử lưu trên hệ thống SAR phục vụ thanh tra, kiểm tra.
