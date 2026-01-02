# Từ điển Dữ liệu (Data Dictionary)

Tài liệu này mô tả các bảng (Table), khung nhìn (View) và luồng dữ liệu chính trong hệ thống HIS.
Dữ liệu được tổ chức theo các phân hệ nghiệp vụ.

## 1. Core (Hành chính & Điều trị)

Các bảng cốt lõi lưu trữ thông tin bệnh nhân và quá trình điều trị.

| Tên Bảng | Mô tả | Key Columns |
|:---|:---|:---|
| `HIS_PATIENT` | Hồ sơ bệnh nhân (nhân khẩu học). | `ID`, `PATIENT_CODE`, `VIR_PATIENT_NAME`, `DOB`, `GENDER_ID` |
| `HIS_TREATMENT` | Đợt điều trị (Nội trú/Ngoại trú). | `ID`, `TREATMENT_CODE`, `PATIENT_ID`, `IN_TIME`, `OUT_TIME`, `TREATMENT_END_TYPE_ID` |
| `HIS_DEPARTMENT` | Danh mục Khoa phòng. | `ID`, `DEPARTMENT_CODE`, `DEPARTMENT_NAME` |
| `HIS_ROOM` | Danh mục Phòng (thuộc Khoa). | `ID`, `ROOM_CODE`, `DEPARTMENT_ID`, `ROOM_TYPE_ID` |

## 2. Patient Management (Tiếp đón & Hàng đợi)

Hỗ trợ quy trình xếp hàng và gọi số (xem [Patient Call Technical](../../03-technical-specs/patient-management/02-technical-design.md)).

| Tên Bảng | Mô tả | Key Columns |
|:---|:---|:---|
| `HIS_SERVICE_REQ` | Yêu cầu dịch vụ (Check-in, Khám, CLS). | `ID`, `SERVICE_REQ_CODE`, `TREATMENT_ID`, `EXECUTE_ROOM_ID`, `REQUEST_ROOM_ID` |
| `HIS_PATIENT_TYPE_ALTER` | Lịch sử chuyển đổi đối tượng (BH/VP). | `ID`, `TREATMENT_ID`, `PATIENT_TYPE_ID`, `LOG_TIME` |
| `V_HIS_ROOM_COUNTER` | (View) Đếm số lượng chờ tại các phòng. | `ROOM_ID`, `WAITING_COUNT`, `PROCESSING_COUNT` |

## 3. Pharmacy (Dược & Vật tư)

Quản lý nhập xuất tồn (xem [Pharmacy Technical](../../03-technical-specs/pharmacy/02-technical-design.md)).

| Tên Bảng | Mô tả | Key Columns |
|:---|:---|:---|
| `HIS_MEDICINE_TYPE` | Danh mục Thuốc. | `ID`, `MEDICINE_TYPE_CODE`, `MEDICINE_TYPE_NAME`, `ACTIVE_INGREDIENT_NAME` |
| `HIS_MATERIAL_TYPE` | Danh mục Vật tư. | `ID`, `MATERIAL_TYPE_CODE`, `MATERIAL_TYPE_NAME` |
| `HIS_MEDI_STOCK` | Danh mục Kho. | `ID`, `MEDI_STOCK_CODE`, `MEDI_STOCK_NAME` |
| `HIS_IMP_MEST` | Phiếu Nhập kho (Header). | `ID`, `IMP_MEST_CODE`, `IMP_TIME`, `IMP_MEST_TYPE_ID` |
| `HIS_IMP_MEST_MEDICINE` | Chi tiết Nhập thuốc. | `ID`, `IMP_MEST_ID`, `MEDICINE_TYPE_ID`, `AMOUNT`, `PRICE` |
| `HIS_EXP_MEST` | Phiếu Xuất kho (Header). | `ID`, `EXP_MEST_CODE`, `EXP_TIME`, `EXP_MEST_TYPE_ID` |

## 4. Laboratory (Xét nghiệm)

Lưu trữ kết quả xét nghiệm từ LIS (xem [LIS Technical](../../03-technical-specs/laboratory/02-technical-design.md)).

| Tên Bảng | Mô tả | Key Columns |
|:---|:---|:---|
| `HIS_SERE_SERV` | Chỉ định dịch vụ (Cha). | `ID`, `SERVICE_REQ_ID`, `SERVICE_ID`, `AMOUNT`, `PRICE`, `TDL_HEIN_PRICE` |
| `HIS_SERE_SERV_TE` | Kết quả xét nghiệm (Chi tiết). | `ID`, `SERE_SERV_ID`, `VALUE`, `RESULT_CODE`, `DESCRIPTION` |
| `HIS_TEST_INDEX` | Chỉ số xét nghiệm (Catalog). | `ID`, `TEST_INDEX_CODE`, `TEST_INDEX_NAME`, `TEST_INDEX_UNIT_ID` |

## 5. View & Reporting

Các view thường dùng cho báo cáo (MPS).

*   `V_HIS_TREATMENT_FEE`: Tổng hợp chi phí điều trị.
*   `V_HIS_HEIN_APPROVAL`: Thông tin duyệt bảo hiểm y tế.
*   `V_HIS_IMP_MEST_MEDICINE`: Chi tiết nhập kho kèm thông tin thuốc.
