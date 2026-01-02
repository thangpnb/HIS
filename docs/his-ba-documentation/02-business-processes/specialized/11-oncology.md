# Quy trình Ung bướu & Xạ trị (Oncology & Radiotherapy)

> **LƯU Ý QUAN TRỌNG**: Module này hiện nằm **NGOÀI PHẠM VI (OUT OF SCOPE)** của dự án hiện tại. Tài liệu này được giữ lại để tham khảo cho các giai đoạn phát triển sau.

## 1. Tình trạng Hiện tại
*   **Mã nguồn**: Không tìm thấy các plugin đặc thù như `Oncology`, `Radiotherapy`, `Chemotherapy`, `Cancer`.
*   **Tài liệu**: Chưa có quy trình quản lý bệnh nhân ung bướu đặc thù.

## 2. Yêu cầu Nghiệp vụ (Dự kiến)
Đối với bệnh viện có khoa Ung bướu hoặc Trung tâm Ung bướu, hệ thống cần:

1.  **Quản lý Phác đồ Hóa trị**:
    *   Lập phác đồ điều trị (chu kỳ, thuốc, liều lượng theo diện tích da/cân nặng).
    *   Theo dõi độc tính và tác dụng phụ.
2.  **Quản lý Xạ trị**:
    *   Lập kế hoạch xạ trị (Simulation, Planning).
    *   Kết nối với hệ thống máy gia tốc (LINAC) qua chuẩn DICOM-RT.
    *   Quản lý liều chiếu thực tế.
3.  **Hội chẩn Ung bướu (Tumor Board)**:
    *   Ghi nhận biên bản hội chẩn đa chuyên khoa.

## 3. Đánh giá Sơ bộ
*   Hiện tại các bệnh nhân ung bướu có thể vẫn được quản lý như bệnh nhân nội trú thông thường (sử dụng module `Treatment`, `Prescription`).
*   Tuy nhiên, việc thiếu các tính năng tính liều hóa chất tự động và kết nối máy xạ trị là một thiếu hụt lớn nếu bệnh viện có triển khai kỹ thuật cao.
