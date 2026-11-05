# Delta TC List — CRM-LOGIN-101 · module `login`

| Mục | Giá trị |
|---|---|
| Ticket | `CRM-LOGIN-101` — Khoá tài khoản khi đăng nhập sai nhiều lần (PO chốt 21-10-2026) |
| Ngày áp | 23-10-2026 |
| Impact Report nguồn | [`impact_CRM-LOGIN-101.md`](../../../requirements/login/impact/impact_CRM-LOGIN-101.md) |
| Kế hoạch đã duyệt | [`impact_plan_CRM-LOGIN-101.md`](impact_plan_CRM-LOGIN-101.md) |
| Mốc git trước khi sửa | web → `web/parts/part_02_web_forgot_phien.md` @ `f8025be` · index → `TEST_CASES_LOGIN_SUMMARY.md` @ `f8025be` |
| Trạng thái | ✅ ĐÃ ĐỒNG BỘ — 18 REQ (51 → 68) đã có TC bổ sung 23-10-2026 (`066` → `083`) |

## TC đã xử lý

| TC ID | Nền tảng | Vòng · Nhánh | Hành động đã làm | Đổi cái gì (cho automation) |
|---|---|---|---|---|
| CRM_LOGIN_TC_036 | web | V2 · Business Rule + Boundary Value Analysis | ✏️ Đã sửa | Expected **đảo ngược**: sai 5 lần → lần 1 → 5 assert `Invalid email or password`, lần thử thứ 6 assert chữ `Your account is locked. Please try again in 15 minutes.` và URL vẫn `/admin/authentication`. Bỏ bước "đăng nhập đúng → Dashboard". Tài khoản đổi sang Project Manager (`EMAIL_PM` · `PASSWORD_PM`), cấm Admin. Giữ `@Skip` (chưa triển khai) + thêm `@NeedsVerify` (kiểu khung chưa có evidence → chỉ assert chữ). Automation `Partial · ⏸️ Hoãn` — **chưa viết script** cho tới khi Dev triển khai |
| CRM_LOGIN_TC_035 | web | V2 · Business Rule | ✅ Không sửa (đã rà) | Tiền điều kiện và Expected vẫn đúng: 1 lần sai < 5 |
| CRM_LOGIN_TC_015 · 016 · 017 · 024 · 025 · 052 · 053 | web | V2 · Validation · Error Guessing · V3 · Security | ✅ Không tác động | Email không tồn tại không bao giờ bị khoá (REQ-57) |
| CRM_LOGIN_TC_050 · 051 | web | V3 · Permission | ✅ Không tác động | Contact / staff vô hiệu hoá không bị khoá (REQ-66 · 67), mỗi TC 1 lần đăng nhập |

**Không có:** TC `@Deprecated` · TC mới trong phạm vi REQ 🟡 · TC `⏸️ @NeedsVerify — chưa sửa` (036 đã sửa phần chữ từ nguồn PO chốt; chỉ phần kiểu hiển thị còn `@NeedsVerify`).

## Ngoài phạm vi

| REQ | Việc còn lại | Command |
|---|---|---|
| REQ-LOGIN-51 → 68 (18 REQ ⚪ chưa build) | ✅ **Đã sinh 23-10-2026** — `CRM_LOGIN_TC_066` → `083` ở [`part_04_web_khoa_tai_khoan.md`](../web/parts/part_04_web_khoa_tai_khoan.md) (TC mới, không phải TC đã sửa — automation dùng `/generate-automation-from-testcases` khi gỡ `@Skip`). Yêu cầu ban đầu: sinh nối tiếp từ `CRM_LOGIN_TC_066`, độ hạt GỘP, `@Skip` lý do "chưa triển khai", tài khoản Project Manager, Pre-Condition ghi ràng buộc chạy tuần tự (trừ REQ-57 · 66 · 67) | `/generate-testcases-from-requirements` (chế độ BỔ SUNG) |
| — | Module chưa có automation script | Không cần `/update-automation-from-impact` cho ticket này |
| — | Khớp lại ma trận truy vết | `/generate-traceability-matrix` |

## Nhật ký

| Ngày | Thay đổi |
|---|---|
| 23-10-2026 | Áp lần đầu theo `impact_plan_CRM-LOGIN-101.md` đã duyệt. Lệch nhỏ so với kế hoạch: (1) Decision Table thành **15 rule** (R1 → R15, đúng theo mục L4 của kế hoạch) thay cho con số "12 rule" ghi ở mục L6 — L6 đếm thiếu; (2) cột "Trục chặn" của 036 ghi `2 · khoá tài khoản dùng chung` (trục 2, tiêu chí 4) để cùng định dạng với các dòng khác trong bảng |
| 23-10-2026 | Mục *Ngoài phạm vi* đã xử lý: `/generate-testcases-from-requirements` (BỔ SUNG) sinh `CRM_LOGIN_TC_066` → `083` cho REQ-51 → 68. Bảng *TC đã xử lý* không đổi — TC mới không thuộc delta, automation sinh bằng `/generate-automation-from-testcases` |
