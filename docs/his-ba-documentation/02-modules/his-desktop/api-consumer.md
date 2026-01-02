## Mục đích và Phạm vi

Lớp API Consumer cung cấp một sự trừu tượng hóa tiêu chuẩn cho tất cả việc truyền thông API backend trong ứng dụng HIS Desktop. Tọa lạc tại `HIS.Desktop.ApiConsumer/` (13 tệp), lớp này xử lý các lời gọi REST API, xử lý phản hồi, xử lý lỗi và các luồng xác thực giữa client desktop và các dịch vụ backend.

Tài liệu này đề cập đến hạ tầng truyền thông API. Để biết thông tin về cách các phản hồi API được đệm cục bộ, hãy xem [LocalStorage & Cấu hình](../../02-modules/his-desktop/core.md). Để biết chi tiết về cách các plugin sử dụngThiết kế để hoạt động liền mạch trong [Kiến trúc Plugin HIS](../01-architecture/plugin-system/01-overview.md).

---

## Tổng quan Kiến trúc

Lớp API Consumer hoạt động như một bên trung gian giữa lớp hiển thị (plugin) và các dịch vụ REST backend, được xây dựng trên nền tảng \`Inventec.Common.WebApiClient\`.

\`\`\`mermaid
graph TB
    subgraph "Lớp_Hiển_thị"
        Plugins["HIS.Desktop.Plugins.*<br/>956 Plugin Nghiệp vụ"]
        UC["Các thành phần UC<br/>131 User Control"]
    end
    
    subgraph "Lớp_API_Consumer_-_HIS.Desktop.ApiConsumer"
        ApiConsumer["HIS.Desktop.ApiConsumer<br/>13 tệp<br/>Các API client cho từng dịch vụ"]
        CommonParam["CommonParam<br/>Siêu dữ liệu yêu cầu<br/>Phiên, Ngôn ngữ, v.v."]
        ApiParam["ApiParam<br/>Wrapper yêu cầu"]
    end
    
    subgraph "Lớp_Truyền_thông_HTTP"
        WebApiClient["Inventec.Common.WebApiClient<br/>Lớp ApiConsumer<br/>Hạ tầng HTTP"]
        HttpClient["System.Net.Http.HttpClient<br/>HTTP cấp thấp"]
    end
    
    subgraph "Dịch_vụ_Backend"
        HisAPI["HIS API<br/>Các dịch vụ bệnh viện cốt lõi"]
        AcsAPI["ACS API<br/>Kiểm soát truy cập"]
        EmrAPI["EMR API<br/>Hồ sơ bệnh án"]
        LisAPI["LIS API<br/>Xét nghiệm"]
        SarAPI["SAR API<br/>Báo cáo"]
    end
    
    Plugins --> ApiConsumer
    UC --> ApiConsumer
    ApiConsumer --> WebApiClient
    WebApiClient --> HttpClient
    HttpClient --> HisAPI
    HttpClient --> AcsAPI
    HttpClient --> EmrAPI
    HttpClient --> LisAPI
    HttpClient --> SarAPI
    
    ApiConsumer -.->|"Sử dụng"| CommonParam
    ApiConsumer -.->|"Bao gói dữ liệu trong"| ApiParam
\`\`\`

**Nguồn:** [[\`Common/Inventec.Aup.Client/Inventec.Aup.Client/ApiConsumerStore.cs:1-32\`](../../../../Common/Inventec.Aup.Client/Inventec.Aup.Client/ApiConsumerStore.cs#L1-L32)], [[\`Common/Inventec.Aup.Client/Inventec.Aup.Client/FileUpload.cs:1-150\`](../../../../Common/Inventec.Aup.Client/Inventec.Aup.Client/FileUpload.cs#L1-L150)]

---

## Các Thành phần Cốt lõi

### Cấu trúc HIS.Desktop.ApiConsumer

Thư mục \`HIS.Desktop.ApiConsumer/\` chứa các lớp API client dành riêng cho từng dịch vụ, bao gói việc truyền thông với các miền API backend khác nhau:

| Thành phần | Mô tả |
|-----------|-------------|
| **HIS API Consumers** | Các lớp client cho các hoạt động hệ thống thông tin bệnh viện cốt lõi (điều trị, đơn thuốc, thanh toán) |
| **ACS API Consumer** | Client cho các dịch vụ kiểm soát truy cập và xác thực |
| **EMR API Consumer** | Client cho các hoạt động hồ sơ bệnh án điện tử |
| **LIS API Consumer** | Client cho tích hợp hệ thống thông tin xét nghiệm |
| **SAR API Consumer** | Client cho các dịch vụ mẫu báo cáo và tạo báo cáo |
| **SDA API Consumer** | Client cho quản trị dữ liệu hệ thống |

Mỗi lớp consumer tuân theo một mô hình nhất quán là bao gói các endpoint dịch vụ và cung cấp các lời gọi phương thức có kiểu dữ liệu mạnh.

### Tích hợp Inventec.Common.WebApiClient

Nền tảng của tất cả việc truyền thông API là lớp \`Inventec.Common.WebApiClient.ApiConsumer\`, cung cấp:

- **Cấu hình Base URI**: Mỗi API consumer được khởi tạo với một URI cơ sở cụ thể.
- **Header Mã Ứng dụng**: Tự động chèn \`APP_CODE\` để định danh dịch vụ.
- **Quản lý Timeout**: Các thiết lập thời gian chờ HTTP có thể cấu hình.
- **Tuần tự hóa JSON**: Tự động tuần tự hóa/giải tuần tự hóa sử dụng Newtonsoft.Json.

Ví dụ về mô hình khởi tạo:

\`\`\`csharp
// Từ ApiConsumerStore.cs
internal static Inventec.Common.WebApiClient.ApiConsumer AupConsumer 
{ 
    get { return new Inventec.Common.WebApiClient.ApiConsumer(
        AupConstant.BASE_URI, 
        AupConstant.APP_CODE
    ); } 
}
\`\`\`

**Nguồn:** [[\`Common/Inventec.Aup.Client/Inventec.Aup.Client/ApiConsumerStore.cs:27-30\`](../../../../Common/Inventec.Aup.Client/Inventec.Aup.Client/ApiConsumerStore.cs#L27-L30)]

---

## Các Mô hình Truyền thông API

### Kiến trúc Luồng Yêu cầu

\`\`\`mermaid
sequenceDiagram
    participant Plugin as "Plugin<br/>(vd: AssignPrescriptionPK)"
    participant Consumer as "HIS.Desktop.ApiConsumer<br/>(vd: HisServiceReqConsumer)"
    participant WebApi as "Inventec.Common.WebApiClient<br/>ApiConsumer"
    participant Http as "HttpClient"
    participant Backend as "Backend API<br/>(vd: /api/HisServiceReq/Create)"
    
    Plugin->>Consumer: Gọi phương thức API<br/>(vd: CreateServiceReq)
    Consumer->>Consumer: Xây dựng ApiParam<br/>với CommonParam + dữ liệu
    Consumer->>WebApi: PostAsJsonAsync(uri, apiParam)
    WebApi->>Http: Yêu cầu POST với các header<br/>(APP_CODE, Session, v.v.)
    Http->>Backend: HTTP POST với body JSON
    Backend-->>Http: Phản hồi HTTP (200/4xx/5xx)
    Http-->>WebApi: HttpResponseMessage
    WebApi->>WebApi: Giải tuần tự hóa JSON<br/>thành ApiResultObject<T>
    WebApi-->>Consumer: Trả về ApiResultObject<T>
    Consumer->>Consumer: Kiểm tra trạng thái thành công<br/>Xử lý lỗi
    Consumer-->>Plugin: Trả về kết quả có kiểu<br/>hoặc ném ngoại lệ
\`\`\`

**Nguồn:** [[\`Common/Inventec.Aup.Client/Inventec.Aup.Client/FileUpload.cs:43-93\`](../../../../Common/Inventec.Aup.Client/Inventec.Aup.Client/FileUpload.cs#L43-L93)]

### Cấu trúc ApiParam

Tất cả các yêu cầu API đều được bao gói trong một đối tượng \`ApiParam\` tiêu chuẩn:

\`\`\`csharp
// Mô hình wrapper yêu cầu được sử dụng xuyên suốt ApiConsumer
Inventec.Common.WebApiClient.ApiParam apiParam = 
    new Inventec.Common.WebApiClient.ApiParam();

apiParam.CommonParam = new CommonParam(); // Siêu dữ liệu phiên, người dùng, ngôn ngữ
apiParam.ApiData = dataObject;            // Dữ liệu nghiệp vụ thực tế
\`\`\`

Đối tượng \`CommonParam\` mang theo siêu dữ liệu yêu cầu xuyên suốt:
- **Session Token**: Token xác thực người dùng.
- **Language Code**: Tùy chọn ngôn ngữ (vd: "vi", "en").
- **User Context**: ID người dùng hiện tại và quyền hạn.
- **Branch/Location**: Ngữ cảnh cơ sở và bộ phận.

**Nguồn:** [[\`Common/Inventec.Aup.Client/Inventec.Aup.Client/FileUpload.cs:54-62\`](../../../../Common/Inventec.Aup.Client/Inventec.Aup.Client/FileUpload.cs#L54-L62)]

### Xử lý Phản hồi

Các phản hồi API tuân theo mô hình \`ApiResultObject<T>\`:

| Thuộc tính | Kiểu | Mô tả |
|----------|------|-------------|
| **Data** | \`T\` | Nội dung phản hồi thành công (null nếu lỗi) |
| **Param** | \`CommonParam\` | Siêu dữ liệu phản hồi bao gồm thông điệp và lỗi |
| **Success** | \`bool\` | Cho biết thao tác có thành công hay không |

Mô hình xử lý phản hồi:

\`\`\`csharp
// Luồng xử lý phản hồi điển hình
if (message.IsSuccessStatusCode)
{
    string jsonString = message.Content.ReadAsStringAsync().Result;
    if (!String.IsNullOrWhiteSpace(jsonString))
    {
        var rsData = Newtonsoft.Json.JsonConvert
            .DeserializeObject<ApiResultObject<T>>(jsonString);
        return rsData != null ? rsData.Data : null;
    }
}
\`\`\`

**Nguồn:** [[\`Common/Inventec.Aup.Client/Inventec.Aup.Client/FileUpload.cs:68-76\`](../../../../Common/Inventec.Aup.Client/Inventec.Aup.Client/FileUpload.cs#L68-L76)]

---

## Quản lý Xác thực và Phiên làm việc

### Luồng Xác thực

\`\`\`mermaid
graph LR
    subgraph "Xác_thực_Ban_đầu"
        Login["Plugin Đăng nhập<br/>Thông tin đăng nhập"]
        Auth["ACS API Consumer<br/>/api/AcsUser/Login"]
        Session["Token Phiên<br/>Lưu trong LocalStorage"]
    end
    
    subgraph "Các_lời_gọi_API_Tiếp_theo"
        Plugin["Bất kỳ Plugin nào<br/>Hoạt động nghiệp vụ"]
        Consumer["API Consumer<br/>Chèn token phiên"]
        Backend["Backend API<br/>Xác thực phiên"]
    end
    
    Login --> Auth
    Auth --> Session
    Session -.->|"Đọc token"| Consumer
    Plugin --> Consumer
    Consumer --> Backend
    Backend -.->|"401 Unauthorized"| Session
    Session -.->|"Kích hoạt đăng nhập lại"| Login
\`\`\`

### Chèn Token Phiên

Mỗi yêu cầu API bao gồm token phiên trong các header yêu cầu. Token được quản lý bởi \`HIS.Desktop.LocalStorage.ConfigApplication\` và được tự động chèn bởi lớp API Consumer.

**Vòng đời Token:**
1. Người dùng đăng nhập qua API ACS Login.
2. Token phiên được lưu trong \`LocalStorage.ConfigApplication\`.
3. Tất cả các lời gọi API tiếp theo đọc token từ LocalStorage.
4. Token được tự động bao gồm trong các header yêu cầu.
5. Khi nhận phản hồi 401 Unauthorized, kích hoạt quy trình xác thực lại.

**Nguồn:** [[\`Common/Inventec.Aup.Client/Inventec.Aup.Client/FileUpload.cs:47-52\`](../../../../Common/Inventec.Aup.Client/Inventec.Aup.Client/FileUpload.cs#L47-L52)], [[\`.devin/wiki.json:45-52\`](../../../../.devin/wiki.json#L45-L52)]

---

## Các Mô hình Xử lý Lỗi

### Xử lý Mã trạng thái HTTP

Lớp API Consumer phân biệt giữa các danh mục lỗi khác nhau:

| Mã trạng thái | Chiến lược Xử lý |
|-------------|-------------------|
| **200-299** | Thành công - giải tuần tự hóa body phản hồi |
| **400** | Bad Request - hiển thị lỗi xác thực cho người dùng |
| **401** | Unauthorized - kích hoạt luồng xác thực lại |
| **403** | Forbidden - hiển thị thông báo từ chối quyền truy cập |
| **404** | Not Found - thực thể không tồn tại |
| **500-599** | Server Error - ghi log lỗi, hiển thị thông báo chung |
| **Timeout** | Hết thời gian mạng - cơ chế thử lại |

### Mô hình Xử lý Ngoại lệ

\`\`\`csharp
// Mô hình xử lý ngoại lệ tiêu chuẩn
try
{
    // Thực hiện lời gọi API
    using (HttpResponseMessage message = client.PostAsJsonAsync(uri, apiParam).Result)
    {
        if (message.IsSuccessStatusCode)
        {
            // Xử lý phản hồi thành công
        }
        else
        {
            throw new CustomApiException(message.StatusCode, message.ReasonPhrase);
        }
    }
}
catch (CustomApiException ex)
{
    // Lỗi API cụ thể - chuyển tiếp tới bên gọi
    throw ex;
}
catch (Exception ex)
{
    // Lỗi không ngờ tới - bao gói và ghi log
    throw new CustomApiException("Ngoại lệ khi gọi API", ex);
}
\`\`\`

**Nguồn:** [[\`Common/Inventec.Aup.Client/Inventec.Aup.Client/FileUpload.cs:84-92\`](../../../../Common/Inventec.Aup.Client/Inventec.Aup.Client/FileUpload.cs#L84-L92)]

---

## Cấu hình HTTP Client

### Cấu hình Timeout

Các lời gọi API sử dụng các timeout có thể cấu hình để ngăn chặn việc bị treo vô hạn:

\`\`\`csharp
client.Timeout = new TimeSpan(0, 0, AupConstant.TIME_OUT); // giây
\`\`\`

Các giá trị timeout mặc định được lưu trữ trong các hằng số cấu hình và có thể được điều chỉnh cho từng miền API (vd: timeout lâu hơn cho việc tạo báo cáo, ngắn hơn cho các truy vấn đơn giản).

### Header Yêu cầu

Các header tiêu chuẩn được chèn vào tất cả các yêu cầu:

| Header | Mục đích | Ví dụ |
|--------|---------|---------|
| **APP_CODE** | Định danh ứng dụng | \`"HIS_DESKTOP"\` |
| **CLIENT_CODE** | Mã client/cơ sở | \`"HOSPITAL_001"\` |
| **Authorization** | Token Bearer | \`"Bearer {session_token}"\` |
| **Accept-Language** | Ngôn ngữ | \`"vi-VN"\` |
| **Content-Type** | Định dạng yêu cầu | \`"application/json"\` |

**Nguồn:** [[\`Common/Inventec.Aup.Client/Inventec.Aup.Client/FileUpload.cs:47-52\`](../../../../Common/Inventec.Aup.Client/Inventec.Aup.Client/FileUpload.cs#L47-L52)], [[\`Common/Inventec.Aup.Client/Inventec.Aup.Client/FileUpload.cs:110-115\`](../../../../Common/Inventec.Aup.Client/Inventec.Aup.Client/FileUpload.cs#L110-L115)]

---

## Tích hợp với LocalStorage

### Chiến lược Đệm Phản hồi

\`\`\`mermaid
graph TB
    Plugin["Plugin thực hiện lời gọi API"]
    Consumer["API Consumer"]
    Cache{"Kiểm tra bộ đệm<br/>BackendData trong LocalStorage"}
    API["Gọi Backend API"]
    Store["Lưu vào BackendData"]
    Return["Trả về cho Plugin"]
    
    Plugin --> Consumer
    Consumer --> Cache
    Cache -->|"Cache HIT"| Return
    Cache -->|"Cache MISS"| API
    API --> Store
    Store --> Return
\`\`\`

Lớp API Consumer tích hợp với \`HIS.Desktop.LocalStorage.BackendData\` để triển khai việc đệm phía client:

**Các danh mục dữ liệu được đệm:**
- **Dữ liệu Tham chiếu**: Dữ liệu hệ thống ít thay đổi (kiểu thuốc, khoa, phòng).
- **Cấu hình**: Các tùy chọn hệ thống và người dùng.
- **Ngữ cảnh Người dùng**: Quyền và vai trò của người dùng hiện tại.

**Làm mới bộ đệm:**
- Xóa bộ đệm rõ ràng khi thực hiện các thao tác sửa đổi dữ liệu.
- Hết hạn dựa trên thời gian cho các dữ liệu dễ biến động.
- Vô hiệu hóa dựa trên phiên bản cho các thay đổi cấu hình.

Để biết chi tiết về cơ chế đệm, hãy xem [LocalStorage & Cấu hình](../../02-modules/his-desktop/core.md).

**Nguồn:** [[\`.devin/wiki.json:45-52\`](../../../../.devin/wiki.json#L45-L52)]

---

## Ví dụ Sử dụng trong Plugin

### Ví dụ 1: Tạo một Yêu cầu Dịch vụ

\`\`\`
// Mô hình gọi API plugin điển hình
// Từ bất kỳ plugin nào trong HIS.Desktop.Plugins.*

// 1. Chuẩn bị đối tượng dữ liệu
var serviceReqData = new ServiceReqSDO 
{
    PatientId = currentPatient.Id,
    TreatmentId = currentTreatment.Id,
    ServiceIds = selectedServices
};

// 2. Gọi thông qua ApiConsumer
var apiConsumer = new HisServiceReqConsumer();
var result = apiConsumer.Create(serviceReqData);

// 3. Xử lý kết quả
if (result != null && result.Data != null)
{
    // Thành công - cập nhật UI
    RefreshServiceRequestList();
}
else
{
    // Lỗi - hiển thị thông điệp từ result.Param
    MessageBox.Show(result.Param.Messages);
}
\`\`\`

### Ví dụ 2: Mô hình Tải tệp lên (File Upload)

Chức năng tải tệp lên trình bày việc truyền thông API chuyên biệt cho dữ liệu nhị phân:

\`\`\`csharp
// Tải các tệp lên máy chủ cập nhật
List<FileUploadInfo> files = new List<FileUploadInfo> 
{
    new FileUploadInfo { Url = "path/to/file.dll" },
    new FileUploadInfo { Url = "path/to/config.xml" }
};

var uploadedFiles = FileUpload.UploadFile(
    clientCode: "HIS_DESKTOP",
    files: files,
    baseUri: updateServerUri
);

foreach (var file in uploadedFiles) 
{
    // Xử lý thông tin tệp đã tải lên
    Console.WriteLine($"Đã tải lên: {file.Url}");
}
\`\`\`

**Nguồn:** [[\`Common/Inventec.Aup.Client/Inventec.Aup.Client/FileUpload.cs:43-93\`](../../../../Common/Inventec.Aup.Client/Inventec.Aup.Client/FileUpload.cs#L43-L93)], [[\`Common/Inventec.Aup.Client/Inventec.Aup.Client/FileUpload.cs:106-146\`](../../../../Common/Inventec.Aup.Client/Inventec.Aup.Client/FileUpload.cs#L106-L146)]

---

## Các Consumer API dành riêng cho Dịch vụ

### Các Lớp Consumer theo Miền

Thư mục \`HIS.Desktop.ApiConsumer/\` chứa các lớp consumer chuyên biệt cho mỗi miền dịch vụ backend:

| Lớp Consumer | Miền API Backend | Các Hoạt động Chính |
|----------------|-------------------|----------------|
| **HisServiceReqConsumer** | \`/api/HisServiceReq/*\` | Tạo, Cập nhật, Xóa yêu cầu dịch vụ |
| **HisTreatmentConsumer** | \`/api/HisTreatment/*\` | CRUD điều trị, thay đổi trạng thái |
| **HisPatientConsumer** | \`/api/HisPatient/*\` | Đăng ký bệnh nhân, cập nhật |
| **HisPrescriptionConsumer** | \`/api/HisPrescription/*\` | Quản lý đơn thuốc |
| **AcsUserConsumer** | \`/api/AcsUser/*\` | Xác thực, quản lý người dùng |
| **EmrDocumentConsumer** | \`/api/EmrDocument/*\` | Các thao tác hồ sơ bệnh án |
| **LisSampleConsumer** | \`/api/LisSample/*\` | Theo dõi mẫu xét nghiệm |
| **SarReportConsumer** | \`/api/SarReport/*\` | Các yêu cầu tạo báo cáo |

Mỗi consumer bao gói:
- Xây dựng URI endpoint.
- Ánh xạ kiểu yêu cầu/phản hồi.
- Xử lý lỗi dành riêng cho miền.
- Logic thử lại cho các thao tác quan trọng.

**Nguồn:** [[\`.devin/wiki.json:55-57\`](../../../../.devin/wiki.json#L55-L57)]

---

## Các Lưu ý về Hiệu năng

### Connection Pooling

Các instance \`HttpClient\` được quản lý thông qua mô hình factory của \`ApiConsumer\` để tận dụng connection pooling và tránh cạn kiệt socket.

### Mô hình Async/Await

Mặc dù triển khai hiện tại sử dụng \`.Result\` cho các thao tác đồng bộ:

\`\`\`csharp
using (HttpResponseMessage message = client.PostAsJsonAsync(uri, apiParam).Result)
\`\`\`

Mô hình này được sử dụng xuyên suốt ứng dụng desktop để duy trì luồng thực thi plugin đồng bộ. Việc tái cấu trúc hiện đại có thể áp dụng các mô hình async/await để cải thiện khả năng phản hồi.

### Gom nhóm Yêu cầu (Request Batching)

Đối với các thao tác yêu cầu nhiều lời gọi API, lớp API Consumer hỗ trợ các yêu cầu gom nhóm để giảm thiểu số lượt mạng. Tuy nhiên, các plugin riêng lẻ chịu trách nhiệm triển khai logic gom nhóm ở lớp nghiệp vụ.

**Nguồn:** [[\`Common/Inventec.Aup.Client/Inventec.Aup.Client/FileUpload.cs:66-81\`](../../../../Common/Inventec.Aup.Client/Inventec.Aup.Client/FileUpload.cs#L66-L81)]

---

## Ghi nhật ký và Chẩn đoán

Tất cả các lời gọi API đều được trang bị ghi nhật ký thông qua \`Inventec.Common.Logging.LogSystem\`:

\`\`\`csharp
Inventec.Common.Logging.LogSystem.Debug(
    "clientCode=" + clientCode + "____" + 
    Inventec.Common.Logging.LogUtil.TraceData(
        Inventec.Common.Logging.LogUtil.GetMemberName(() => BASE_URI), 
        BASE_URI
    )
);
\`\`\`

**Thông tin được ghi nhật ký:**
- URI yêu cầu và tham số.
- Thời gian yêu cầu/phản hồi.
- Chi tiết lỗi và dấu vết ngăn xếp (stack trace).
- Ngữ cảnh phiên làm việc.

Để biết chi tiết về hệ thống ghi nhật ký, hãy xem [Thư viện Chung Inventec](../../02-modules/common-libraries/libraries.md#inventec-common).

**Nguồn:** [[\`Common/Inventec.Aup.Client/Inventec.Aup.Client/FileUpload.cs:60-64\`](../../../../Common/Inventec.Aup.Client/Inventec.Aup.Client/FileUpload.cs#L60-L64)]

---

## Tóm tắt

Lớp API Consumer cung cấp một giao diện mạnh mẽ, tiêu chuẩn hóa cho tất cả các việc truyền thông backend trong ứng dụng HIS Desktop. Các đặc điểm chính:

- **Truyền thông Tập trung**: Tất cả các lời gọi API chảy qua \`HIS.Desktop.ApiConsumer/\` (13 tệp).
- **An toàn Kiểu dữ liệu**: Các đối tượng yêu cầu/phản hồi có kiểu dữ liệu mạnh thông qua \`ApiParam\` và \`ApiResultObject<T>\`.
- **Quản lý Phiên**: Tự động chèn các token xác thực.
- **Xử lý Lỗi**: Xử lý ngoại lệ và giải thích mã trạng thái nhất quán.
- **Nền tảng**: Được xây dựng trên \`Inventec.Common.WebApiClient.ApiConsumer\`.
- **Tích hợp**: Kết nối liền mạch với lớp đệm LocalStorage.

Kiến trúc này cho phép 956 plugin nghiệp vụ có thể truyền thông với các dịch vụ backend mà không cần trực tiếp triển khai logic truyền thông HTTP.