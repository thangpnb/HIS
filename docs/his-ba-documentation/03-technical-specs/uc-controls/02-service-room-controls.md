## Mục đích và Phạm vi

Tài liệu này đề cập đến các thành phần Điều khiển người dùng (User Control - UC) trong mô-đun \`UC/\` quản lý các dịch vụ, buồng phòng và các thực thể hạ tầng bệnh viện liên quan. Các UC này cung cấp các thành phần UI có thể tái sử dụng để chọn dịch vụ, quản lý buồng phòng, cấu hình dịch vụ khám bệnh và theo dõi quốc tịch/tình trạng bệnh của bệnh nhân.

Các thành phần chính được ghi lại ở đây bao gồm:
- \`HIS.UC.ServiceRoom\` (48 tệp) - Quản lý mối liên kết buồng-dịch vụ
- \`HIS.UC.ServiceUnit\` (48 tệp) - Cấu hình đơn vị dịch vụ
- \`HIS.UC.RoomExamService\` (40 tệp) - Thiết lập dịch vụ phòng khám
- \`HIS.UC.ServiceRoomInfo\` (43 tệp) - Hiển thị thông tin buồng dịch vụ
- \`HIS.UC.Sick\` (43 tệp) - Theo dõi tình trạng bệnh/bệnh tật của bệnh nhân
- \`HIS.UC.National\` (41 tệp) - Quản lý quốc tịch bệnh nhân

Đối với các UC thuốc và vật tư, hãy xem [1.3.3](#1.3.3). Đối với các UC bệnh nhân và điều trị, hãy xem [1.3.2](#1.3.2). Đối với form engine, hãy xem [1.3.1](#1.3.1).

Nguồn: [[\`.devin/wiki.json:235-237\`](../../../../.devin/wiki.json#L235-L237)](../../../../.devin/wiki.json#L235-L237)

---

## Tổng quan Kiến trúc

Các UC Dịch vụ & Buồng phòng tạo thành một nhóm quy mô trung bình trong thư viện UC gồm 131 thành phần. Các thành phần này được nhúng trong các plugin khác nhau trên toàn hệ thống HIS, đặc biệt là trong các quy trình đăng ký, khám bệnh và thực hiện dịch vụ.

\`\`\`mermaid
graph TB
    subgraph "Lớp_Plugin_HIS"
        Register["HIS.Desktop.Plugins.Register<br/>Quy trình Đăng ký"]
        Exam["HIS.Desktop.Plugins.Exam<br/>Quy trình Khám bệnh"]
        ServiceExecute["HIS.Desktop.Plugins.ServiceExecute<br/>Thực hiện dịch vụ"]
        RoomConfig["HIS.Desktop.Plugins.*Room*<br/>Cấu hình Buồng phòng"]
    end
    
    subgraph "Các_thành_phần_UC_Dịch_vụ_&_Buồng_phòng"
        ServiceRoom["HIS.UC.ServiceRoom<br/>48 tệp<br/>Ánh xạ Buồng-Dịch vụ"]
        ServiceUnit["HIS.UC.ServiceUnit<br/>48 tệp<br/>Đơn vị dịch vụ"]
        RoomExamService["HIS.UC.RoomExamService<br/>40 tệp<br/>Dịch vụ phòng khám"]
        ServiceRoomInfo["HIS.UC.ServiceRoomInfo<br/>43 tệp<br/>Thông tin buồng phòng"]
        Sick["HIS.UC.Sick<br/>43 tệp<br/>Tình trạng bệnh tật"]
        National["HIS.UC.National<br/>41 tệp<br/>Dữ liệu quốc tịch"]
    end
    
    subgraph "Lớp_Nền_tảng"
        InventecUC["Inventec.UC<br/>1060 tệp<br/>Các điều khiển cơ sở"]
        BackendData["HIS.Desktop.LocalStorage.BackendData<br/>Đệm dữ liệu Dịch vụ/Buồng phòng"]
    end
    
    Register --> ServiceRoom
    Register --> National
    Exam --> RoomExamService
    Exam --> Sick
    ServiceExecute --> ServiceUnit
    RoomConfig --> ServiceRoomInfo
    
    ServiceRoom --> InventecUC
    ServiceUnit --> InventecUC
    RoomExamService --> InventecUC
    ServiceRoomInfo --> InventecUC
    Sick --> InventecUC
    National --> InventecUC
    
    ServiceRoom -.->|Cache| BackendData
    ServiceUnit -.->|Cache| BackendData
    RoomExamService -.->|Cache| BackendData
\`\`\`

**Sơ đồ 1: Kiến trúc Tích hợp UC Dịch vụ & Buồng phòng**

Sơ đồ này cho thấy cách các UC dịch vụ và buồng phòng được sử dụng bởi các plugin HIS khác nhau. Các UC được xây dựng trên lớp nền tảng \`Inventec.UC\` và tương tác với \`HIS.Desktop.LocalStorage.BackendData\` để lấy siêu dữ liệu dịch vụ và buồng phòng đã được đệm.

Nguồn: [[\`.devin/wiki.json:200-207\`](../../../../.devin/wiki.json#L200-L207)](../../../../.devin/wiki.json#L200-L207), [[\`.devin/wiki.json:235-237\`](../../../../.devin/wiki.json#L235-L237)](../../../../.devin/wiki.json#L235-L237)

---

## Cấu trúc Thành phần

Mỗi UC Dịch vụ & Buồng phòng tuân theo một cấu trúc nội bộ nhất quán:

| Thành phần | Số tệp | Trách nhiệm chính | Các Lớp/Interface chính |
|-----------|-------|------------------------|------------------------|
| \`HIS.UC.ServiceRoom\` | 48 | Ánh xạ dịch vụ tới buồng, quản lý tính khả dụng | \`UCServiceRoom\`, \`ServiceRoomProcessor\`, \`ServiceRoomADO\` |
| \`HIS.UC.ServiceUnit\` | 48 | Cấu hình đơn vị đo lường dịch vụ | \`UCServiceUnit\`, \`ServiceUnitProcessor\`, \`ServiceUnitADO\` |
| \`HIS.UC.RoomExamService\` | 40 | Cấu hình dịch vụ phòng khám | \`UCRoomExamService\`, \`RoomExamServiceProcessor\` |
| \`HIS.UC.ServiceRoomInfo\` | 43 | Hiển thị thông tin dịch vụ-buồng phòng | \`UCServiceRoomInfo\`, \`ServiceRoomInfoProcessor\` |
| \`HIS.UC.Sick\` | 43 | Theo dõi tình trạng bệnh tật của bệnh nhân | \`UCSick\`, \`SickProcessor\`, \`SickADO\` |
| \`HIS.UC.National\` | 41 | Quản lý dữ liệu quốc tịch bệnh nhân | \`UCNational\`, \`NationalProcessor\`, \`NationalADO\` |

Mỗi UC thường bao gồm:
- Lớp điều khiển người dùng chính ([\`UC*.cs\`](../../../UC*.cs))
- Lớp Processor cho logic nghiệp vụ ([\`*Processor.cs\`](../../../*Processor.cs))
- Các lớp ADO (Active Data Object) cho mô hình hóa dữ liệu ([\`*ADO.cs\`](../../../*ADO.cs))
- Các tệp thiết kế ([[\`.Designer.cs\`](../../../.Designer.cs)](../../../.Designer.cs))
- Các tệp tài nguyên (\`Properties/\`)
- Logic cấu hình và khởi tạo

Nguồn: [[\`.devin/wiki.json:235-237\`](../../../../.devin/wiki.json#L235-L237)](../../../../.devin/wiki.json#L235-L237)

---

## UC ServiceRoom

\`\`\`mermaid
graph LR
    subgraph "UC/HIS.UC.ServiceRoom/"
        MainControl["UCServiceRoom.cs<br/>Điều khiển chính"]
        Processor["ServiceRoomProcessor.cs<br/>Logic nghiệp vụ"]
        ADO["ServiceRoomADO.cs<br/>Mô hình dữ liệu"]
        Run["Run/<br/>Logic khởi tạo"]
        Design["Design/<br/>Bố cục UI"]
    end
    
    subgraph "Tích_hợp_Plugin"
        RegisterPlugin["Các Plugin Đăng ký<br/>Chọn phòng"]
        ServicePlugin["Các Plugin Dịch vụ<br/>Chỉ định dịch vụ"]
    end
    
    subgraph "Nguồn_Dữ_liệu"
        BackendData["BackendData.Rooms<br/>Dữ liệu buồng đã đệm"]
        BackendServices["BackendData.Services<br/>Dữ liệu dịch vụ đã đệm"]
        HISROOM["Thực thể V_HIS_ROOM<br/>Mô hình Backend"]
    end
    
    RegisterPlugin --> Processor
    ServicePlugin --> Processor
    Processor --> MainControl
    Processor --> ADO
    
    Processor -.->|Tải| BackendData
    Processor -.->|Tải| BackendServices
    BackendData -.->|Đồng bộ từ| HISROOM
\`\`\`

**Sơ đồ 2: Kiến trúc thành phần UC ServiceRoom**

Thành phần \`HIS.UC.ServiceRoom\` cung cấp UI để ánh xạ các dịch vụ tới các buồng cụ thể và quản lý tính sẵn có của buồng-dịch vụ. Nó tải dữ liệu từ \`HIS.Desktop.LocalStorage.BackendData\`, nơi đệm các thực thể backend như \`V_HIS_ROOM\`.

### Các Trách nhiệm Chính

- **Ánh xạ Buồng-Dịch vụ**: Hiển thị các dịch vụ có sẵn cho các buồng được chọn.
- **Tính sẵn có của Dịch vụ**: Cho biết dịch vụ nào có thể được thực hiện ở buồng nào.
- **UI chọn Buồng**: Cung cấp các lưới chọn buồng có thể lọc và tìm kiếm.
- **Quản lý Cấu hình**: Xử lý cấu hình mối quan hệ buồng-dịch vụ.

### Mô hình Sử dụng Thông thường

Các plugin thường khởi tạo UC \`ServiceRoom\` thông qua mô hình processor:

\`\`\`
UC/HIS.UC.ServiceRoom/
├── ServiceRoomProcessor.cs - Điểm thâm nhập cho các plugin
├── UCServiceRoom.cs - Điều khiển chính với UI dạng lưới/danh sách
├── ServiceRoomADO.cs - Đối tượng truyền dữ liệu cho dữ liệu buồng-dịch vụ
└── Run/
    └── UCServiceRoomRun.cs - Factory để khởi tạo điều khiển
\`\`\`

Processor để lộ các phương thức cho:
- Tải các ánh xạ buồng-dịch vụ.
- Lọc theo loại dịch vụ hoặc loại buồng.
- Xử lý các sự kiện lựa chọn.
- Làm mới dữ liệu từ bộ đệm.

Nguồn: [[\`.devin/wiki.json:235-237\`](../../../../.devin/wiki.json#L235-L237)](../../../../.devin/wiki.json#L235-L237)

---

## UC ServiceUnit

Thành phần \`HIS.UC.ServiceUnit\` (48 tệp) quản lý các đơn vị đo lường dịch vụ và chuyển đổi đơn vị cho các dịch vụ y tế.

### Cấu trúc

\`\`\`mermaid
graph TB
    subgraph "UC/HIS.UC.ServiceUnit/"
        UCMain["UCServiceUnit.cs<br/>Hiển thị đơn vị dạng lưới"]
        Processor["ServiceUnitProcessor.cs<br/>Logic quản lý đơn vị"]
        ADO["ServiceUnitADO.cs<br/>Mô hình dữ liệu đơn vị"]
    end
    
    subgraph "Ngữ_cảnh_Sử_dụng"
        PrescriptionPlugin["Các Plugin Đơn thuốc<br/>Đơn vị liều dùng"]
        ServicePlugin["Các Plugin Dịch vụ<br/>Đơn vị kết quả"]
        LISPlugin["Các Plugin LIS<br/>Đơn vị xét nghiệm"]
    end
    
    subgraph "Thực_thể_Backend"
        HISUNIT["HIS_SERVICE_UNIT<br/>Bảng Backend"]
        ConversionRules["Quy tắc chuyển đổi đơn vị"]
    end
    
    PrescriptionPlugin --> Processor
    ServicePlugin --> Processor
    LISPlugin --> Processor
    
    Processor --> UCMain
    Processor --> ADO
    Processor -.->|Tải| HISUNIT
    Processor -.->|Áp dụng| ConversionRules
\`\`\`

**Sơ đồ 3: Luồng dữ liệu UC ServiceUnit**

### Các Tính năng Chính

- **Hiển thị Đơn vị**: Hiển thị tất cả các đơn vị đo lường đã cấu hình (mg, ml, đơn vị quốc tế UI, v.v.).
- **Chuyển đổi Đơn vị**: Xử lý chuyển đổi giữa các đơn vị tương thích.
- **Liên kết Dịch vụ**: Liên kết các đơn vị với các loại dịch vụ cụ thể.
- **Xác thực**: Đảm bảo lựa chọn đơn vị hợp lệ cho liều lượng và các số đo.

Nguồn: [[\`.devin/wiki.json:235-237\`](../../../../.devin/wiki.json#L235-L237)](../../../../.devin/wiki.json#L235-L237)

---

## UC RoomExamService

Thành phần \`HIS.UC.RoomExamService\` (40 tệp) cấu hình những dịch vụ khám bệnh nào có sẵn trong các phòng khám cụ thể.

### Kiến trúc

| Tệp/Thành phần | Mục đích |
|----------------|---------|
| [[\`UCRoomExamService.cs\`](../../../UCRoomExamService.cs)](../../../UCRoomExamService.cs) | Điều khiển chính với lưới dịch vụ-buồng |
| [[\`RoomExamServiceProcessor.cs\`](../../../RoomExamServiceProcessor.cs)](../../../RoomExamServiceProcessor.cs) | Tải và lọc các cấu hình dịch vụ khám |
| [[\`RoomExamServiceADO.cs\`](../../../RoomExamServiceADO.cs)](../../../RoomExamServiceADO.cs) | Đối tượng truyền dữ liệu cho các cặp buồng-dịch vụ |
| [[\`Run/UCRoomExamServiceRun.cs\`](../../../Run/UCRoomExamServiceRun.cs)](../../../Run/UCRoomExamServiceRun.cs) | Factory để khởi tạo điều khiển |
| \`Reload/\` | Logic làm mới dữ liệu |
| \`Get/\` | Các phương thức truy vấn để lấy thông tin cấu hình |

### Các điểm tích hợp

\`\`\`mermaid
graph LR
    subgraph "Quy_trình_Khám_bệnh"
        ExamPlugin["Các Plugin Khám"]
        CallPatientExam["Plugin Gọi bệnh nhân khám"]
    end
    
    subgraph "UC_RoomExamService"
        RoomExamProcessor["RoomExamServiceProcessor"]
        ConfigUI["UI Cấu hình Dịch vụ"]
    end
    
    subgraph "Dữ_liệu_Cấu_hình"
        RoomConfig["V_HIS_EXECUTE_ROOM<br/>Cấu hình phòng thực hiện"]
        ServiceConfig["V_HIS_SERVICE<br/>Định nghĩa dịch vụ"]
    end
    
    ExamPlugin -->|"Khởi tạo"| RoomExamProcessor
    CallPatientExam -->|"Tải cấu hình"| RoomExamProcessor
    RoomExamProcessor --> ConfigUI
    RoomExamProcessor -.->|"Truy vấn"| RoomConfig
    RoomExamProcessor -.->|"Truy vấn"| ServiceConfig
\`\`\`

**Sơ đồ 4: Luồng cấu hình RoomExamService**

UC \`RoomExamService\` chủ yếu được sử dụng trong việc thiết lập phòng khám và các plugin quy trình khám bệnh để xác định dịch vụ nào có thể được thực hiện trong mỗi phòng khám.

Nguồn: [[\`.devin/wiki.json:235-237\`](../../../../.devin/wiki.json#L235-L237)](../../../../.devin/wiki.json#L235-L237)

---

## UC ServiceRoomInfo

Thành phần \`HIS.UC.ServiceRoomInfo\` (43 tệp) hiển thị thông tin chi tiết về các liên kết dịch vụ-buồng phòng, bao gồm lịch trình, tính sẵn có và công suất.

### Các Thành phần Chính

\`\`\`
UC/HIS.UC.ServiceRoomInfo/
├── UCServiceRoomInfo.cs - Điều khiển hiển thị thông tin chính
├── ServiceRoomInfoProcessor.cs - Tải và định dạng dữ liệu
├── ServiceRoomInfoADO.cs - Mô hình thông tin buồng-dịch vụ mở rộng
├── Design/ - Các định nghĩa bố cục UI
├── Validation/ - Logic xác thực dữ liệu
└── Resources/ - Bản địa hóa và hình ảnh
\`\`\`

### Các Tính năng Hiển thị

- **Chi tiết Buồng**: Hiển thị tên buồng, mã, khoa và loại buồng.
- **Danh sách Dịch vụ**: Hiển thị tất cả các dịch vụ có sẵn trong buồng.
- **Thông tin Lịch trình**: Hiển thị tính sẵn có của dịch vụ theo ngày/giờ.
- **Thông tin Công suất**: Hiển thị công suất buồng và số lượng bệnh nhân hiện tại.
- **Chỉ báo Trạng thái**: Hiển thị trạng thái hoạt động của buồng.

### Sử dụng trong các Plugin

UC \`ServiceRoomInfo\` thường được sử dụng trong:
- Các plugin Dashboard để xem tổng quan trạng thái buồng.
- Các plugin Đăng ký để chọn buồng.
- Các plugin Thực hiện dịch vụ để xác minh buồng.
- Các plugin Báo cáo để phân tích hiệu suất sử dụng buồng.

Nguồn: [[\`.devin/wiki.json:235-237\`](../../../../.devin/wiki.json#L235-L237)](../../../../.devin/wiki.json#L235-L237)

---

## UC Sick

Thành phần \`HIS.UC.Sick\` (43 tệp) quản lý các tình trạng bệnh tật, triệu chứng và theo dõi mức độ nghiêm trọng của bệnh nhân.

### Mô hình Dữ liệu

\`\`\`mermaid
graph TB
    subgraph "Các_thành_phần_UC_Sick"
        UCSick["UCSick.cs<br/>UI chọn tình trạng"]
        SickProcessor["SickProcessor.cs<br/>Logic về bệnh tật"]
        SickADO["SickADO.cs<br/>Mô hình dữ liệu bệnh tật"]
    end
    
    subgraph "Dữ_liệu_Backend"
        HISSICK["HIS_SICK<br/>Định nghĩa bệnh tật"]
        SickType["HIS_SICK_TYPE<br/>Danh mục tình trạng"]
    end
    
    subgraph "Sử_dụng_trong_Plugin"
        ExamPlugin["Các Plugin Khám<br/>Ghi lại tình trạng"]
        TreatmentPlugin["Các Plugin Điều trị<br/>Theo dõi tiến triển"]
        EmergencyPlugin["Các Plugin Cấp cứu<br/>Phân loại bệnh"]
    end
    
    ExamPlugin --> SickProcessor
    TreatmentPlugin --> SickProcessor
    EmergencyPlugin --> SickProcessor
    
    SickProcessor --> UCSick
    SickProcessor --> SickADO
    
    SickProcessor -.->|Tải| HISSICK
    SickProcessor -.->|Phân loại| SickType
\`\`\`

**Sơ đồ 5: Tích hợp UC Sick với quy trình Khám bệnh**

### Các Tính năng Chính

- **Chọn Tình trạng**: Cung cấp danh sách các tình trạng bệnh tật đã định nghĩa có thể tìm kiếm.
- **Mức độ Nghiêm trọng**: Theo dõi mức độ nghiêm trọng (nhẹ, trung bình, nặng, nguy kịch).
- **Nhiều Tình trạng**: Hỗ trợ nhiều tình trạng đồng thời cho mỗi bệnh nhân.
- **Lọc theo Danh mục**: Lọc các tình trạng theo loại (truyền nhiễm, mãn tính, cấp tính, v.v.).
- **Theo dõi Lịch sử**: Duy trì lịch sử tình trạng bệnh trong suốt quá trình điều trị.

### Cấu trúc Dữ liệu

Lớp \`SickADO\` thường chứa:
- \`SICK_CODE\` - Định danh duy nhất cho tình trạng bệnh
- \`SICK_NAME\` - Tên hiển thị của tình trạng bệnh
- \`SICK_TYPE_CODE\` - Phân loại danh mục
- \`SEVERITY_LEVEL\` - Chỉ báo mức độ nghiêm trọng
- \`ONSET_DATE\` - Ngày ghi nhận tình trạng lần đầu
- \`RESOLUTION_DATE\` - Ngày tình trạng được giải quyết (nếu có)

Nguồn: [[\`.devin/wiki.json:235-237\`](../../../../.devin/wiki.json#L235-L237)](../../../../.devin/wiki.json#L235-L237)

---

## UC National

Thành phần \`HIS.UC.National\` (41 tệp) quản lý dữ liệu quốc tịch bệnh nhân cho mục đích đăng ký và báo cáo.

### Cấu trúc

\`\`\`
UC/HIS.UC.National/
├── UCNational.cs - Điều khiển chọn quốc tịch
├── NationalProcessor.cs - Xử lý dữ liệu quốc tịch
├── NationalADO.cs - Mô hình dữ liệu quốc tịch
├── Combo/ - Các triển khai combo box
├── Grid/ - Các triển khai grid view
└── Validation/ - Logic xác thực quốc tịch
\`\`\`

### Chức năng

| Tính năng | Mô tả |
|---------|-------------|
| **Danh sách Quốc tịch** | Hiển thị tất cả các quốc tịch đã cấu hình từ \`SDA_NATIONAL\` |
| **Lựa chọn Mặc định** | Tự động chọn quốc tịch mặc định (thường là Việt Nam) |
| **Tìm kiếm/Lọc** | Tìm kiếm nhanh theo tên hoặc mã quốc tịch |
| **Hỗ trợ Đa ngôn ngữ** | Hiển thị tên quốc tịch bằng nhiều ngôn ngữ |
| **Xác thực** | Đảm bảo lựa chọn quốc tịch hợp lệ cho báo cáo pháp lý |

### Tích hợp với Hệ thống SDA

UC National tích hợp với mô-đun SDA (System Data Administration):

\`\`\`mermaid
graph LR
    subgraph "UC_National"
        UCNational["Điều khiển UCNational"]
        NationalProcessor["NationalProcessor"]
    end
    
    subgraph "Dữ_liệu_Hệ_thống_SDA"
        SDANational["SDA_NATIONAL<br/>Định nghĩa Quốc tịch"]
        SDAProvince["SDA_PROVINCE<br/>Dữ liệu Tỉnh/Thành"]
        SDADistrict["SDA_DISTRICT<br/>Dữ liệu Quận/Huyện"]
    end
    
    subgraph "Sử_dụng_trong_Plugin"
        RegisterPlugin["Các Plugin Đăng ký<br/>Đăng ký bệnh nhân"]
        PatientPlugin["Các Plugin Bệnh nhân<br/>Hồ sơ bệnh nhân"]
        ReportPlugin["Các Plugin Báo cáo<br/>Nhân khẩu học"]
    end
    
    RegisterPlugin --> NationalProcessor
    PatientPlugin --> NationalProcessor
    ReportPlugin --> NationalProcessor
    
    NationalProcessor --> UCNational
    NationalProcessor -.->|Tải| SDANational
    SDANational -.->|Liên kết| SDAProvince
    SDAProvince -.->|Liên kết| SDADistrict
\`\`\`

**Sơ đồ 6: Mối quan hệ dữ liệu UC National**

### Mô hình Sử dụng Thông thường

Các plugin thường sử dụng UC National trong các biểu mẫu đăng ký bệnh nhân:

1. Khởi tạo processor với dữ liệu quốc tịch hiện tại từ \`BackendData\`.
2. Hiển thị điều khiển trong biểu mẫu đăng ký.
3. Xử lý các sự kiện thay đổi lựa chọn.
4. Xác thực quốc tịch trước khi lưu hồ sơ bệnh nhân.
5. Lưu trữ \`NATIONAL_CODE\` đã chọn cùng với dữ liệu bệnh nhân.

Nguồn: [[\`.devin/wiki.json:235-237\`](../../../../.devin/wiki.json#L235-L237)](../../../../.devin/wiki.json#L235-L237), [[\`.devin/wiki.json:160-168\`](../../../../.devin/wiki.json#L160-L168)](../../../../.devin/wiki.json#L160-L168)

---

## Các Mô hình Thiết kế Chung

Tất cả các UC Dịch vụ & Buồng phòng đều tuân theo các mô hình thiết kế nhất quán kế thừa từ kiến trúc thư viện UC:

### Mô hình Processor

\`\`\`mermaid
graph LR
    Plugin["Mã Plugin"] -->|1. Tạo| Processor["UC Processor"]
    Processor -->|2. Khởi tạo| UCControl["User Control"]
    Processor -->|3. Cấu hình| Data["Liên kết Dữ liệu"]
    UCControl -->|4. Sự kiện| Callback["Callback Plugin"]
    Data -.->|Tải từ| Cache["Bộ đệm BackendData"]
\`\`\`

**Sơ đồ 7: Mô hình UC Processor tiêu chuẩn**

Mỗi UC để lộ một lớp processor thực hiện:
- Cung cấp các phương thức factory để tạo điều khiển.
- Xử lý việc tải dữ liệu và đệm dữ liệu.
- Quản lý việc đăng ký sự kiện.
- Cung cấp các phương thức để làm mới và xác thực dữ liệu.

### Mô hình ADO (Active Data Object)

Các lớp ADO đóng vai trò là các đối tượng truyền dữ liệu giữa các plugin và các UC:

- **Phân tách Trách nhiệm**: Các ADO tách biệt các thực thể backend khỏi các mô hình UI.
- **Thuộc tính Mở rộng**: Thêm các thuộc tính dành riêng cho UI (trạng thái lựa chọn, định dạng hiển thị).
- **Logic Xác thực**: Bao gồm các quy tắc nghiệp vụ để xác thực dữ liệu.
- **Tuần tự hóa (Serialization)**: Hỗ trợ tuần tự hóa JSON để đệm và ghi log.

### Xử lý Sự kiện

Các UC Dịch vụ & Buồng phòng thường để lộ các loại sự kiện sau:

| Loại Sự kiện | Mục đích | Cách dùng |
|------------|---------|-------|
| \`SelectionChanged\` | Mục được chọn trong lưới/combo | Cập nhật các điều khiển phụ thuộc |
| \`DataLoaded\` | Dữ liệu được tải từ cache/backend | Bật/tắt các yếu tố UI |
| \`ValidationFailed\` | Nhập dữ liệu không hợp lệ | Hiển thị thông báo lỗi |
| \`ConfigChanged\` | Cấu hình bị thay đổi | Kích hoạt các hành động lưu |

Nguồn: [[\`.devin/wiki.json:200-207\`](../../../../.devin/wiki.json#L200-L207)](../../../../.devin/wiki.json#L200-L207)

---

## Tích hợp BackendData

Tất cả các UC Dịch vụ & Buồng phòng tương tác với \`HIS.Desktop.LocalStorage.BackendData\` để lấy dữ liệu đã được đệm:

\`\`\`mermaid
graph TB
    subgraph "Bộ_đệm_BackendData"
        Rooms["BackendData.Rooms<br/>Các thực thể V_HIS_ROOM"]
        Services["BackendData.Services<br/>Các thực thể V_HIS_SERVICE"]
        ExecuteRooms["BackendData.ExecuteRooms<br/>V_HIS_EXECUTE_ROOM"]
        Nationals["BackendData.Nationals<br/>Các thực thể SDA_NATIONAL"]
        Sicks["BackendData.Sicks<br/>Các thực thể HIS_SICK"]
    end
    
    subgraph "Các_UC_Dịch_vụ_&_Buồng_phòng"
        ServiceRoom["UC ServiceRoom"]
        ServiceUnit["UC ServiceUnit"]
        RoomExamService["UC RoomExamService"]
        ServiceRoomInfo["UC ServiceRoomInfo"]
        National["UC National"]
        Sick["UC Sick"]
    end
    
    ServiceRoom -.->|Truy vấn| Rooms
    ServiceRoom -.->|Truy vấn| Services
    ServiceUnit -.->|Truy vấn| Services
    RoomExamService -.->|Truy vấn| ExecuteRooms
    RoomExamService -.->|Truy vấn| Services
    ServiceRoomInfo -.->|Truy vấn| Rooms
    ServiceRoomInfo -.->|Truy vấn| Services
    National -.->|Truy vấn| Nationals
    Sick -.->|Truy vấn| Sicks
\`\`\`

**Sơ đồ 8: Mô hình truy cập bộ đệm BackendData**

### Trình tự Tải Bộ đệm

1. Khởi động ứng dụng: \`BackendData\` tải tất cả các thực thể từ API backend.
2. Khởi tạo UC: Processor truy vấn các bộ sưu tập \`BackendData\`.
3. Hiển thị UI: Điều khiển liên kết với dữ liệu đã được lọc/sắp xếp.
4. Tương tác người dùng: Các thay đổi cập nhật các đối tượng ADO cục bộ.
5. Hành động lưu: Plugin gửi các bản cập nhật tới API backend.
6. Làm mới bộ đệm: \`BackendData\` tải lại các thực thể bị ảnh hưởng.

### Các cân nhắc về Hiệu năng

- **Lọc trong Bộ nhớ**: Tất cả việc lọc/sắp xếp diễn ra trong bộ nhớ trên dữ liệu đã được đệm.
- **Tải lười (Lazy Loading)**: Các UC chỉ tải dữ liệu khi điều khiển được hiển thị.
- **Chiến lược làm mới**: Các UC đăng ký các sự kiện \`PubSub\` để cập nhật bộ đệm.
- **Khối lượng Dữ liệu**: Danh sách buồng và dịch vụ thường chứa từ 100-1000 mục.

Nguồn: [[\`.devin/wiki.json:46-52\`](../../../../.devin/wiki.json#L46-L52)](../../../../.devin/wiki.json#L46-L52), [[\`.devin/wiki.json:235-237\`](../../../../.devin/wiki.json#L235-L237)](../../../../.devin/wiki.json#L235-L237)

---

## Các Ví dụ Tích hợp Plugin

### Tích hợp Plugin Đăng ký

Các plugin đăng ký sử dụng đồng thời nhiều UC Dịch vụ & Buồng phòng:

\`\`\`
HIS.Desktop.Plugins.Register (81-102 tệp)
├── Sử dụng UC ServiceRoom để chọn phòng
├── Sử dụng UC National cho quốc tịch bệnh nhân
├── Sử dụng UC Sick để ghi lại tình trạng ban đầu
└── Sử dụng UC RoomExamService cho các dịch vụ khám có sẵn
\`\`\`

Quy trình đăng ký thường:
1. Tải buồng mặc định từ các buồng được gán cho người dùng.
2. Hiển thị các dịch vụ khám có sẵn cho buồng đã chọn.
3. Thu thập quốc tịch bệnh nhân trong quá trình nhập dữ liệu nhân khẩu học.
4. Ghi lại các tình trạng bệnh tật ban đầu nếu là đăng ký cấp cứu.
5. Xác thực tất cả các lựa chọn trước khi tạo hồ sơ điều trị.

### Tích hợp Plugin Khám bệnh

Các plugin khám bệnh phụ thuộc nhiều vào các UC này:

\`\`\`
Các plugin HIS.Desktop.Plugins.Exam*
├── UC RoomExamService - Hiển thị các dịch vụ có sẵn trong phòng khám hiện tại
├── UC Sick - Ghi lại các tình trạng bệnh nhân trong quá trình khám
├── UC ServiceUnit - Chọn đơn vị đo lường cho các kết quả xét nghiệm
└── UC ServiceRoomInfo - Hiển thị khả năng và lịch trình của phòng
\`\`\`

### Tích hợp Thực hiện Dịch vụ

Các plugin thực hiện dịch vụ sử dụng:

\`\`\`
HIS.Desktop.Plugins.ServiceExecute (119 tệp)
├── UC ServiceRoom - Xác minh dịch vụ có thể được thực hiện trong buồng hiện tại
├── UC ServiceUnit - Ghi lại kết quả với các đơn vị phù hợp
└── UC RoomExamService - Tải cấu hình phòng thực hiện
\`\`\`

Nguồn: [[\`.devin/wiki.json:70-77\`](../../../../.devin/wiki.json#L70-L77)](../../../../.devin/wiki.json#L70-L77), [[\`.devin/wiki.json:235-237\`](../../../../.devin/wiki.json#L235-L237)](../../../../.devin/wiki.json#L235-L237)

---

## Tóm tắt

Các thành phần UC Dịch vụ & Buồng phòng cung cấp các chức năng UI có thể tái sử dụng thiết yếu cho:

1. **Quản lý Dịch vụ-Buồng phòng**: Ánh xạ dịch vụ tới các buồng vật lý (\`ServiceRoom\`, \`RoomExamService\`)
2. **Cấu hình Dịch vụ**: Quản lý đơn vị dịch vụ và các phép đo (\`ServiceUnit\`, \`ServiceRoomInfo\`)
3. **Dữ liệu Bệnh nhân**: Theo dõi quốc tịch và tình trạng bệnh tật (\`National\`, \`Sick\`)

261 tệp này (tổng cộng) tạo thành một phần quan trọng của thư viện UC 131 thành phần, cho phép quản lý dịch vụ và buồng phòng nhất quán trên 956 plugin HIS. Chúng tuân theo các mô hình UC tiêu chuẩn (Processor, ADO, sự kiện) và tích hợp với hệ thống đệm \`BackendData\` để đạt hiệu năng cao.

Để biết thêm thông tin về các UC liên quan, hãy xem:
- Các UC thuốc và vật tư: [1.3.3](#1.3.3)
- Các UC bệnh nhân và điều trị: [1.3.2](#1.3.2)
- Form engine: [1.3.1](#1.3.1)

Nguồn: [[\`.devin/wiki.json:200-207\`](../../../../.devin/wiki.json#L200-L207)](../../../../.devin/wiki.json#L200-L207), [[\`.devin/wiki.json:235-237\`](../../../../.devin/wiki.json#L235-L237)](../../../../.devin/wiki.json#L235-L237)