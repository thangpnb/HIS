# Technical Overview: Financial Integrations (Tích hợp Tài chính)

## 1. Tổng quan Kiến trúc

Phân hệ `financial-integrations` (Tích hợp Tài chính) bao gồm các thư viện và module chịu trách nhiệm giao tiếp với hệ thống Ngân hàng (Banking) và Cổng thanh toán (Payment Gateway) để phục vụ cho luồng thu viện phí không dùng tiền mặt.

### Phạm vi
Hiện tại, module chính trong hệ thống là **Tạo mã VietQR (Bank QR)**. Tính năng này cho phép bệnh nhân quét mã QR trên phiếu chỉ định hoặc phiếu thu để chuyển khoản trực tiếp vào tài khoản bệnh viện với nội dung chuyển khoản được cấu trúc chuẩn hóa.

## 2. Kiến trúc Module

| Tên Module | Namespace | Mô tả |
|:---|:---|:---|
| **Bank QR Code** | `Inventec.Common.BankQrCode` | Tạo mã QR thanh toán theo chuẩn VietQR hoặc chuẩn riêng của từng ngân hàng (BIDV, VietinBank...). |
| **Electronic Bill** | `Inventec.Common.ElectronicBill` | (Xem chi tiết tại [Common Libraries](../common-libraries/02-external-integrations.md)) Tích hợp hóa đơn điện tử. |

## 3. Danh sách Tài liệu

| File Tài liệu | Hạng mục | Mô tả ngắn |
|:---|:---|:---|
| `01-bank-qr-code.md` | **Bank QR Code** | Chi tiết kỹ thuật về thư viện tạo mã QR ngân hàng. |
