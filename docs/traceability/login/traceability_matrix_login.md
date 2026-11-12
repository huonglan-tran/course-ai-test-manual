# Ma Trận Truy Vết (RTM) — Login (`LOGIN`)

> **Tóm tắt:** 68/68 REQ đã có TC · **0/83 TC có script automation** · lần chạy gần nhất 49 PASS / 4 FAIL, 30 TC chưa có kết quả · 19 REQ khoá tài khoản (`CRM-LOGIN-101`) chưa triển khai — mới phủ trên giấy

| Thông tin | Nội dung |
|---|---|
| Ngày sinh | 12-11-2026 |
| Phạm vi | Module `LOGIN` · web |
| Mốc git | `c816e8a` — TC/script sửa sau mốc này thì RTM đã cũ |
| Nguồn đã quét | REQ: `requirements/login/` (68 REQ) · TC: `testcases/login/web/parts/` (4 part · 83 TC) · Automation: **không tìm thấy script** — ẩn cột `Automation` · Checklist: `checklist_regression_20261112.md` (28 mục) · Kết quả chạy: `run_1794453634` (bộ TC) — lần chạy checklist `run_1794460519` không tính · Bug: `docs/bugs/README.md` |
| Tổng hợp | [../traceability_matrix.md](../traceability_matrix.md) — ⬛ module ngoài tầm nhìn của RTM nằm ở đó |

## 1. Tổng quan

| Chỉ số | Tiến độ | Giá trị |
|---|---|---|
| Requirement Coverage | `▰▰▰▰▰▰▰▰▰▰` | **68/68** (100%) REQ |
| Platform Coverage · web | `▰▰▰▰▰▰▰▰▰▰` | 68/68 cặp REQ × nền tảng |
| Automation Coverage | `▱▱▱▱▱▱▱▱▱▱` | **0/83** TC (0%) |
| Pass rate lần chạy gần nhất | `▰▰▰▰▰▰▰▰▰▱` | 49/53 (92%) · ❌ 4 FAIL · 30 TC chưa có kết quả (26 BLOCKED · 4 SKIPPED) |

**Phân bổ REQ theo trạng thái:**

| ✅ Đã automate | 🟣 Automate một phần | 🟡 Thủ công | 🟠 Chỉ checklist | 🔴 Chưa có TC | ⏭️ Loại trừ |
|:-:|:-:|:-:|:-:|:-:|:-:|
| 0 | 0 | 68 | 0 | 0 | 0 |

## 2. Việc cần làm

1. ❌ **4 TC FAIL chưa có bug** — `CRM_LOGIN_TC_028` là lỗi đã biết (REQ-39, `AMB-LOGIN-03`) chờ quyết mở bug · `CRM_LOGIN_TC_015`-b, `016`, `053`-d (REQ-12) chờ PO chốt hành vi mong đợi → `/review-testcases` mode FIX hoặc `/create-bug-report`
2. ▶️ **30 TC chưa có kết quả** — 19 TC khoá tài khoản (`036`, `066 → 083`) chờ Dev triển khai `CRM-LOGIN-101` lên demo · 11 TC thiếu điều kiện chạy (tài khoản staff/contact test, hộp thư, bộ đếm giờ, Edge/Firefox, phiên 8 giờ) → cấp điều kiện rồi `/execute-test-cases`
3. 🐞 **Bug `BUG_login_1792569135_TC005` đang mở nhưng TC005 PASS** — TC chỉ assert `href="/"`, bug nói đích cuối là `/authentication/login` → PO chốt đóng bug hay sửa Expected TC005
4. 🏷️ **9 TC `@NeedsVerify` đã xác nhận trên UI nhưng chưa gỡ tag** (`005`, `012`, `013`, `020`, `058`, `061`, `062`, `063`, `064`) → `/review-testcases` mode FIX
5. 🟡 **Bắt đầu automate:** 26 TC Critical/High `Automation = Yes` đang chạy được (bỏ `@Skip`), ưu tiên `@Smoke` · `@CriticalPath` → `/generate-automation-from-testcases`

## 3. Ma trận chi tiết

### 3.1. Giao diện trang Login (STORY-LOGIN-01)

`▰▰▰▰▰` 7/7 REQ có TC

| REQ ID | Yêu cầu | TC IDs | Kết quả gần nhất | Trạng thái |
|---|---|---|:-:|---|
| REQ-LOGIN-01 | Trang Login có đủ thành phần form đăng nhập | CRM_LOGIN_TC_001, CRM_LOGIN_TC_064 | ✅ 2 | 🟡 Thủ công |
| REQ-LOGIN-02 | Ô Email Address được focus sẵn khi mở trang | CRM_LOGIN_TC_001, CRM_LOGIN_TC_062 | ✅ 2 | 🟡 Thủ công |
| REQ-LOGIN-03 | Ô Password che ký tự nhập | CRM_LOGIN_TC_002, CRM_LOGIN_TC_023 | ✅ 2 | 🟡 Thủ công |
| REQ-LOGIN-04 | Checkbox Remember me mặc định không được tick | CRM_LOGIN_TC_001, CRM_LOGIN_TC_021 | ✅ 2 | 🟡 Thủ công |
| REQ-LOGIN-05 | Tiêu đề tab trang Login chứa "Login" | CRM_LOGIN_TC_001 | ✅ 1 | 🟡 Thủ công |
| REQ-LOGIN-06 | Logo trên trang Login liên kết về trang chủ "/" | CRM_LOGIN_TC_005 | ✅ 1 | 🟡 Thủ công |
| REQ-LOGIN-07 | Link "Forgot Password?" mở trang Forgot Password | CRM_LOGIN_TC_004 | ✅ 1 | 🟡 Thủ công |

