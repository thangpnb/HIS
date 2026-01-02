# Quy trình Lưu trữ & Quản lý Hồ sơ Bệnh án (Medical Record Storage)

## 1. Tổng quan
Quy trình quản lý vòng đời của hồ sơ bệnh án giấy sau khi kết thúc điều trị, từ khâu nhận bàn giao, lưu kho, cho đến việc quản lý mượn/trả phục vụ nghiên cứu, kiểm tra.

## 2. Lưu đồ Quy trình

```mermaid
graph TB
    Start((Bắt đầu)) --> TiepNhan[1. Nhận bàn giao HS (Từ khoa LS)]
    TiepNhan --> KiemTra[2. Kiểm tra Danh mục HS]
    KiemTra --> MaHoa[3. Mã hóa & Lưu kho]
    
    subgraph "Mượn Hồ sơ"
        MaHoa -.-> YeuCau[4a. Yêu cầu Mượn]
        YeuCau --> Duyet[4b. Duyệt Mượn]
        Duyet --> XuatKho[4c. Xuất Kho cho Mượn]
        XuatKho --> TraKho[4d. Trả Kho]
    end
    
    TraKho --> End((Kết thúc))
```

## 3. Chi tiết Các bước & Mapping Plugin

### 3.1. Nhập kho Lưu trữ
Phòng Kế hoạch Tổng hợp tiếp nhận hồ sơ từ các khoa lâm sàng.
*   **Plugin Chính**: `HIS.Desktop.Plugins.MedicalStore` (Quản lý kho hồ sơ): Nhập thông tin hồ sơ vào kho lưu trữ.
        *   Ghi nhận: Số lưu trữ, Vị trí (Kệ/Tủ), Tình trạng hồ sơ.

### 3.2. Quản lý Mượn Trả (Borrowing Management)
Kiểm soát việc khai thác hồ sơ bệnh án, đảm bảo không thất lạc.
*   **Plugin chính**:
    *   `HIS.Desktop.Plugins.HisMediRecordBorrow`: Quản lý phiếu mượn trả hồ sơ.
        *   *Chức năng*:
            *   Tạo phiếu mượn: Ghi rõ người mượn, mục đích, hạn trả.
            *   Ghi nhận trả: Kiểm tra tình trạng hồ sơ khi nhận lại.
            *   Cảnh báo quá hạn: Nhắc nhở các hồ sơ chưa trả.

### 3.3. Tiêu hủy Hồ sơ
Xử lý các hồ sơ hết thời hạn lưu trữ theo quy chế.
*   **Quy định**:
    *   Hồ sơ bệnh án nội trú, ngoại trú: Lưu trữ ít nhất 10 năm.
    *   Hồ sơ tai nạn lao động, tử vong: Lưu trữ ít nhất 15 năm.
    *   Hồ sơ tâm thần, tử vong đặc biệt: Lưu trữ ít nhất 20 năm.

## 4. Dữ liệu Đầu ra
*   **Sổ Lưu trữ Hồ sơ**: Danh sách toàn bộ hồ sơ trong kho.
*   **Sổ Mượn trả**: Theo dõi lịch sử khai thác.

## 5. Liên kết Tài liệu
*   [Quy trình Xuất dữ liệu BHYT](../finance/04-xml-data-export.md).
