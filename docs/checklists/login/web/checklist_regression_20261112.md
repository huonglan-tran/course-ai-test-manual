# Login — Checklist Regression (28 mục · ~40 phút)

**Loại:** Regression (module) — rà toàn bộ module Login trước khi bàn giao; lấy mục từ Vòng 1 + Vòng 2 + nhánh `Permission` của Vòng 3
**Nguồn:** TC-based — [`TEST_CASES_LOGIN_SUMMARY.md`](../../../testcases/login/TEST_CASES_LOGIN_SUMMARY.md) → [`part_01_web_smoke_form_login.md`](../../../testcases/login/web/parts/part_01_web_smoke_form_login.md) · [`part_02_web_forgot_phien.md`](../../../testcases/login/web/parts/part_02_web_forgot_phien.md) · [`part_03_web_technical_nonfunc.md`](../../../testcases/login/web/parts/part_03_web_technical_nonfunc.md)
**Requirements:** [`REQUIREMENTS_LOGIN_SUMMARY.md`](../../../requirements/login/REQUIREMENTS_LOGIN_SUMMARY.md) (bản 1.5)
**Scope:** Perfex CRM khu Admin · module `LOGIN` · nền tảng Web · role Admin · lập 12-11-2026
**Mã checklist:** `CL-regression_20261112` — theo tên file `checklist_regression_20261112.md`. Mã mục khi chạy = `CL-regression_20261112-<số mục 2 chữ số>` (VD mục 8 → `CL-regression_20261112-08`, mục 19 → `CL-regression_20261112-19`)

## Chuẩn bị trước khi chạy

| Hạng mục | Yêu cầu |
|---|---|
| Trình duyệt | Google Chrome, **cửa sổ ẩn danh mới**, rộng ≥ 1600px — không cookie, không mật khẩu đã lưu |
| Địa chỉ | Mọi đường dẫn tính trên `BASE_URL` trong `.env` |
| Tài khoản Admin | `EMAIL_ADMIN` · `PASSWORD_ADMIN` trong `.env` — 🔒 tự nhập, không ghi giá trị vào báo cáo |
| Dữ liệu lỗi | Chỉ dùng email `@auto.test` không tồn tại — **cấm** thử sai mật khẩu trên tài khoản thật (`RISK-LOGIN-01`) |
| Bộ đếm giờ | Thanh trên cùng của Dashboard **không** có bộ đếm giờ chấm công đang chạy (mục 10, 25, 28 — nếu có, Logout sẽ hỏi xác nhận, `AMB-LOGIN-17`) |
| Thứ tự | Chạy **đúng thứ tự** trong cùng một cửa sổ ẩn danh. Mục 7 chạy **trước** mục 8 (khi chưa đăng nhập). Mục mở đầu bằng "Đăng nhập" hoặc kết thúc bằng "Kết thúc: …" là mục đổi trạng thái đăng nhập — đọc kỹ trước khi chạy tiếp |

### Nhóm: Trang Login & Forgot Password (chưa đăng nhập)

| # | Trạng thái | Hạng mục kiểm tra | Kết quả kỳ vọng | Priority | REQ ID | TC ID liên quan |
|---|---|---|---|---|---|---|
| 1 | ✅ | Mở `/admin/authentication`, đối chiếu thành phần trang Login | Từ trên xuống: logo `ANHTESTER Automation Testing` · tiêu đề `Login` · ô `Email Address` (viền xanh, chọn sẵn) · ô `Password` · ô `Remember me` chưa tích · nút xanh `Login` · liên kết `Forgot Password?`; tiêu tab có chữ `Login`; không có khung đỏ | P2 | REQ-LOGIN-01, REQ-LOGIN-02, REQ-LOGIN-04, REQ-LOGIN-05 | CRM_LOGIN_TC_001 |
| 2 | ✅ | Gõ `Auto_20261112_x` vào ô Password | Hiện 15 dấu chấm tròn, không hiện chữ đã gõ; không có biểu tượng con mắt để hiện mật khẩu | P1 | REQ-LOGIN-03 | CRM_LOGIN_TC_002 |
| 3 | ✅ | Bấm liên kết `Forgot Password?` dưới nút Login | Tab hiện tại chuyển sang `/admin/authentication/forgot_password`; tiêu đề `Forgot Password`; có ô `Email Address` trống (không viền xanh) và nút xanh `Confirm` | P2 | REQ-LOGIN-07, REQ-LOGIN-36 | CRM_LOGIN_TC_003, CRM_LOGIN_TC_004 |

