# Module 10 — Tasks · Timesheets (`TASK`)

> ← [Bản đồ hệ thống](../system_map.md) · Trạng thái recon: [`README.md`](../../README.md) · Tầng khám phá — **không** chứa mã REQ.

| Thuộc tính | Giá trị |
|---|---|
| Tên UI | Tasks · Timesheets (My Timesheets ở menu người dùng) |
| Route | Danh sách `/admin/tasks` (có chế độ Kanban) · `/admin/tasks/list_tasks` · chi tiết `/admin/tasks/view/{id}` · tạo mới: **modal** (menu tạo nhanh `Task` trỏ `#`) · `/admin/staff/timesheets` |
| Loại màn hình | Danh sách + Kanban + modal |
| Bộ đếm trạng thái | `Not Started` · `In Progress` · `Testing` · `Awaiting Feedback` · `Complete` (kèm "Tasks assigned to me") |
| Nút thanh công cụ | `New Task` · `Tasks Overview` · `Bulk Actions` |
| Cột bảng danh sách | (checkbox) · `#` · `Name` · `Status` · `Start Date` · `Due Date` · `Assigned to` · `Tags` · `Priority` |
| Timesheets — cột | `Staff Member` · `Task` · `Timesheet Tags` · `Start Time` · `End Time` · `Note` · `Related` · `Time (h)` · `Time (decimal)` |
| Status flow | ✅ 5 trạng thái + Priority |
| Phụ thuộc | Gắn với nhiều entity qua `Related` (Project, Customer, Contract, chứng từ…) · staff (Assigned) |
| Lý do gộp Timesheets | Giờ công luôn gắn một task — user chốt 12-10-2026. Màn hình tổng quan `Timesheets overview` thuộc `RPT` |
| Risk | 🔴 Cao — dùng hằng ngày; Kanban kéo thả đổi trạng thái; bấm giờ; liên kết đa entity |
| Ước REQ | 40–50 |

## Tầng network

| Request | Quan sát |
|---|---|
| `/admin/tasks/delete_task/{id}` | Link **xoá bằng GET** xuất hiện trên Dashboard — ghi nhận, **không** mở. Cần kiểm ở recon module: có hộp xác nhận không (rủi ro xoá nhầm qua link) |

## Vùng chưa xác minh

| Vùng | Lý do |
|---|---|
| Field modal tạo task · checklist · bấm giờ · nhắc việc | Chưa mở modal |
| Kéo thả Kanban | Không ghi dữ liệu khi khám phá |

## Evidence

> Ảnh chụp 12-10-2026 (Playwright MCP, viewport 1600×780), đã mở lại xác nhận.

| Ảnh | Màn hình | Trạng thái chụp | Phạm vi |
|---|---|---|---|
| [tasks_list_viewport.png](../evidence/tasks_list_viewport.png) | `/admin/tasks` | Tasks Summary 5 trạng thái · danh sách 305 bản ghi · dropdown trạng thái và Priority ngay trên dòng · nhãn `Recurring Task` | Viewport |
