---
name: module-chua-khao-sat
description: 20 module Perfex CRM đã có prefix nhưng chưa khảo sát + vùng Setup/cổng Customer — dùng khi bắt đầu module mới
type: project
updated: 12-11-2026
---

# Module chưa khảo sát

> Trạng thái chính thức chỉ ghi ở [`docs/requirements/README.md`](../../docs/requirements/README.md) (bảng mục 1). Khi một module có tài liệu: **tạo `memories/module/<module>.md`** theo mẫu [`login.md`](login.md) / [`customers.md`](customers.md), thêm vào [`../MEMORIES.md`](../MEMORIES.md), rồi gạch khỏi bảng dưới.

| # | Module (tên UI) | Prefix REQ | Prefix TC ID | Ghi chú đã biết |
|---|---|---|---|---|
| 3 | Contacts | `CONT` | `CRM_CONT_TC_` | Tab Contacts nằm trong hồ sơ KH nhưng thuộc `CONT`; checkbox `Show primary contact full name…` của KH chỉ hiện khi có primary contact (`AMB-CUS-05`) |
| 4 | Items | `ITEM` | `CRM_ITEM_TC_` | Phụ thuộc trước chứng từ Sales |
| 5 | Invoices | `INV` | `CRM_INV_TC_` | Trước `PAY`, `CRN`; KH Inactive biến khỏi ô chọn KH |
| 6 | Payments | `PAY` | `CRM_PAY_TC_` | |
| 7 | Estimates | `EST` | `CRM_EST_TC_` | |
| 8 | Proposals | `PROP` | `CRM_PROP_TC_` | 6 trạng thái |
| 9 | Credit Notes | `CRN` | `CRM_CRN_TC_` | Đang rỗng trên demo — tự tạo dữ liệu traceable |
| 10 | Projects | `PRJ` | `CRM_PRJ_TC_` | Trước `TASK`; 18 tab |
| 11 | Tasks · Timesheets | `TASK` | `CRM_TASK_TC_` | Bộ đếm giờ đang chạy làm Logout hỏi xác nhận (`AMB-LOGIN-17`) |
| 12 | Contracts | `CTR` | `CRM_CTR_TC_` | |
| 13 | Leads | `LEAD` | `CRM_LEAD_TC_` | Có trạng thái rác `jjjj` (`AMB-SYS-02`) |
| 14 | Support (Tickets) | `TKT` | `CRM_TKT_TC_` | Đang rỗng |
| 15 | Subscriptions | `SUB` | `CRM_SUB_TC_` | Kiểu Stripe, chưa rõ cấu hình (`AMB-SYS-04`); đang rỗng |
| 16 | Expenses | `EXP` | `CRM_EXP_TC_` | Cột `#` có trong DOM nhưng ẩn |
| 17 | Reports | `RPT` | `CRM_RPT_TC_` | |
| 18 | Dashboard | `DASH` | `CRM_DASH_TC_` | |
| 19 | My Profile · To Do · Reminders · Notifications · Announcements · Language | `PROF` | `CRM_PROF_TC_` | Đổi mật khẩu trong Edit Profile thuộc đây |
| 20 | Knowledge Base | `KBASE` | `CRM_KBASE_TC_` | Đang rỗng |
| 21 | Estimate Request | `ESTR` | `CRM_ESTR_TC_` | Đang rỗng |
| 22 | Utilities (Media · Bulk PDF Export · Calendar) | `UTIL` | `CRM_UTIL_TC_` | |

- Prefix **dành riêng**: `SYS` (AMB/RISK cấp hệ thống) — không cấp cho module.
- **Chưa cấp prefix:** khu **Setup** (Staff, Roles, Settings… — bị chặn, `AMB-SYS-01`) và **cổng Customer/Contact**.
- Chi tiết khám phá từng module: `docs/requirements/_discovery/modules/module_NN_<slug>.md` (20 file, slug tiếng Việt không dấu). Chưa file nào chứa mã REQ.
- Recon module mới: ước lượng ban đầu thường **thấp hơn thực tế nhiều** (LOGIN ước 15–20 → 68 REQ; CUS ước 50–70 → 141). Chọn độ hạt/tách Story theo thực tế.
