# Báo Cáo Review Test Cases — Login · Web

> Workflow: `/review-testcases` **Mode REVIEW** — chỉ báo cáo, **không** sửa file TC nào. Ngày review: 21-10-2026.
>
> ← Index TC: [`../TEST_CASES_LOGIN_SUMMARY.md`](../TEST_CASES_LOGIN_SUMMARY.md) · Requirements: [`requirements_login_web.md`](../../../requirements/login/web/requirements_login_web.md) bản 1.2 (19-10-2026)

## Tổng quan

- **Nguồn:** [`web/parts/part_01_web_smoke_form_login.md`](../web/parts/part_01_web_smoke_form_login.md) (001–025) · [`part_02_web_forgot_phien.md`](../web/parts/part_02_web_forgot_phien.md) (026–048) · [`part_03_web_technical_nonfunc.md`](../web/parts/part_03_web_technical_nonfunc.md) (049–065)
- **Số TC review:** 65 (`CRM_LOGIN_TC_001` → `065`) · `@Deprecated`: 0
- **Kết quả:** 🟢 64 tốt | 🟡 1 cần sửa | 🔴 0 nên viết lại
- **Điểm trung bình:** 11.75/12 (52 TC đạt 12 · 11 TC đạt 11 · 1 TC đạt 10 · 1 TC đạt 9)
- **Traceability:** đánh giá được — có requirements đầy đủ 50 REQ
- **Loại trừ theo requirements:** đăng nhập cổng Customer/Contact · đổi mật khẩu trong Edit Profile (`PROF`) · 2FA / cấu hình reCAPTCHA · quản lý staff & role · `AMB-LOGIN-07` (link quay lại Login) · `AMB-LOGIN-08` (tiêu đề tab Forgot) · `AMB-LOGIN-09` (mã chuyển hướng) · `AMB-LOGIN-10` (`reset_password` lỗi máy chủ → bug report, chưa sinh REQ) · `AMB-LOGIN-15` (thứ tự thông báo) · `AMB-LOGIN-18` (role khác Admin) · thử sai / Forgot trên tài khoản `.env` (`AMB-LOGIN-01` ⏭️, `RISK-LOGIN-01`) — không chấm, không đề xuất
- **Đối chiếu kết quả chạy:** chưa có `docs/executions/login/` → không có ghi chú `⚠️ chưa có evidence` / `@NeedsVerify` nào đã được giải quyết cần dọn

> **Nhận định chung:** cách viết từng TC rất tốt — steps đánh số khớp Expected, nhãn nguyên văn, test data cụ thể, phần kỹ thuật đã tách xuống `🔧 Ghi chú kỹ thuật` + `@TechCheck`, không còn mã HTTP / selector ở phần TC chính (đã quét). Điểm yếu nằm ở **mức bộ TC**: 4 lỗ hổng ở nhánh `Security` (V3), Forgot Email thiếu 1 mục của bảng 15 loại, và một số nhãn/tag trong index ghi chưa đúng thực tế.

## Chi tiết từng TC

> C1 Rõ ràng · C2 Expected đo được · C3 Độc lập · C4 Test data · C5 Truy vết · C6 Đúng trọng tâm.