### 3.2. Validation & xử lý lỗi đăng nhập (STORY-LOGIN-02)

`▰▰▰▰▰` 11/11 REQ có TC

| REQ ID | Yêu cầu | TC IDs | Kết quả gần nhất | Trạng thái |
|---|---|---|:-:|---|
| REQ-LOGIN-08 | Bỏ trống Email Address bị máy chủ từ chối kèm thông báo bắt buộc | CRM_LOGIN_TC_009, CRM_LOGIN_TC_012 | ✅ 2 | 🟡 Thủ công |
| REQ-LOGIN-09 | Bỏ trống Password bị máy chủ từ chối kèm thông báo bắt buộc | CRM_LOGIN_TC_010, CRM_LOGIN_TC_013 | ✅ 2 | 🟡 Thủ công |
| REQ-LOGIN-10 | Bỏ trống cả hai ô hiển thị đồng thời cả hai thông báo bắt buộc | CRM_LOGIN_TC_011 | ✅ 1 | 🟡 Thủ công |
| REQ-LOGIN-11 | Email sai định dạng bị trình duyệt chặn, không gửi request | CRM_LOGIN_TC_014, CRM_LOGIN_TC_065 | ✅ 1 · 🚫 1 | 🟡 Thủ công |
| REQ-LOGIN-12 | Email không tồn tại bị từ chối với thông báo chung | CRM_LOGIN_TC_015, CRM_LOGIN_TC_016, CRM_LOGIN_TC_017, CRM_LOGIN_TC_024, CRM_LOGIN_TC_025, CRM_LOGIN_TC_052, CRM_LOGIN_TC_053 | ✅ 4 · ❌ 3 | 🟡 Thủ công |
| REQ-LOGIN-13 | Sai mật khẩu với email tồn tại nhận cùng thông báo chung | CRM_LOGIN_TC_035 | 🚫 1 | 🟡 Thủ công |
| REQ-LOGIN-14 | Máy chủ không điền lại email đã nhập sau khi đăng nhập lỗi | CRM_LOGIN_TC_010, CRM_LOGIN_TC_018 | ✅ 2 | 🟡 Thủ công |
| REQ-LOGIN-15 | Ô Password được xoá trống sau khi đăng nhập lỗi | CRM_LOGIN_TC_019 | ✅ 1 | 🟡 Thủ công |
| REQ-LOGIN-16 | Nhấn Enter trong ô Password gửi form đăng nhập | CRM_LOGIN_TC_022 | ✅ 1 | 🟡 Thủ công |
| REQ-LOGIN-44 | Contact đăng nhập khu Admin bị từ chối như email không tồn tại | CRM_LOGIN_TC_050 | 🚫 1 | 🟡 Thủ công |
| REQ-LOGIN-48 | Staff bị vô hiệu hoá không đăng nhập được | CRM_LOGIN_TC_051 | 🚫 1 | 🟡 Thủ công |

### 3.3. Đăng nhập thành công & chặn truy cập (STORY-LOGIN-03)

`▰▰▰▰▰` 10/10 REQ có TC

| REQ ID | Yêu cầu | TC IDs | Kết quả gần nhất | Trạng thái |
|---|---|---|:-:|---|
| REQ-LOGIN-17 | Đăng nhập thành công chuyển tới Dashboard | CRM_LOGIN_TC_006, CRM_LOGIN_TC_020, CRM_LOGIN_TC_065 | ✅ 2 · 🚫 1 | 🟡 Thủ công |
| REQ-LOGIN-18 | Sau đăng nhập không quay lại trang đã định truy cập | CRM_LOGIN_TC_037 | ✅ 1 | 🟡 Thủ công |
| REQ-LOGIN-19 | Chưa đăng nhập truy cập trang nội bộ bị chuyển về trang Login | CRM_LOGIN_TC_049 | ✅ 1 | 🟡 Thủ công |
| REQ-LOGIN-20 | Đã đăng nhập mở trang Login bị chuyển về Dashboard | CRM_LOGIN_TC_038 | ✅ 1 | 🟡 Thủ công |
| REQ-LOGIN-21 | Đã đăng nhập mở trang Forgot Password bị chuyển về Dashboard | CRM_LOGIN_TC_039 | ✅ 1 | 🟡 Thủ công |
| REQ-LOGIN-22 | Cookie phiên được bảo vệ bằng cờ HttpOnly, Secure, SameSite=Lax | CRM_LOGIN_TC_055 | ✅ 1 | 🟡 Thủ công |
| REQ-LOGIN-23 | Hạn cookie phiên luôn tính từ lần tải trang gần nhất cộng 8 giờ | CRM_LOGIN_TC_056 | ✅ 1 | 🟡 Thủ công |
| REQ-LOGIN-24 | Form Login gửi kèm trường ẩn CSRF token | CRM_LOGIN_TC_057, CRM_LOGIN_TC_061 | ✅ 2 | 🟡 Thủ công |
| REQ-LOGIN-25 | Máy chủ từ chối submit Login thiếu hoặc sai CSRF token | CRM_LOGIN_TC_058 | ✅ 1 | 🟡 Thủ công |
| REQ-LOGIN-47 | Phiên hết hạn sau 8 giờ không hoạt động | CRM_LOGIN_TC_048 | ⏩ 1 | 🟡 Thủ công |

### 3.4. Remember me (STORY-LOGIN-04)

`▰▰▰▰▰` 4/4 REQ có TC

