# Module 04 — Items (`ITEM`)

> ← [Bản đồ hệ thống](../system_map.md) · Trạng thái recon: [`README.md`](../../README.md) · Tầng khám phá — **không** chứa mã REQ.

| Thuộc tính | Giá trị |
|---|---|
| Tên UI | Items (menu Sales) · tiêu đề trang `Invoice Items` |
| Route | `/admin/invoice_items` |
| Loại màn hình | Danh sách + modal |
| Nút thanh công cụ | `New Item` · `Import Items` · `Groups` |
| Cột bảng danh sách | (checkbox) · `Description` · `Long Description` · `Rate` · `Tax 1` · `Tax 2` · `Unit` · `Group Name` |
| CRUD | Tạo/sửa qua modal (dự kiến) · Import · quản lý Group |
| Status flow | Không |
| Phụ thuộc | Thuế (Setup — bị chặn) · được dùng làm dòng hàng của `INV` · `EST` · `PROP` · `CRN` |
| Risk | 🟡 Trung bình — CRUD thường nhưng là nguồn giá/thuế cho mọi chứng từ Sales |
| Ước REQ | 15–20 |

## Vùng chưa xác minh

| Vùng | Lý do |
|---|---|
| Field modal, validation `Rate` | Chưa mở modal |
| Danh sách thuế lựa chọn | Taxes nằm ở Setup (`AMB-SYS-01`) |
| Import Items | Chưa mở |

## Evidence

> Ảnh chụp 12-10-2026 (Playwright MCP, viewport 1600×780), đã mở lại xác nhận.

| Ảnh | Màn hình | Trạng thái chụp | Phạm vi |
|---|---|---|---|
| [items_list_viewport.png](../evidence/items_list_viewport.png) | `/admin/invoice_items` | Danh sách có dữ liệu (86 bản ghi) · nút New Item · Import Items · Groups | Viewport |
