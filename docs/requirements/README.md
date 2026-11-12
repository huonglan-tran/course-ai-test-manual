# Danh mục Requirements — Perfex CRM (khu Admin)

> **Điểm vào cấp hệ thống.** Đọc file này đầu tiên trước khi sinh / cập nhật tài liệu của bất kỳ module nào: module nào đã có tài liệu · prefix nào đã bị chiếm · mã REQ kế tiếp · ambiguity 🔴 còn treo.
>
> Bản đồ hệ thống: [`_discovery/system_map.md`](_discovery/system_map.md)

## Thuộc tính hệ thống

| Thuộc tính | Giá trị |
|---|---|
| **Hệ thống** | Perfex CRM — bản demo "Anh Tester Demo" |
| **Phạm vi danh mục** | Khu **Admin** (`/admin/...`). Cổng **Customer/Contact** dùng URL khác — chưa khảo sát, sẽ bổ sung bằng `/discover-system` mode ADD |
| **Nền tảng** | Web (chưa có mặt mobile / API) |
| **URL · tài khoản** | Lưu ở `.env` (`BASE_URL` · `EMAIL_ADMIN` · `PASSWORD_ADMIN`) — **không** ghi vào `docs/` |
| **Tiền tố TC ID** | `CRM_` → `CRM_<MODULE>_TC_<3 số>` (VD `CRM_LOGIN_TC_001`) — chốt 12-10-2026, dùng xuyên suốt |
| **Role của hệ thống** | **Chỉ có role Admin** — user xác nhận 19-10-2026. Ma trận phân quyền của mọi module **không** cần cột role khác; vẫn phải xét các loại tài khoản không phải role: staff bị vô hiệu hoá, contact (cổng khách hàng). Ticket `CRM-LOGIN-101` (21-10-2026) yêu cầu TC khoá dùng "tài khoản Project Manager" — 22-10-2026 user cấp tài khoản này (`.env`: `EMAIL_PM` / `PASSWORD_PM`). User xác nhận tài khoản này **cùng role Admin** (`AMB-LOGIN-33` ✅) — hệ thống vẫn chỉ có role Admin |
| **Môi trường dùng chung** | ✅ **Có** — demo công khai, nhiều người cùng ghi dữ liệu. User cho phép submit (12-10-2026); dữ liệu test phải sinh động, traceable và dọn sau khi tạo. Dữ liệu nền **biến động** (xem `RISK-SYS-01`) |
| **Năng lực kiểm thử của QA** | Chốt 12-10-2026 — dùng cho nhánh Vòng 3 của **mọi** bộ TC:<br>• Gọi API: ❔ chưa xác nhận — mặc định đội Dev xác minh<br>• Truy vấn CSDL: ❔ chưa xác nhận — mặc định đội Dev xác minh<br>• Kiểm tầng tích hợp (email, cron, cổng thanh toán): ❔ chưa xác nhận — mặc định đội Dev xác minh<br>• Xem nhật ký hoạt động: ❌ **đã đo** — `/admin/utilities/activity_log` → *Access denied*, chuyển về Dashboard. Cần tài khoản Admin đầy đủ (`AMB-SYS-01`)<br>• DevTools trình duyệt: ✅ có |

## 1. Bảng danh mục module

> Thứ tự dòng = **thứ tự khảo sát đã chốt** (12-10-2026). `Trạng thái recon` **chỉ** được ghi ở file này.

