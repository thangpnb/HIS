# Quy trình Xuất kho Dược & Vật tư (Inventory Export)

## 1. Tổng quan
Quy trình xuất kho quản lý việc phân phối thuốc và vật tư y tế từ kho tổng/kho dược đến các kho khoa phòng (xuất nội bộ), xuất bán lẻ cho bệnh nhân ngoại trú, hoặc các hình thức xuất khác (trả lại, thanh lý).

## 2. Lưu đồ Quy trình

```mermaid
graph TB
    Start((Bắt đầu)) --> NhuCau[1. Phát sinh Nhu cầu]
    
    subgraph "Hình thức Xuất"
        NhuCau -- Khoa phòng lĩnh --> PhiduTru[2a. Lập Phiếu Dự trù/Lĩnh]
        NhuCau -- Bán lẻ/Đơn thuốc --> DonThuoc[2b. Đơn thuốc/Bán lẻ]
        NhuCau -- Trả/Thanh lý --> DeNghi[2c. Đề nghị Xuất khác]
    end
    
    PhiduTru --> DuyetLinh[3a. Duyệt cấp phát (ExpMestDepa)]
    DonThuoc --> XuatBan[3b. Xuất bán (ExpMestSale)]
    DeNghi --> XuatKhac[3c. Xuất khác (ExpMestOther)]
    
    DuyetLinh & XuatBan & XuatKhac --> TruKho[4. Trừ tồn kho & Thẻ kho]
    TruKho --> End((Kết thúc))
```

## 3. Chi tiết Các bước & Mapping Plugin

### 3.1. Xuất Nội bộ (Internal Export)
Thực hiện xuất kho cho các khoa lâm sàng/cận lâm sàng theo phiếu lĩnh định kỳ hoặc đột xuất.
*   **Plugin chính**:
    *   `HIS.Desktop.Plugins.ExpMestDepaCreate`: Lập và duyệt phiếu xuất kho nội bộ.
        *   *Chức năng*: Duyệt các phiếu dự trù từ khoa phòng gửi lên, điều chỉnh số lượng thực xuất nếu cần.
    *   `HIS.Desktop.Plugins.ExpMestAggrExam`: Tổng hợp xuất cho bệnh nhân khám bệnh (nếu có cơ chế cấp phát tập trung).

### 3.2. Xuất Bán / Xuất Ngoại trú (Sale Export)
Xuất thuốc cho bệnh nhân ngoại trú (có BHYT hoặc dịch vụ) tại các quầy thuốc/kho BHYT.
*   **Plugin chính**:
    *   `HIS.Desktop.Plugins.ExpMestSaleCreate`: Màn hình bán thuốc/cấp thuốc BHYT.
        *   *Chức năng*: Load đơn thuốc đã kê, chọn lô/hạn dùng (hoặc hệ thống tự chọn FIFO), in phiếu và trừ kho.
    *   `HIS.Desktop.Plugins.ExpMestSaleCreateV2`: Phiên bản nâng cấp (tối ưu hiệu năng cho bệnh viện lớn).

### 3.3. Xuất Khác (Other Export)
Các nghiệp vụ xuất đặc thù như trả lại nhà cung cấp, xuất kiểm nghiệm, xuất thanh lý/hủy.
*   **Plugin chính**:
    *   `HIS.Desktop.Plugins.ExpMestOtherExport`: Quản lý các phiếu xuất khác.
        *   *Chức năng*: Chọn loại lý do xuất (Trả NCC, Hủy, Thanh lý...), chọn lô thuốc cụ thể để xuất.

## 4. Nguyên tắc Xuất kho
1.  **FEFO/FIFO**: Ưu tiên xuất thuốc hết hạn trước (First Expired First Out) hoặc nhập trước xuất trước (FIFO). Hệ thống tự động gợi ý lô.
2.  **Kiểm soát tồn kho**: Không cho phép xuất quá số lượng tồn khả dụng (Available Stock).
3.  **Duyệt cấp**: Phiếu xuất nội bộ phải qua bước duyệt (Approve) mới chính thức trừ kho.

## 5. Dữ liệu Đầu ra
*   **Phiếu Xuất kho**: Mẫu C31-HD hoặc phiếu xuất kho kiêm vận chuyển nội bộ.
*   **Biên bản giao nhận**: Ký nhận giữa thủ kho và điều dưỡng/nhà cung cấp.
*   **Bảng kê chi tiết**: Kèm theo hóa đơn tài chính (nếu xuất bán).

## 6. Liên kết Tài liệu
*   [Quy trình Nhập kho](./03-inventory-import.md).
*   [Quy trình Kiểm kê](./05-inventory-check.md).
