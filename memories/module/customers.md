---
name: module-customers
description: Module CUS (Customers) — trạng thái REQ/TC/chạy, quyết định AMB của user, ứng viên bug, TC tag, việc còn treo
type: project
updated: 12-11-2026
---

# Module `CUS` — Customers (route `clients`)

> Số liệu sống: [`docs/requirements/README.md`](../../docs/requirements/README.md) · [`docs/testcases/README.md`](../../docs/testcases/README.md). Lệch thì **tin README/index**.

## Trạng thái (12-11-2026)

| Hạng mục | Giá trị | Vị trí |
|---|---|---|
| Requirements | v2.1 · `REQ-CUS-01 → 141` · mã kế tiếp `REQ-CUS-142` · 🟢113 🟡9 ⚪19 | [`REQUIREMENTS_CUSTOMERS_SUMMARY.md`](../../docs/requirements/customers/REQUIREMENTS_CUSTOMERS_SUMMARY.md) + 8 file Story `customers/web/stories/` |
| AMB / RISK | `AMB-CUS-01 → 28`; **chỉ còn `AMB-CUS-28` 🟡** · `RISK-CUS-01 → 09` · mã kế tiếp `AMB-CUS-29` | cùng index |
| Test case | 169 TC · 272 biến thể · 6 part · phủ 141/141 REQ · TC kế tiếp `CRM_CUS_TC_170` | [`TEST_CASES_CUSTOMERS_SUMMARY.md`](../../docs/testcases/customers/TEST_CASES_CUSTOMERS_SUMMARY.md) |
| Tag | `@Skip` 14 · `@KnownBug` 10 · `@PersonalOnly` 1 (142) · `@TechCheck` 4 · `@NeedsVerify` 41 · `@Smoke` 19 (001→019) | index TC |
| Checklist | Smoke 19 mục | `docs/checklists/customers/web/checklist_smoke_20261112.md` |
| Bug | 2 file **đang mở** (12-11-2026): `TC042` (Company toàn khoảng trắng → KH rỗng, 🟠 Major P2 — REQ-31, user yêu cầu) · `TC044` (Company > 191 bị cắt âm thầm, 🟡 Minor P3 — REQ-32) | `docs/bugs/customers/web/` |

## ⚠️ Lịch sử mã REQ

- 04-11-2026: bản 1.0 (`REQ-CUS-01→101`). 11-11-2026: **user chọn khảo sát lại từ đầu và đánh lại mã từ `REQ-CUS-01`** → bản 2.0/2.1. Cùng mã giữa hai bản **không** cùng nghĩa; bản 1.0 chỉ còn trong git. Mọi TC/automation phải bám bản 2.1.

## Quyết định của user (11-11-2026)

- `AMB-CUS-01`: Company chỉ khoảng trắng tạo KH tên rỗng **là lỗi** → `REQ-CUS-31` 🟡, **user yêu cầu mở bug**.
- `AMB-CUS-02`: Company giới hạn **191 ký tự**, UI phải **chặn** và QA tự đặt câu thông báo (`Please enter no more than 191 characters.`) → `REQ-CUS-32` 🟡.
- Toàn bộ AMB còn lại: **chốt theo Assumption tạm**. Trong đó đã thành quyết định: Note trống là lỗi (15) · Bulk Confirm không chọn nhóm là lỗi (10) · Map validate vĩ độ [-90,90] / kinh độ [-180,180] là lỗi (20) · Statement chặn From > To (17) · Port Vault 1–65535 (19) · Send File/Gửi sao kê trống phải báo `This field is required.` (16, 18) · xoá KH có chứng từ → hệ thống **chặn** (07) · Import thật bỏ qua dòng lỗi (11) · KH Inactive ẩn ở **mọi** form tạo mới (06) · bộ lọc lưu là dùng chung, nhiều rule là OR (21).
- ⏭️ bỏ qua: `12` (Import/Download/Export), `23` (staff thứ hai), `25` (gửi email). `22`: user tự chạy tay TC xem mật khẩu Vault.
- 9 REQ 🟡 (31·32·82·99·110·117·121·126·137) = **ứng viên bug**, danh sách ở mục 6.3 của index requirements. Hình thức chặn + nguyên văn thông báo của 82·117·126·137 còn treo ở `AMB-CUS-28` → TC chỉ assert hành vi chặn + có thông báo, **không** assert nguyên văn.

## Kết quả chạy

- **11-11-2026 · `run_1794391198`** (bộ TC 001→169, 8 phiên): ✅127 · ❌19 · ⚠️18 · ⏭️5 → pass rate 77.4% (không tính SKIPPED). FAIL chia nhóm: (a) `@KnownBug` đúng hiện trạng — 042, 044, 090, 103, 122, 128, 135, 146, 147; (b) nghi lệch REQ/TC ↔ UI, **cần đối chiếu trước khi báo bug** — 018 (nút `Open in Google Map`), 032, 045-e, 059…; (c) `@NeedsVerify` phiên 8 — 153 (bấm đúp công tắc Active lưu sai 3/10 lần), 156-b/157-b (chuỗi `<img …>` làm mất chữ phía sau và để lại thẻ thật) , 124 (ngày `31-02-2026` được nạp vào sao kê). 2 BLOCKED (071, 078) là TC **thiếu bước**.
- **12-11-2026 · `run_1794468325`** (checklist smoke 19 mục): ✅17 · ❌1 (mục 15 — thiếu nút `Open in Google Map`) · ⚠️1 (mục 19 — không bắt được toast `Customer deleted`).
- **RTM 12-11-2026** ([`traceability_matrix_customers.md`](../../docs/traceability/customers/traceability_matrix_customers.md)): mọi REQ 🟡 Thủ công (chưa có script); 99/141 REQ mọi TC PASS. FAIL nhóm (c) gồm cả 2 TC không tag `032` · `059`. Pass rate trong RTM = PASS/(PASS+FAIL), **khác** cách tính của execution report — đừng "sửa" cho khớp.
- Review TC 11-11-2026: 169 TC 🟢, TB 11.86/12. Chỉ áp **2 sửa** (156-d, 021 — xem [`../quy-uoc/chay-test-moi-truong-dung-chung.md`](../quy-uoc/chay-test-moi-truong-dung-chung.md)); các đề xuất còn lại **chưa áp dụng** (user dừng Mode FIX).

## Việc còn treo / cần biết

- Đã mở bug cho `042`, `044` theo yêu cầu user (12-11); 17 FAIL còn lại **chưa** có bug — chỉ mở khi user yêu cầu. Nếu sau này mở: bug phải trỏ TC ID + evidence trong `executions/`, `REQ-31` là cái user đã yêu cầu.
- `ASM-CUS-01`: xung đột REQ-103 (tab Files có nút `Choose from Google Drive`) ↔ ảnh không thấy nút — cần recon lại để sửa REQ-103.
- Không tạo được nhóm KH riêng (Setup bị chặn) → dùng nhóm có sẵn `customer group b` và nhóm thứ hai bất kỳ, ghi tên vào báo cáo (`ASM-CUS-07`).
- Ngoài phạm vi `CUS`: tab Contacts → `CONT`; 11 tab chứng từ → module chứng từ; Reminders → `PROF`; quản lý Customer Groups/Currency/Google API key → Setup (`AMB-SYS-01`).
- Rủi ro bảo mật cho Dev: đường dẫn tuyệt đối của máy chủ lộ ở `data-path`/`file_path` của tab Files (`RISK-CUS-04`).
