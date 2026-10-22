# Module 18 — Knowledge Base (`KBASE`)

> ← [Bản đồ hệ thống](../system_map.md) · Trạng thái recon: [`README.md`](../../README.md) · Tầng khám phá — **không** chứa mã REQ.

| Thuộc tính | Giá trị |
|---|---|
| Tên UI | Knowledge Base |
| Route | Danh sách `/admin/knowledge_base` · tạo mới `/admin/knowledge_base/article` |
| Loại màn hình | Danh sách + Form |
| Nút thanh công cụ | `New Article` · `Groups` |
| Cột bảng danh sách | `Article Name` · `Group` · `Date Published` |
| Form tạo mới | 5 field nhập (đếm DOM) |
| Status flow | ⚠️ Có thể có trạng thái nháp/công bố — chưa thấy |
| Phụ thuộc | Hiển thị cho khách trên cổng Customer (⚠️ suy diễn) · báo cáo `RPT` → KB Articles |
| Risk | 🟢 Thấp — nội dung tĩnh |
| Ước REQ | 10–15 |

## Evidence

> Ảnh chụp 12-10-2026 (Playwright MCP, viewport 1600×780), đã mở lại xác nhận.

| Ảnh | Màn hình | Trạng thái chụp | Phạm vi |
|---|---|---|---|
| [knowledge_base_list_viewport.png](../evidence/knowledge_base_list_viewport.png) | `/admin/knowledge_base` | Nút New Article · Groups · danh sách **rỗng** | Viewport |
