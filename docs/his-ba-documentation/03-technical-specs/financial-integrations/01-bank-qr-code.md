# Bank QR Code (`Inventec.Common.BankQrCode`)

## 1. Giới thiệu

`Inventec.Common.BankQrCode` là thư viện dùng để sinh chuỗi dữ liệu (Data Payload) và hình ảnh QR Code cho mục đích thanh toán chuyển khoản. Thư viện hỗ trợ cả chuẩn VietQR chung và các chuẩn đặc thù (Legacy) của một số ngân hàng lớn liên kết với bệnh viện.

## 2. Các Provider Hỗ trợ

Thư viện được thiết kế theo Factory Pattern để hỗ trợ nhiều ngân hàng khác nhau thông qua Enum `ProvinceType`.

| Ngân hàng | Class xử lý | Ghi chú |
|:---|:---|:---|
| **BIDV** | `BIDVProcessor` | Ngân hàng Đầu tư và Phát triển Việt Nam. |
| **VIETINBANK** | `VietinBankProcessor` | Ngân hàng Công thương Việt Nam. |
| **PVCB** | `PvcomBankProcessor` | Ngân hàng TMCP Đại Chúng Việt Nam. |
| **LPBANK** | `LPBankProcessor` | Ngân hàng Lộc Phát Việt Nam (LienVietPostBank). |

## 3. Kiến trúc Kỹ thuật

### 3.1. Interface `IRun`
Mọi Processor ngân hàng đều implement interface `IRun` để chuẩn hóa đầu ra.

```csharp
public interface IRun
{
    // Tạo QR thanh toán hóa đơn
    ResultQrCode Run(); 
    
    // Tạo QR định danh khách hàng (Consumer)
    ResultQrCode RunConsumer(); 
}
```

### 3.2. Processor Logic (`BankQrCodeProcessor`)
Class `BankQrCodeProcessor` đóng vai trò là Factory/Facade. Nó nhận vào dữ liệu đầu vào (`BankQrCodeInputADO`) và loại ngân hàng (`ProvinceType`), sau đó khởi tạo Processor tương ứng.

```csharp
public ResultQrCode GetQrCode(ProvinceType type)
{
    IRun iRunQrData = null;
    switch (type)
    {
        case ProvinceType.BIDV:
            iRunQrData = new BIDVProcessor(this.InputData);
            break;
        case ProvinceType.VIETINBANK:
            iRunQrData = new VietinBankProcessor(this.InputData);
            break;
        // ...
    }
    return iRunQrData.Run();
}
```

### 3.3. Dữ liệu Đầu vào (`BankQrCodeInputADO`)
Để tạo QR, cần cung cấp các thông tin sau:
*   `Amount`: Số tiền cần thanh toán.
*   `TransactionCode`: Mã giao dịch (thường là Mã phiếu thu hoặc Mã hồ sơ điều trị) để định danh khoản thu.
*   `SystemConfig`: Cấu hình hệ thống chứa số tài khoản thụ hưởng.
*   `ConsumerInfo`: Thông tin khách hàng (nếu tạo QR định danh).

## 4. Quy trình Tích hợp

1.  **Chuẩn bị dữ liệu**: Lấy thông tin số tiền, mã phiếu thu từ module Viện phí.
2.  **Khởi tạo Input**:
    ```csharp
    var input = new BankQrCodeInputADO {
        Amount = 150000,
        TransactionCode = "TU123456",
        // ...
    };
    ```
3.  **Gọi Processor**:
    ```csharp
    var processor = new BankQrCodeProcessor(input);
    var qrResult = processor.GetQrCode(ProvinceType.BIDV); // Hoặc lấy từ Config
    ```
4.  **Hiển thị**: Kết quả `qrResult` chứa chuỗi QrContent. Dùng thư viện `Inventec.Common.QRCoder` để vẽ chuỗi này thành hình ảnh và hiển thị lên Phiếu thu/Màn hình.
