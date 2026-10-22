# Module 19 — Estimate Request (`ESTR`)

> ← [Bản đồ hệ thống](../system_map.md) · Trạng thái recon: [`README.md`](../../README.md) · Tầng khám phá — **không** chứa mã REQ.

| Thuộc tính | Giá trị |
|---|---|
| Tên UI | Estimate Request |
| Route | `/admin/estimate_request` |
| Loại màn hình | Danh sách yêu cầu + trình dựng form (`New Form`) |
| Nút thanh công cụ | `New Form` |
| Cột bảng danh sách | `#` · `Email` · `Tags` · `Assigned` · `Status` · `Created` |
| Status flow | ✅ Có cột `Status` — danh sách trạng thái chưa thấy |
| Phụ thuộc | Form công khai nhúng ra ngoài (⚠️ suy diễn) · staff (Assigned) · ⚠️ liên hệ `EST` / `LEAD` |
| Risk | 🟢 Thấp — ít dùng; nhưng form công khai nhận dữ liệu người ngoài |
| Ước REQ | 10–15 |

## Vùng chưa xác minh

| Vùng | Lý do |
|---|---|
| Trình dựng form · link form công khai · chuyển yêu cầu thành báo giá | Chưa mở |

## Evidence

> Ảnh chụp 12-10-2026 (Playwright MCP, viewport 1600×780), đã mở lại xác nhận.

| Ảnh | Màn hình | Trạng thái chụp | Phạm vi |
|---|---|---|---|
| [estimate_request_list_viewport.png](../evidence/estimate_request_list_viewport.png) | `/admin/estimate_request` | Nút New Form · danh sách **rỗng** | Viewport |
