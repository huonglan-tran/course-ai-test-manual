# Module 09 — Projects (`PRJ`)

> ← [Bản đồ hệ thống](../system_map.md) · Trạng thái recon: [`README.md`](../../README.md) · Tầng khám phá — **không** chứa mã REQ.

| Thuộc tính | Giá trị |
|---|---|
| Tên UI | Projects |
| Route | Danh sách `/admin/projects` · tạo mới `/admin/projects/project` · chi tiết `/admin/projects/view/{id}` |
| Loại màn hình | Danh sách + Form 2 tab + chi tiết nhiều tab |
| Bộ đếm trạng thái | `Not Started` · `In Progress` · `On Hold` · `Cancelled` · `Finished` |
| Nút thanh công cụ | `New Project` |
| Cột bảng danh sách | `#` · `Project Name` · `Customer` · `Tags` · `Start Date` · `Deadline` · `Members` · `Status` |
| Form tạo mới | 36 field nhập (đếm DOM) · 2 tab: `Project` · `Project Settings` · có tuỳ chọn `Send contacts notifications` / `Select contacts to notify` |
| Chi tiết — 18 tab | Overview · Tasks · Timesheets · Milestones · Files · Discussions · Gantt · Tickets · Contracts · Sales · Proposals · Estimates · Invoices · Subscriptions · Expenses · Credit Notes · Notes · Activity |
| Status flow | ✅ 5 trạng thái |
| Phụ thuộc | `CUS` · staff (Members) · chứa `TASK` · chứng từ Sales gắn theo project |
| Risk | 🔴 Cao — 18 tab; quyền hiển thị cho contact (Project Settings); thông báo cho contact; tổng hợp giờ công và tài chính theo dự án |
| Ước REQ | 60–80 |

## Ranh giới với module khác

| Tab | REQ thuộc module |
|---|---|
| Overview · Milestones · Files · Discussions · Gantt · Notes · Activity | `PRJ` — không có entity riêng, **không** tách |
| Tasks · Timesheets | `TASK` |
| Tickets · Contracts · Proposals · Estimates · Invoices · Subscriptions · Expenses · Credit Notes · Sales | Module chứng từ đó — `PRJ` chỉ giữ REQ về **lọc theo dự án** (`RISK-SYS-04`) |

## Vùng chưa xác minh

| Vùng | Lý do |
|---|---|
| Tab Project Settings (quyền contact thấy gì trên cổng) | Chưa mở chi tiết field |
| Billing type, tính giờ / tính tiền theo dự án | Chưa mở |
| Chuyển trạng thái (VD `Finished` có đóng task không) | Chưa thao tác |

## Evidence

> Ảnh chụp 12-10-2026 (Playwright MCP, viewport 1600×780), đã mở lại xác nhận.

| Ảnh | Màn hình | Trạng thái chụp | Phạm vi |
|---|---|---|---|
| [projects_list_viewport.png](../evidence/projects_list_viewport.png) | `/admin/projects` | Projects Summary 5 trạng thái · danh sách 327 bản ghi | Viewport |