### Nhóm: Thông báo lỗi khi gửi form

| # | Trạng thái | Hạng mục kiểm tra | Kết quả kỳ vọng | Priority | REQ ID | TC ID liên quan |
|---|---|---|---|---|---|---|
| 4 | ✅ | Mở lại `/admin/authentication`, để trống cả hai ô, bấm `Login` | Vẫn ở trang Login; hiện **2** khung nền đỏ nhạt: `The Password field is required.` và `The Email Address field is required.` (không đối chiếu thứ tự); không vào Dashboard | P1 | REQ-LOGIN-10 | CRM_LOGIN_TC_011 |
| 5 | ✅ | Nhập Email `auto_smoke_20261112_01@auto.test`, Password `Auto_20261112_x`, bấm `Login` | Vẫn ở trang Login; hiện đúng **1** khung đỏ `Invalid email or password`; hai ô Email và Password trống lại; không vào Dashboard | P1 | REQ-LOGIN-12, REQ-LOGIN-14, REQ-LOGIN-15 | CRM_LOGIN_TC_015 |
| 6 | ✅ | Mở `/admin/authentication/forgot_password`, để trống ô Email, bấm `Confirm` | Vẫn ở trang Forgot Password; hiện đúng **1** khung đỏ `Email not found` | P2 | REQ-LOGIN-37 | CRM_LOGIN_TC_026 |

### Nhóm: Cổng truy cập, đăng nhập & đăng xuất

| # | Trạng thái | Hạng mục kiểm tra | Kết quả kỳ vọng | Priority | REQ ID | TC ID liên quan |
|---|---|---|---|---|---|---|
| 7 | ✅ | Khi chưa đăng nhập, gõ thẳng `/admin/clients` vào thanh địa chỉ, Enter | Dừng ở `/admin/authentication`, tiêu đề `Login`; không có khung đỏ; không thấy menu bên trái hay dữ liệu khách hàng | P1 | REQ-LOGIN-19 | CRM_LOGIN_TC_049 |
| 8 | ✅ | Đăng nhập tài khoản Admin trong `.env`, không tích `Remember me`, bấm `Login` | Không có khung đỏ; thanh địa chỉ `/admin/`; tiêu tab `Dashboard`; menu bên trái hiện; ảnh đại diện ở góc trên bên phải | P1 | REQ-LOGIN-17 | CRM_LOGIN_TC_006 |
| 9 | ✅ | Bấm ảnh đại diện ở góc trên bên phải | Menu thả xuống đúng 5 mục từ trên xuống: `My Profile` · `My Timesheets` · `Edit Profile` · `Language` (có mũi tên ►) · `Logout` | P2 | REQ-LOGIN-30 | CRM_LOGIN_TC_007 |
| 10 | ✅ | Bấm mục `Logout` trong menu ảnh đại diện | Đăng xuất ngay, không hộp thoại xác nhận; thanh địa chỉ `/admin/authentication`; tiêu đề `Login`; form hiện; không có khung đỏ | P1 | REQ-LOGIN-31 | CRM_LOGIN_TC_008 |

### Nhóm: Form Login — bắt buộc nhập & dữ liệu đầu vào

