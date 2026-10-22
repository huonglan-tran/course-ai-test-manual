# Module 06 — Payments (`PAY`)

> ← [Bản đồ hệ thống](../system_map.md) · Trạng thái recon: [`README.md`](../../README.md) · Tầng khám phá — **không** chứa mã REQ.

| Thuộc tính | Giá trị |
|---|---|
| Tên UI | Payments (menu Sales) |
| Route | `/admin/payments` |
| Loại màn hình | Danh sách — **không** có nút tạo trên trang danh sách |
| Cột bảng danh sách | `Payment #` · `Invoice #` · `Payment Mode` · `Transaction ID` · `Customer` · `Amount` · `Date` |
| CRUD | Xem ✅ · Tạo: dự kiến từ hoá đơn (`Record Payment`) hoặc `Batch Payments` ở `INV` — chưa mở |
| Status flow | Không (trạng thái nằm ở hoá đơn) |
| Phụ thuộc | `INV` · `CUS` · Payment Modes (Setup — bị chặn) |
| Risk | 🔴 Cao — tiền; ghi nhận thanh toán đổi trạng thái hoá đơn (Paid / Partially Paid); hoàn tiền |
| Ước REQ | 15–20 |

## Vùng chưa xác minh

| Vùng | Lý do |
|---|---|
| Nơi ghi nhận thanh toán, field, validation số tiền vượt dư nợ | Chưa mở từ hoá đơn |
| Màn hình chi tiết payment · biên lai PDF | Chưa mở |
| Cổng thanh toán online | Thuộc cổng Customer + Settings — ngoài phạm vi / bị chặn |

## Evidence

> Ảnh chụp 12-10-2026 (Playwright MCP, viewport 1600×780), đã mở lại xác nhận.

| Ảnh | Màn hình | Trạng thái chụp | Phạm vi |
|---|---|---|---|
| [payments_list_viewport.png](../evidence/payments_list_viewport.png) | `/admin/payments` | Danh sách 10 bản ghi · không có nút tạo · Payment Mode quan sát được: `Bank` | Viewport — tên KH là dữ liệu test tự sinh |
