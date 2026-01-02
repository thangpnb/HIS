# Danh sách Lệnh Build

## Các lệnh MSBuild cơ bản

```bash
MSBuild.exe Inventec.Common.sln /p:Configuration=Release /p:Platform=AnyCPU
---

MSBuild.exe Inventec.Desktop.sln /p:Configuration=Release /p:Platform=AnyCPU
---

cd HIS/Plugins/HIS.Desktop.Plugins.Register
MSBuild.exe HIS.Desktop.Plugins.Register.csproj /p:Configuration=Debug /t:Rebuild
---

MSBuild.exe HIS.Desktop.csproj /t:Clean
---

MSBuild.exe HIS/HIS.Desktop.sln /t:Clean
MSBuild.exe MPS/MPS.sln /t:Clean
MSBuild.exe UC/HIS.UC.sln /t:Clean
MSBuild.exe Common/Inventec.Common.sln /t:Clean
---

copy bin\Debug\*.dll ..\..\bin\Debug\Plugins\
```

### Cấu hình Debug

Trong quá trình phát triển, hãy sử dụng cấu hình Debug để có các ký hiệu (symbols) hỗ trợ gỡ lỗi tốt hơn:

```bash
MSBuild.exe HIS.Desktop.csproj /p:Configuration=Debug /p:Platform=AnyCPU
```

Bản build Debug bao gồm:
- Toàn bộ file ký hiệu PDB cho tất cả assembly
- Không có tối ưu hóa mã nguồn (giúp việc gỡ lỗi dễ dàng hơn)
- Kích hoạt các xác nhận gỡ lỗi (Debug assertions)
- Kích thước file lớn hơn (~gấp 2 lần bản Release)

### Chu kỳ Phát triển Plugin

Để tìm hiểu về quy trình phát triển plugin mới,Để biết thêm chi tiết về cấu trúc plugin, xem [Tài liệu Hệ thống Plugin](../01-architecture/plugin-system/01-overview.md).

Các bước build plugin điển hình:
1. Tạo project plugin trong thư mục `HIS/Plugins/`
2. Triển khai interface `IModule`
3. Build plugin: [`MSBuild.exe [PluginName].csproj`](../../MSBuild.exe [PluginName].csproj)
4. Copy file DLL vào thư mục `HIS/bin/Debug/Plugins/`
5. Chạy `HIS.Desktop.exe` để kiểm tra

### Clean Build (Dọn dẹp bản build)

Xóa tất cả các file rác phát sinh sau khi build:

```bash
MSBuild.exe HIS.Desktop.sln /t:Clean
```