| # | Trạng thái | Hạng mục kiểm tra | Kết quả kỳ vọng | Priority | REQ ID | TC ID liên quan |
|---|---|---|---|---|---|---|
| 11 | ✅ | Để trống Email, nhập Password `Auto_20261112_x`, bấm `Login` | Vẫn ở `/admin/authentication`; hiện đúng **1** khung đỏ `The Email Address field is required.`; ô Password trống lại; không vào Dashboard | P1 | REQ-LOGIN-08 | CRM_LOGIN_TC_009 |
| 12 | ✅ | Nhập Email `auto_login_20261112_02@auto.test`, để trống Password, bấm `Login` | Vẫn ở trang Login; hiện đúng **1** khung đỏ `The Password field is required.`; ô Email trống lại (email vừa nhập không còn) | P1 | REQ-LOGIN-09, REQ-LOGIN-14 | CRM_LOGIN_TC_010 |
| 13 | ✅ | Nhập Email `abc` (thiếu `@`), Password `Auto_20261112_x`, bấm `Login` | Trình duyệt hiện bong bóng cảnh báo dưới ô Email (không đối chiếu nguyên văn); trang **không** nạp lại; không khung đỏ; ô Email còn `abc`, ô Password còn dấu chấm | P1 | REQ-LOGIN-11 | CRM_LOGIN_TC_014 |
| 14 | ✅ | Nhập Email `auto_login_20261112_03@auto.test`, Password `!@#$%^&*()_+-=[]{};:,.<>/?~`, bấm `Login` | Mọi ký tự hiện thành dấu chấm; trang nạp lại; hiện **1** khung đỏ `Invalid email or password`; không trang báo lỗi máy chủ | P2 | REQ-LOGIN-12 | CRM_LOGIN_TC_017 |

### Nhóm: Hành vi form Login

| # | Trạng thái | Hạng mục kiểm tra | Kết quả kỳ vọng | Priority | REQ ID | TC ID liên quan |
|---|---|---|---|---|---|---|
| 15 | ✅ | Bấm ô `Remember me` hai lần, rồi bấm ô Password, nhấn `Tab` và `Space` | Lần 1 tích, lần 2 bỏ tích; sau `Tab` ô Remember me có viền chọn; sau `Space` ô được tích; trang không gửi form | P2 | REQ-LOGIN-04 | CRM_LOGIN_TC_021 |
| 16 | ✅ | Nhập Email `auto_login_20261112_04@auto.test`, Password `Auto_20261112_x`, nhấn `Enter` trong ô Password | Form được gửi như bấm `Login`: trang nạp lại, hiện **1** khung đỏ `Invalid email or password` | P1 | REQ-LOGIN-16 | CRM_LOGIN_TC_022 |
| 17 | ✅ | Nhập Email `auto_login_20261112_05@auto.test`, Password `Auto_20261112_x`, bấm `Login` hai lần thật nhanh | Trang nạp lại xong, không treo; chỉ **1** khung đỏ `Invalid email or password` (không lặp 2 khung); không trang báo lỗi | P2 | REQ-LOGIN-12 | CRM_LOGIN_TC_024 |

### Nhóm: Forgot Password

| # | Trạng thái | Hạng mục kiểm tra | Kết quả kỳ vọng | Priority | REQ ID | TC ID liên quan |
|---|---|---|---|---|---|---|
| 18 | ✅ | Mở `/admin/authentication/forgot_password`, nhập Email `abc`, bấm `Confirm` | Trình duyệt hiện bong bóng cảnh báo dưới ô Email; trang **không** nạp lại; không khung đỏ của hệ thống; ô Email còn `abc` | P2 | REQ-LOGIN-38 | CRM_LOGIN_TC_027 |
| 19 | ❌ | Nhập Email `auto_forgot_20261112_01@auto.test` (không tồn tại), bấm `Confirm`, đọc thông báo | Hiện nguyên văn `Nếu địa chỉ email này có tài khoản trong hệ thống, chúng tôi đã gửi liên kết đặt lại mật khẩu tới đó. Vui lòng kiểm tra hộp thư (và cả thư mục Spam).`; **không** hiện `Email not found`. ⚠️ **Dự kiến FAIL** — hiện trạng báo `Email not found` (`AMB-LOGIN-03`, `@KnownBug`), chờ Dev sửa | P1 | REQ-LOGIN-39 | CRM_LOGIN_TC_028 |
| 20 | ✅ | Nhập Email `auto_forgot_20261112_02@auto.test`, bấm `Confirm`, nhìn ô Email sau khi trang nạp lại | Trang nạp lại, không trang báo lỗi máy chủ; ô Email vẫn chứa **đúng** chuỗi vừa nhập. Nội dung thông báo kiểm ở mục 19 | P2 | REQ-LOGIN-40 | CRM_LOGIN_TC_029 |

