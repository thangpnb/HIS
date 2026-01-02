# Quy trình Nhập kho Dược & Vật tư (Inventory Import)

## 1. Tổng quan
Quy trình nhập kho quản lý toàn bộ các hoạt động đưa thuốc và vật tư y tế vào hệ thống kho của bệnh viện, bao gồm nhập mua từ nhà cung cấp (theo thầu/ngoài thầu), nhập tặng biếu, và nhập điều chuyển từ cơ sở khác.

## 2. Lưu đồ Quy trình

```mermaid
graph TB
    Start((Bắt đầu)) --> NhanHang[1. Tiếp nhận Hàng hóa]
    NhanHang --> KiemNhap[2. Kiểm nhập & Tạo phiếu]
    
    subgraph "Hệ thống Phần mềm"
        KiemNhap -- Nhập Mua --> TaoPhieu["3a. Lập Phiếu Nhập (ImpMestCreate)"]
        KiemNhap -- Nhập Điều chuyển --> NhapChms["3b. Lập Phiếu Điều chuyển (ImpMestChms)"]
    end
    
    TaoPhieu & NhapChms --> DuyetKho[4. Duyệt Nhập kho]
    DuyetKho --> KeToan[5. Xử lý Thanh toán (ImpMestPay)]
    KeToan --> End((Kết thúc))
```

## 3. Chi tiết Các bước & Mapping Plugin

### 3.1. Lập Phiếu Nhập Mua (Purchase Import)
Thủ kho hoặc Dược sĩ thực hiện nhập kho dựa trên hóa đơn VAT của nhà cung cấp. Hệ thống hỗ trợ liên kết chặt chẽ với Quy trình Đấu thầu để kiểm soát giá và số lượng.
*   **Plugin chính**:
    *   `HIS.Desktop.Plugins.ImpMestCreate`: Giao diện chính để tạo và quản lý phiếu nhập.
        *   *Chức năng*: Nhập thông tin hóa đơn, chọn nhà cung cấp, nhập chi tiết thuốc (Lô, Hạn dùng, Giá).
        *   *Kiểm soát*: Tự động hiển thị và trừ lùi số lượng thầu còn lại (`ImpMestCreate__Plus__GoiThau`).
    *   `HIS.Desktop.Plugins.ImpMestTypeUser`: Cấu hình các loại phiếu nhập (Nhập mua, Nhập nội bộ...).

### 3.2. Nhập Điều chuyển (Transfer Import)
Sử dụng khi nhận thuốc/vật tư điều chuyển từ kho khác hoặc đơn vị khác (liên quan đến nghiệp vụ Chuyển Mua Sắm hoặc Điều chuyển đặc thù).
*   **Plugin chính**:
    *   `HIS.Desktop.Plugins.ImpMestChmsCreate`: Giao diện lập phiếu nhập điều chuyển (Chuyển Mua Sắm).

### 3.3. Xử lý Thanh toán (Payment Processing)
Sau khi phiếu nhập được duyệt, bộ phận kế toán dược hoặc tài chính thực hiện rà soát công nợ và làm thủ tục thanh toán.
*   **Plugin chính**:
    *   `HIS.Desktop.Plugins.ImpMestPay`: Quản lý thanh toán hóa đơn nhập kho.
        *   *Chức năng*: Chọn phiếu nhập, xác nhận chi tiền, theo dõi công nợ nhà cung cấp.

## 4. Các Loại Nhập kho Chính
*   **Nhập theo thầu**: Bắt buộc chọn Gói thầu hợp lệ.
*   **Nhập mua sao y/trực tiếp**: Dùng cho các mặt hàng thuốc mua ngoài (cần phê duyệt đặc biệt).
*   **Nhập tặng/biếu/viện trợ**: Giá vốn có thể bằng 0 hoặc theo định giá.
*   **Nhập trả lại**: Nhận lại hàng đã xuất cho khoa phòng hoặc bệnh nhân (thu hồi).

## 5. Dữ liệu Đầu ra
*   **Phiếu Nhập kho (Mẫu C30-HD)**: Chứng từ nhập kho chính thức.
*   **Thẻ kho**: Cập nhật tức thì số lượng tồn đầu/nhập/xuất/tồn cuối.
*   **Báo cáo Nhập xuất tồn**: Dữ liệu tổng hợp kỳ.

## 6. Liên kết Tài liệu
*   [Quy trình Dự trù & Đấu thầu](./02-anticipation-bidding.md) (Bước tiền đề).
*   [Quy trình Xuất kho](./04-inventory-export.md).
