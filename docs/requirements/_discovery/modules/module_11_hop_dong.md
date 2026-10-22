# Module 11 — Contracts (`CTR`)

> ← [Bản đồ hệ thống](../system_map.md) · Trạng thái recon: [`README.md`](../../README.md) · Tầng khám phá — **không** chứa mã REQ.

| Thuộc tính | Giá trị |
|---|---|
| Tên UI | Contracts |
| Route | Danh sách `/admin/contracts` · tạo mới `/admin/contracts/contract` · chi tiết `/admin/contracts/contract/{id}` |
| Loại màn hình | Danh sách + Form + chi tiết nhiều tab · có biểu đồ `Contracts by Type` · `Contracts Value by Type` |
| Bộ đếm | `Active` · `Expired` · `About to Expire` · `Recently Added` · `Trash` |
| Nút thanh công cụ | `New Contract` |
| Cột bảng danh sách | `#` · `Subject` · `Customer` · `Contract Type` · `Contract Value` · `Start Date` · `End Date` · `Project` · `Signature` |
| Form tạo mới | 10 field nhập (đếm DOM) · nhãn bắt buộc quan sát được: `Customer` |
| Chi tiết — 7 tab | Contract · Attachments · Comments · Renewal History · Tasks · Notes · Templates |
| Status flow | ✅ Theo ngày (Active / About to Expire / Expired) + Trash + chữ ký |
| Phụ thuộc | `CUS` · `PRJ` · Contract Types (Setup — bị chặn) · ⚠️ ký / bình luận bởi contact qua cổng Customer (thông báo "New comment from customer on contract") |
| Risk | 🟡 Trung bình — trạng thái theo ngày, gia hạn, chữ ký; nhưng form ngắn |
| Ước REQ | 30–40 |

## Vùng chưa xác minh

| Vùng | Lý do |
|---|---|
| Ngưỡng "About to Expire" · luồng gia hạn · Trash/khôi phục | Chưa thao tác; cấu hình ở Settings (bị chặn) |
| Ký hợp đồng | Cổng Customer — ngoài phạm vi |

## Evidence

> Ảnh chụp 12-10-2026 (Playwright MCP, viewport 1600×780), đã mở lại xác nhận.

| Ảnh | Màn hình | Trạng thái chụp | Phạm vi |
|---|---|---|---|
| [contracts_list_viewport.png](../evidence/contracts_list_viewport.png) | `/admin/contracts` | Contract Summary 5 bộ đếm · 2 biểu đồ theo loại hợp đồng (tên loại có dữ liệu rác `Auto Type <timestamp>`) | Viewport — bảng danh sách nằm dưới màn hình đầu |