### Nhóm: Chuyển hướng theo trạng thái đăng nhập

| # | Trạng thái | Hạng mục kiểm tra | Kết quả kỳ vọng | Priority | REQ ID | TC ID liên quan |
|---|---|---|---|---|---|---|
| 21 | ✅ | Chưa đăng nhập, gõ `/admin/clients` rồi đăng nhập Admin `.env` ở trang Login hiện ra | Bị đưa về `/admin/authentication`; sau đăng nhập vào `/admin/` tiêu tab `Dashboard` — **không** phải trang Customers (`/admin/clients`). *Kết thúc: đang đăng nhập* | P2 | REQ-LOGIN-18 | CRM_LOGIN_TC_037 |
| 22 | ✅ | Đang đăng nhập, gõ `/admin/authentication` vào thanh địa chỉ, Enter | Không hiện form đăng nhập; thanh địa chỉ `/admin/`; tiêu tab `Dashboard`; menu bên trái hiện | P2 | REQ-LOGIN-20 | CRM_LOGIN_TC_038 |
| 23 | ✅ | Đang đăng nhập, gõ `/admin/authentication/forgot_password`, Enter | Không hiện form Forgot Password; thanh địa chỉ `/admin/`; tiêu tab `Dashboard`; menu bên trái hiện | P2 | REQ-LOGIN-21 | CRM_LOGIN_TC_039 |

### Nhóm: Phiên đăng nhập, đăng xuất & chặn truy cập

| # | Trạng thái | Hạng mục kiểm tra | Kết quả kỳ vọng | Priority | REQ ID | TC ID liên quan |
|---|---|---|---|---|---|---|
| 24 | ✅ | Mở tab B tại `/admin/clients`; ở tab A bấm `Logout`; ở tab B bấm `Dashboard` ở menu trái | Tab B hiện danh sách khách hàng trước khi Logout; sau Logout, tab B bị đưa về `/admin/authentication`, không mở được Dashboard. *Kết thúc: đã đăng xuất, đóng tab B* | P1 | REQ-LOGIN-32 | CRM_LOGIN_TC_047 |
| 25 | ✅ | Đăng nhập Admin `.env`, bấm `Logout`, rồi gõ thẳng `/admin/clients` vào thanh địa chỉ | Dừng ở `/admin/authentication`, tiêu đề `Login`; không thấy danh sách khách hàng hay menu bên trái | P1 | REQ-LOGIN-32 | CRM_LOGIN_TC_044 |
| 26 | ✅ | Ngay sau mục 25, bấm nút Back (←) của trình duyệt hai lần | Không hiện lại trang nội bộ; dừng ở `/admin/authentication`, tiêu đề `Login`; không có menu bên trái hay dữ liệu nội bộ | P1 | REQ-LOGIN-33 | CRM_LOGIN_TC_045 |
| 27 | ✅ | Khi chưa đăng nhập, gõ lần lượt `/admin/invoices/invoice` và `/admin/profile` | Mỗi đường dẫn đều dừng ở `/admin/authentication`, tiêu đề `Login`; không khung đỏ; không thấy nội dung trang nội bộ | P1 | REQ-LOGIN-19 | CRM_LOGIN_TC_049 |
| 28 | ✅ | Đăng nhập Admin `.env` có tích `Remember me`, bấm `Logout`, rồi gõ `/admin/` vào thanh địa chỉ | Vẫn ở trang Login (`/admin/authentication`) — **không** tự đăng nhập lại, không vào Dashboard | P1 | REQ-LOGIN-29 | CRM_LOGIN_TC_043 |

### Không áp dụng / ngoài phạm vi checklist này

