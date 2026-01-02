# Technical Spec: Dược Lâm Sàng (Clinical Pharmacy)

## 1. Business Mapping
*   **Ref**: [Quy trình Dược lâm sàng](../../02-business-processes/pharmacy/06-clinical-pharmacy.md)
*   **Scope**: Các tính năng hỗ trợ duyệt thuốc, cảnh báo tương tác và báo cáo ADR.
*   **Key Plugins**:
    *   `HIS.Desktop.Plugins.ActiveIngredientAndConflict`: Cảnh báo tương tác.
    *   `HIS.Desktop.Plugins.HisAdr`: Báo cáo phản ứng có hại.
    *   `HIS.Desktop.Plugins.AntibioticRequest`: Duyệt phiếu kháng sinh.

## 2. Core Components

### 2.1. Cảnh báo Tương tác (Interaction Warning)
*   **Plugin**: `HIS.Desktop.Plugins.ActiveIngredientAndConflict`.
*   **Logic**:
    *   Khi bác sĩ kê đơn (`PrescriptionMaker`), hệ thống call API check tương tác.
    *   Dữ liệu: Bảng `HIS_INTERACTION` (cặp tương tác) hoặc tích hợp với cơ sở dữ liệu bên thứ 3 (nếu có).
    *   Logic kiểm tra: Dựa trên `ACTIVE_INGREDIENT_ID` (Hoạt chất) của thuốc.

### 2.2. Báo cáo ADR
*   **Plugin**: `HIS.Desktop.Plugins.HisAdr`.
*   **UI**: Form nhập phiếu báo cáo ADR theo mẫu Bộ Y tế.
*   **Features**:
    *   Ghi nhận thuốc nghi ngờ.
    *   Mô tả biểu hiện lâm sàng.
    *   Export XML/PDF báo cáo.

### 2.3. Duyệt Kháng sinh
*   **Plugin**: `HIS.Desktop.Plugins.AntibioticRequest`.
*   **Process**:
    1.  Bác sĩ yêu cầu sử dụng KS hạn chế.
    2.  Dược lâm sàng nhận thông báo và duyệt trên giao diện `AntibioticRequestList`.
    3.  Kết quả duyệt (Đồng ý/Từ chối) update lại trạng thái y lệnh để cho phép/chặn kho dược xuất thuốc.

## 3. Database Schema

### 3.1. HIS_ADR (Báo cáo ADR)
*   `ID`: PK.
*   `TREATMENT_ID`: FK.
*   `DRUG_INFO`: Thông tin thuốc nghi ngờ.
*   `REACTION_DESC`: Mô tả phản ứng.
*   `SEVERITY`: Mức độ nghiêm trọng.

### 3.2. HIS_ANTIBIOTIC_REQUEST
*   `ID`: PK.
*   `TREATMENT_ID`: FK.
*   `SERVICE_REQ_ID`: FK ref `HIS_SERVICE_REQ`.
*   `APPR_STT_ID`: Trạng thái duyệt.
*   `NOTE`: Ý kiến dược sĩ lâm sàng.

## 4. Helper Libraries
*   `HIS.Desktop.Plugins.Library.DrugInterventionInfo`: Thư viện tra cứu thông tin can thiệp dược.

## 5. Tích hợp
*   **Kê đơn (Prescription)**: Block hoặc Warning ngay khi chọn thuốc nếu có tương tác mức độ nghiêm trọng.
*   **Xuất dược**: Chặn xuất nếu phiếu duyệt kháng sinh chưa được Approve.
