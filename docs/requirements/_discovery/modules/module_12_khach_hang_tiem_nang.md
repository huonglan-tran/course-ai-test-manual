# Module 12 — Leads (`LEAD`)

> ← [Bản đồ hệ thống](../system_map.md) · Trạng thái recon: [`README.md`](../../README.md) · Tầng khám phá — **không** chứa mã REQ.

| Thuộc tính | Giá trị |
|---|---|
| Tên UI | Leads |
| Route | `/admin/leads` (danh sách + chế độ Kanban) · tạo/xem: **modal** |
| Loại màn hình | Danh sách + Kanban + modal |
| Bộ đếm Leads Summary | Theo trạng thái lead (12-10-2026: `Active` · `jjjj` · `Customer`) + `Lost Leads` (%) |
| Nút thanh công cụ | `New Lead` · `Bulk Actions` |
| Cột bảng danh sách | (checkbox) · `#` · `Name` · `Company` · `Email` · `Phone` · `Value` · `Tags` · `Assigned` · `Status` · `Source` · `Last Contact` · `Created` |
| Status flow | ✅ Trạng thái do Setup cấu hình (Lead Statuses — bị chặn) + Lost / Junk (⚠️ theo bộ đếm `Lost Leads`) |
| Phụ thuộc | Lead Sources/Statuses (Setup) · staff (Assigned) · ⚠️ chuyển thành `CUS` · ⚠️ nhận Proposal (`PROP`) · ⚠️ nhận lead từ `ESTR` |
| Risk | 🔴 Cao — chuyển đổi Lead → Customer tạo dữ liệu ở module khác; Kanban; dữ liệu cá nhân; danh sách trạng thái biến động (`AMB-SYS-02`) |
| Ước REQ | 35–45 |

## Vùng chưa xác minh

| Vùng | Lý do |
|---|---|
| Modal lead (field, validation email/phone, trùng lặp) | Chưa mở |
| Luồng chuyển đổi sang Customer | Không ghi dữ liệu khi khám phá |
| Danh sách trạng thái/nguồn đầy đủ | Setup bị chặn (`AMB-SYS-01`) · có trạng thái lạ `jjjj` (`AMB-SYS-02`) |
| Import leads · web-to-lead form | Chưa thấy nút trên trang |

## Evidence

> Ảnh chụp 12-10-2026 (Playwright MCP, viewport 1600×780), đã mở lại xác nhận.

| Ảnh | Màn hình | Trạng thái chụp | Phạm vi |
|---|---|---|---|
| [leads_list_header_clip.png](../evidence/leads_list_header_clip.png) | `/admin/leads` | Nút New Lead · nút chuyển Kanban · hàng tiêu đề 12 cột | Cắt tới hết hàng tiêu đề — **cố ý bỏ dòng dữ liệu** (email, điện thoại lead) |