| REQ ID | Yêu cầu | TC IDs | Kết quả gần nhất | Trạng thái |
|---|---|---|:-:|---|
| REQ-LOGIN-26 | Tick Remember me khi đăng nhập tạo cookie ghi nhớ | CRM_LOGIN_TC_040 | ✅ 1 | 🟡 Thủ công |
| REQ-LOGIN-27 | Cookie ghi nhớ tự đăng nhập lại khi mất cookie phiên | CRM_LOGIN_TC_041 | ✅ 1 | 🟡 Thủ công |
| REQ-LOGIN-28 | Không tick Remember me thì không tạo cookie ghi nhớ | CRM_LOGIN_TC_042 | ✅ 1 | 🟡 Thủ công |
| REQ-LOGIN-29 | Đăng xuất vô hiệu cookie ghi nhớ | CRM_LOGIN_TC_043 | ✅ 1 | 🟡 Thủ công |

### 3.5. Đăng xuất (STORY-LOGIN-05)

`▰▰▰▰▰` 6/6 REQ có TC

| REQ ID | Yêu cầu | TC IDs | Kết quả gần nhất | Trạng thái |
|---|---|---|:-:|---|
| REQ-LOGIN-30 | Menu avatar trên header có mục Logout | CRM_LOGIN_TC_007 | ✅ 1 | 🟡 Thủ công |
| REQ-LOGIN-31 | Bấm Logout đưa người dùng về trang Login | CRM_LOGIN_TC_008 | ✅ 1 | 🟡 Thủ công |
| REQ-LOGIN-32 | Sau đăng xuất, trang nội bộ bị chặn | CRM_LOGIN_TC_044, CRM_LOGIN_TC_047 | ✅ 2 | 🟡 Thủ công |
| REQ-LOGIN-33 | Sau đăng xuất, nút Back không hiển thị lại trang nội bộ | CRM_LOGIN_TC_045 | ✅ 1 | 🟡 Thủ công |
| REQ-LOGIN-34 | Đăng xuất cấp cookie phiên mới | CRM_LOGIN_TC_060 | ✅ 1 | 🟡 Thủ công |
| REQ-LOGIN-35 | Có timer chấm công đang chạy thì Logout hiện cảnh báo xác nhận | CRM_LOGIN_TC_046 | 🚫 1 | 🟡 Thủ công |

### 3.6. Forgot Password (STORY-LOGIN-06)

`▰▰▰▰▰` 11/11 REQ có TC

| REQ ID | Yêu cầu | TC IDs | Kết quả gần nhất | Trạng thái |
|---|---|---|:-:|---|
| REQ-LOGIN-36 | Trang Forgot Password có đủ thành phần form | CRM_LOGIN_TC_003, CRM_LOGIN_TC_063 | ✅ 2 | 🟡 Thủ công |
| REQ-LOGIN-37 | Bỏ trống email ở Forgot Password nhận thông báo "Email not found" | CRM_LOGIN_TC_026 | ✅ 1 | 🟡 Thủ công |
| REQ-LOGIN-38 | Email sai định dạng ở Forgot Password bị trình duyệt chặn | CRM_LOGIN_TC_027 | ✅ 1 | 🟡 Thủ công |
| REQ-LOGIN-39 | Email không tồn tại ở Forgot Password nhận thông báo trung tính, không tiết lộ email có tài khoản hay không | CRM_LOGIN_TC_028 | ❌ 1 | 🟡 Thủ công |
| REQ-LOGIN-40 | Email đã nhập được giữ lại sau khi Forgot Password báo lỗi | CRM_LOGIN_TC_029, CRM_LOGIN_TC_054 | ✅ 2 | 🟡 Thủ công |
| REQ-LOGIN-41 | Form Forgot Password gửi kèm trường ẩn CSRF token | CRM_LOGIN_TC_059 | ✅ 1 | 🟡 Thủ công |
| REQ-LOGIN-42 | Email tồn tại thì hệ thống gửi email đặt lại mật khẩu | CRM_LOGIN_TC_031 | 🚫 1 | 🟡 Thủ công |
| REQ-LOGIN-43 | Link trong email đặt lại mật khẩu dùng được để đặt mật khẩu mới | CRM_LOGIN_TC_032 | 🚫 1 | 🟡 Thủ công |
| REQ-LOGIN-45 | Contact dùng Forgot Password khu Admin được xử lý như email không tồn tại | CRM_LOGIN_TC_033 | 🚫 1 | 🟡 Thủ công |
| REQ-LOGIN-49 | Email tồn tại ở Forgot Password nhận cùng thông báo trung tính như email không tồn tại | CRM_LOGIN_TC_030 | 🚫 1 | 🟡 Thủ công |
| REQ-LOGIN-50 | Staff bị vô hiệu hoá dùng Forgot Password được xử lý như email không tồn tại | CRM_LOGIN_TC_034 | 🚫 1 | 🟡 Thủ công |

### 3.7. Khoá tài khoản khi đăng nhập sai nhiều lần (STORY-LOGIN-07 · ticket `CRM-LOGIN-101`)

`▰▰▰▰▰` 19/19 REQ có TC · 18 REQ chưa implement, REQ-46 chưa triển khai lên demo

