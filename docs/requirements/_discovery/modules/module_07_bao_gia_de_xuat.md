# Module 07 — Estimates (`EST`) · Proposals (`PROP`)

> ← [Bản đồ hệ thống](../system_map.md) · Trạng thái recon: [`README.md`](../../README.md) · Tầng khám phá — **không** chứa mã REQ.
>
> Gộp chung file vì cùng phân hệ Sales, cấu trúc chứng từ giống nhau và nối tiếp nhau trong luồng bán hàng. **Hai prefix riêng**, về sau sinh **hai** tài liệu requirements riêng.

## Estimates — `EST`

| Thuộc tính | Giá trị |
|---|---|
| Tên UI | Estimates (menu Sales) |
| Route | Danh sách `/admin/estimates` · tạo mới `/admin/estimates/estimate` · chi tiết chia đôi `/admin/estimates/list_estimates` |
| Loại màn hình | Danh sách + Form + chi tiết chia đôi |
| Bộ đếm trạng thái | `Draft` · `Sent` · `Expired` · `Declined` · `Accepted` |
| Nút thanh công cụ | `Create New Estimate` |
| Cột bảng danh sách | `Estimate #` · `Amount` · `Total Tax` · `Year` · `Customer` · `Project` · `Tags` · `Date` · `Expiry Date` · `Reference #` · `Status` |
| Form tạo mới | 47 field nhập (đếm DOM) |
| Status flow | ✅ 5 trạng thái trên bộ đếm — có trạng thái phụ thuộc ngày (`Expired`) và phản hồi khách (`Accepted` / `Declined`). Widget **Estimate overview** trên Dashboard có thêm dòng `Not Sent` (3 / 100%) — trạng thái gửi hay chỉ là bộ lọc: kiểm ở recon module |
| Phụ thuộc | `CUS` · `PRJ` · `ITEM` · thuế/tiền tệ (Setup) · ⚠️ chuyển thành `INV` (suy diễn) |
| Risk | 🔴 Cao — tiền; status flow 5 trạng thái phụ thuộc ngày và hành động của khách qua cổng Customer |
| Ước REQ | 35–45 |

## Proposals — `PROP`

| Thuộc tính | Giá trị |
|---|---|
| Tên UI | Proposals (menu Sales) |
| Route | Danh sách `/admin/proposals` · tạo mới `/admin/proposals/proposal` · chi tiết chia đôi `/admin/proposals/list_proposals` |
| Loại màn hình | Danh sách + Form + chi tiết chia đôi |
| Nút thanh công cụ | `New Proposal` |
| Cột bảng danh sách | `Proposal #` · `Subject` · `To` · `Total` · `Date` · `Open Till` · `Project` · `Tags` · `Date Created` · `Status` |
| Form tạo mới | 41 field nhập (đếm DOM) |
| Status flow | ✅ 6 trạng thái — `Draft` · `Sent` · `Open` · `Revised` · `Declined` · `Accepted` (đọc từ widget **Proposal overview** trên Dashboard, ảnh `dashboard_overview_viewport.png`; trang danh sách không có bộ đếm) |
| Phụ thuộc | Cột `To` — ⚠️ có thể gửi cho Lead hoặc Customer · `ITEM` · ⚠️ chuyển thành `EST` / `INV` |
| Risk | 🟡 Trung bình — giống `EST` nhưng không có bộ đếm trạng thái; người nhận có thể là lead |
| Ước REQ | 30–40 |

## Vùng chưa xác minh

| Vùng | Module | Lý do |
|---|---|---|
| Luồng chuyển đổi Proposal → Estimate → Invoice | `PROP` · `EST` | Chưa bấm (không ghi dữ liệu khi khám phá) |
| Khách chấp nhận/từ chối, ký | `EST` · `PROP` | Cổng Customer — ngoài phạm vi |

## Evidence

> Ảnh chụp 12-10-2026 (Playwright MCP, viewport 1600×780), đã mở lại xác nhận.

| Ảnh | Màn hình | Trạng thái chụp | Phạm vi |
|---|---|---|---|
| [estimates_list_viewport.png](../evidence/estimates_list_viewport.png) | `/admin/estimates` | Danh sách 3 bản ghi, cả 3 `Expired` | Viewport |
| [proposals_list_header_clip.png](../evidence/proposals_list_header_clip.png) | `/admin/proposals` | Nút New Proposal · hàng tiêu đề 10 cột | Cắt tới hết hàng tiêu đề — **cố ý bỏ dòng dữ liệu** (cột `To` chứa email người nhận) |
