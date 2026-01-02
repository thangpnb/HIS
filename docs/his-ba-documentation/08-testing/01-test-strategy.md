# Chiến lược Kiểm thử (Test Strategy)

Tài liệu này quy định phương pháp tiếp cận kiểm soát chất lượng (QA) cho hệ thống HIS.

## 1. Các Cấp độ Kiểm thử

### 1.1. Unit Test (Kiểm thử đơn vị)
*   **Phạm vi**: Các hàm xử lý logic (Processor), tính toán viện phí, xử lý tồn kho.
*   **Người thực hiện**: Developer.
*   **Công cụ**: MS Test / NUnit.
*   **Yêu cầu**: Coverage > 80% đối với các module tính tiền.

### 1.2. Integration Test (Kiểm thử tích hợp)
*   **Phạm vi**: Giao tiếp giữa Client và API, giữa HIS và MPS (In ấn).
*   **Người thực hiện**: Developer / Tester.
*   **Môi trường**: Staging (Dữ liệu giả lập gần giống thật).

### 1.3. System Test (Kiểm thử hệ thống)
*   **Phạm vi**: Kiểm thử luồng nghiệp vụ trọn vẹn (End-to-End).
    *   VD: Tiếp đón -> Khám -> Kê đơn -> Thu ngân -> Xuất thuốc.
*   **Người thực hiện**: QA/Tester.

### 1.4. User Acceptance Test (UAT)
*   **Phạm vi**: Người dùng cuối (Bác sĩ, Điều dưỡng) kiểm thử trên môi trường Staging trước khi golive.
*   **Mục tiêu**: Xác nhận phần mềm đáp ứng đúng nhu cầu nghiệp vụ thực tế.

## 2. Quy trình Báo lỗi (Bug Report)
Khi phát hiện lỗi, Tester cần log lên hệ thống (Jira/Trello) với mẫu sau:
1.  **Tiêu đề**: [Module] Tóm tắt lỗi.
2.  **Môi trường**: Test / Staging / Production.
3.  **Các bước tái hiện (Steps to reproduce)**:
    1.  Đăng nhập user A.
    2.  Vào chức năng B.
    3.  Nhập dữ liệu C.
    4.  Nhấn Lưu.
4.  **Kết quả mong đợi**: Lưu thành công.
5.  **Kết quả thực tế**: Báo lỗi "NullReferenceException".
6.  **Ảnh chụp màn hình/Log**: (Đính kèm).

## 3. Tiêu chí Chấp nhận (Acceptance Criteria)
Một tính năng được coi là "Hoàn thành" (Done) khi:
*   Đã pass tất cả Test Case tích cực (Happy path) và tiêu cực (Edge case).
*   Không còn lỗi nghiêm trọng (Critical/High).
*   Đã cập nhật tài liệu hướng dẫn sử dụng.