| # | Module | Prefix | Nền tảng | Trạng thái recon | Mức phủ tài liệu | Tài liệu | REQ đã dùng | Mã kế tiếp | AMB treo | Story | Cập nhật |
|---|---|---|---|---|---|---|---|---|---|---|---|
| 1 | Login · Forgot Password | `LOGIN` | Web ✅ | ✅ Đã có tài liệu | 🟨 Một phần (khảo sát UI + ticket `CRM-LOGIN-101`) | [`login/REQUIREMENTS_LOGIN_SUMMARY.md`](login/REQUIREMENTS_LOGIN_SUMMARY.md) | 01 → 68 | `REQ-LOGIN-69` | — (33/33 AMB đã chốt 22-10-2026) | 7 | 22-10-2026 |
| 2 | Customers | `CUS` | Web ✅ | ✅ Đã có tài liệu | ⬜ Trắng (khảo sát UI, không có tài liệu) | [`customers/REQUIREMENTS_CUSTOMERS_SUMMARY.md`](customers/REQUIREMENTS_CUSTOMERS_SUMMARY.md) | 01 → 141 | `REQ-CUS-142` | 1 🟡 (`AMB-CUS-28`) — 27/28 AMB đã chốt 11-11-2026 | 8 | 11-11-2026 |
| 3 | Contacts | `CONT` | Web ⬜ | ⬜ Chưa khảo sát | ⬜ Trắng | — | — | `REQ-CONT-01` | — | — | 12-10-2026 |
| 4 | Items | `ITEM` | Web ⬜ | ⬜ Chưa khảo sát | ⬜ Trắng | — | — | `REQ-ITEM-01` | — | — | 12-10-2026 |
| 5 | Invoices | `INV` | Web ⬜ | ⬜ Chưa khảo sát | ⬜ Trắng | — | — | `REQ-INV-01` | — | — | 12-10-2026 |
| 6 | Payments | `PAY` | Web ⬜ | ⬜ Chưa khảo sát | ⬜ Trắng | — | — | `REQ-PAY-01` | — | — | 12-10-2026 |
| 7 | Estimates | `EST` | Web ⬜ | ⬜ Chưa khảo sát | ⬜ Trắng | — | — | `REQ-EST-01` | — | — | 12-10-2026 |
| 8 | Proposals | `PROP` | Web ⬜ | ⬜ Chưa khảo sát | ⬜ Trắng | — | — | `REQ-PROP-01` | — | — | 12-10-2026 |
| 9 | Credit Notes | `CRN` | Web ⬜ | ⬜ Chưa khảo sát | ⬜ Trắng | — | — | `REQ-CRN-01` | — | — | 12-10-2026 |
| 10 | Projects | `PRJ` | Web ⬜ | ⬜ Chưa khảo sát | ⬜ Trắng | — | — | `REQ-PRJ-01` | — | — | 12-10-2026 |
| 11 | Tasks · Timesheets | `TASK` | Web ⬜ | ⬜ Chưa khảo sát | ⬜ Trắng | — | — | `REQ-TASK-01` | — | — | 12-10-2026 |
| 12 | Contracts | `CTR` | Web ⬜ | ⬜ Chưa khảo sát | ⬜ Trắng | — | — | `REQ-CTR-01` | — | — | 12-10-2026 |
| 13 | Leads | `LEAD` | Web ⬜ | ⬜ Chưa khảo sát | ⬜ Trắng | — | — | `REQ-LEAD-01` | — | — | 12-10-2026 |
| 14 | Support (Tickets) | `TKT` | Web ⬜ | ⬜ Chưa khảo sát | ⬜ Trắng | — | — | `REQ-TKT-01` | — | — | 12-10-2026 |
| 15 | Subscriptions | `SUB` | Web ⬜ | ⬜ Chưa khảo sát | ⬜ Trắng | — | — | `REQ-SUB-01` | — | — | 12-10-2026 |
| 16 | Expenses | `EXP` | Web ⬜ | ⬜ Chưa khảo sát | ⬜ Trắng | — | — | `REQ-EXP-01` | — | — | 12-10-2026 |
| 17 | Reports | `RPT` | Web ⬜ | ⬜ Chưa khảo sát | ⬜ Trắng | — | — | `REQ-RPT-01` | — | — | 12-10-2026 |
| 18 | Dashboard | `DASH` | Web ⬜ | ⬜ Chưa khảo sát | ⬜ Trắng | — | — | `REQ-DASH-01` | — | — | 12-10-2026 |
| 19 | My Profile · To Do · Reminders · Notifications · Announcements · Language | `PROF` | Web ⬜ | ⬜ Chưa khảo sát | ⬜ Trắng | — | — | `REQ-PROF-01` | — | — | 12-10-2026 |
| 20 | Knowledge Base | `KBASE` | Web ⬜ | ⬜ Chưa khảo sát | ⬜ Trắng | — | — | `REQ-KBASE-01` | — | — | 12-10-2026 |
| 21 | Estimate Request | `ESTR` | Web ⬜ | ⬜ Chưa khảo sát | ⬜ Trắng | — | — | `REQ-ESTR-01` | — | — | 12-10-2026 |
| 22 | Utilities (Media · Bulk PDF Export · Calendar) | `UTIL` | Web ⬜ | ⬜ Chưa khảo sát | ⬜ Trắng | — | — | `REQ-UTIL-01` | — | — | 12-10-2026 |