| REQ ID | Yêu cầu | TC IDs | Kết quả gần nhất | Trạng thái |
|---|---|---|:-:|---|
| REQ-LOGIN-46 | Đăng nhập sai mật khẩu 5 lần liên tiếp với cùng một email thì tài khoản bị khoá | CRM_LOGIN_TC_036 | 🚫 1 | 🟡 Thủ công |
| REQ-LOGIN-51 | Tài khoản đang bị khoá thì trang Login hiển thị thông báo khoá | CRM_LOGIN_TC_066 | 🚫 1 | 🟡 Thủ công · ⚪ REQ chưa implement |
| REQ-LOGIN-52 | Đang bị khoá thì nhập đúng mật khẩu vẫn không đăng nhập được | CRM_LOGIN_TC_067 | 🚫 1 | 🟡 Thủ công · ⚪ REQ chưa implement |
| REQ-LOGIN-53 | Hết 15 phút khoá thì đăng nhập đúng thành công trở lại | CRM_LOGIN_TC_068 | ⏩ 1 | 🟡 Thủ công · ⚪ REQ chưa implement |
| REQ-LOGIN-54 | Đăng nhập thành công trước khi đủ 5 lần sai đặt lại bộ đếm về 0 | CRM_LOGIN_TC_069 | 🚫 1 | 🟡 Thủ công · ⚪ REQ chưa implement |
| REQ-LOGIN-55 | Khoá một email không chặn email khác đăng nhập từ cùng máy, cùng IP | CRM_LOGIN_TC_070 | 🚫 1 | 🟡 Thủ công · ⚪ REQ chưa implement |
| REQ-LOGIN-56 | Email đang bị khoá vẫn bị khoá khi đăng nhập từ IP khác | CRM_LOGIN_TC_071 | 🚫 1 | 🟡 Thủ công · ⚪ REQ chưa implement |
| REQ-LOGIN-57 | Email không có trong hệ thống không bị khoá dù gửi sai nhiều lần | CRM_LOGIN_TC_072 | 🚫 1 | 🟡 Thủ công · ⚪ REQ chưa implement |
| REQ-LOGIN-58 | Bộ đếm lần sai không phân biệt hoa/thường của email | CRM_LOGIN_TC_073 | 🚫 1 | 🟡 Thủ công · ⚪ REQ chưa implement |
| REQ-LOGIN-59 | Thử đăng nhập trong lúc đang bị khoá không kéo dài thời gian khoá | CRM_LOGIN_TC_074 | ⏩ 1 | 🟡 Thủ công · ⚪ REQ chưa implement |
| REQ-LOGIN-60 | Hết thời gian khoá thì bộ đếm lần sai về 0 | CRM_LOGIN_TC_075 | ⏩ 1 | 🟡 Thủ công · ⚪ REQ chưa implement |
| REQ-LOGIN-61 | Gửi form bỏ trống Password không tính vào bộ đếm lần sai | CRM_LOGIN_TC_076 | 🚫 1 | 🟡 Thủ công · ⚪ REQ chưa implement |
| REQ-LOGIN-62 | Tài khoản đang bị khoá vẫn dùng được Forgot Password | CRM_LOGIN_TC_077 | 🚫 1 | 🟡 Thủ công · ⚪ REQ chưa implement |
| REQ-LOGIN-63 | Đặt lại mật khẩu không mở khoá tài khoản sớm | CRM_LOGIN_TC_078 | 🚫 1 | 🟡 Thủ công · ⚪ REQ chưa implement |
| REQ-LOGIN-64 | Khoá tài khoản không đẩy phiên đang mở ra ngoài | CRM_LOGIN_TC_079 | 🚫 1 | 🟡 Thủ công · ⚪ REQ chưa implement |
| REQ-LOGIN-65 | Cookie ghi nhớ vẫn tự đăng nhập khi tài khoản đang bị khoá | CRM_LOGIN_TC_080 | 🚫 1 | 🟡 Thủ công · ⚪ REQ chưa implement |
| REQ-LOGIN-66 | Email của contact không bị khoá dù gửi sai nhiều lần | CRM_LOGIN_TC_081 | 🚫 1 | 🟡 Thủ công · ⚪ REQ chưa implement |
| REQ-LOGIN-67 | Staff bị vô hiệu hoá không bị khoá dù gửi sai nhiều lần | CRM_LOGIN_TC_082 | 🚫 1 | 🟡 Thủ công · ⚪ REQ chưa implement |
| REQ-LOGIN-68 | Bộ đếm lần sai bỏ khoảng trắng đầu/cuối của email | CRM_LOGIN_TC_083 | 🚫 1 | 🟡 Thủ công · ⚪ REQ chưa implement |

## 4. Kết quả thực thi từng TC

Nguồn: [`run_1794453634`](../../executions/login/web/run_1794453634/execution_report.md) — bộ TC, chạy 12-11-2026 (2 đợt), 83/83 TC có kết quả (49 PASS · 4 FAIL · 26 BLOCKED · 4 SKIPPED). Chưa có `retest_*` nào cho `LOGIN`; lần chạy checklist `run_1794460519` không tính. Mỗi TC chỉ có một lần chạy nên kết quả dưới đây cũng là kết quả mới nhất.

| Mức ưu tiên | ✅ PASS | ❌ FAIL | 🚫 BLOCKED | ⏩ SKIPPED | Tổng |
|---|:-:|:-:|:-:|:-:|:-:|
| Critical | 3 | 0 | 1 | 0 | 4 |
| High | 20 | 3 | 18 | 2 | 43 |
| Medium | 24 | 1 | 7 | 2 | 34 |
| Low | 2 | 0 | 0 | 0 | 2 |
| **Tổng** | **49** | **4** | **26** | **4** | **83** |

