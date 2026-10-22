# Module 20 — Utilities (`UTIL`)

> ← [Bản đồ hệ thống](../system_map.md) · Trạng thái recon: [`README.md`](../../README.md) · Tầng khám phá — **không** chứa mã REQ.
>
> Một module gom 3 trang công cụ nhỏ dưới menu Utilities — mỗi trang không có entity nghiệp vụ riêng đủ lớn để cấp prefix.

| Thuộc tính | Giá trị |
|---|---|
| Tên UI | Utilities (menu cha) |
| Risk | 🟢 Thấp — công cụ phụ trợ; riêng Bulk PDF Export kéo dữ liệu hàng loạt |
| Ước REQ | 15–20 |

## 3 trang

| Trang | Route | Quan sát (DOM 12-10-2026) |
|---|---|---|
| Media | `/admin/utilities/media` | Trình quản lý file (tiêu đề tab `Files`) |
| Bulk PDF Export | `/admin/utilities/bulk_pdf_exporter` | Form 30 field nhập (chọn loại chứng từ, khoảng ngày, trạng thái…) · nút `Export` |
| Calendar | `/admin/utilities/calendar` | Lịch + bộ lọc 15 field · nút `Clear` · `Apply` · tạo sự kiện từ menu tạo nhanh (`?new_event=true&date=<DD-MM-YYYY>`) |

## Vùng chưa xác minh

| Vùng | Lý do |
|---|---|
| Upload trên Media (định dạng, dung lượng) | Chưa thao tác |
| File PDF xuất ra | Không bấm Export khi khám phá |
| Tạo sự kiện lịch | Không ghi dữ liệu khi khám phá |

## Evidence

> Ảnh chụp 12-10-2026 (Playwright MCP, viewport 1600×780), đã mở lại xác nhận.

| Ảnh | Màn hình | Trạng thái chụp | Phạm vi |
|---|---|---|---|
| [utilities_calendar_viewport.png](../evidence/utilities_calendar_viewport.png) | `/admin/utilities/calendar` | Lịch tháng 10-2026 · nút Month / Week / Day / Filter By · hiển thị task và project theo ngày | Viewport |
