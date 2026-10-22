# Bản đồ hệ thống — Perfex CRM (khu Admin)

> **INDEX tầng khám phá — tên file bất biến.** Bản đồ đã tách file (22 module > ngưỡng 8): chi tiết từng module nằm trong `modules/`, xem [Bản đồ tài liệu](#bản-đồ-tài-liệu).
>
> ❌ File này và mọi file trong `_discovery/` **không** chứa mã `REQ-XXX-NN` — tầng khám phá chỉ cấp **prefix**.
>
> Trạng thái recon của từng module: **chỉ** ghi ở [`../README.md`](../README.md).

---

## 1. Bối cảnh khảo sát

| Thuộc tính | Giá trị |
|---|---|
| Ngày khảo sát | 12-10-2026 |
| Mode | **UI** — chỉ có hệ thống đang chạy, không có tài liệu (user chỉ định) |
| Mặt | **Web** — 1 mặt. Chưa có mobile / API |
| Hệ thống | Perfex CRM — "Anh Tester Demo". Ngôn ngữ giao diện khi khảo sát: English |
| URL | `BASE_URL` trong `.env` — tài liệu chỉ ghi route tương đối `/admin/...` |
| Phạm vi | Khu **Admin** (`/admin/...`). Cổng Customer/Contact dùng URL khác → **ngoài phạm vi**, khảo sát sau bằng mode ADD |
| Tài khoản đã dùng | 1 tài khoản staff mang tên "Admin" (`EMAIL_ADMIN` trong `.env`) — **thực tế không có quyền Setup** (`AMB-SYS-01`) |
| Môi trường dùng chung | ✅ Có. Lượt khám phá **không ghi dữ liệu** — không bấm Save/Delete/Convert nào |
| Trình duyệt / viewport | Lượt crawl: trình duyệt tích hợp của Claude desktop (Chromium), viewport giả lập **1600×770** · lượt chụp evidence: Playwright MCP + Google Chrome headed, viewport đo được **1600×780** (cửa sổ 1618×915 — không bị cắt) |
| Phương pháp | Crawl menu sidebar + menu tạo nhanh + menu người dùng trên header · mở **trực tiếp bằng GET** 30 route trong menu, 11 form tạo mới, 4 màn hình chi tiết và 29 route khu Setup (chỉ đọc trang HTML, **không** gọi endpoint API) · đọc DOM: tiêu đề cột bảng, nút thao tác, số field form, tab con, bộ đếm trạng thái |
| Tầng network | Quan sát thụ động trên trang Customers: danh sách nạp bằng `POST /admin/clients/table` (DataTables phía server). Các module khác **chưa** quan sát tầng network — dự kiến cùng mẫu `POST /admin/<module>/table`, kiểm ở recon cấp module |
| Evidence | ✅ **23 ảnh cho 22 module** trong `evidence/` (`LOGIN` có 2 ảnh: Login · Forgot Password) (chụp 12-10-2026, đã mở lại xác nhận từng ảnh) — danh mục ở mục Evidence của từng file module. Chụp **viewport**, không full-page. 4 màn hình có email/điện thoại trong dòng dữ liệu (Customers · Contacts · Proposals · Leads) chỉ chụp **tới hết hàng tiêu đề cột** (`*_header_clip.png`) — đủ chứng minh module tồn tại, không kéo dữ liệu cá nhân vào repo. Các ảnh khác vẫn có tên KH / chứng từ dạng dữ liệu test tự sinh (`AT Auto …`) |

## 2. Sơ đồ điều hướng

### 2.1. Sidebar (thứ tự nguyên trạng trên UI)

```
Dashboard ............................ /admin/
Customers ............................ /admin/clients
Projects ............................. /admin/projects
Tasks ................................ /admin/tasks
Contracts ............................ /admin/contracts
Sales ▸
  ├─ Proposals ....................... /admin/proposals
  ├─ Estimates ....................... /admin/estimates
  ├─ Invoices ........................ /admin/invoices
  ├─ Payments ........................ /admin/payments
  ├─ Credit Notes .................... /admin/credit_notes
  └─ Items ........................... /admin/invoice_items
Subscriptions ........................ /admin/subscriptions
Expenses ............................. /admin/expenses
Support .............................. /admin/tickets
Leads ................................ /admin/leads
Estimate Request ..................... /admin/estimate_request
Knowledge Base ....................... /admin/knowledge_base
Utilities ▸
  ├─ Media ........................... /admin/utilities/media
  ├─ Bulk PDF Export ................. /admin/utilities/bulk_pdf_exporter
  └─ Calendar ........................ /admin/utilities/calendar
Reports ▸
  ├─ Sales ........................... /admin/reports/sales
  ├─ Expenses ........................ /admin/reports/expenses
  ├─ Expenses vs Income .............. /admin/reports/expenses_vs_income
  ├─ Leads ........................... /admin/reports/leads
  ├─ Timesheets overview ............. /admin/staff/timesheets?view=all
  └─ KB Articles ..................... /admin/reports/knowledge_base_articles
Setup (panel riêng) .................. ⛔ RỖNG với tài khoản hiện có
```

### 2.2. Header

| Vùng | Mục | Route |
|---|---|---|
| Menu tạo nhanh (`+`) | Invoice · Estimate · Proposal · Credit Note · Customer · Subscription · Project · Task (mở modal) · Expense · Contract · Article · Ticket · Event | `invoices/invoice` · `estimates/estimate` · `proposals/proposal` · `credit_notes/credit_note` · `clients/client` · `subscriptions/create` · `projects/project` · `#` · `expenses/expense` · `contracts/contract` · `knowledge_base/article` · `tickets/add` · `utilities/calendar?new_event=true&date=<DD-MM-YYYY>` |
| Menu người dùng | My Profile · My Timesheets · Edit Profile · Logout | `profile` · `staff/timesheets` · `staff/edit_profile` · `authentication/logout` |
| Biểu tượng | To Do (có badge số) · Thông báo (Mark all as read · View all notifications) · Language (25 ngôn ngữ) · ô "Share documents, ideas.." | `todo` · `profile?notifications=true` · `staff/change_language/<lang>` |

### 2.3. Route chỉ tới được từ hành động (không có trong menu)

| Route | Tới từ | Module |
|---|---|---|
| `/admin/authentication/forgot_password` | Link `Forgot Password?` trên trang Login (chỉ khi chưa đăng nhập) | `LOGIN` |
| `/admin/clients/client/{id}?group=<tab>` | Bấm tên khách hàng trong bảng | `CUS` |
| `/admin/clients/all_contacts` | Nút **Contacts** trên trang Customers | `CONT` |
| `/admin/projects/view/{id}` | Bấm tên dự án | `PRJ` |
| `/admin/tasks/view/{id}` | Bấm tên task | `TASK` |
| `/admin/contracts/contract/{id}` | Bấm tên hợp đồng / thông báo | `CTR` |
| `/admin/invoices/list_invoices/{id}` · `/admin/estimates/list_estimates` · `/admin/proposals/list_proposals` | Bấm số chứng từ (màn hình chia đôi danh sách + chi tiết) | `INV` · `EST` · `PROP` |
| `/admin/misc/reminders` · `/admin/announcements` | Widget Dashboard | `PROF` |

## 3. Bảng module tổng

> **22 module** — tổng phải khớp với [`../README.md`](../README.md) và [Bản đồ tài liệu](#bản-đồ-tài-liệu). Risk chi tiết kèm lý do: xem file module.

| # | Module (tên UI) | Bí danh | Prefix | Nền tảng | File khám phá | Loại màn hình | Risk | Ước REQ |
|---|---|---|---|---|---|---|---|---|
| 1 | Login | Authentication | `LOGIN` | Web | [module_01](modules/module_01_dang_nhap.md) | Form | 🔴 | 15–20 |
| 2 | Customers | Clients | `CUS` | Web | [module_02](modules/module_02_khach_hang.md) | Danh sách + Form + Hồ sơ 19 tab | 🔴 | 50–70 |
| 3 | Contacts | — | `CONT` | Web | [module_03](modules/module_03_lien_he.md) | Danh sách + modal (trong hồ sơ KH) | 🔴 | 20–30 |
| 4 | Items | Invoice Items | `ITEM` | Web | [module_04](modules/module_04_hang_hoa.md) | Danh sách + modal | 🟡 | 15–20 |
| 5 | Invoices | — | `INV` | Web | [module_05](modules/module_05_hoa_don.md) | Danh sách + Form + chi tiết chia đôi | 🔴 | 50–60 |
| 6 | Payments | — | `PAY` | Web | [module_06](modules/module_06_thanh_toan.md) | Danh sách | 🔴 | 15–20 |
| 7 | Estimates | — | `EST` | Web | [module_07](modules/module_07_bao_gia_de_xuat.md) | Danh sách + Form + chi tiết chia đôi | 🔴 | 35–45 |
| 8 | Proposals | — | `PROP` | Web | [module_07](modules/module_07_bao_gia_de_xuat.md) | Danh sách + Form + chi tiết chia đôi | 🟡 | 30–40 |
| 9 | Credit Notes | — | `CRN` | Web | [module_08](modules/module_08_ghi_co.md) | Danh sách + Form | 🟡 | 25–35 |
| 10 | Projects | — | `PRJ` | Web | [module_09](modules/module_09_du_an.md) | Danh sách + Form 2 tab + chi tiết 18 tab | 🔴 | 60–80 |
| 11 | Tasks | Timesheets | `TASK` | Web | [module_10](modules/module_10_cong_viec_cham_cong.md) | Danh sách + Kanban + modal | 🔴 | 40–50 |
| 12 | Contracts | — | `CTR` | Web | [module_11](modules/module_11_hop_dong.md) | Danh sách + Form + chi tiết 7 tab | 🟡 | 30–40 |
| 13 | Leads | — | `LEAD` | Web | [module_12](modules/module_12_khach_hang_tiem_nang.md) | Danh sách + Kanban + modal | 🔴 | 35–45 |
| 14 | Support | Tickets | `TKT` | Web | [module_13](modules/module_13_ho_tro.md) | Danh sách + Form | 🟡 | 30–40 |
| 15 | Subscriptions | — | `SUB` | Web | [module_14](modules/module_14_dang_ky_dinh_ky.md) | Danh sách + Form | 🟡 | 15–25 |
| 16 | Expenses | — | `EXP` | Web | [module_15](modules/module_15_chi_phi.md) | Danh sách + Form | 🟡 | 25–30 |
| 17 | Reports | — | `RPT` | Web | [module_16](modules/module_16_bao_cao.md) | Báo cáo (6 trang) | 🟡 | 20–30 |
| 18 | Dashboard | — | `DASH` | Web | [module_17](modules/module_17_trang_chu_ho_so_ca_nhan.md) | Dashboard | 🟢 | 10–15 |
| 19 | My Profile · To Do · Reminders · Notifications · Announcements · Language | — | `PROF` | Web | [module_17](modules/module_17_trang_chu_ho_so_ca_nhan.md) | Cấu hình cá nhân | 🟢 | 15–20 |
| 20 | Knowledge Base | KB | `KBASE` | Web | [module_18](modules/module_18_co_so_tri_thuc.md) | Danh sách + Form | 🟢 | 10–15 |
| 21 | Estimate Request | — | `ESTR` | Web | [module_19](modules/module_19_yeu_cau_bao_gia.md) | Danh sách + Form builder | 🟢 | 10–15 |
| 22 | Utilities | Media · Bulk PDF Export · Calendar | `UTIL` | Web | [module_20](modules/module_20_tien_ich.md) | Công cụ rời | 🟢 | 15–20 |

**Tổng ước lượng:** ~ 590–790 REQ cho toàn khu Admin (chưa tính Setup).

## 4. Bản đồ entity & phụ thuộc

### 4.1. Phụ thuộc quan sát được (cột bảng / field trên form)

| Entity | Tham chiếu tới | Căn cứ |
|---|---|---|
| Contact | Customer | Cột `Company` ở `/clients/all_contacts`; tab `Contacts` trong hồ sơ KH |
| Project | Customer | Cột `Customer` ở danh sách Projects |
| Task | (Project / Customer / chứng từ khác — "Related") | Tab `Tasks` trong Project, Customer, Contract |
| Timesheet | Task · Staff | Cột `Task` · `Staff Member` · `Related` ở Timesheets overview |
| Invoice · Estimate · Proposal · Credit Note · Contract · Subscription · Expense | Customer · Project | Cột `Customer` / `To` và `Project` ở từng danh sách |
| Payment | Invoice · Customer · Payment Mode | Cột `Invoice #` · `Customer` · `Payment Mode` |
| Expense | Category · Invoice · Payment Mode | Cột `Category` · `Invoice` · `Payment Mode` |
| Item | Tax 1 · Tax 2 · Group | Cột ở `/invoice_items` |
| Ticket | Contact · Department · Service | Cột ở `/tickets` |
| Lead | Status · Source · Staff (Assigned) | Cột ở `/leads` |

### 4.2. Phụ thuộc suy diễn — ⚠️ chưa kiểm chứng

| Luồng | Vì sao nghi | Kiểm ở |
|---|---|---|
| Lead → chuyển thành Customer | Bộ đếm Leads có trạng thái `Customer` | `LEAD` |
| Proposal → Estimate / Invoice · Estimate → Invoice | Cùng phân hệ Sales, cùng cấu trúc dòng hàng | `PROP` · `EST` |
| Credit Note áp vào Invoice | Cột `Remaining Amount` · `Applied Credits` ở báo cáo Sales | `CRN` |
| Expense → lập hoá đơn cho KH | Cột `Invoice` ở Expenses | `EXP` |
| Contract ký bởi Contact qua cổng KH | Cột `Signature` · thông báo "New comment from customer on contract" | `CTR` + cổng Customer |
| Subscription gắn cổng thanh toán Stripe | 8 trạng thái trùng tên trạng thái Stripe | `SUB` (`AMB-SYS-04`) |
| Master data (Taxes · Currencies · Payment Modes · Customer Groups · Lead Sources/Statuses · Ticket Departments/Services…) do khu Setup quản lý | Route nằm trong Setup, bị chặn | `AMB-SYS-01` |

### 4.3. Phụ thuộc cho thứ tự khảo sát

```
LOGIN ─→ CUS ─→ CONT
          │
ITEM ──→ INV ─→ PAY            LEAD ··→ CUS (chuyển đổi, ⚠️)
          ↑ ↑
   EST ───┘ └── CRN            PROP ··→ EST / INV (⚠️)
          
CUS ─→ PRJ ─→ TASK (+ Timesheets) ─→ RPT
CUS ─→ CTR · TKT · SUB · EXP
```

## 5. Ma trận phân quyền sơ bộ (cấp module)

> Chỉ có **1 tài khoản**. Hệ thống **chỉ có role Admin** — user xác nhận 19-10-2026 (trả lời `AMB-LOGIN-18`). Trang `/admin/roles` và `/admin/staff` vẫn bị chặn nên chưa tự đọc được danh sách role.

| Module | Tài khoản role Admin (`.env`) | Role khác |
|---|---|---|
| 22 module trong bảng mục 3 | ✅ Thấy menu và mở được trang danh sách / form tạo mới (12-10-2026). Quyền **ghi** (tạo / sửa / xoá) chưa thử | — |
| Setup (26 route) | ❌ **Đã kiểm chứng** — *Access denied*, chuyển về Dashboard | — |
| Announcements | ✅ Xem được danh sách · ❔ không thấy nút tạo (có thể do quyền) | — |
| Payments | ✅ Xem được · không có nút tạo trên trang danh sách (dự kiến tạo từ Invoice) | — |

**Tổng 8 ô = Đã kiểm chứng 4 · Suy diễn 0 · Chưa rõ 0 · Không áp dụng 4.** 4 ô "không áp dụng" là cột *Role khác* — hệ thống chỉ có role Admin (user xác nhận 19-10-2026). Ma trận phân quyền ở tầng module **không** cần cột role khác; chỉ còn xét các loại tài khoản không phải role (staff bị vô hiệu hoá, contact).

## 6. Thứ tự khảo sát & vùng bị chặn

### 6.1. Thứ tự đã chốt (12-10-2026)

`LOGIN` → `CUS` → `CONT` → `ITEM` → `INV` → `PAY` → `EST` → `PROP` → `CRN` → `PRJ` → `TASK` → `CTR` → `LEAD` → `TKT` → `SUB` → `EXP` → `RPT` → `DASH` → `PROF` → `KBASE` → `ESTR` → `UTIL`

Nguyên tắc: module được phụ thuộc trước (`CUS` · `ITEM` trước chứng từ Sales; `INV` trước `PAY` · `CRN`; `PRJ` trước `TASK`), risk sau.

### 6.2. BLOCKED — khu Setup

| Route (`/admin/...`) | Kết quả 12-10-2026 |
|---|---|
| `settings` · `staff` · `roles` · `clients/groups` · `departments` · `leads/sources` · `leads/statuses` · `taxes` · `currencies` · `paymentmodes` · `expenses/categories` · `contracts/types` · `emails` · `custom_fields` · `utilities/activity_log` · `goals` · `surveys` · `backup` · `gdpr` · `menu_setup/main_menu` · `theme_style` · `tickets/predefined_replies` · `tickets/priorities` · `tickets/statuses` · `tickets/services` | ❌ *Access denied* → chuyển về Dashboard |
| `modules` | ❌ Chuyển về Dashboard **không** kèm thông báo *Access denied* (`AMB-SYS-03`) |
| `spam_filters` · `utilities/ticket_pipe_log` | 404 — route đoán theo tên, **không tồn tại** ở phiên bản này → loại khỏi bản đồ |

> Ảnh hưởng: không đọc được danh sách role, không xem/chuẩn bị được master data (thuế, tiền tệ, phương thức thanh toán, nhóm KH, nguồn/trạng thái lead, phòng ban ticket). TC của các module Sales phải dùng master data **đang có sẵn** trên môi trường.

### 6.3. Ngoài phạm vi đợt này

| Vùng | Lý do |
|---|---|
| Cổng Customer/Contact | URL khác — user sẽ cung cấp, chạy `/discover-system` mode ADD |

## 7. Nhật ký khám phá

| Ngày | Mode · Mặt | Phạm vi | Kết quả | Nguồn |
|---|---|---|---|---|
| 12-10-2026 | UI · Web | Khu Admin — sidebar, header, 30 route menu, 11 form tạo mới, 4 màn hình chi tiết, 29 route Setup | 22 module, 22 prefix; Setup bị chặn; chưa có ảnh evidence (chưa có Playwright MCP) | `/discover-system` — UI thực tế |
| 12-10-2026 | ADD · Web | Chụp evidence còn thiếu — không crawl lại, không thêm/bớt module, không đổi prefix | 22 ảnh / 22 module. Bổ sung từ ảnh: trạng thái Proposal (6) · `Not Sent` ở Invoice/Estimate · dấu hiệu trạng thái `Cancelled` của Invoice · cột `#` của Expenses có trong DOM nhưng ẩn trên UI · logo Stripe ở Subscriptions (củng cố `AMB-SYS-04`) · Credit Notes, Tickets, Subscriptions, Knowledge Base, Estimate Request **đang rỗng** | `/discover-system` mode ADD — UI thực tế |
| 12-10-2026 | ADD · Web | Trang **Forgot Password** của `LOGIN` — user chỉ ra lượt trước chưa mở | Route `/admin/authentication/forgot_password` · 1 field `Email Address` (`type=email`, không `required`) · nút `Confirm` · không có link quay lại Login · tiêu đề tab trùng trang Login · không CAPTCHA. Không submit. +1 ảnh evidence | `/discover-system` mode ADD — UI thực tế (nguồn: user bổ sung) |
| 12-10-2026 | Recon module · Web | `LOGIN` — `/generate-requirements-from-website login` | Lệch so với bản đồ: module lớn hơn ước lượng (**43 REQ** so với ước 15–20) · thêm route `/admin/authentication/reset_password` (không kèm tham số trả 500) · đăng xuất qua menu avatar, có cảnh báo khi đang chạy timer · Remember me tạo cookie `autologin`. Ranh giới module không đổi | `/generate-requirements-from-website` — Kiểm chứng thực tế |
| 19-10-2026 | Cập nhật từ user | Phân quyền cấp hệ thống | User xác nhận **chỉ có role Admin** → mục 5: cột *Role khác* sang *Không áp dụng*; `AMB-SYS-01` bổ sung mâu thuẫn "role Admin nhưng bị chặn Setup" (vẫn 🔴) | User trả lời `AMB-LOGIN-18` |

## 8. Ambiguity & Risk cấp hệ thống

### 8.1. Ambiguity

| Mã | Mức | Nội dung | Assumption tạm |
|---|---|---|---|
| `AMB-SYS-01` | 🔴 | Tài khoản mang tên "Admin" trong `.env` bị chặn **toàn bộ** khu Setup (menu Setup rỗng, 25 route *Access denied* + `modules` chuyển hướng im lặng). Đây có phải tài khoản Admin thật không? Xin tài khoản Admin đầy đủ để khảo sát Staff · Roles · Settings · master data và kiểm chứng ma trận phân quyền. **Cập nhật 19-10-2026:** user xác nhận hệ thống chỉ có role Admin → mâu thuẫn: tài khoản thuộc role Admin mà vẫn bị chặn Setup. ⚠️ Giả thuyết chưa kiểm chứng: quyền vào Setup gắn với một cờ quản trị riêng của staff chứ không theo role. Câu hỏi còn lại: cần cấp tài khoản nào / bật quyền gì để vào được Setup? | Tài khoản hiện có thuộc role Admin, có quyền trên 22 module nghiệp vụ, không có quyền vào khu Setup |
| `AMB-SYS-02` | 🟡 | Trạng thái lead tên `jjjj` xuất hiện ở bộ đếm Leads — cấu hình cố ý hay dữ liệu rác do người khác tạo trên môi trường dùng chung? | Coi là dữ liệu rác; TC không assert danh sách trạng thái lead cố định |
| `AMB-SYS-03` | 🟡 | `/admin/modules` chuyển về Dashboard **không** kèm thông báo *Access denied*, khác 25 route Setup còn lại — cố ý hay thiếu thông báo? | Coi là cùng một cơ chế chặn quyền |
| `AMB-SYS-04` | 🟡 | Subscriptions có 8 trạng thái kiểu Stripe — môi trường đã cấu hình cổng Stripe (test mode) chưa? Không kiểm được vì Settings bị chặn | Chưa cấu hình → luồng tạo subscription có thể BLOCKED |

### 8.2. Risk

| Mã | Mức | Rủi ro | Giảm thiểu |
|---|---|---|---|
| `RISK-SYS-01` | 🔴 | Môi trường dùng chung, dữ liệu biến động (12-10-2026: 2.610 khách hàng, nhiều bản ghi `TC022 ... <timestamp>` do người khác tạo) → assertion đếm số lượng / bộ đếm Summary không ổn định | Assert theo bản ghi do chính test tạo (dữ liệu traceable), không assert tổng số tuyệt đối; dọn dữ liệu sau test |
| `RISK-SYS-02` | 🔴 | Không có quyền Setup → không tạo/đổi được master data; TC Sales phụ thuộc thuế/tiền tệ có sẵn có thể vỡ khi người khác sửa | Ghi master data dùng trong TC vào điều kiện tiên quyết; chờ `AMB-SYS-01` |
| `RISK-SYS-03` | 🟡 | Giao diện đổi được 25 ngôn ngữ theo từng người dùng → locator theo text tiếng Anh gãy nếu tài khoản bị đổi ngôn ngữ | Automation đặt ngôn ngữ ở bước chuẩn bị; ưu tiên locator theo `id` / `name` / `href` |
| `RISK-SYS-04` | 🟡 | Hồ sơ KH 19 tab, Project 18 tab — chứng từ Sales xuất hiện lại trong tab của KH và Project → dễ sinh REQ/TC trùng giữa module | REQ của danh sách chứng từ thuộc module chứng từ; tab trong KH/Project chỉ REQ về **lọc theo KH/Project** |
| `RISK-SYS-05` | 🟡 | 5 module **đang rỗng** trên môi trường (12-10-2026): `CRN` · `TKT` · `SUB` · `KBASE` · `ESTR` — không có bản ghi để mở màn hình chi tiết, nên recon cấp module phải **tự tạo dữ liệu** | User đã cho phép submit (12-10-2026) → tạo dữ liệu traceable (`auto_<module>_<timestamp>`), ghi lại bản ghi đã tạo và dọn sau recon |

## Bản đồ tài liệu

> **22 module trong 20 file** — tổng khớp bảng mục 3. Trạng thái recon: xem [`../README.md`](../README.md).

| File | Module bao phủ | Prefix |
|---|---|---|
| [modules/module_01_dang_nhap.md](modules/module_01_dang_nhap.md) | Login | `LOGIN` |
| [modules/module_02_khach_hang.md](modules/module_02_khach_hang.md) | Customers | `CUS` |
| [modules/module_03_lien_he.md](modules/module_03_lien_he.md) | Contacts | `CONT` |
| [modules/module_04_hang_hoa.md](modules/module_04_hang_hoa.md) | Items | `ITEM` |
| [modules/module_05_hoa_don.md](modules/module_05_hoa_don.md) | Invoices | `INV` |
| [modules/module_06_thanh_toan.md](modules/module_06_thanh_toan.md) | Payments | `PAY` |
| [modules/module_07_bao_gia_de_xuat.md](modules/module_07_bao_gia_de_xuat.md) | Estimates · Proposals | `EST` · `PROP` |
| [modules/module_08_ghi_co.md](modules/module_08_ghi_co.md) | Credit Notes | `CRN` |
| [modules/module_09_du_an.md](modules/module_09_du_an.md) | Projects | `PRJ` |
| [modules/module_10_cong_viec_cham_cong.md](modules/module_10_cong_viec_cham_cong.md) | Tasks · Timesheets | `TASK` |
| [modules/module_11_hop_dong.md](modules/module_11_hop_dong.md) | Contracts | `CTR` |
| [modules/module_12_khach_hang_tiem_nang.md](modules/module_12_khach_hang_tiem_nang.md) | Leads | `LEAD` |
| [modules/module_13_ho_tro.md](modules/module_13_ho_tro.md) | Support | `TKT` |
| [modules/module_14_dang_ky_dinh_ky.md](modules/module_14_dang_ky_dinh_ky.md) | Subscriptions | `SUB` |
| [modules/module_15_chi_phi.md](modules/module_15_chi_phi.md) | Expenses | `EXP` |
| [modules/module_16_bao_cao.md](modules/module_16_bao_cao.md) | Reports | `RPT` |
| [modules/module_17_trang_chu_ho_so_ca_nhan.md](modules/module_17_trang_chu_ho_so_ca_nhan.md) | Dashboard · My Profile | `DASH` · `PROF` |
| [modules/module_18_co_so_tri_thuc.md](modules/module_18_co_so_tri_thuc.md) | Knowledge Base | `KBASE` |
| [modules/module_19_yeu_cau_bao_gia.md](modules/module_19_yeu_cau_bao_gia.md) | Estimate Request | `ESTR` |
| [modules/module_20_tien_ich.md](modules/module_20_tien_ich.md) | Utilities | `UTIL` |
