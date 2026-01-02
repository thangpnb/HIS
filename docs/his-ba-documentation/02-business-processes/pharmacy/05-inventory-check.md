# Quy trình Kiểm kê & Chốt Kho (Inventory Check & Closing)

## 1. Tổng quan
Quy trình kiểm kê nhằm xác thực số lượng tồn kho thực tế so với số liệu trên phần mềm, phát hiện chênh lệch và thực hiện các bút toán điều chỉnh. Quy trình này thường đi kèm với việc chốt kỳ (khóa số liệu) để đảm bảo tính toàn vẹn dữ liệu cho kỳ kế toán.

## 2. Lưu đồ Quy trình

```mermaid
graph TB
    Start((Bắt đầu)) --> ChonKy[1. Chọn Kỳ Kiểm kê]
    ChonKy --> InPhieu[2. In Phiếu Kiểm kê]
    InPhieu --> DemSoLuong[3. Kiểm điếm Thực tế]
    DemSoLuong --> NhapLieu[4. Nhập Số lượng Thực tế (MediStockInventory)]
    NhapLieu --> XuLy[5. Xử lý Chênh lệch]
    
    XuLy -- Thừa --> NhapThua[Tạo Phiếu Nhập thừa]
    XuLy -- Thiếu --> XuatThieu[Tạo Phiếu Xuất thiếu]
    
    NhapThua & XuatThieu --> ChotKy[6. Chốt Kỳ/Khóa sổ (MediStockPeriod)]
    ChotKy --> End((Kết thúc))
```

## 3. Chi tiết Các bước & Mapping Plugin

### 3.1. Nhập liệu Kiểm kê (Inventory Count)
Sau khi hội đồng kiểm kê hoàn tất việc đếm số lượng thuốc/vật tư tại kho, thủ kho nhập số liệu vào hệ thống.
*   **Plugin chính**:
    *   `HIS.Desktop.Plugins.MediStockInventory`: Giao diện nhập liệu kiểm kê.
        *   *Chức năng*: Load danh sách thuốc tồn trên phần mềm (Lý thuyết), cho phép nhập số lượng thực tế. Tự động tính toán chênh lệch.
        *   *Tính năng phụ*: Import kết quả kiểm kê từ Excel.

### 3.2. Xử lý Chênh lệch (Variance Handling)
Hệ thống hỗ trợ tạo các phiếu điều chỉnh để cân bằng kho.
*   **Xử lý**:
    *   Nếu Thực tế > Lý thuyết: Hệ thống gợi ý tạo **Phiếu Nhập Kiểm kê** (Nhập thừa).
    *   Nếu Thực tế < Lý thuyết: Hệ thống gợi ý tạo **Phiếu Xuất Kiểm kê** (Xuất thiếu/Xuất hao hụt).

### 3.3. Quản lý Kỳ & Khóa sổ (Period Management)
Để đảm bảo dữ liệu không bị thay đổi sau khi đã báo cáo, kho cần thực hiện thao tác khóa kỳ.
*   **Plugin chính**:
    *   `HIS.Desktop.Plugins.MediStockPeriod`: Quản lý danh sách các kỳ kiểm kê.
        *   *Chức năng*: Tạo kỳ mới (Tháng, Quý, Năm), Thực hiện Khóa kỳ (Lock) - ngăn chặn mọi thao tác nhập/xuất thuộc kỳ đã khóa.
    *   `HIS.Desktop.Plugins.MediStockSummary`: Xem báo cáo tổng hợp tồn kho, thẻ kho của từng kỳ.

## 4. Các Loại Báo cáo Kiểm kê
1.  **Biên bản Kiểm kê**: Danh sách chi tiết số lượng Lý thuyết vs Thực tế.
2.  **Báo cáo Cân đối Nhập Xuất Tồn**: Tổng hợp đầu kỳ - nhập - xuất - cuối kỳ.
3.  **Thẻ kho (Stock Card)**: Chi tiết biến động của một mã thuốc cụ thể.

## 5. Liên kết Tài liệu
*   [Quy trình Nhập kho](./03-inventory-import.md).
*   [Quy trình Xuất kho](./04-inventory-export.md).
