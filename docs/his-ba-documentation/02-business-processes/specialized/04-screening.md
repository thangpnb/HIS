# Quản lý Sàng lọc & Báo cáo Chuyên sâu (Screening - SCN)

## 1. Tổng quan
Phân hệ Sàng lọc (SCN - Screening) tập trung vào việc thu thập dữ liệu chuyên sâu cho các đối tượng đặc thù, phục vụ các chương trình quản lý sức khỏe cộng đồng và báo cáo thống kê bệnh tật.

## 2. Các Module Sàng lọc

### 2.1. Sàng lọc Khuyết tật (`ScnDisability`)
Quản lý hồ sơ người khuyết tật trên địa bàn và tại viện.
*   **Đánh giá mức độ**: Phân loại mức độ khuyết tật (Nhẹ, Nặng, Đặc biệt nặng).
*   **Dạng khuyết tật**: Vận động, Nghe nói, Nhìn, Thần kinh tâm thần, Trí tuệ.
*   **Nhu cầu hỗ trợ**: Xác định nhu cầu phục hồi chức năng và trợ giúp xã hội.

### 2.2. Yếu tố Nguy cơ Sức khỏe (`ScnHealthRisk`)
Sàng lọc các yếu tố nguy cơ của các bệnh không lây nhiễm (NCDs) tại cộng đồng.
*   **Hút thuốc lá/Rượu bia**: Mức độ sử dụng.
*   **Vận động thể lực**: Thói quen vận động.
*   **Dinh dưỡng**: Thói quen ăn uống (Ăn mặn, Ăn ít rau).
*   **Chỉ số cơ thể**: BMI, Vòng bụng (Nguy cơ béo phì).

### 2.3. Sàng lọc Tử vong & Tai nạn (`ScnDeath`, `ScnAccidentHurt`)
Hỗ trợ việc ghi nhận chi tiết nguyên nhân cho các báo cáo chuyên sâu (Báo cáo Tai nạn thương tích).
*   **Tai nạn thương tích**: Ghi nhận nguyên nhân (Giao thông, Lao động, Sinh hoạt), địa điểm xảy ra, phương tiện gây tai nạn.
*   **Tử vong**: Phân tích nguyên nhân tử vong theo mã ICD-10 và hoàn cảnh tử vong.

## 3. Liên kết Dữ liệu
Dữ liệu từ phân hệ SCN thường được sử dụng để:
1.  **Lập Hồ sơ sức khỏe điện tử (EHR)**: Cập nhật thông tin xã hội và tiền sử bệnh nhân.
2.  **Báo cáo Y tế công cộng**: Cung cấp số liệu cho CDC và Bộ Y tế.

```mermaid
graph TD
    SCN[Sàng lọc SCN]
    SCN --> Disability[Khuyết tật]
    SCN --> Risk[Yếu tố Nguy cơ]
    SCN --> Injury[Tai nạn Thương tích]
    
    Risk --> NCDs[Quản lý Bệnh không lây nhiễm]
    Disability --> Rehab[Phục hồi Chức năng]
    Injury --> Report[Báo cáo TNTT (Bộ Y tế)]
```
