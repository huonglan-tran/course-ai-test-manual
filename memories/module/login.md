---
name: module-login
description: Module LOGIN (Login · Forgot Password) — trạng thái REQ/TC/chạy/bug, quyết định AMB của user, ticket CRM-LOGIN-101, việc còn treo
type: project
updated: 12-11-2026
---

# Module `LOGIN` — Login · Forgot Password

> Số liệu sống: [`docs/requirements/README.md`](../../docs/requirements/README.md) · [`docs/testcases/README.md`](../../docs/testcases/README.md). Nếu lệch file này, **tin README/index**, rồi sửa lại file này.

## Trạng thái (12-11-2026)

| Hạng mục | Giá trị | Vị trí |
|---|---|---|
| Requirements | v1.5 · `REQ-LOGIN-01 → 68` · mã kế tiếp `REQ-LOGIN-69` · 🟢35 🟡2 ⚪31 | [`REQUIREMENTS_LOGIN_SUMMARY.md`](../../docs/requirements/login/REQUIREMENTS_LOGIN_SUMMARY.md) + `login/web/requirements_login_web.md` (1 file, không tách story) |
| AMB / RISK | `AMB-LOGIN-01 → 33` **không còn treo** · `RISK-LOGIN-01 → 09` · mã kế tiếp `AMB-LOGIN-34` | cùng index |
| Test case | 83 TC · 133 biến thể · 4 part · phủ 68/68 REQ · TC kế tiếp `CRM_LOGIN_TC_084` | [`TEST_CASES_LOGIN_SUMMARY.md`](../../docs/testcases/login/TEST_CASES_LOGIN_SUMMARY.md) |
| Tag | `@Skip` 28 · `@KnownBug` 1 (028) · `@PersonalOnly` 4 (048·068·074·075) · `@TechCheck` 14 · `@NeedsVerify` 16 · `@Smoke` 9 (001→008, 049) | index TC |
| Checklist | Regression 28 mục | `docs/checklists/login/web/checklist_regression_20261112.md` |
| Bug | 1 file **đang mở** (12-11-2026): [`BUG_login_1794476046_TC028`](../../docs/bugs/login/web/BUG_login_1794476046_TC028.md) — Forgot Password lộ `Email not found` (REQ-39), 🔴 Critical P2. Bug `TC015` (015-b, sinh cùng ngày) và bug cũ `TC005` đều **đã bị xoá khỏi working tree** (không do agent) — **không** tự tạo lại, hỏi user | [`docs/bugs/README.md`](../../docs/bugs/README.md) |
| RTM | 12-11-2026 · REQ 68/68 có TC · automation 0/83 · REQ đã kiểm chứng đủ 35/68 · 29 REQ chưa có bằng chứng chạy | [`traceability_matrix_login.md`](../../docs/traceability/login/traceability_matrix_login.md) — chỉ RTM module, **chưa** có file toàn hệ thống |

## Kết quả chạy

- **12-11-2026 · `run_1794453634`** (bộ TC 001→083): ✅49 · ❌4 · ⚠️26 · ⏭️4. 4 FAIL: `028` (`@KnownBug` — REQ-39), `015-b`, `016` (nhiều khả năng **TC giả định sai**), `053-d` (máy chủ trả 403, khác Expected). 26 BLOCKED chủ yếu do ticket khoá tài khoản chưa triển khai + thiếu tài khoản/hộp thư test.
- **12-11-2026 · `run_1794460519`** (checklist regression 28 mục): ✅27 · ❌1 (mục 19, `@KnownBug`). Không cộng vào số của bộ TC.
- Hai run đầu 21-10-2026 (`run_1792568115`, `run_1792570380`) đã **bị xoá khỏi working tree** (còn trong lịch sử git; `git status` hiện là `D`).
- Bug `TC005` (21-10-2026): bấm logo Login dẫn tới `/authentication/login` thay vì `/`. **Chờ PO chốt** có phải lỗi (a: đúng thiết kế → đóng bug và sửa TC_005 · b: lỗi thật → Dev sửa).

## Quyết định của user về AMB (đừng hỏi lại)

