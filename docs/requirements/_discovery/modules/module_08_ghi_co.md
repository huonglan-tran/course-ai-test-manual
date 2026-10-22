# Module 08 — Credit Notes (`CRN`)

> ← [Bản đồ hệ thống](../system_map.md) · Trạng thái recon: [`README.md`](../../README.md) · Tầng khám phá — **không** chứa mã REQ.

| Thuộc tính | Giá trị |
|---|---|
| Tên UI | Credit Notes (menu Sales) |
| Route | Danh sách `/admin/credit_notes` · tạo mới `/admin/credit_notes/credit_note` |
| Loại màn hình | Danh sách + Form |
| Nút thanh công cụ | `New Credit Note` |
| Cột bảng danh sách | `Credit Note #` · `Credit Note Date` · `Customer` · `Status` · `Project` · `Reference #` · `Amount` · `Remaining Amount` |
| Form tạo mới | 43 field nhập (đếm DOM) |
| Status flow | ✅ Có cột `Status` — danh sách trạng thái chưa thấy |
| Phụ thuộc | `CUS` · `PRJ` · `ITEM` · ⚠️ áp vào `INV` (cột `Remaining Amount`; báo cáo Sales có `Applied Credits` · `Refunded Amount`) |
| Risk | 🟡 Trung bình — tiền, nhưng ít dùng hơn hoá đơn; số dư còn lại phải khớp khi áp vào nhiều hoá đơn |
| Ước REQ | 25–35 |

## Vùng chưa xác minh

| Vùng | Lý do |
|---|---|
| Danh sách trạng thái · luồng áp ghi có vào hoá đơn · hoàn tiền | Chưa thao tác |

## Evidence

> Ảnh chụp 12-10-2026 (Playwright MCP, viewport 1600×780), đã mở lại xác nhận.

| Ảnh | Màn hình | Trạng thái chụp | Phạm vi |
|---|---|---|---|
| [credit_notes_list_viewport.png](../evidence/credit_notes_list_viewport.png) | `/admin/credit_notes` | Danh sách **rỗng** — `No entries found` | Viewport |
