# Module 17 — Dashboard (`DASH`) · My Profile (`PROF`)

> ← [Bản đồ hệ thống](../system_map.md) · Trạng thái recon: [`README.md`](../../README.md) · Tầng khám phá — **không** chứa mã REQ.
>
> Gộp chung file vì luôn khảo sát cùng nhau (widget Dashboard mở thẳng To Do · Reminders · Announcements) và cả hai đều 🟢. **Hai prefix riêng**.

## Dashboard — `DASH`

| Thuộc tính | Giá trị |
|---|---|
| Tên UI | Dashboard |
| Route | `/admin/` · đặt lại bố cục `/admin/staff/reset_dashboard` |
| Loại màn hình | Dashboard — widget |
| Widget / tab quan sát được | To Do · Announcements · My Projects · My Reminders · tab Tasks · tab Tickets (anchor `#home_*`) |
| Risk | 🟢 Thấp — chủ yếu hiển thị; số liệu lấy từ module khác |
| Ước REQ | 10–15 |

## My Profile — `PROF`

| Thuộc tính | Giá trị |
|---|---|
| Phạm vi | Thứ thuộc về **người dùng đang đăng nhập**: hồ sơ · đổi thông tin · việc cần làm · nhắc việc · thông báo · thông báo chung · ngôn ngữ |
| My Profile | `/admin/profile` — có bảng dự án của tôi (`Project Name` · `Start Date` · `Deadline` · `Status`) |
| Edit Profile | `/admin/staff/edit_profile` — 16 field nhập (đếm DOM) · nút `Save` |
| To Do | `/admin/todo` — `New To Do` · `Load More` · badge số trên header |
| Reminders | `/admin/misc/reminders` — cột `Related to` · `Description` · `Date` · `Remind` · `Is notified?` |
| Notifications | `/admin/profile?notifications=true` · `Mark all as read` trên header |
| Announcements | `/admin/announcements` — cột `Name` · `Date` · **không** có nút tạo (tạo nằm ở Setup) |
| Language | `/admin/staff/change_language/<lang>` — 25 ngôn ngữ (có `vietnamese`) |
| Risk | 🟢 Thấp — nhưng **đổi mật khẩu / ngôn ngữ trên tài khoản dùng chung** ảnh hưởng mọi người dùng tài khoản đó (`RISK-SYS-03`) |
| Ước REQ | 15–20 |

## Vùng chưa xác minh

| Vùng | Module | Lý do |
|---|---|---|
| Đổi mật khẩu trong Edit Profile | `PROF` | ⚠️ Không được thử trên tài khoản dùng chung — cần tài khoản riêng |
| Kéo thả / ẩn widget | `DASH` | Chưa thao tác |

## Evidence

> Ảnh chụp 12-10-2026 (Playwright MCP, viewport 1600×780), đã mở lại xác nhận.

| Ảnh | Màn hình | Trạng thái chụp | Phạm vi |
|---|---|---|---|
| [dashboard_overview_viewport.png](../evidence/dashboard_overview_viewport.png) | `/admin/` | Widget Invoice/Estimate/Proposal overview · Outstanding/Past Due/Paid Invoices · hàng thẻ thống kê | Viewport — phần dưới (tab Tasks/Tickets) nằm ngoài màn hình đầu |
| [profile_todo_viewport.png](../evidence/profile_todo_viewport.png) | `/admin/todo` | Unfinished to do's · Latest finished to do's · nút sửa / xoá trên từng mục | Viewport — chọn To Do thay cho Edit Profile để ảnh **không** chứa email đăng nhập của tài khoản |
