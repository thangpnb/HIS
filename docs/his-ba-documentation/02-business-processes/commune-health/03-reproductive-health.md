# Sức khỏe Sinh sản & KHHGĐ (Reproductive Health)

## 1. Tổng quan
Chăm sóc sức khỏe sinh sản là nhiệm vụ trọng tâm của tuyến y tế cơ sở. Hệ thống cung cấp các công cụ để quản lý thai nghén, sinh đẻ và kế hoạch hóa gia đình.

## 2. Quản lý Thai sản (`TYTFetus`)

### 2.1. Quản lý Thai nghén (`TYTFetusExam`)
Theo dõi quá trình mang thai của thai phụ trên địa bàn ("Quản lý thai 3 tháng đầu/giữa/cuối").
*   **Chức năng**:
    *   **Lập số khám thai**: Ghi nhận thông tin hành chính, tiền sử sản khoa.
    *   **Khám thai định kỳ**: Ghi nhận các chỉ số sinh tồn, bề cao tử cung, tim thai, tiêm uốn ván (VAT).
    *   **Dự kiến sinh**: Tự động tính ngày dự kiến sinh.

### 2.2. Quản lý Đẻ (`TYTFetusBorn`)
Ghi nhận thông tin về các ca sinh tại trạm hoặc đẻ rớt được trạm hỗ trợ.
*   **Thông tin mẹ**: Diễn biến cuộc chuyển dạ, can thiệp (cắt TSM, v.v.).
*   **Thông tin con**: Cân nặng, giới tính, tình trạng sau sinh (Apgar), tiêm Vitamin K1, Viêm gan B.
*   **Kết quả**: Tai biến sản khoa (nếu có).

## 3. Kế hoạch hóa Gia đình (`TYTKhh`)
Quản lý các biện pháp tránh thai (BPTT) cho phụ nữ trong độ tuổi sinh đẻ (15-49).
*   **Đối tượng**: Phụ nữ có chồng trong độ tuổi sinh đẻ.
*   **Dịch vụ**:
    *   Đặt vòng tránh thai (IUD).
    *   Cấp phát thuốc uống tránh thai, bao cao su.
    *   Tiêm thuốc tránh thai.
*   **Báo cáo**: Tỷ lệ áp dụng các biện pháp tránh thai hiện đại.

## 4. Dòng Chảy Dữ liệu (Workflow)

```mermaid
sequenceDiagram
    participant Woman as Phụ nữ/Sản phụ
    participant Midwife as Nữ hộ sinh (TYT)
    participant System as Phần mềm TYT

    Note over Woman, System: Quy trình Khám thai & Sinh đẻ
    
    Woman->>Midwife: Đến khám thai
    Midwife->>System: Tra cứu hồ sơ (TYTFetusExam)
    alt Chưa có hồ sơ
        Midwife->>System: Lập sổ khám thai mới
    end
    Midwife->>System: Ghi nhận kết quả khám (Tim thai, HA, Cân nặng)
    System-->>Midwife: Cảnh báo nguy cơ cao (nếu có)
    
    Note over Woman, System: Khi chuyển dạ
    
    Woman->>Midwife: Nhập trạm chờ sinh
    Midwife->>System: Ghi nhận diễn biến chuyển dạ (TYTFetusBorn)
    Midwife->>System: Ghi nhận thông tin trẻ sơ sinh
    System-->>Midwife: Lưu hồ sơ sinh & Cập nhật thống kê
```
