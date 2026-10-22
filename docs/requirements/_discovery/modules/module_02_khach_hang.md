# Module 02 — Customers (`CUS`)

> ← [Bản đồ hệ thống](../system_map.md) · Trạng thái recon: [`README.md`](../../README.md) · Tầng khám phá — **không** chứa mã REQ.

| Thuộc tính | Giá trị |
|---|---|
| Tên UI | Customers (bí danh trong route: `clients`) |
| Route | Danh sách `/admin/clients` · tạo mới `/admin/clients/client` · hồ sơ `/admin/clients/client/{id}?group=<tab>` |
| Loại màn hình | Danh sách + Form + Hồ sơ nhiều tab |
| Bộ đếm Summary | Total Customers · Active Customers · Inactive Customers · Active Contacts · Inactive Contacts · Contacts Logged In Today |
| Nút thanh công cụ | `New Customer` · `Import Customers` · `Contacts` · `Bulk Actions` |
| Cột bảng danh sách | (checkbox) · `#` · `Company` · `Primary Contact` · `Primary Email` · `Phone` · `Active` · `Groups` · `Date Created` |
| Form tạo mới | 24 field nhập (đếm DOM) · 2 tab: `Customer Details` · `Billing & Shipping` |
| Hồ sơ KH — 19 tab | Profile · Contacts · Notes · Statement · Invoices · Payments · Proposals · Credit Notes · Estimates · Subscriptions · Expenses · Contracts · Projects · Tasks · Tickets · Files · Vault · Reminders · Map |
| CRUD | Tạo · Xem · Sửa (hồ sơ) · Xoá / Bulk Actions (nút có, chưa thử) · Import |
| Status flow | Bật/tắt `Active` (không phải status flow nhiều bước) |
| Risk | 🔴 Cao — entity gốc của gần như mọi module Sales/Project; dữ liệu cá nhân khách hàng; Import hàng loạt; 19 tab |
| Ước REQ | 50–70 |

## Ranh giới với module khác

| Tab trong hồ sơ KH | REQ thuộc module |
|---|---|
| Profile · Notes · Statement · Files · Vault · Map | `CUS` |
| Contacts | `CONT` |
| Invoices · Payments · Proposals · Credit Notes · Estimates · Subscriptions · Expenses · Contracts · Projects · Tasks · Tickets | Module của chứng từ đó — `CUS` chỉ giữ REQ về **lọc theo khách hàng** (`RISK-SYS-04`) |
| Reminders | `PROF` (nhắc việc dùng chung nhiều entity) |

## Tầng network

| Request | Quan sát |
|---|---|
| `POST /admin/clients/table` → 200 | DataTables phía server — nạp danh sách (thụ động khi mở trang, 12-10-2026). Danh sách hiển thị `Showing 1 to 25 of 2,610 entries` |

## Vùng chưa xác minh

| Vùng | Lý do |
|---|---|
| Field bắt buộc · validation · VAT / mã số | Khám phá không submit form |
| Customer Groups · Currency · Language mặc định | Master data nằm ở Setup (bị chặn — `AMB-SYS-01`) |
| Import Customers (định dạng file, cột bắt buộc) | Chưa mở |
| Tab Vault (lưu thông tin bí mật) · Map | Chưa mở nội dung |
| Xoá KH đang có chứng từ | Chưa thử — môi trường dùng chung |

## Evidence

> Ảnh chụp 12-10-2026 (Playwright MCP, viewport 1600×780), đã mở lại xác nhận.

| Ảnh | Màn hình | Trạng thái chụp | Phạm vi |
|---|---|---|---|
| [customers_list_header_clip.png](../evidence/customers_list_header_clip.png) | `/admin/clients` | Thanh công cụ (New Customer · Import Customers · Contacts) · Customers Summary 6 bộ đếm · thanh Export/Bulk Actions · hàng tiêu đề cột | Cắt tới hết hàng tiêu đề — **cố ý bỏ dòng dữ liệu** (tên, email, điện thoại khách hàng) |