*FAIL chi tiết (bước tái hiện, Expected/Actual) xem mục 3 của [execution report](../../executions/login/web/run_1794453634/execution_report.md#3-chi-tiết-tc-fail) theo số `FAIL #n` ở cột Ghi chú. Thư mục evidence của lần chạy có 16 ảnh, chủ yếu cho TC FAIL và TC smoke — TC không có ảnh ghi `—`.*

<details><summary>Kết quả đầy đủ 83 TC (xếp theo TC ID)</summary>

| TC ID | REQ | Kết quả | Bước fail | Ghi chú | Evidence |
|---|---|:-:|---|---|---|
| CRM_LOGIN_TC_001 | 01 · 02 · 04 · 05 | ✅ PASS | — | — | [ảnh](../../executions/login/web/run_1794453634/evidence/CRM_LOGIN_TC_001_login_mac_dinh.png) |
| CRM_LOGIN_TC_002 | 03 | ✅ PASS | — | — | [ảnh](../../executions/login/web/run_1794453634/evidence/CRM_LOGIN_TC_002_o_password_che.png) |
| CRM_LOGIN_TC_003 | 36 | ✅ PASS | — | — | [ảnh](../../executions/login/web/run_1794453634/evidence/CRM_LOGIN_TC_003_forgot_mac_dinh.png) |
| CRM_LOGIN_TC_004 | 07 | ✅ PASS | — | — | — |
| CRM_LOGIN_TC_005 | 06 | ✅ PASS | — | — | [ảnh](../../executions/login/web/run_1794453634/evidence/CRM_LOGIN_TC_005_trang_dich_logo.png) |
| CRM_LOGIN_TC_006 | 17 | ✅ PASS | — | — | [ảnh](../../executions/login/web/run_1794453634/evidence/CRM_LOGIN_TC_006_header_dashboard.png) |
| CRM_LOGIN_TC_007 | 30 | ✅ PASS | — | — | [ảnh](../../executions/login/web/run_1794453634/evidence/CRM_LOGIN_TC_007_menu_avatar.png) |
| CRM_LOGIN_TC_008 | 31 | ✅ PASS | — | — | [ảnh](../../executions/login/web/run_1794453634/evidence/CRM_LOGIN_TC_008_sau_logout.png) |
| CRM_LOGIN_TC_009 | 08 | ✅ PASS | — | — | — |
| CRM_LOGIN_TC_010 | 09 · 14 | ✅ PASS | — | — | — |
| CRM_LOGIN_TC_011 | 10 | ✅ PASS | — | — | [ảnh](../../executions/login/web/run_1794453634/evidence/CRM_LOGIN_TC_011_hai_khung_do.png) |
| CRM_LOGIN_TC_012 | 08 | ✅ PASS | — | — | — |
| CRM_LOGIN_TC_013 | 09 | ✅ PASS | — | — | — |
| CRM_LOGIN_TC_014 | 11 | ✅ PASS | — | — | — |
| CRM_LOGIN_TC_015 | 12 | ❌ FAIL | 5 · biến thể `b` | Biến thể `a`, `c`, `d` PASS. Xem FAIL #1 | [ảnh](../../executions/login/web/run_1794453634/evidence/CRM_LOGIN_TC_015_bien_the_b_thong_bao.png) |
| CRM_LOGIN_TC_016 | 12 | ❌ FAIL | 6 · biến thể `a`, `b`, `c`, `d` | Không lỗi trang, đuôi `@auto.test` còn nguyên, nhưng thông báo khác Expected ở cả 4 biến thể. Xem FAIL #2 | [ảnh](../../executions/login/web/run_1794453634/evidence/CRM_LOGIN_TC_016_bien_the_a_thong_bao.png) |
| CRM_LOGIN_TC_017 | 12 | ✅ PASS | — | — | — |
| CRM_LOGIN_TC_018 | 14 | ✅ PASS | — | — | — |
| CRM_LOGIN_TC_019 | 15 | ✅ PASS | — | — | — |
| CRM_LOGIN_TC_020 | 17 | ✅ PASS | — | — | — |
| CRM_LOGIN_TC_021 | 04 | ✅ PASS | — | — | — |
| CRM_LOGIN_TC_022 | 16 | ✅ PASS | — | — | — |
| CRM_LOGIN_TC_023 | 03 | ✅ PASS | — | — | — |
| CRM_LOGIN_TC_024 | 12 | ✅ PASS | — | — | — |
| CRM_LOGIN_TC_025 | 12 | ✅ PASS | — | — | [ảnh](../../executions/login/web/run_1794453634/evidence/CRM_LOGIN_TC_025_mat_mang.png) |
| CRM_LOGIN_TC_026 | 37 | ✅ PASS | — | — | — |
| CRM_LOGIN_TC_027 | 38 | ✅ PASS | — | — | — |
| CRM_LOGIN_TC_028 | 39 | ❌ FAIL | 4 | `@KnownBug` — dự kiến FAIL (`AMB-LOGIN-03`). Xem FAIL #3 | [ảnh](../../executions/login/web/run_1794453634/evidence/CRM_LOGIN_TC_028_email_not_found.png) |
| CRM_LOGIN_TC_029 | 40 | ✅ PASS | — | — | — |
| CRM_LOGIN_TC_030 | 49 | 🚫 BLOCKED | — | `@Skip` — cần tài khoản staff test riêng (`AMB-LOGIN-01`) | — |
| CRM_LOGIN_TC_031 | 42 | 🚫 BLOCKED | — | `@Skip` — cần tài khoản staff test riêng có hộp thư đọc được | — |
| CRM_LOGIN_TC_032 | 43 | 🚫 BLOCKED | — | `@Skip` — phụ thuộc 031, cần hộp thư | — |
| CRM_LOGIN_TC_033 | 45 | 🚫 BLOCKED | — | `@Skip` — cần tài khoản contact có hộp thư | — |
| CRM_LOGIN_TC_034 | 50 | 🚫 BLOCKED | — | `@Skip` — cần staff vô hiệu hoá có hộp thư | — |
| CRM_LOGIN_TC_035 | 13 | 🚫 BLOCKED | — | `@Skip` — cần tài khoản staff test riêng, cấm thử sai trên tài khoản `.env` | — |
| CRM_LOGIN_TC_036 | 46 | 🚫 BLOCKED | — | `@Skip` — tính năng khoá (`CRM-LOGIN-101`) chưa xác nhận triển khai; chạy sẽ khoá tài khoản Project Manager dùng chung 15 phút | — |
| CRM_LOGIN_TC_037 | 18 | ✅ PASS | — | — | — |
| CRM_LOGIN_TC_038 | 20 | ✅ PASS | — | — | — |
| CRM_LOGIN_TC_039 | 21 | ✅ PASS | — | — | — |
| CRM_LOGIN_TC_040 | 26 | ✅ PASS | — | — | — |
| CRM_LOGIN_TC_041 | 27 | ✅ PASS | — | — | — |
| CRM_LOGIN_TC_042 | 28 | ✅ PASS | — | — | — |
| CRM_LOGIN_TC_043 | 29 | ✅ PASS | — | — | — |
| CRM_LOGIN_TC_044 | 32 | ✅ PASS | — | — | — |
| CRM_LOGIN_TC_045 | 33 | ✅ PASS | — | — | — |
| CRM_LOGIN_TC_046 | 35 | 🚫 BLOCKED | — | `@Skip` — chưa được phép bật bộ đếm giờ trên tài khoản dùng chung (`AMB-LOGIN-17`) | — |
| CRM_LOGIN_TC_047 | 32 | ✅ PASS | — | — | — |
| CRM_LOGIN_TC_048 | 47 | ⏩ SKIPPED | — | `@PersonalOnly` — chờ 8 giờ 10 phút thật, QA tự chạy ngoài workflow | — |
| CRM_LOGIN_TC_049 | 19 | ✅ PASS | — | — | — |
| CRM_LOGIN_TC_050 | 44 | 🚫 BLOCKED | — | `@Skip` — cần tài khoản contact test (`AMB-LOGIN-20`) | — |
| CRM_LOGIN_TC_051 | 48 | 🚫 BLOCKED | — | `@Skip` — cần tài khoản staff vô hiệu hoá (`AMB-LOGIN-19`) | — |
| CRM_LOGIN_TC_052 | 12 | ✅ PASS | — | — | — |
| CRM_LOGIN_TC_053 | 12 | ❌ FAIL | 5 · biến thể `d` | Biến thể `a`, `b`, `c` PASS (không hộp thoại nào bật). Biến thể `d` bị máy chủ trả **403 Forbidden**. Xem FAIL #4 | [ảnh](../../executions/login/web/run_1794453634/evidence/CRM_LOGIN_TC_053_bien_the_d_403.png) |
| CRM_LOGIN_TC_054 | 40 | ✅ PASS | — | — | — |
| CRM_LOGIN_TC_055 | 22 | ✅ PASS | — | — | — |
| CRM_LOGIN_TC_056 | 23 | ✅ PASS | — | — | — |
| CRM_LOGIN_TC_057 | 24 | ✅ PASS | — | — | — |
| CRM_LOGIN_TC_058 | 25 | ✅ PASS | — | — | [ảnh](../../executions/login/web/run_1794453634/evidence/CRM_LOGIN_TC_058_bien_the_a_tu_choi.png) |
| CRM_LOGIN_TC_059 | 41 | ✅ PASS | — | — | — |
| CRM_LOGIN_TC_060 | 34 | ✅ PASS | — | — | — |
| CRM_LOGIN_TC_061 | 24 | ✅ PASS | — | — | [ảnh](../../executions/login/web/run_1794453634/evidence/CRM_LOGIN_TC_061_buoc4_het_han.png) |
| CRM_LOGIN_TC_062 | 02 | ✅ PASS | — | — | [ảnh](../../executions/login/web/run_1794453634/evidence/CRM_LOGIN_TC_062_buoc8_forgot_focus.png) |
| CRM_LOGIN_TC_063 | 36 | ✅ PASS | — | — | — |
| CRM_LOGIN_TC_064 | 01 | ✅ PASS | — | — | — |
| CRM_LOGIN_TC_065 | 11 · 17 | 🚫 BLOCKED | — | `@NeedsVerify` — công cụ chạy (Playwright MCP) chỉ có Chromium, không có Edge / Firefox (`ASM-LOGIN-11`) | — |
| CRM_LOGIN_TC_066 | 51 | 🚫 BLOCKED | — | `@Skip` — `CRM-LOGIN-101` chưa triển khai lên demo (Điều kiện #7 · #8) | — |
| CRM_LOGIN_TC_067 | 52 | 🚫 BLOCKED | — | `@Skip` — như 066 | — |
| CRM_LOGIN_TC_068 | 53 | ⏩ SKIPPED | — | `@PersonalOnly` (kiêm `@Skip`) — chờ 15 phút khoá, tính năng chưa triển khai; QA tự chạy | — |
| CRM_LOGIN_TC_069 | 54 | 🚫 BLOCKED | — | `@Skip` — như 066 | — |
| CRM_LOGIN_TC_070 | 55 | 🚫 BLOCKED | — | `@Skip` — như 066 | — |
| CRM_LOGIN_TC_071 | 56 | 🚫 BLOCKED | — | `@Skip` — như 066 và cần mạng thứ hai có IP khác (Điều kiện #9) | — |
| CRM_LOGIN_TC_072 | 57 | 🚫 BLOCKED | — | `@Skip` — như 066 (kiểm bộ đếm khoá, chưa có tính năng) | — |
| CRM_LOGIN_TC_073 | 58 | 🚫 BLOCKED | — | `@Skip` — như 066 | — |
| CRM_LOGIN_TC_074 | 59 | ⏩ SKIPPED | — | `@PersonalOnly` (kiêm `@Skip`) — chờ 15 phút khoá; QA tự chạy | — |
| CRM_LOGIN_TC_075 | 60 | ⏩ SKIPPED | — | `@PersonalOnly` (kiêm `@Skip`) — chờ 2 chu kỳ khoá 15 phút (`ASM-LOGIN-13`); QA tự chạy | — |
| CRM_LOGIN_TC_076 | 61 | 🚫 BLOCKED | — | `@Skip` — như 066 | — |
| CRM_LOGIN_TC_077 | 62 | 🚫 BLOCKED | — | `@Skip` — như 066 và cần hộp thư Project Manager (Điều kiện #2) | — |
| CRM_LOGIN_TC_078 | 63 | 🚫 BLOCKED | — | `@Skip` `@NeedsVerify` — như 077 | — |
| CRM_LOGIN_TC_079 | 64 | 🚫 BLOCKED | — | `@Skip` — như 066 | — |
| CRM_LOGIN_TC_080 | 65 | 🚫 BLOCKED | — | `@Skip` `@TechCheck` — như 066 | — |
| CRM_LOGIN_TC_081 | 66 | 🚫 BLOCKED | — | `@Skip` — như 066 và cần tài khoản contact (Điều kiện #3) | — |
| CRM_LOGIN_TC_082 | 67 | 🚫 BLOCKED | — | `@Skip` — như 066 và cần staff vô hiệu hoá (Điều kiện #3) | — |
| CRM_LOGIN_TC_083 | 68 | 🚫 BLOCKED | — | `@Skip` `@NeedsVerify` — như 066 | — |

</details>

## 5. 🟡 TC chưa automate

> **Chưa có project automation nào** — cả 83 TC đều chạy tay. Cột `Automation dự kiến` là **dự định** ghi trong file TC, không phải thực tế. TC `@Skip` / `@PersonalOnly` chưa chạy được → automate sau cùng.

| Priority | Automation = Yes | Partial | No | Tổng |
|---|:-:|:-:|:-:|:-:|
| Critical | 3 | 1 | 0 | 4 |
| High | 24 | 19 | 0 | 43 |
| Medium | 25 | 9 | 0 | 34 |
| Low | 2 | 0 | 0 | 2 |
| **Tổng** | **54** | **29** | **0** | **83** |

<details><summary>Danh sách đầy đủ 83 TC (xếp theo Priority, rồi khả năng automate)</summary>

| TC ID | Priority | Automation dự kiến | Ghi chú |
|---|---|---|---|
| CRM_LOGIN_TC_006 | Critical | Yes | `@Smoke` · `@CriticalPath` |
| CRM_LOGIN_TC_008 | Critical | Yes | `@Smoke` · `@CriticalPath` |
| CRM_LOGIN_TC_049 | Critical | Yes | `@Smoke` · `@CriticalPath` |
| CRM_LOGIN_TC_067 | Critical | Partial | `@Skip` · 🚫 lần chạy gần nhất |
| CRM_LOGIN_TC_002 | High | Yes | `@Smoke` |
| CRM_LOGIN_TC_009 | High | Yes | — |
| CRM_LOGIN_TC_010 | High | Yes | — |
| CRM_LOGIN_TC_011 | High | Yes | — |
| CRM_LOGIN_TC_014 | High | Yes | — |
| CRM_LOGIN_TC_015 | High | Yes | ❌ lần chạy gần nhất |
| CRM_LOGIN_TC_019 | High | Yes | — |
| CRM_LOGIN_TC_022 | High | Yes | — |
| CRM_LOGIN_TC_028 | High | Yes | `@KnownBug` · ❌ lần chạy gần nhất |
| CRM_LOGIN_TC_040 | High | Yes | `@TechCheck` |
| CRM_LOGIN_TC_041 | High | Yes | `@TechCheck` |
| CRM_LOGIN_TC_042 | High | Yes | `@TechCheck` |
| CRM_LOGIN_TC_043 | High | Yes | `@TechCheck` |
| CRM_LOGIN_TC_044 | High | Yes | — |
| CRM_LOGIN_TC_045 | High | Yes | — |
| CRM_LOGIN_TC_047 | High | Yes | — |
| CRM_LOGIN_TC_052 | High | Yes | — |
| CRM_LOGIN_TC_053 | High | Yes | ❌ lần chạy gần nhất |
| CRM_LOGIN_TC_054 | High | Yes | — |
| CRM_LOGIN_TC_055 | High | Yes | `@TechCheck` |
| CRM_LOGIN_TC_057 | High | Yes | `@TechCheck` |
| CRM_LOGIN_TC_058 | High | Yes | `@TechCheck` · `@NeedsVerify` |
| CRM_LOGIN_TC_060 | High | Yes | `@TechCheck` |
| CRM_LOGIN_TC_072 | High | Yes | `@Skip` · 🚫 lần chạy gần nhất |
| CRM_LOGIN_TC_030 | High | Partial | `@Skip` · 🚫 lần chạy gần nhất |
| CRM_LOGIN_TC_031 | High | Partial | `@Skip` · 🚫 lần chạy gần nhất |
| CRM_LOGIN_TC_032 | High | Partial | `@Skip` · `@NeedsVerify` · 🚫 lần chạy gần nhất |
| CRM_LOGIN_TC_033 | High | Partial | `@Skip` · 🚫 lần chạy gần nhất |
| CRM_LOGIN_TC_034 | High | Partial | `@Skip` · 🚫 lần chạy gần nhất |
| CRM_LOGIN_TC_035 | High | Partial | `@Skip` · 🚫 lần chạy gần nhất |
| CRM_LOGIN_TC_050 | High | Partial | `@Skip` · 🚫 lần chạy gần nhất |
| CRM_LOGIN_TC_051 | High | Partial | `@Skip` · 🚫 lần chạy gần nhất |
| CRM_LOGIN_TC_066 | High | Partial | `@Skip` · `@NeedsVerify` · 🚫 lần chạy gần nhất |
| CRM_LOGIN_TC_068 | High | Partial | `@Skip` · `@PersonalOnly` · ⏩ lần chạy gần nhất |
| CRM_LOGIN_TC_069 | High | Partial | `@Skip` · 🚫 lần chạy gần nhất |
| CRM_LOGIN_TC_070 | High | Partial | `@Skip` · 🚫 lần chạy gần nhất |
| CRM_LOGIN_TC_071 | High | Partial | `@Skip` · 🚫 lần chạy gần nhất |
| CRM_LOGIN_TC_073 | High | Partial | `@Skip` · 🚫 lần chạy gần nhất |
| CRM_LOGIN_TC_074 | High | Partial | `@Skip` · `@PersonalOnly` · ⏩ lần chạy gần nhất |
| CRM_LOGIN_TC_077 | High | Partial | `@Skip` · 🚫 lần chạy gần nhất |
| CRM_LOGIN_TC_078 | High | Partial | `@Skip` · `@NeedsVerify` · 🚫 lần chạy gần nhất |
| CRM_LOGIN_TC_079 | High | Partial | `@Skip` · 🚫 lần chạy gần nhất |
| CRM_LOGIN_TC_080 | High | Partial | `@Skip` · `@TechCheck` · 🚫 lần chạy gần nhất |
| CRM_LOGIN_TC_001 | Medium | Yes | `@Smoke` |
| CRM_LOGIN_TC_003 | Medium | Yes | `@Smoke` |
| CRM_LOGIN_TC_004 | Medium | Yes | `@Smoke` |
| CRM_LOGIN_TC_007 | Medium | Yes | `@Smoke` |
| CRM_LOGIN_TC_012 | Medium | Yes | `@NeedsVerify` |
| CRM_LOGIN_TC_013 | Medium | Yes | `@NeedsVerify` |
| CRM_LOGIN_TC_016 | Medium | Yes | ❌ lần chạy gần nhất |
| CRM_LOGIN_TC_017 | Medium | Yes | — |
| CRM_LOGIN_TC_018 | Medium | Yes | `@TechCheck` |
| CRM_LOGIN_TC_020 | Medium | Yes | `@NeedsVerify` |
| CRM_LOGIN_TC_021 | Medium | Yes | — |
| CRM_LOGIN_TC_023 | Medium | Yes | — |
| CRM_LOGIN_TC_024 | Medium | Yes | — |
| CRM_LOGIN_TC_025 | Medium | Yes | `@TechCheck` |
| CRM_LOGIN_TC_026 | Medium | Yes | — |
| CRM_LOGIN_TC_027 | Medium | Yes | — |
| CRM_LOGIN_TC_029 | Medium | Yes | — |
| CRM_LOGIN_TC_037 | Medium | Yes | — |
| CRM_LOGIN_TC_038 | Medium | Yes | — |
| CRM_LOGIN_TC_039 | Medium | Yes | — |
| CRM_LOGIN_TC_056 | Medium | Yes | `@TechCheck` |
| CRM_LOGIN_TC_059 | Medium | Yes | `@TechCheck` |
| CRM_LOGIN_TC_061 | Medium | Yes | `@TechCheck` · `@NeedsVerify` |
| CRM_LOGIN_TC_062 | Medium | Yes | `@NeedsVerify` |
| CRM_LOGIN_TC_065 | Medium | Yes | `@NeedsVerify` · 🚫 lần chạy gần nhất |
| CRM_LOGIN_TC_036 | Medium | Partial | `@Skip` · `@NeedsVerify` · 🚫 lần chạy gần nhất |
| CRM_LOGIN_TC_046 | Medium | Partial | `@Skip` · `@NeedsVerify` · 🚫 lần chạy gần nhất |
| CRM_LOGIN_TC_048 | Medium | Partial | `@PersonalOnly` · ⏩ lần chạy gần nhất |
| CRM_LOGIN_TC_064 | Medium | Partial | `@NeedsVerify` |
| CRM_LOGIN_TC_075 | Medium | Partial | `@Skip` · `@PersonalOnly` · ⏩ lần chạy gần nhất |
| CRM_LOGIN_TC_076 | Medium | Partial | `@Skip` · 🚫 lần chạy gần nhất |
| CRM_LOGIN_TC_081 | Medium | Partial | `@Skip` · 🚫 lần chạy gần nhất |
| CRM_LOGIN_TC_082 | Medium | Partial | `@Skip` · 🚫 lần chạy gần nhất |
| CRM_LOGIN_TC_083 | Medium | Partial | `@Skip` · `@NeedsVerify` · 🚫 lần chạy gần nhất |
| CRM_LOGIN_TC_005 | Low | Yes | `@Smoke` · `@NeedsVerify` |
| CRM_LOGIN_TC_063 | Low | Yes | `@NeedsVerify` |

</details>

## 6. Kiểm tra khác

| Hạng mục | Số lượng | Kết quả |
|---|:-:|---|
| 🔴 REQ chưa có TC | 0 | ✅ Không có |
| 🟠 REQ chỉ có checklist | 0 | ✅ Không có — 28 mục checklist (phủ 29 REQ) đều trỏ về REQ đã có TC |
| ⚪ Orphan test (không map về REQ) | 0 | ✅ Không có — chưa có script automation |
| 🧩 TC trỏ tới REQ không tồn tại | 0 | ✅ Không có — 83/83 TC trỏ về REQ đang có trong tài liệu |
| 🗑️ Script trỏ vào TC Deprecated | 0 | ✅ Không có — chưa có TC `@Deprecated` |
| ⚠️ Mapping suy luận cần xác nhận | 0 | ✅ Không có — 100% mapping theo ID |