- **Data Table / List · CRUD Lifecycle · Status Flow** — không áp dụng: module chỉ có gửi form xác thực, không có bản ghi hay danh sách.
- **Modal / Dialog** — Login và Forgot Password là trang riêng, không phải hộp thoại; hộp xác nhận Logout chỉ xuất hiện khi có bộ đếm giờ chạy (`CRM_LOGIN_TC_046` — `@Skip`, `AMB-LOGIN-17`).
- **Permission** — hệ thống chỉ có role Admin (`AMB-LOGIN-18`); phân quyền của module là loại tài khoản. Phần chạy được: chặn truy cập khi chưa đăng nhập (mục 7, 27) và chuyển hướng khi đã đăng nhập (mục 22, 23). Contact / staff vô hiệu hoá (`CRM_LOGIN_TC_050` · `051`) — `@Skip`, chưa có tài khoản test.
- **Cần DevTools hoặc thao tác nhiều bước — chạy bằng bộ TC, không đưa vào checklist tick nhanh:** Remember me cấp/dùng cookie (`CRM_LOGIN_TC_040` · `041` · `042`) · dán/sao chép ô Password (`023`) · mất mạng giữa chừng (`025`) · chuỗi 256–1000 ký tự (`016` · `017-b` · `c`) · cookie phiên, mã chống giả mạo, SQLi/XSS, phiên mới sau Logout (`052` → `061`).
- **Chờ evidence (`@NeedsVerify`):** khoảng trắng đầu/cuối (`012` · `013` · `020`) · logo trỏ về trang chủ (`005`).
- **Vòng 4 Non-functional** (bàn phím, responsive, Edge/Firefox — `062` → `065`) — ngoài phạm vi Regression module.
- **Chờ điều kiện (`@Skip` / `@PersonalOnly`):** quên mật khẩu qua email (`030` → `034`) · sai mật khẩu tài khoản thật (`035`) · phiên hết hạn sau 8 giờ (`048`) · khoá tài khoản sau nhiều lần sai — ticket `CRM-LOGIN-101`, **chưa triển khai** (`036` · `066` → `083`).

## Kết quả chạy — `run_1794460519`

> Nguồn: [`execution_report.md`](../../../executions/login/web/run_1794460519/execution_report.md) (12-11-2026, Chromium, web). Run này chạy theo **Checklist regression** (`Loại nguồn: Checklist regression · CL-regression_20261112`) — mã mục là mã gốc `CL-regression_20261112-<nn>`. Cột Trạng thái ở các bảng trên đã đánh dấu theo kết quả: ✅ = PASS · ❌ = FAIL (mục 19); chi tiết ghi ở bảng dưới.
>
> **Tổng: 27 PASS · 1 FAIL · 0 BLOCKED · 0 SKIPPED** (27/28 = 96,4%).

