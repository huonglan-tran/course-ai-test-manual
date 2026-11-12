# Ma Trận Truy Vết (RTM) — Perfex CRM (khu Admin) · Tổng hợp

> **Tóm tắt:** 209/209 REQ trong phạm vi đã có TC (100%) · **0/252 TC có script automation** · lần chạy gần nhất 176 PASS / 23 FAIL, 53 TC chưa có kết quả · **chỉ 2/22 module** nằm trong RTM này — 20 module chưa khảo sát nên con số 100% **không** đại diện cho hệ thống

| Thông tin | Nội dung |
|---|---|
| Ngày cập nhật | 12-11-2026 |
| Lần sinh gần nhất | `customers` (mục đích: báo cáo coverage trước release) — dòng `LOGIN` giữ nguyên từ lần sinh trước cùng ngày |
| Mốc git | `c816e8a` — từng module có mốc riêng ở Bản đồ tài liệu |
| Nguồn đã quét | Automation: **không tìm thấy script** trong repo · Checklist: `login/web/checklist_regression_20261112.md` · `customers/web/checklist_smoke_20261112.md` (nguồn phụ, không cộng vào số TC) · Kết quả chạy: `login/web/run_1794453634` · `customers/web/run_1794391198` (bộ TC — lần chạy checklist không tính) · Bug: `docs/bugs/README.md` (1 bug `LOGIN` đang mở, `CUS` chưa có bug) |

## ⬛ Ngoài tầm nhìn của RTM — đọc trước mọi con số

| Nhóm | Số module | Module | Ý nghĩa |
|---|:-:|---|---|
| ⬜ Chưa khảo sát | 20 | `CONT` · `ITEM` · `INV` · `PAY` · `EST` · `PROP` · `CRN` · `PRJ` · `TASK` · `CTR` · `LEAD` · `TKT` · `SUB` · `EXP` · `RPT` · `DASH` · `PROF` · `KBASE` · `ESTR` · `UTIL` | Không có REQ — không xuất hiện trong mọi %. Gồm cả các module nghiệp vụ lõi phụ thuộc `CUS` (Invoices, Payments, Projects…) |
| 📄 Có REQ, chưa có TC | 0 | — | — |
| 🕓 Chưa sinh RTM | 0 | — | — |
| ⛔ Chưa cấp prefix | — | Khu **Setup** (26 route) · cổng **Customer/Contact** | Setup bị *Access denied* với tài khoản hiện có (`AMB-SYS-01` 🔴); cổng Customer ngoài phạm vi đợt này. Cả hai nằm ngoài danh mục module, chưa có REQ |

> RTM chỉ chứng minh độ phủ của **`LOGIN` + `CUS`** (209 REQ). Báo cáo trước release phải nói rõ: phần còn lại của hệ thống **chưa được khảo sát, chưa có requirement, chưa có test** — không phải "đã phủ 100%".

## 1. Độ phủ theo module

| Module | REQ trong phạm vi | Requirement Coverage | TC | Automation | Lần chạy gần nhất |
|---|:-:|---|:-:|---|---|
| **Login · Forgot Password** `LOGIN` | 68 | `▰▰▰▰▰` 68/68 | 83 | `▱▱▱▱▱` 0/83 | `▰▰▰▰▰` 49 PASS · ❌ 4 FAIL · 30 chưa có kết quả |
| **Customers** `CUS` | 141 | `▰▰▰▰▰` 141/141 | 169 | `▱▱▱▱▱` 0/169 | `▰▰▰▰▱` 127 PASS · ❌ 19 FAIL · 23 chưa có kết quả |
| **Tổng** | **209** | `▰▰▰▰▰` **209/209** | **252** | `▱▱▱▱▱` **0/252** | `▰▰▰▰▱` **176 PASS · ❌ 23 FAIL** · 53 chưa có kết quả |

*Thanh của cột `Lần chạy gần nhất` = PASS / (PASS + FAIL): `LOGIN` 49/53 (92%) · `CUS` 127/146 (87%) · tổng 176/199 (88%). TC chưa có kết quả = BLOCKED + SKIPPED.*

## 2. Việc cần làm — gộp mọi module, xếp theo mức nghiêm trọng

| # | Việc | Module | Số lượng | Command |
|:-:|---|---|:-:|---|
| 1 | ❌ TC FAIL chưa có bug — `LOGIN`: 028 lỗi đã biết + 3 TC chờ PO chốt (REQ-LOGIN-12) · `CUS`: 10 `@KnownBug` (9 REQ ứng viên bug, REQ-CUS-31 user đã yêu cầu mở bug) + 9 TC cần đối chiếu REQ/TC trước | `LOGIN` · `CUS` | 4 · 19 TC | `/review-testcases` mode FIX → `/create-bug-report` (khi user quyết) |
| 2 | 🐞 Bug đang mở nhưng TC đã PASS — `BUG_login_1792569135_TC005` | `LOGIN` | 1 bug | PO chốt đóng bug hoặc sửa Expected TC005 |
| 3 | ▶️ TC chưa có kết quả — `LOGIN`: 19 TC khoá tài khoản chờ Dev triển khai `CRM-LOGIN-101` + 11 thiếu điều kiện · `CUS`: 14 `@Skip` chờ môi trường riêng, 4 Mass Delete (có 086 Critical), 2 TC thiếu bước, 3 chạy tay | `LOGIN` · `CUS` | 30 · 23 TC | Cấp điều kiện → `/execute-test-cases` |
| 4 | 🟡 Bắt đầu automate TC Critical/High `Automation = Yes` đang chạy được, ưu tiên `@Smoke` · `@CriticalPath` | `LOGIN` · `CUS` | 26 · 37 TC | `/generate-automation-from-testcases` |
| 5 | ⬜ Khảo sát module kế tiếp theo thứ tự đã chốt (`CONT` → `ITEM` → `INV` …) | 20 module | — | `/generate-requirements-from-website` |

## Bản đồ tài liệu

| Module | File RTM | Ngày sinh | Mốc git |
|---|---|---|---|
| `LOGIN` | [login/traceability_matrix_login.md](login/traceability_matrix_login.md) | 12-11-2026 | `c816e8a` |
| `CUS` | [customers/traceability_matrix_customers.md](customers/traceability_matrix_customers.md) | 12-11-2026 | `c816e8a` |
