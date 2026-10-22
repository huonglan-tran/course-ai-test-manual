# Module 13 — Support (`TKT`)

> ← [Bản đồ hệ thống](../system_map.md) · Trạng thái recon: [`README.md`](../../README.md) · Tầng khám phá — **không** chứa mã REQ.

| Thuộc tính | Giá trị |
|---|---|
| Tên UI | Support (menu) · tiêu đề trang `Support Tickets` |
| Route | Danh sách `/admin/tickets` · tạo mới `/admin/tickets/add` |
| Loại màn hình | Danh sách + Form · biểu đồ `Weekly Ticket Openings Statistics` |
| Bộ đếm trạng thái | `Open` · `In Progress` · `Answered` · `On Hold` · `Closed` (12-10-2026: tất cả = 0) |
| Nút thanh công cụ | `New Ticket` · `Bulk Actions` |
| Cột bảng danh sách | (checkbox) · `#` · `Subject` · `Tags` · `Department` · `Service` · `Contact` · `Status` · `Priority` · `Last Reply` · `Created` |
| Form tạo mới | 15 field nhập (đếm DOM) |
| Status flow | ✅ 5 trạng thái + Priority |
| Phụ thuộc | `CONT` (người gửi) · Departments / Services / Priorities / Statuses / Predefined Replies (Setup — bị chặn) |
| Risk | 🟡 Trung bình — status flow + trả lời qua lại với contact; master data ở Setup |
| Ước REQ | 30–40 |

## Vùng chưa xác minh

| Vùng | Lý do |
|---|---|
| Màn hình chi tiết ticket, trả lời, đổi trạng thái | Danh sách rỗng (0 ticket) — không có bản ghi để mở |
| Email piping, thông báo cho contact | Kiểm tầng tích hợp ❔ |

## Evidence

> Ảnh chụp 12-10-2026 (Playwright MCP, viewport 1600×780), đã mở lại xác nhận.

| Ảnh | Màn hình | Trạng thái chụp | Phạm vi |
|---|---|---|---|
| [tickets_list_viewport.png](../evidence/tickets_list_viewport.png) | `/admin/tickets` | Tickets Summary 5 trạng thái = 0 · danh sách **rỗng** | Viewport |