**Tổng: 22 module · 2 đã có tài liệu · 20 chưa khảo sát.**

### Danh sách prefix đã chiếm

`LOGIN` · `CUS` · `CONT` · `ITEM` · `INV` · `PAY` · `EST` · `PROP` · `CRN` · `PRJ` · `TASK` · `CTR` · `LEAD` · `TKT` · `SUB` · `EXP` · `RPT` · `DASH` · `PROF` · `KBASE` · `ESTR` · `UTIL`

Prefix **dành riêng**: `SYS` (AMB/RISK cấp hệ thống — không cấp cho module).

> Module mới **phải** chọn prefix chưa có trong danh sách trên. Prefix đã cấp là **vĩnh viễn**, kể cả khi module đổi tên trên UI.

### Vùng chưa cấp prefix

| Vùng | Route đã thử | Lý do | Xử lý |
|---|---|---|---|
| **Setup** (Staff · Roles · Settings · Customer Groups · Departments · Taxes · Currencies · Payment Modes · Lead Sources/Statuses · Contract Types · Expense Categories · Ticket Priorities/Statuses/Services/Predefined Replies · Email Templates · Custom Fields · Menu Setup · Theme Style · GDPR · Goals · Surveys · Backup · Modules · Activity Log) | 26 route — xem [`system_map.md` mục 6](_discovery/system_map.md#6-thứ-tự-khảo-sát--vùng-bị-chặn) | ⛔ Tài khoản hiện có bị **Access denied** | Chưa cấp prefix vì chưa nhìn thấy nội dung (Staff/Roles có thể là module riêng). Chờ tài khoản Admin đầy đủ → `/discover-system` mode ADD — `AMB-SYS-01` |
| **Cổng Customer/Contact** | — | URL khác, ngoài phạm vi đợt này | User sẽ cung cấp sau → mode ADD. Nghiệp vụ trùng module đã có thì **dùng lại prefix** (một nghiệp vụ một prefix) |

## 2. Trạng thái REQ toàn hệ thống

| Module | 🟢 Đã kiểm chứng | 🟡 Một phần | 🔴 Deprecated | ⚪ Chưa implement | Tổng |
|---|---|---|---|---|---|
| `LOGIN` | 35 | 2 | 0 | 31 | 68 |
| `CUS` | 113 | 9 | 0 | 19 | 141 |
| **Tổng** | **148** | **11** | **0** | **50** | **209** |

*`LOGIN`: 31 REQ ⚪ là REQ **chưa kiểm chứng được** (thiếu tài khoản test · nằm ở tầng server · chốt từ câu trả lời AMB hoặc ticket, chưa chạy trên UI) theo skill mục 4.3.8. Riêng **REQ-LOGIN-51 → 68** là tính năng **chưa build** — Dev chưa triển khai ticket `CRM-LOGIN-101` lên demo (user xác nhận 22-10-2026); 13 REQ ⚪ còn lại đã có trên hệ thống, chỉ chưa kiểm chứng được. REQ 🟡 `REQ-LOGIN-39`: hành vi mong đợi đã đổi theo `AMB-LOGIN-03` — **hệ thống hiện chưa đáp ứng, chờ Dev sửa**. REQ 🟡 `REQ-LOGIN-46`: đổi từ "không giới hạn số lần sai" sang khoá tài khoản theo ticket `CRM-LOGIN-101` — **chưa triển khai**, demo vẫn không giới hạn số lần sai.*

*`CUS` (khảo sát lại 11-11-2026, mã đánh lại từ 01 theo lựa chọn của user): 16 REQ ⚪ là chức năng **có trên hệ thống nhưng chưa kiểm chứng** — không được phép / không an toàn trên môi trường dùng chung (Import thật, Export/Download/PDF, gửi email, xoá KH có chứng từ, lưu bộ lọc dùng chung, tạo nhóm KH) hoặc cần thao tác agent không làm (nhập mật khẩu đăng nhập, tài khoản thứ hai, cổng khách hàng, Google API key); thêm 3 REQ ⚪ chốt từ câu trả lời AMB 11-11-2026, chưa chạy trên UI (REQ-CUS-139 → 141) — tổng 19. 9 REQ 🟡 (REQ-CUS-31 · 32 · 82 · 99 · 110 · 117 · 121 · 126 · 137): hành vi mong đợi đã đổi theo kết luận AMB 11-11-2026 — **hệ thống hiện chưa đáp ứng, ứng viên bug** (danh sách ở mục 6.3 của index `CUS`); REQ-CUS-31 user yêu cầu mở bug.*

## 3. Ambiguity 🔴 High còn treo

| Mã | Nội dung | Chặn module | Cần ai trả lời |
|---|---|---|---|
| `AMB-SYS-01` | Tài khoản "Admin" trong `.env` bị chặn toàn bộ khu Setup (26 route) — xin tài khoản Admin đầy đủ để khảo sát cấu hình, Staff/Roles và kiểm chứng ma trận phân quyền | Setup (chưa cấp prefix) · phân quyền của **mọi** module · master data cho `CUS` `INV` `EST` `PROP` `CRN` `EXP` `SUB` `LEAD` `TKT` | PO / người quản trị môi trường |

*`CUS`: không còn AMB 🔴 treo. User chốt `AMB-CUS-01` → `27` ngày 11-11-2026 (01 · 02 trả lời trực tiếp, còn lại theo Assumption tạm; 12 · 23 · 25 ⏭️). Còn treo `AMB-CUS-28` 🟡 — nguyên văn thông báo chặn của REQ-CUS-82 · 117 · 126 · 137.*

*`LOGIN`: không còn AMB 🔴 treo. `AMB-LOGIN-01` → `21` chốt 19-10-2026; ticket `CRM-LOGIN-101` mở `AMB-LOGIN-22` → `30`, user chốt toàn bộ `AMB-LOGIN-22` → `32` ngày 22-10-2026 (7 mã 🟡 theo Assumption tạm) và `AMB-LOGIN-33` — **module không còn AMB treo**.*

Chi tiết và các AMB/RISK cấp hệ thống khác: [`_discovery/system_map.md` mục 8](_discovery/system_map.md#8-ambiguity--risk-cấp-hệ-thống).

## 4. Cấu trúc thư mục chuẩn

```
docs/requirements/
├── README.md                              ← DANH MỤC (file này)
├── _discovery/                            ← TẦNG KHÁM PHÁ — cấp hệ thống, KHÔNG có mã REQ
│   ├── system_map.md                      ← INDEX — TÊN FILE BẤT BIẾN
│   ├── modules/module_NN_<slug>.md        ← chi tiết từng module (hệ thống > 8 module)
│   └── evidence/*.png                     ← 1 ảnh tổng quan mỗi module
└── <module>/
    ├── REQUIREMENTS_<TÊN_MODULE>_SUMMARY.md ← INDEX module — TÊN FILE BẤT BIẾN
    ├── web/
    │   ├── requirements_<module>_web.md
    │   ├── evidence/*.png
    │   └── stories/story_NN_<slug>.md     ← chỉ khi vượt ngưỡng tách
    ├── mobile/ · api/                     ← khi có nền tảng đó
    ├── analysis/analysis_<TICKET-ID>.md
    └── impact/impact_<TICKET-ID>.md
```

Thư mục module = tên module chữ thường không dấu (VD `login`, `customers`); file index viết HOA theo tên thư mục (`customers` → `REQUIREMENTS_CUSTOMERS_SUMMARY.md`) — **không** theo prefix.

## 5. Quy trình sử dụng

| Tình huống | Workflow | Ghi vào đâu |
|---|---|---|
| Recon một module (web) | `/generate-requirements-from-website <module>` | `<module>/REQUIREMENTS_<TÊN_MODULE>_SUMMARY.md` + `<module>/web/` · cập nhật bảng mục 1 |
| Có ticket thay đổi module đã có tài liệu | `/update-requirements-from-ticket` | Sửa tại chỗ, giữ REQ ID · `<module>/impact/` |
| Phân tích tài liệu / ticket mới | `/analyze-requirement-document` | `<module>/analysis/` |
| Bổ sung module bị sót · cổng Customer · khu Setup | `/discover-system` (mode ADD) | `_discovery/` + bảng mục 1 |
| Hệ thống vừa deploy, rà lại toàn bộ | `/discover-system` (mode DELTA) | `_discovery/` — chỉ ghi phần chênh |

## 6. Nhật ký danh mục

| Ngày | Thay đổi | Nguồn |
|---|---|---|
| 12-10-2026 | Khởi tạo danh mục — 22 module khu Admin, cấp 22 prefix. Khu Setup chưa cấp prefix (bị chặn). Cổng Customer ngoài phạm vi | `/discover-system` mode UI |
| 12-10-2026 | Đổi 2 prefix trước khi phát hành so với bản đề xuất ở checkpoint: `CN` → `CRN`, `KB` → `KBASE` — quy ước prefix 3–6 ký tự. Chưa có tài liệu nào dùng mã cũ | `/discover-system` mode UI |
| 12-10-2026 | Bổ sung evidence tầng khám phá: 22 ảnh / 22 module trong `_discovery/evidence/`. Không thêm/bớt module, không đổi prefix | `/discover-system` mode ADD |
| 12-10-2026 | Đổi **tên hiển thị** của 22 module về đúng tên tiếng Anh trên UI (không dịch) — ở bảng danh mục, Bản đồ tài liệu của `system_map.md` và tiêu đề 20 file module. Prefix, tên file, đường dẫn giữ nguyên | User yêu cầu |
| 12-10-2026 | Đổi **tên file** 20 file `_discovery/modules/` sang slug tiếng Anh theo tên UI (VD `module_12_khach_hang_tiem_nang.md` → `module_12_leads.md`), số `NN` giữ nguyên, đã sửa 42 link trong `system_map.md`. Chưa có tài liệu nào khác link tới các file này | User yêu cầu |
| 12-10-2026 | Đưa **tên file** 20 file `_discovery/modules/` về slug tiếng Việt không dấu theo `discover-system.md` mục 6.5 (hoàn tác lần đổi sang tiếng Anh cùng ngày), sửa lại 42 link trong `system_map.md`. Tên hiển thị module vẫn giữ tiếng Anh | User yêu cầu |
| 12-10-2026 | `LOGIN` → ✅ Đã có tài liệu: index `login/REQUIREMENTS_LOGIN_SUMMARY.md` + `login/web/requirements_login_web.md` — REQ-LOGIN-01 → 43, mã kế tiếp `REQ-LOGIN-44` | `/generate-requirements-from-website login` |
| 19-10-2026 | User xác nhận **chỉ có role Admin**: thêm dòng "Role của hệ thống" vào bảng thuộc tính · `AMB-LOGIN-18` ✅ (gỡ khỏi mục 3) · `AMB-SYS-01` vẫn 🔴, bổ sung mâu thuẫn role Admin nhưng bị chặn Setup | User trả lời AMB |
| 19-10-2026 | `LOGIN` chốt AMB đợt 1: thêm REQ-LOGIN-44 → 49 (mã kế tiếp `REQ-LOGIN-50`) · REQ-LOGIN-39 🟡 (thông báo Forgot Password đổi theo `AMB-LOGIN-03`, chờ Dev sửa) · AMB treo còn `AMB-LOGIN-21` | User trả lời AMB |
| 19-10-2026 | `LOGIN`: `AMB-LOGIN-21` ✅ → thêm REQ-LOGIN-50 (mã kế tiếp `REQ-LOGIN-51`) · module không còn AMB treo | User trả lời AMB |
| 22-10-2026 | `LOGIN`: ticket khoá tài khoản — REQ-LOGIN-46 🟡 (đảo từ "không giới hạn" sang khoá sau 5 lần sai) · thêm REQ-LOGIN-51 → 56 ⚪ (mã kế tiếp `REQ-LOGIN-57`) · STORY-LOGIN-07 · mở `AMB-LOGIN-22` → `30` (4 🔴 vào mục 3) · mức phủ tài liệu ⬜ → 🟨. Impact Report: [`login/impact/impact_CRM-LOGIN-101.md`](login/impact/impact_CRM-LOGIN-101.md) | `/update-requirements-from-ticket` · `CRM-LOGIN-101` |
| 22-10-2026 | `LOGIN`: user chốt `AMB-LOGIN-22` · `23` · `27` · `30` → REQ-LOGIN-46 sửa biên (thông báo khoá từ lần thử thứ 6) · thêm REQ-LOGIN-57 · 58 (mã kế tiếp `REQ-LOGIN-59`) · TC khoá dùng tài khoản Admin `.env` (bỏ ràng buộc AC#6 của ticket) · gỡ 4 AMB khỏi mục 3, mở `AMB-LOGIN-31` · `32` 🟡 | User trả lời AMB |
| 22-10-2026 | `LOGIN`: ticket `CRM-LOGIN-101` **chưa triển khai** lên demo → REQ-LOGIN-51 → 58 là ⚪ chưa build, STORY-LOGIN-07 BLOCKED | User xác nhận |
| 22-10-2026 | `LOGIN`: user chốt 7 AMB 🟡 theo Assumption tạm → thêm REQ-LOGIN-59 → 68 ⚪ chưa build (mã kế tiếp `REQ-LOGIN-69`) · TC khoá đổi sang tài khoản Project Manager (`EMAIL_PM` / `PASSWORD_PM`), không dùng Admin · mở `AMB-LOGIN-33` 🟡 | User trả lời AMB |
| 22-10-2026 | `LOGIN`: `AMB-LOGIN-33` ✅ — tài khoản Project Manager cùng role Admin, thuộc tính "Role của hệ thống" giữ nguyên · module không còn AMB treo | User trả lời AMB |
| 04-11-2026 | `CUS` → ✅ Đã có tài liệu: index `customers/REQUIREMENTS_CUSTOMERS_SUMMARY.md` + `customers/web/` (8 file Story — 101 REQ vượt ngưỡng 80) — REQ-CUS-01 → 101, mã kế tiếp `REQ-CUS-102` · 19 AMB (8 🔴 vào mục 3) · 9 RISK. Danh mục khớp với thư mục thực tế | `/generate-requirements-from-website CUSTOMER` |
| 11-11-2026 | `CUS` **khảo sát lại từ đầu** (user chọn làm lại, thư mục `customers/` bản 04-11-2026 đã bị gỡ khỏi máy, còn trong git) — mã **đánh lại từ `REQ-CUS-01`** theo lựa chọn của user: REQ-CUS-01 → 138 (mã kế tiếp `REQ-CUS-139`) · 27 AMB (9 🔴 vào mục 3, thay bộ 🔴 cũ) · 9 RISK · 8 Story · 22 ảnh evidence. ⚠️ Cùng mã giữa bản 04-11 và bản này **không** cùng nghĩa — TC/automation theo bản cũ phải map lại. Đối chiếu danh mục: khớp thư mục thực tế | `/generate-requirements-from-website` (khảo sát lại) |
| 11-11-2026 | `CUS`: user chốt `AMB-CUS-01` → `27` (01 · 02 trả lời trực tiếp, còn lại theo Assumption tạm) → 9 REQ 🟡 đổi sang hành vi mong đợi, hệ thống chưa đạt (REQ-CUS-31 · 32 · 82 · 99 · 110 · 117 · 121 · 126 · 137 — ứng viên bug, REQ-31 user yêu cầu mở bug) · thêm REQ-CUS-139 → 141 ⚪ (mã kế tiếp `REQ-CUS-142`) · gỡ 9 AMB 🔴 khỏi mục 3 · mở `AMB-CUS-28` 🟡 | User trả lời AMB |