| # | Mã mục | TC ID liên quan | Kết quả | Ghi chú |
|---|---|---|---|---|
| 1 | `CL-regression_20261112-01` | CRM_LOGIN_TC_001 | ✅ PASS | Đủ thành phần; [ảnh](../../../executions/login/web/run_1794460519/evidence/CL-regression_20261112-01_trang_login.png) |
| 2 | `CL-regression_20261112-02` | CRM_LOGIN_TC_002 | ✅ PASS | `type=password`, 15 ký tự, không biểu tượng hiện mật khẩu |
| 3 | `CL-regression_20261112-03` | CRM_LOGIN_TC_003, 004 | ✅ PASS | |
| 4 | `CL-regression_20261112-04` | CRM_LOGIN_TC_011 | ✅ PASS | 2 khung đỏ |
| 5 | `CL-regression_20261112-05` | CRM_LOGIN_TC_015 | ✅ PASS | |
| 6 | `CL-regression_20261112-06` | CRM_LOGIN_TC_026 | ✅ PASS | `Email not found` |
| 7 | `CL-regression_20261112-07` | CRM_LOGIN_TC_049 | ✅ PASS | |
| 8 | `CL-regression_20261112-08` | CRM_LOGIN_TC_006 | ✅ PASS | Tester đăng nhập thủ công |
| 9 | `CL-regression_20261112-09` | CRM_LOGIN_TC_007 | ✅ PASS | 5 mục menu; [ảnh](../../../executions/login/web/run_1794460519/evidence/CL-regression_20261112-09_menu_avatar.png) |
| 10 | `CL-regression_20261112-10` | CRM_LOGIN_TC_008 | ✅ PASS | Không hộp thoại xác nhận |
| 11 | `CL-regression_20261112-11` | CRM_LOGIN_TC_009 | ✅ PASS | |
| 12 | `CL-regression_20261112-12` | CRM_LOGIN_TC_010 | ✅ PASS | |
| 13 | `CL-regression_20261112-13` | CRM_LOGIN_TC_014 | ✅ PASS | Làm lại trên trang sạch (lần đầu còn khung đỏ cũ của mục 12) |
| 14 | `CL-regression_20261112-14` | CRM_LOGIN_TC_017 | ✅ PASS | |
| 15 | `CL-regression_20261112-15` | CRM_LOGIN_TC_021 | ✅ PASS | |
| 16 | `CL-regression_20261112-16` | CRM_LOGIN_TC_022 | ✅ PASS | |
| 17 | `CL-regression_20261112-17` | CRM_LOGIN_TC_024 | ✅ PASS | Chỉ 1 khung đỏ |
| 18 | `CL-regression_20261112-18` | CRM_LOGIN_TC_027 | ✅ PASS | |
| 19 | `CL-regression_20261112-19` | CRM_LOGIN_TC_028 | ❌ FAIL | `@KnownBug` `AMB-LOGIN-03` — hiện `Email not found`, Expected là thông báo trung tính (FAIL #1); [ảnh](../../../executions/login/web/run_1794460519/evidence/CL-regression_20261112-19_email_not_found.png) |
| 20 | `CL-regression_20261112-20` | CRM_LOGIN_TC_029 | ✅ PASS | Giữ nguyên chuỗi email. Thông báo vẫn `Email not found` (lỗi của mục 19) |
| 21 | `CL-regression_20261112-21` | CRM_LOGIN_TC_037 | ✅ PASS | Tester đăng nhập thủ công |
| 22 | `CL-regression_20261112-22` | CRM_LOGIN_TC_038 | ✅ PASS | |
| 23 | `CL-regression_20261112-23` | CRM_LOGIN_TC_039 | ✅ PASS | |
| 24 | `CL-regression_20261112-24` | CRM_LOGIN_TC_047 | ✅ PASS | |
| 25 | `CL-regression_20261112-25` | CRM_LOGIN_TC_044 | ✅ PASS | Tester đăng nhập thủ công |
| 26 | `CL-regression_20261112-26` | CRM_LOGIN_TC_045 | ✅ PASS | |
| 27 | `CL-regression_20261112-27` | CRM_LOGIN_TC_049 | ✅ PASS | Cùng TC với mục 7 |
| 28 | `CL-regression_20261112-28` | CRM_LOGIN_TC_043 | ✅ PASS | Tích Remember me; cookie `autologin` còn nhưng không dùng để tự đăng nhập lại |

**Các điểm cần lưu ý khi đọc kết quả:**

- **Mục 19 là lỗi đã biết** (`@KnownBug`) — FAIL là đúng kỳ vọng cho tới khi Dev sửa REQ-39.
- Mục 8, 21, 25, 28 cần đăng nhập — agent không tự nhập mật khẩu, tester đăng nhập thủ công trên cửa sổ Playwright.
- Checklist **không** bảo vệ biến thể `015-b` và TC `016` (FAIL ở lần chạy bộ TC `run_1794453634`). Muốn bảo vệ ở Regression cần thêm mục hoặc xử lý trước khi báo bug.
- Lần chạy trước theo **Bộ TC** ([`run_1794453634`](../../../executions/login/web/run_1794453634/execution_report.md), 83 TC) cho cùng kết quả 27 PASS · 1 FAIL trên 28 mục này.

### Ký nhận

| Môi trường | Build / Version | Người thực hiện | Ngày | Kết quả (Pass/Fail/Blocked) |
|---|---|---|---|---|
|  |  |  |  |  |