| TC ID | C1 | C2 | C3 | C4 | C5 | C6 | Điểm | Xếp loại | Vấn đề chính | Đề xuất sửa |
|---|---|---|---|---|---|---|---|---|---|---|
| CRM_LOGIN_TC_001 | 2 | 2 | 2 | 2 | 2 | 2 | 12/12 | 🟢 | — (Bảng kiểm Kiểu B hợp lệ, mỗi mục có REQ) | — |
| CRM_LOGIN_TC_002 | 2 | 2 | 2 | 2 | 2 | 2 | 12/12 | 🟢 | — | — |
| CRM_LOGIN_TC_003 | 2 | 2 | 2 | 2 | 2 | 2 | 12/12 | 🟢 | — | — |
| CRM_LOGIN_TC_004 | 2 | 2 | 2 | 2 | 2 | 2 | 12/12 | 🟢 | — | — |
| CRM_LOGIN_TC_005 | 2 | 2 | 2 | 2 | 2 | 2 | 12/12 | 🟢 | Ngoài rubric: Priority `Low` + `@NeedsVerify` mà vẫn gắn `@Smoke` — logo không phải luồng sống còn | Bỏ `@Smoke`, giữ `@Regression @NeedsVerify` (xem mục Ưu tiên) |
| CRM_LOGIN_TC_006 | 2 | 2 | 2 | 2 | 2 | 2 | 12/12 | 🟢 | — | — |
| CRM_LOGIN_TC_007 | 2 | 2 | 2 | 2 | 2 | 2 | 12/12 | 🟢 | — (Pre-Condition dẫn `TC_006` như cách làm, không đòi chạy trước) | — |
| CRM_LOGIN_TC_008 | 2 | 2 | 2 | 2 | 2 | 2 | 12/12 | 🟢 | — | — |
| CRM_LOGIN_TC_009 | 2 | 2 | 2 | 2 | 2 | 2 | 12/12 | 🟢 | — | — |
| CRM_LOGIN_TC_010 | 2 | 2 | 2 | 2 | 2 | 1 | 11/12 | 🟢 | Gộp thêm hành vi REQ-14 (*"(REQ-14) ô Email Address trống lại, email vừa nhập không còn"*) — **trùng nguyên** kịch bản `TC_018-a` (thiếu Password → ô Email trống) | Bỏ `REQ-LOGIN-14` khỏi cột REQ ID và bỏ vế `(REQ-14)…` ở Expected bước 5. REQ-14 vẫn có `TC_018` (2 biến thể) chống lưng — cập nhật dòng REQ-14 ở Bảng Đối Soát Coverage và phép thử 6b |
| CRM_LOGIN_TC_011 | 2 | 2 | 2 | 2 | 2 | 2 | 12/12 | 🟢 | — | — |
| CRM_LOGIN_TC_012 | 2 | 2 | 2 | 2 | 2 | 2 | 12/12 | 🟢 | — | — |
| CRM_LOGIN_TC_013 | 2 | 2 | 2 | 2 | 2 | 2 | 12/12 | 🟢 | — | — |
| CRM_LOGIN_TC_014 | 2 | 2 | 2 | 2 | 2 | 2 | 12/12 | 🟢 | Ngoài rubric: biến thể `b`→`f` dựa `ASM-LOGIN-12` (chỉ `abc` có evidence) nhưng TC **không** có `@NeedsVerify` | Thêm `@NeedsVerify`, ghi chú `⚠️ b–f chưa có evidence (ASM-LOGIN-12)`, thêm dòng vào *Vùng chưa có evidence* |
| CRM_LOGIN_TC_015 | 2 | 2 | 2 | 2 | 2 | 2 | 12/12 | 🟢 | — | — |
| CRM_LOGIN_TC_016 | 2 | 2 | 2 | 2 | 2 | 2 | 12/12 | 🟢 | Ngoài rubric: Expected dựa `ASM-LOGIN-05` (chưa kiểm) nhưng thiếu `@NeedsVerify` | Thêm `@NeedsVerify` |
| CRM_LOGIN_TC_017 | 2 | 2 | 2 | 2 | 1 | 2 | 11/12 | 🟢 | Truy vết lệch: REQ-12 nói về **email không tồn tại**; với email không tồn tại máy chủ không cần so mật khẩu → TC chỉ chứng minh **độ bền**, không chứng minh ô Password xử lý đúng | Đổi Test Scenario thành *"Ô Password nhận chuỗi rất dài, ký tự đặc biệt, emoji mà không làm lỗi trang (độ bền)"*; ở đối soát field, ghi `017` là mức độ bền, không tính là phủ mục *"Yêu cầu ký tự đặc biệt"* |
| CRM_LOGIN_TC_018 | 2 | 2 | 2 | 2 | 2 | 2 | 12/12 | 🟢 | — (2 biến thể khác thông báo nhưng hành vi được chấm — ô Email trống — là một; Expected đã ghi riêng từng biến thể) | — |
| CRM_LOGIN_TC_019 | 2 | 2 | 2 | 2 | 2 | 2 | 12/12 | 🟢 | — | — |
| CRM_LOGIN_TC_020 | 2 | 2 | 2 | 2 | 2 | 2 | 12/12 | 🟢 | — | — |
| CRM_LOGIN_TC_021 | 2 | 2 | 2 | 2 | 2 | 2 | 12/12 | 🟢 | — | — |
| CRM_LOGIN_TC_022 | 2 | 2 | 2 | 2 | 2 | 2 | 12/12 | 🟢 | — | — |
| CRM_LOGIN_TC_023 | 1 | 2 | 2 | 2 | 2 | 2 | 11/12 | 🟢 | Bước 2, 5, 6 mỗi bước 2–3 thao tác: *"Gõ `PasteMe_01` vào ô Email Address, bôi đen toàn bộ (`Ctrl+A`) rồi sao chép (`Ctrl+C`)"*. Ngoài rubric: dựa `ASM-LOGIN-04` mà thiếu `@NeedsVerify` | Tách: `2. Gõ PasteMe_01 vào ô Email Address` · `3. Nhấn Ctrl+A` · `4. Nhấn Ctrl+C` · `5. Bấm vào ô Password` · `6. Nhấn Ctrl+V` · `7. Gõ thêm X9` · `8. Nhấn Ctrl+A rồi Ctrl+C trong ô Password` · … ; đánh lại số Expected khớp 1-1. Thêm `@NeedsVerify` |
| CRM_LOGIN_TC_024 | 2 | 2 | 2 | 2 | 2 | 2 | 12/12 | 🟢 | — | Tuỳ chọn: thay *"2 lần liên tiếp thật nhanh"* bằng *"nhấp đúp vào nút Login"* cho dễ lặp lại |
| CRM_LOGIN_TC_025 | 1 | 2 | 2 | 2 | 2 | 2 | 11/12 | 🟢 | Bước 7 gộp 3 thao tác: *"Nhập lại Email Address và Password theo Test Data, bấm `Login`"* — đúng là thao tác được chấm | Tách `7. Nhập Email` · `8. Nhập Password` · `9. Bấm Login`; Expected `7.` chuyển thành `9.` |
| CRM_LOGIN_TC_026 | 2 | 2 | 2 | 2 | 2 | 2 | 12/12 | 🟢 | — | — |
| CRM_LOGIN_TC_027 | 2 | 2 | 2 | 2 | 2 | 2 | 12/12 | 🟢 | Ngoài rubric: biến thể `b`→`d` dựa `ASM-LOGIN-12`, thiếu `@NeedsVerify`. Thiếu mục *Domain không hợp lệ* (xem Coverage Gap #5) | Thêm `@NeedsVerify`; thêm biến thể `e` (xem Gap #5) |
| CRM_LOGIN_TC_028 | 2 | 2 | 2 | 2 | 2 | 2 | 12/12 | 🟢 | — (`@KnownBug` đứng riêng 1 TC — đúng luật cấm gộp TC thiết kế để FAIL) | — |
| CRM_LOGIN_TC_029 | 2 | 2 | 2 | 2 | 2 | 2 | 12/12 | 🟢 | Ngoài rubric: biến thể `d` dựa `ASM-LOGIN-05`, thiếu `@NeedsVerify` | Thêm `@NeedsVerify` cho biến thể `d` |
| CRM_LOGIN_TC_030 | 2 | 2 | 2 | 2 | 2 | 2 | 12/12 | 🟢 | — (`@Skip`, dữ liệu chờ cấp được đánh dấu ⛔ rõ ràng) | — |
| CRM_LOGIN_TC_031 | 2 | 2 | 1 | 2 | 2 | 2 | 11/12 | 🟢 | Pre-Condition và Test Data không nêu **chưa đăng nhập** — trong khi Expected 1 *"Trang Forgot Password hiển thị"* chỉ đúng khi chưa đăng nhập (đã đăng nhập thì bị chuyển về Dashboard — `TC_039`). Bảng đầu `part_02` cũng không có dòng `Trình duyệt` như `part_01` | Thêm đầu Pre-Condition: `Cửa sổ ẩn danh Chrome mới, chưa đăng nhập ·` (như `TC_026`–`030`); thêm dòng `Trình duyệt` vào bảng đầu `part_02` |
| CRM_LOGIN_TC_032 | 1 | 1 | 1 | 2 | 2 | 2 | 9/12 | 🟡 | (1) Phụ thuộc TC khác: *"Đã có email đặt lại mật khẩu từ `CRM_LOGIN_TC_031`"*. (2) Bước 2 gộp nhiều thao tác trên trang chưa khảo sát: *"Nhập mật khẩu mới … vào các ô trên trang đặt lại mật khẩu, xác nhận"*. (3) Expected 2 *"Hệ thống báo đặt lại mật khẩu thành công"* không nguyên văn, không nói trang đích, không có dấu hiệu FAIL. (4) Mật khẩu mới cố định `Auto_New_20261020_x` → chạy lần 2 thì *"mật khẩu cũ"* đã khác, và có thể vướng luật cấm dùng lại mật khẩu | **Pre-Condition:** `Cửa sổ ẩn danh Chrome mới, chưa đăng nhập · ⛔ Cần tài khoản staff test riêng có hộp thư (AMB-LOGIN-01 ⏭️) · Đã gửi yêu cầu đặt lại mật khẩu cho tài khoản đó (bước 1–3 của TC_031) và đang mở email vừa nhận`. **Steps:** `1. Bấm liên kết đặt lại mật khẩu trong email` · `2. Nhập mật khẩu mới vào ô mật khẩu` · `3. Nhập lại vào ô xác nhận (nếu trang có)` · `4. Bấm nút xác nhận` · `5. Mở /admin/authentication, đăng nhập bằng mật khẩu mới` · `6. Đăng xuất` · `7. Đăng nhập bằng mật khẩu cũ`. **Expected 4:** *"Trang hiện thông báo thành công — chép nguyên văn + đường dẫn trang đích vào execution report (`@NeedsVerify`). FAIL nếu vẫn ở trang đặt lại mật khẩu mà không có thông báo"*. **Test Data:** `Auto_New_<timestamp>_x`; sau khi chạy cập nhật mật khẩu mới vào `.env` của tài khoản test (không ghi vào báo cáo) |
| CRM_LOGIN_TC_033 | 2 | 2 | 1 | 2 | 2 | 2 | 11/12 | 🟢 | Như `TC_031` — Pre-Condition không nêu chưa đăng nhập | Thêm `Cửa sổ ẩn danh Chrome mới, chưa đăng nhập ·` đầu Pre-Condition |
| CRM_LOGIN_TC_034 | 2 | 2 | 1 | 2 | 2 | 2 | 11/12 | 🟢 | Như `TC_031` | Như `TC_033` |
| CRM_LOGIN_TC_035 | 2 | 2 | 2 | 2 | 2 | 2 | 12/12 | 🟢 | — | — |
| CRM_LOGIN_TC_036 | 1 | 2 | 2 | 2 | 2 | 2 | 11/12 | 🟢 | Bước 2 gộp 3 thao tác — chính là thao tác được lặp 10 lần: *"Đăng nhập bằng email tài khoản test + Password sai, bấm `Login`"*. Ngoài rubric: Risk `High` nhưng Priority `Medium` | Tách `2. Nhập email tài khoản test` · `3. Nhập Password sai thứ n` · `4. Bấm Login` · `5. Lặp lại bước 2–4 cho đủ 10 lần`. Đồng bộ Priority `High` hoặc hạ Risk Level kèm lý do |
| CRM_LOGIN_TC_037 | 2 | 2 | 2 | 2 | 2 | 2 | 12/12 | 🟢 | — | — |
| CRM_LOGIN_TC_038 | 2 | 2 | 2 | 2 | 2 | 2 | 12/12 | 🟢 | — | — |
| CRM_LOGIN_TC_039 | 2 | 2 | 2 | 2 | 2 | 2 | 12/12 | 🟢 | — | — |
| CRM_LOGIN_TC_040 | 2 | 2 | 2 | 2 | 2 | 2 | 12/12 | 🟢 | — (cookie không có biểu hiện nhìn thấy được trong cửa sổ ẩn danh — DevTools là bắt buộc, đã gắn `@TechCheck`) | — |
| CRM_LOGIN_TC_041 | 2 | 2 | 1 | 2 | 2 | 2 | 11/12 | 🟢 | Phụ thuộc TC khác: *"Vừa chạy xong `CRM_LOGIN_TC_040`"* | Pre-Condition: `Cửa sổ ẩn danh Chrome mới · Đăng nhập bằng tài khoản Admin .env có tích Remember me (bước 1–4 của TC_040), đang ở Dashboard` |
| CRM_LOGIN_TC_042 | 2 | 2 | 2 | 2 | 2 | 2 | 12/12 | 🟢 | — | — |
| CRM_LOGIN_TC_043 | 2 | 2 | 2 | 2 | 2 | 2 | 12/12 | 🟢 | — (🔧 *"không còn hoặc không còn dùng được"* chỉ kiểm phía trình duyệt — xem Coverage Gap #4) | — |
| CRM_LOGIN_TC_044 | 2 | 2 | 2 | 2 | 2 | 2 | 12/12 | 🟢 | — | — |
| CRM_LOGIN_TC_045 | 2 | 2 | 1 | 2 | 2 | 2 | 11/12 | 🟢 | Phụ thuộc TC khác: *"Vừa chạy xong `CRM_LOGIN_TC_044`"* — lịch sử Back phụ thuộc đúng chuỗi trang của 044 | Pre-Condition: `Đã đăng nhập, đang ở Dashboard · Bấm ảnh đại diện → Logout · Gõ /admin/clients, Enter (bị đưa về Login)` — hoặc đưa 3 thao tác này thành bước 1–3 của Steps |
| CRM_LOGIN_TC_046 | 2 | 2 | 2 | 2 | 2 | 2 | 12/12 | 🟢 | — | — |
| CRM_LOGIN_TC_047 | 2 | 2 | 2 | 2 | 2 | 2 | 12/12 | 🟢 | — | — |
| CRM_LOGIN_TC_048 | 2 | 2 | 2 | 2 | 2 | 2 | 12/12 | 🟢 | — | — |
| CRM_LOGIN_TC_049 | 2 | 2 | 2 | 2 | 2 | 2 | 12/12 | 🟢 | — (biến thể cùng phép chặn URL — được gộp trong smoke) | — |
| CRM_LOGIN_TC_050 | 2 | 2 | 2 | 2 | 2 | 2 | 12/12 | 🟢 | — | — |
| CRM_LOGIN_TC_051 | 2 | 2 | 2 | 2 | 2 | 2 | 12/12 | 🟢 | — (nguyên văn chưa biết đã ghi rõ, Expected vẫn có dấu hiệu chấm được) | — |
| CRM_LOGIN_TC_052 | 2 | 2 | 2 | 2 | 2 | 2 | 12/12 | 🟢 | — | — |
| CRM_LOGIN_TC_053 | 2 | 2 | 2 | 2 | 2 | 2 | 12/12 | 🟢 | — | — |
| CRM_LOGIN_TC_054 | 2 | 2 | 2 | 2 | 2 | 2 | 12/12 | 🟢 | — (chỉ phủ chuỗi **đúng định dạng email** — xem Coverage Gap #2) | — |
| CRM_LOGIN_TC_055 | 2 | 2 | 2 | 2 | 2 | 2 | 12/12 | 🟢 | — | — |
| CRM_LOGIN_TC_056 | 2 | 2 | 2 | 2 | 2 | 2 | 12/12 | 🟢 | — | — |
| CRM_LOGIN_TC_057 | 2 | 2 | 2 | 2 | 2 | 2 | 12/12 | 🟢 | — | — |
| CRM_LOGIN_TC_058 | 2 | 2 | 2 | 2 | 2 | 2 | 12/12 | 🟢 | — (dấu hiệu phân biệt rõ: thấy `Invalid email or password` = máy chủ không kiểm mã) | — |
| CRM_LOGIN_TC_059 | 2 | 2 | 2 | 2 | 2 | 2 | 12/12 | 🟢 | — | — |
| CRM_LOGIN_TC_060 | 2 | 2 | 2 | 2 | 2 | 2 | 12/12 | 🟢 | — | — |
| CRM_LOGIN_TC_061 | 1 | 2 | 2 | 2 | 1 | 2 | 10/12 | 🟢 | Bước 5 gộp 3 thao tác: *"Mở lại `/admin/authentication`, nhập lại dữ liệu bước 3, bấm `Login`"*. Truy vết: REQ-24 chỉ nói form **có** mã; hành vi khi mã hết hạn không có REQ nào, chỉ dựa `ASM-LOGIN-07` | Tách bước 5 thành 3 bước. Cột REQ ID ghi `REQ-LOGIN-24 (liên quan) · ASM-LOGIN-07`; đề xuất bổ sung một REQ mới cho hành vi mã hết hạn qua `/update-requirements-from-ticket` sau khi chạy có evidence |
| CRM_LOGIN_TC_062 | 2 | 2 | 2 | 2 | 2 | 2 | 12/12 | 🟢 | — | — |
| CRM_LOGIN_TC_063 | 2 | 2 | 2 | 2 | 2 | 2 | 12/12 | 🟢 | — | — |
| CRM_LOGIN_TC_064 | 2 | 2 | 2 | 2 | 2 | 2 | 12/12 | 🟢 | Ngoài rubric: Pre-Condition dùng *"DevTools → Toggle device toolbar"* (375px không đạt được bằng kéo cửa sổ) nhưng TC **không** có `@TechCheck` | Thêm `@TechCheck`, chuyển hướng dẫn DevTools xuống `🔧 Ghi chú kỹ thuật` |
| CRM_LOGIN_TC_065 | 1 | 2 | 2 | 2 | 2 | 2 | 11/12 | 🟢 | Bước 2 gộp 3 thao tác, bước 3 gộp 4 thao tác: *"Xoá 2 ô, nhập Email · Password tài khoản Admin `.env`, bấm `Login`"* | Tách: `2. Nhập Email abc` · `3. Nhập Password` · `4. Bấm Login` · `5. Xoá 2 ô` · `6. Nhập Email · Password Admin` · `7. Bấm Login`; đánh lại số Expected |

## Đối soát loại kiểm thử (4 vòng)

| Vòng | Nhánh | Trạng thái | Ghi chú |
|---|---|---|---|
| 1 | UI cơ bản | ✅ | 001 · 002 · 003 · 007 (4 TC · 11 mục Bảng kiểm) — nhãn nguyên văn, thứ tự, trạng thái mặc định, ô được chọn sẵn đều có |
| 1 | Open form | ✅ | 004 · 005 + đường dẫn trực tiếp ở mọi TC |
| 1 | Display | ✅ | 009 · 011 · 015 · 026 |
| 1 | Input valid data | ✅ | 006 (bộ tối thiểu) · 040 (bộ đầy đủ) |
| 1 | Save | ✅ | 006 · 008 |
| 1 | Verify data | ✅ | 006 · 008 |
| 2 | UI Behavior | ✅ | 021 · 022 · 023. Nhỏ: chưa có bấm vào **chữ nhãn** `Remember me` (Gap #9) |
| 2 | Required | ✅ | 009 · 010 · 011 · 012 · 013 · 026 |
| 2 | Validation | 🟡 Nông | Email (Login) 9/9 ✅ · Email (Forgot) **8/9** — thiếu *Domain không hợp lệ* (Gap #5), độ dài chỉ thử 254 ký tự · Password: index ghi `7/7` nhưng 017 chỉ là mức độ bền (email không tồn tại) — xem Đối soát field |
| 2 | Equivalence Partitioning | ✅ | 014 · 015 · 035 · 006 · 050 · 051 |
| 2 | Boundary Value Analysis | ✅ | 016 · 017 · 029-d (không có min/max — kiểm độ dài cực trị) |
| 2 | Business Rule | ✅ | 028 · 030 · 033 · 034 · 035 · 036 · 037 · 038 · 039 |
| 2 | Decision Table | ✅ | R1 → R9, mỗi rule ≥ 1 TC |
| 2 | State Transition | ✅ | Ma trận phiên 3 trạng thái — đã đối chiếu từng ô với mục 5 requirements |
| 2 | Dependency | ✅ | 040 · 041 · 042 · 043 |
| 2 | Use Case / Scenario | ✅ | 037 · 043 · 032 |
| 2 | Save / Edit / Delete | ➖ | Không có CRUD — đúng nhãn |
| 2 | Error Guessing | ✅ | 024 · 025 · 045 · 047. Nhỏ: chưa có F5 sau khi bị từ chối (Gap #8) |
| 3 | Permission | ✅ | 049 · 050 · 051 · 033 · 034 · 038 · 039 — chỉ có role Admin (`AMB-LOGIN-18`), phân theo loại tài khoản. Lưu ý: 4/7 TC đang `@Skip` |
| 3 | Security | 🟡 Nông | Có chống dò tài khoản, huỷ phiên, SQLi định dạng hợp lệ, cookie, CSRF. **Thiếu:** header bảo mật / chống nhúng iframe (Gap #1) · XSS phản chiếu ở ô Email Forgot khi vượt kiểm tra của trình duyệt (Gap #2) · phiên mới khi đăng nhập (Gap #3) · ghi nhớ đăng nhập bị vô hiệu **phía máy chủ** sau Logout (Gap #4) |
| 3 | API | ➖ | Quyền gọi API chưa xác nhận — đội Dev (`docs/requirements/README.md`, 12-10-2026) — đúng nhãn |
| 3 | Database | ➖ | Như trên — đúng nhãn |
| 3 | Integration | ✅ | 031 · 032 — **0 TC chạy được** (đều `@Skip`, `AMB-LOGIN-01` ⏭️). Nên ghi rõ con số này ở index |
| 3 | Logging / Audit | ➖ | Requirements không có yêu cầu audit + QA không xem được nhật ký (đã đo) — đúng nhãn |
| 4 | Compatibility | ✅ | 065 (Edge · Firefox). Nhỏ: trang Forgot chưa chạy trên Edge/Firefox (Gap #10) |
| 4 | Responsive / UI Stability | 🟡 Nông | 064 chỉ phủ trang **Login**. Thiếu lối Logout ở màn hẹp — requirements REQ-30 ghi DOM có mục `Logout` riêng trong thanh điều hướng di động (Gap #6) · trang Forgot ở màn hẹp |
| 4 | Accessibility | 🟡 Nông | 062 · 063 phủ Tab/Enter/Space/viền chọn. *Văn bản thay thế của logo* đang ghi `➖ cần công cụ quét riêng` — **sai nhãn**: kiểm được bằng DevTools (QA có ✅), và *"chưa có người phụ trách"* không phải lý do kỹ thuật (Gap #7) |
| 4 | Performance | ➖ | Không có ngưỡng cam kết — đúng nhãn |
| 4 | Regression | ➖ | Chưa có bug đóng — đúng nhãn |
| 4 | E2E | ➖ | Login chỉ là tiền đề — đúng nhãn |

## Đối soát bảng 15 loại field

| Field | Loại | Kết quả | Mục thiếu / ghi chú |
|---|---|---|---|
| Email Address (Login) | Email — 9 mục | ✅ 9/9 | — (thêm cả bắt buộc, khoảng trắng, ký tự có dấu) |
| Password (Login) | Password — 7 mục | ✅ đạt, **sửa nhãn** | Copy-paste (023) ✅ · Hiện/ẩn (002) ✅ · Min/Max (017-a·b·c) ✅ ở mức độ bền · *Yêu cầu ký tự đặc biệt* — index ghi phủ bằng `017-d·e·053`, nhưng form đăng nhập không đặt luật độ mạnh → phải là `➖` cùng nhóm với *hoa/thường · số · xác nhận* (4 ➖, không phải 3). Phép thử trên tài khoản thật thuộc loại trừ (`AMB-LOGIN-01` ⏭️) |
| Remember me | Checkbox — 4 mục | ✅ 4/4 | Mặc định · tích/bỏ tích ✅ · bắt buộc, nhóm radio ➖ đúng |
| Email Address (Forgot) | Email — 9 mục | 🟡 8/9 | **Thiếu *Domain không hợp lệ*** — 027 có thiếu `@` · thiếu tên miền · thiếu phần trước `@` · nhiều `@`, nhưng không có tên miền sai. Index tính *"thiếu phần trước `@`"* thay cho mục này. *Max length* chỉ 254 ký tự (029-d) — bên Login có 255 · 256 · 1000 |

## Coverage Gaps (TC còn thiếu)

> TC mới cấp số **nối tiếp** từ `CRM_LOGIN_TC_066` khi chạy Mode FIX.

| # | Kịch bản thiếu | Vòng / Nhánh | Priority đề xuất |
|---|---|---|---|
| 1 | Trang Login **không** bị nhúng vào khung (iframe) của trang web khác, và trả kèm các header bảo mật — `RISK-LOGIN-05` ghi mitigation *"TC phi chức năng kiểm header response"*, nhưng index đang chấm `➖ đội Dev / Security`. QA có DevTools nên làm được dạng `@TechCheck`: 🔧 DevTools → Network → request trang `/admin/authentication` → kiểm `X-Frame-Options` / `Content-Security-Policy` / `Strict-Transport-Security` / `X-Content-Type-Options`. Hiện trạng thiếu cả 4 → cần PO chốt hành vi mong đợi (REQ mới) trước, rồi viết TC dạng `@KnownBug` | V3 · Security | High |
| 2 | Ô Email trang Forgot Password **hiển thị lại** chuỗi đã nhập (REQ-40) → nguy cơ XSS phản chiếu. 054 chỉ thử chuỗi **đúng định dạng email** (trình duyệt chặn mọi chuỗi có `<` `>` `"`). Cần TC `@TechCheck`: 🔧 DevTools → Elements đổi ô Email sang kiểu chữ thường để vượt kiểm tra của trình duyệt → nhập `"><img src=x onerror=alert(1)>` → bấm `Confirm` → **không** bật hộp thoại nào, ô Email hiện lại đúng nguyên văn dạng chữ. Có thể kèm biến thể SQLi sai định dạng cho ô Email trang Login (`admin' OR '1'='1' -- `) | V3 · Security | High |
| 3 | **Đăng nhập** cũng cấp phiên mới (chống cố định phiên): ghi 6 ký tự cuối của cookie phiên **trước** khi đăng nhập → đăng nhập bằng tài khoản Admin `.env` → 6 ký tự cuối phải **khác**. Cùng cách so sánh với 060 (REQ-34 chỉ phủ chiều đăng xuất) | V3 · Security | Medium |
| 4 | Ghi nhớ đăng nhập bị vô hiệu **phía máy chủ** sau Logout — 043 chỉ kiểm trình duyệt không tự đăng nhập lại (🔧 *"không còn hoặc không còn dùng được"* — chấp nhận cả trường hợp trình duyệt chỉ xoá cookie). Cookie `autologin` không có cờ chặn script (`AMB-LOGIN-11`) nên có thể bị đánh cắp trước khi Logout. Cần TC `@TechCheck`: lưu cookie `autologin` (trong DevTools, 🔒 không chép vào báo cáo) → Logout → khôi phục cookie đó → mở `/admin/` → phải về trang Login | V3 · Security | High |
| 5 | Forgot Password: email có **tên miền không hợp lệ** — thêm `027-e` `auto_forgot@-auto.test` (trình duyệt chặn) · `027-f` `tếst@auto.test`; còn `auto_forgot@auto` (không dấu chấm, trình duyệt cho gửi) đưa vào `029-e` (giữ nguyên email sau khi gửi) | V2 · Validation | Medium |
| 6 | Đăng xuất ở màn hình hẹp (768 · 375): ảnh đại diện trên header bị thay bằng thanh điều hướng di động (REQ-30 ghi DOM có mục `Logout` riêng ở đó) → người dùng vẫn tìm thấy và bấm được `Logout`, về trang Login. Kèm: trang Forgot Password hiển thị đủ ở 4 kích thước của 064 | V4 · Responsive | Medium |
| 7 | Logo trang Login và Forgot có văn bản thay thế (rê chuột / 🔧 DevTools → Elements xem thuộc tính mô tả ảnh) — hoặc đổi nhãn sang `⏭️` kèm **người quyết + điều kiện rà lại** nếu cố ý không kiểm | V4 · Accessibility | Low |
| 8 | Bị từ chối vì bỏ trống ô (trang trả lại sau khi gửi form) → nhấn `F5` → trình duyệt hỏi gửi lại biểu mẫu → chọn gửi lại → vẫn chỉ 1 khung đỏ, không trang lỗi | V2 · Error Guessing | Low |
| 9 | Bấm vào **chữ** `Remember me` (không bấm ô vuông) → ô được tích / bỏ tích — thêm bước vào 021 | V2 · UI Behavior | Low |
| 10 | Forgot Password trên Edge / Firefox: email sai định dạng bị chặn (REQ-38 dựa vào kiểm tra của trình duyệt) — thêm bước vào 065 hoặc TC riêng | V4 · Compatibility | Low |

## TC trùng lặp — đề xuất merge

- **`TC_010` (vế REQ-14) ≈ `TC_018-a`** — cùng thao tác *"nhập email, bỏ trống Password, bấm Login"*, cùng kiểm *"ô Email trống lại"*. Giữ `TC_018` làm TC của REQ-14; ở `TC_010` chỉ bỏ vế REQ-14 khỏi cột REQ ID và Expected — **không** cần `@Deprecated` vì `TC_010` vẫn là TC duy nhất cho thông báo bắt buộc của Password.
- `TC_021` bước 4–5 ≈ `TC_062` bước 5–6 (Tab tới Remember me + `Space`) — chồng một phần nhưng khác mục đích (UI Behavior vs Accessibility). **Giữ cả hai**, không merge.

## Độ hạt GỘP — đếm biến thể

- 16 TC có Bảng biến thể · **57 biến thể** · 49 TC đơn → **106 biến thể thật** (Kiểu A).
- Index ghi `115 biến thể` = 106 + 11 mục Bảng kiểm Kiểu B của `001` (6) và `003` (5) trừ 2. Đề xuất ghi tách: `106 biến thể · 11 mục Bảng kiểm` để không lẫn hai loại.
- Không TC nào vượt trần 6 biến thể · mọi biến thể có mã riêng · không vi phạm bảng CẤM gộp (`028` `@KnownBug` đứng riêng, `049` gộp biến thể cùng hành vi trong smoke — hợp lệ).
- Số biến thể **không** ít bất thường: Validation 9 TC · 33 biến thể khớp index.

## Ưu tiên & nhãn

| Vấn đề | TC | Đề xuất |
|---|---|---|
| `@Smoke` gắn cho TC Priority `Low`, đang `@NeedsVerify` | 005 | Bỏ `@Smoke` — bộ smoke còn 8 TC (001–004 · 006–008 · 049) |
| Risk `High` nhưng Priority `Medium` | 036 | Đồng bộ một trong hai, ghi lý do |
| TC dựa Assumption chưa kiểm mà thiếu `@NeedsVerify` | 014 (b–f) · 016 · 023 · 027 (b–d) · 029-d | Thêm tag + dòng vào *Vùng chưa có evidence* — hiện bảng `@NeedsVerify` đếm 12, thực tế phải là 17 |
| Dùng DevTools nhưng thiếu `@TechCheck` | 064 | Thêm tag — bảng `@TechCheck` thành 14 |
| `Security` ở bảng ISO 25010 ghi header bảo mật là `➖ đội Dev / Security` | index | Trái mitigation của `RISK-LOGIN-05` — xem Gap #1 |
| Bảng đầu `part_02` thiếu dòng `Trình duyệt` | part_02 | Chép dòng `Trình duyệt` của `part_01` |

Phân bổ Priority còn lại hợp lý với rủi ro: Critical cho 006 · 008 · 049 (cổng vào), High cho validation chính, Forgot chống dò tài khoản, huỷ phiên, SQLi/XSS, cookie.

## Kết luận & Khuyến nghị

1. **Bổ sung 4 TC Security (Gap #1–#4)** — đây là lỗ hổng lớn nhất của bộ: module rủi ro `Cao`, QA có DevTools, mà chưa kiểm XSS phản chiếu ở Forgot, phiên mới khi đăng nhập, ghi nhớ đăng nhập phía máy chủ, header chống nhúng iframe. Gap #1 cần PO chốt hành vi mong đợi trước (`/update-requirements-from-ticket`).
2. **Sửa `TC_032`** (🟡 duy nhất) theo đề xuất — gỡ phụ thuộc vào `TC_031`, sinh mật khẩu mới theo timestamp, có dấu hiệu FAIL rõ. Gỡ phụ thuộc tương tự ở `TC_041` · `TC_045`, thêm *"chưa đăng nhập"* cho `TC_031` · `033` · `034`.
3. **Tách bước gộp** ở `TC_023` · `025` · `036` · `061` · `065`; bỏ vế REQ-14 trùng ở `TC_010`.
4. **Bổ sung mục *Domain không hợp lệ* cho Email Forgot** (Gap #5) và lối Logout ở màn hẹp (Gap #6).
5. **Dọn sổ sách index:** thêm `@NeedsVerify` cho 5 TC dựa Assumption, `@TechCheck` cho 064, bỏ `@Smoke` của 005, sửa nhãn Password (*ký tự đặc biệt* → `➖`), Accessibility, Security ISO, ghi `106 biến thể · 11 mục Bảng kiểm`, ghi Integration *0 TC chạy được*.

> Muốn agent sửa tại chỗ các mục trên → chạy `/review-testcases FIX docs\testcases\login\web`. Lưu ý trước khi FIX: `docs/testcases/` hiện **chưa được git theo dõi** (`git status` báo `??`) — cần commit bộ TC trước để có mốc git ghi vào Nhật ký.