| AMB | Quyết định |
|---|---|
| `18` | Chỉ có role Admin (19-10) |
| `20` | Contact đăng nhập khu Admin → **bị từ chối như email không tồn tại** (→ REQ-44, 45) |
| `21` | Staff vô hiệu hoá dùng Forgot Password → **không nhận email reset**; thông báo trên trang giống REQ-39 |
| `03` | Forgot Password sau khi Dev sửa phải hiện (tiếng Việt, PO chốt): *"Nếu địa chỉ email này có tài khoản trong hệ thống, chúng tôi đã gửi liên kết đặt lại mật khẩu tới đó. Vui lòng kiểm tra hộp thư (và cả thư mục Spam)."* → REQ-39 🟡 (hệ thống **hiện chưa đạt**, `@KnownBug` 028) |
| `02` | 19-10: không giới hạn lần sai → **bị ticket CRM-LOGIN-101 (22-10) thay**: khoá 15 phút sau 5 lần sai (REQ-46 🟡, REQ-51→68 ⚪) |
| `22` | Email **không tồn tại** nhập sai nhiều lần → **không khoá** (22-10) |
| `23` | Lần sai **thứ 5** vẫn hiện `Invalid email or password`; lần **thứ 6** mới hiện thông báo khoá (22-10) |
| `27` | Bộ đếm khoá **không phân biệt hoa/thường** (22-10) |
| `30` → `33` | TC khoá dùng tài khoản **Project Manager** (không dùng Admin); tài khoản này **cùng role Admin** (22-10) |
| 7 AMB còn lại của ticket (24·25·26·28·29·31·32) + AMB 01–19 khác | **Chốt theo Assumption tạm** (19-10 và 22-10) |

## Ticket `CRM-LOGIN-101` — khoá tài khoản

- Dev **chưa đưa lên demo** (user xác nhận 22-10-2026) → REQ-LOGIN-51→68 ⚪ chưa build; `STORY-LOGIN-07` BLOCKED; TC `036`, `066→083` `@Skip`.
- Impact: [`login/impact/impact_CRM-LOGIN-101.md`](../../docs/requirements/login/impact/impact_CRM-LOGIN-101.md) · [`impact_plan`](../../docs/testcases/login/impact/impact_plan_CRM-LOGIN-101.md) · [`delta_tc`](../../docs/testcases/login/impact/delta_tc_CRM-LOGIN-101.md). Đã áp: sửa `CRM_LOGIN_TC_036` (23-10), bổ sung 18 TC `066→083` vào `part_04_web_khoa_tai_khoan.md` (23-10).
- Khi Dev triển khai: bỏ `@Skip`, chạy tài khoản PM, **mỗi TC khoá = 15 phút** (không chạy liền/song song — `RISK-LOGIN-09`; `ASM-LOGIN-13`: REQ-60 cần 2 chu kỳ).

## Việc còn treo / cần biết

- `AMB-LOGIN-01` ⏭️: chưa có tài khoản staff test riêng có hộp thư → REQ-13/42/43 (sai mật khẩu email thật, gửi/mở email reset) không có TC chạy được.
- Chưa kiểm: cookie `autologin` bị vô hiệu khi Logout (`AMB-LOGIN-12`), phiên hết hạn 8 giờ (`AMB-LOGIN-13`), CSRF server-side (Dev xác minh, `AMB-LOGIN-16`).
- Rủi ro bảo mật đã ghi cho Dev/PO: thiếu security header (`RISK-LOGIN-05`), cookie `autologin` không `HttpOnly` (`AMB-LOGIN-11`), khoá theo email có thể bị lợi dụng (`RISK-LOGIN-08`), `/reset_password` không tham số trả 500 (`AMB-LOGIN-10`).
- Hành vi không assert: tiêu đề tab Forgot Password (`AMB-LOGIN-08`), mã chuyển hướng (`AMB-LOGIN-09`), thứ tự thông báo 2 ô trống (`AMB-LOGIN-15`), chuỗi validation do trình duyệt sinh (`RISK-LOGIN-03` → assert `checkValidity() === false`).
- Review TC: 21-10-2026 (65 TC, TB 11.75/12). Chưa chạy Mode FIX.
- RTM 12-11-2026 phát hiện: bug `TC005` vẫn mở nhưng TC005 PASS ở `run_1794453634` (TC chỉ assert `href`) → chờ PO chốt (a)/(b); REQ-12 là REQ duy nhất ngoài REQ-39 có TC FAIL, 3 FAIL (`015-b` · `016` · `053-d`) nghi Expected sai; checklist regression 27/28 PASS **không** bảo vệ 3 FAIL đó. Ở thời điểm sinh RTM, `docs/checklists/login/` và 2 run 12-11 vẫn untracked (chưa commit).
