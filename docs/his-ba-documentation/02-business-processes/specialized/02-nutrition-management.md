# Quy trình Quản lý Dinh dưỡng (Nutrition Management)

## 1. Tổng quan
Quy trình mô tả việc chỉ định chế độ ăn bệnh lý cho bệnh nhân nội trú, báo suất ăn xuống nhà bếp và hạch toán chi phí suất ăn.

## 2. Lưu đồ Quy trình

```mermaid
graph TB
    Start((Bắt đầu)) --> ChiDinh[1. Chỉ định Chế độ ăn (Bác sĩ)]
    ChiDinh --> BaoAn[2. Báo ăn (Điều dưỡng)]
    
    BaoAn --> TongHop["3. Tổng hợp Suất ăn (Khoa Dinh dưỡng)"]
    TongHop --> CheBien["4. Chế biến & Chia suất (Nhà bếp)"]
    CheBien --> GiaoNhan["5. Giao nhận Suất ăn (Tại Khoa)"]
    
    GiaoNhan --> ThanhToan["6. Tổng hợp Chi phí & Thanh toán"]
    ThanhToan --> End((Kết thúc))
```

## 3. Chi tiết Các bước & Mapping Plugin

### 3.1. Chỉ định Chế độ ăn (Diet Prescription)
Bác sĩ chỉ định mã chế độ ăn phù hợp với tình trạng bệnh lý (VD: DD01 - Cơm thường, DD04 - Cháo tiểu đường).
*   **Plugin chính**:
    *   `SCN.Desktop.Plugins.ScnNutrition`: Màn hình quản lý thông tin dinh dưỡng bệnh nhân.
    *   `HIS.Desktop.Plugins.AssignNutrition`: Giao diện chỉ định suất ăn hàng ngày (Sáng/Trưa/Chiều/Tối).

### 3.2. Báo ăn (Meal Ordering)
Điều dưỡng tổng hợp số lượng báo ăn của khoa và gửi yêu cầu xuống khoa Dinh dưỡng/Nhà bếp.
*   **Plugin chính**:
    *   `HIS.Desktop.Plugins.MealRationDetail`: Chi tiết khẩu phần ăn.
    *   `HIS.Desktop.Plugins.HisRationSumList`: Tổng hợp suất ăn toàn khoa.

### 3.3. Chế biến và Cung cấp (Processing & Delivery)
Khoa Dinh dưỡng tiếp nhận yêu cầu, lên thực đơn và chế biến.
*   **Plugin chính**:
    *   `HIS.Desktop.Plugins.AssignNutritionRefectory`: Quản lý tại nhà ăn/bếp (Refectory).
    *   `HIS.Desktop.Plugins.HisInventoryContent`: Quản lý kho thực phẩm (nhập xuất nguyên liệu).

### 3.4. Thanh toán (Payment)
Chi phí suất ăn được tự động tính vào viện phí của bệnh nhân hoặc thanh toán trực tiếp.
*   **Plugin chính**:
    *   `HIS.Desktop.Plugins.TransactionBill`: Hệ thống tự động đẩy phí suất ăn sang hóa đơn viện phí.

## 4. Các Loại Chế độ ăn
*   **Chế độ ăn thường**: Cơm, Cháo, Phở.
*   **Chế độ ăn bệnh lý**: Tiểu đường, Suy thận, Hậu phẫu.
*   **Sữa/Nuôi ăn qua sonde**: Sữa cao năng lượng.

## 5. Dữ liệu Đầu ra
*   **Phiếu Báo ăn**: Tổng hợp số lượng suất ăn theo loại.
*   **Phiếu Công khai**: Ghi nhận chi phí ăn uống của BN.

## 6. Liên kết Tài liệu
*   [Quy trình Điều trị Nội trú](../clinical/04-daily-treatment.md).
