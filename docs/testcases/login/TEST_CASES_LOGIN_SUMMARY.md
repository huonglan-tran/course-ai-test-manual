# Test Cases — Login (tổng 83 TC · 133 biến thể · 1 nền tảng)

> **INDEX module — tên file bất biến.** Không chứa dòng TC — TC chi tiết nằm ở các part trong [Bản đồ tài liệu](#bản-đồ-tài-liệu).
>
> Danh mục: [`../README.md`](../README.md) · Requirements: [`REQUIREMENTS_LOGIN_SUMMARY.md`](../../requirements/login/REQUIREMENTS_LOGIN_SUMMARY.md) → [`web/requirements_login_web.md`](../../requirements/login/web/requirements_login_web.md)

| Thông tin | Nội dung |
|---|---|
| **Hệ thống · module** | Perfex CRM khu Admin · `LOGIN` — Login · Forgot Password · Remember me · Đăng xuất |
| **Nguồn requirement** | [`requirements_login_web.md`](../../requirements/login/web/requirements_login_web.md) — bản 1.5 (22-10-2026), `REQ-LOGIN-01` → `REQ-LOGIN-68` |
| **Phạm vi REQ** | 68/68 REQ có TC — không REQ nào bị loại khỏi phạm vi viết TC. REQ-LOGIN-51 → 68 (ticket `CRM-LOGIN-101`, ⚪ chưa triển khai) có TC viết trước, `@Skip` |
| **Nền tảng** | Web ✅ 83 TC · Mobile — chưa có requirements · API — chưa có requirements |
| **Dải TC ID đã dùng** | `CRM_LOGIN_TC_001` → `CRM_LOGIN_TC_083` — chung mọi nền tảng. TC kế tiếp: `CRM_LOGIN_TC_084` |
| **Mode · độ hạt** | QUICK (`/generate-testcases-from-requirements`) · **GỘP** — biến thể cùng trường, cùng loại phản hồi chung 1 TC, mỗi biến thể có mã `a`, `b`… (báo FAIL ghi `CRM_LOGIN_TC_014-c`) |
| **Mức rủi ro · độ sâu** | `Cao` → **Đầy đủ** — V1 đủ 6 nhánh, V2 đủ mọi nhánh có điều kiện kích hoạt, V3 · V4 chấm từng nhánh.<br>Căn cứ chấm Cao: chạm **xác thực & phiên đăng nhập** · là **cổng vào** mọi module khác (hỏng thì chặn cả đợt kiểm thử) · có thao tác **không hồi lại được** (gửi email đặt lại mật khẩu ra ngoài).<br>**Rà lại độ sâu khi:** thêm xác thực 2 lớp / CAPTCHA · thêm role thứ hai ngoài Admin · mở cổng Customer dùng chung prefix `LOGIN` |
| **Ngày sinh** | 20-10-2026 · bổ sung 23-10-2026 (`CRM_LOGIN_TC_066` → `083`, chế độ BỔ SUNG) |

## Bản đồ tài liệu

| Nền tảng | File | Nhóm chức năng | Số TC | TC ID | REQ bao phủ |
|---|---|---|---|---|---|
| Web | [web/parts/part_01_web_smoke_form_login.md](web/parts/part_01_web_smoke_form_login.md) | V1 Smoke · V2a form Login (bắt buộc nhập, Email, Password, Remember me, Enter, dán/sao chép, bấm 2 lần, mất mạng) | 25 | 001–025 | 01 → 12 · 14 → 17 · 30 · 31 · 36 |
| Web | [web/parts/part_02_web_forgot_phien.md](web/parts/part_02_web_forgot_phien.md) | V2b Forgot Password · V2c luật nghiệp vụ & phiên (Remember me, đăng xuất, hết hạn phiên) | 23 | 026–048 | 13 · 18 · 20 · 21 · 26 → 29 · 32 · 33 · 35 · 37 → 40 · 42 · 43 · 45 → 47 · 49 · 50 |
| Web | [web/parts/part_03_web_technical_nonfunc.md](web/parts/part_03_web_technical_nonfunc.md) | V3 chặn truy cập, loại tài khoản, SQLi/XSS, cookie, mã chống giả mạo · V4 bàn phím, responsive, Edge/Firefox | 17 | 049–065 | 01 · 02 · 11 · 12 · 17 · 19 · 22 → 25 · 34 · 36 · 40 · 41 · 44 · 48 |
| Web | [web/parts/part_04_web_khoa_tai_khoan.md](web/parts/part_04_web_khoa_tai_khoan.md) | STORY-LOGIN-07 khoá tài khoản khi đăng nhập sai nhiều lần (ticket `CRM-LOGIN-101`) — thông báo khoá, thời hạn, bộ đếm, IP, Forgot Password, phiên, loại tài khoản | 18 | 066–083 | 51 → 68 |

> Tách 4 part vì file nền tảng web (83 TC) vượt ngưỡng 50 TC của độ hạt GỘP — cắt tại ranh giới vòng / nhóm chức năng. Part 4 (23-10-2026) gom trọn STORY-LOGIN-07 — trừ TC biên của REQ-46 (`036`, part 2).

### Nhãn đặc biệt trong bộ TC

| Tag | Số TC | TC ID | Ý nghĩa khi chạy |
|---|---|---|---|
| `@Skip` | 28 | 030 · 031 · 032 · 033 · 034 · 035 · 036 · 046 · 050 · 051 · 066 → 083 | REQ ⚪ chưa đủ điều kiện chạy — thiếu tài khoản staff test riêng / hộp thư (`AMB-LOGIN-01` ⏭️), tài khoản contact, staff vô hiệu hoá, quyền bật bộ đếm giờ (`AMB-LOGIN-17`); tính năng chưa triển khai lên demo (036 · 066 → 083 — `CRM-LOGIN-101`). `/execute-test-cases` chấm **BLOCKED**, không tính FAIL |
| `@KnownBug` | 1 | 028 | Viết theo hành vi mong đợi PO chốt (`AMB-LOGIN-03`) — **dự kiến FAIL** cho tới khi Dev sửa REQ-39 |
| `@PersonalOnly` | 4 | 048 · 068 · 074 · 075 | Phải chờ thời gian thật (048: 8 giờ không hoạt động · 068 · 074 · 075: 15 phút khoá) — QA tự chạy ngoài workflow, `/execute-test-cases` bỏ qua |
| `@TechCheck` | 14 | 018 · 025 · 040 · 041 · 042 · 043 · 055 · 056 · 057 · 058 · 059 · 060 · 061 · 080 | Cần DevTools (QA có quyền). 025 · 018 chỉ dùng DevTools ở bước phụ |
| `@NeedsVerify` | 16 | 005 · 012 · 013 · 020 · 032 · 036 · 046 · 058 · 061 · 062 · 063 · 064 · 065 · 066 · 078 · 083 | Expected có phần chưa có evidence — xem [Vùng chưa có evidence](#vùng-chưa-có-evidence) |
| `@Smoke` | 9 | 001 → 008 · 049 | Chạy đầu tiên — 3 TC `@CriticalPath`: 006 · 008 · 049 |

## Assumptions

> QUICK mode không dừng hỏi — mọi điểm requirements chưa nói rõ đều ghi giả định ở đây. **Xung đột tài liệu ↔ evidence: không có** — 13/13 ảnh khớp mô tả trong requirements.

| Mã | Điểm chưa rõ | Giả định đã áp dụng | TC bị ảnh hưởng |
|---|---|---|---|
| ASM-LOGIN-01 | Ô Email chỉ gồm khoảng trắng | Trình duyệt bỏ khoảng trắng của ô email → máy chủ nhận như bỏ trống → `The Email Address field is required.` | 012 |
| ASM-LOGIN-02 | Password chỉ gồm khoảng trắng | Máy chủ cắt khoảng trắng khi kiểm bắt buộc → `The Password field is required.` | 013 |
| ASM-LOGIN-03 | Email đăng nhập có phân biệt hoa/thường, có bỏ khoảng trắng đầu/cuối không — requirements không nói | Không phân biệt hoa/thường; khoảng trắng đầu/cuối bị bỏ → đăng nhập thành công. Kết quả ngược lại → đề xuất mở AMB mới — mã kế tiếp ở metadata requirements (hiện `AMB-LOGIN-34`), không tự sửa Expected. Liên quan REQ-LOGIN-58 (bộ đếm khoá không phân biệt hoa/thường) · REQ-LOGIN-68 (bộ đếm khoá bỏ khoảng trắng đầu/cuối email) | 020 |
| ASM-LOGIN-04 | Ô Password có chặn dán / chặn sao chép không | Cho dán, không cho sao chép ra (hành vi mặc định của trình duyệt với ô mật khẩu) | 023 |
| ASM-LOGIN-05 | Email rất dài (đến 1000 ký tự) — giao diện không giới hạn, máy chủ có giới hạn không | Máy chủ xử lý như email không tồn tại → `Invalid email or password`, không lỗi trang | 016 · 029-d |
| ASM-LOGIN-06 | Logo trỏ về `/` — chưa bấm thử (REQ-06) | Trang đích là trang chủ phía khách hàng | 005 |
| ASM-LOGIN-07 | Hành vi khi mã chống giả mạo hết hạn (cookie ≈ 1 giờ) | Hệ thống không để người dùng kẹt ở trang trắng; nạp lại trang thì đăng nhập lại được | 061 |
| ASM-LOGIN-08 | Nguyên văn thông báo khi mã chống giả mạo bị sửa/xoá (REQ-25) | Có trang/thông báo từ chối, khác thông báo `Invalid email or password`. Chép nguyên văn khi chạy | 058 |
| ASM-LOGIN-09 | Nguyên văn thông báo khi staff vô hiệu hoá đăng nhập (REQ-48 ghi "chưa biết") | Chỉ kiểm có khung thông báo đỏ và không vào được; chép nguyên văn để bổ sung REQ | 051 |
| ASM-LOGIN-10 | Viền chọn khi Tab tới Remember me / nút Login / liên kết; thứ tự Tab trang Forgot Password | Thứ tự trang Login theo DOM đã đo (requirements mục 2.1); viền chọn nhìn thấy được; trang Forgot: logo → ô Email → nút Confirm | 062 · 063 |
| ASM-LOGIN-11 | Kích thước màn hình khác 1600×780, trình duyệt Edge/Firefox — chưa khảo sát | Trang Login hiển thị đủ, dùng được ở 4 breakpoint + phóng to 125%; Edge/Firefox chạy như Chrome | 064 · 065 |
| ASM-LOGIN-12 | Biến thể sai định dạng email ngoài `abc` — chỉ `abc` có ảnh evidence | Bị trình duyệt chặn theo quy tắc định dạng email chuẩn của Chrome | 014-b → f · 027-b → d |
| ASM-LOGIN-13 | REQ-60 (hết khoá thì bộ đếm về 0) chỉ kiểm trọn được khi khoá tài khoản **2 chu kỳ** — mâu thuẫn mitigation "mỗi TC khoá đúng 1 chu kỳ" của `RISK-LOGIN-08` | Giữ đúng AC của REQ-60 (2 chu kỳ), ghi cảnh báo ở Pre-Condition: sau TC chờ thêm ≥ 15 phút. Muốn rút về 1 chu kỳ thì dừng ở bước 4 (5 lần sai sau mở khoá đều `Invalid email or password`) — cần user chốt | 075 |
| ASM-LOGIN-14 | Forgot Password của email **tồn tại** khi đang khoá hiển thị nguyên văn gì — REQ-62 trỏ về thông báo trung tính của REQ-39, nhưng REQ-39 đang lệch (`@KnownBug` 028) | TC chỉ kiểm **không bị chặn** (không thông báo khoá, không `Email not found`) và **nhận được email**; nguyên văn thông báo kiểm ở 030 — tránh 077 FAIL vì bug REQ-39 | 077 |
| ASM-LOGIN-15 | Ô Email kiểu email — trình duyệt có tự bỏ khoảng trắng đầu/cuối trước khi gửi không (cùng gốc `ASM-LOGIN-01`) | Có thể có → phép thử qua giao diện **không** chứng minh được REQ-68 phía máy chủ; TC vẫn chạy, ghi chú kết quả, đội Dev xác minh bằng request thủ công (QA chưa xác nhận quyền gọi API) | 083 |

## Bảng Đối Soát Coverage (toàn module)

> Chỉ có nền tảng Web → mọi REQ phải có ≥ 1 TC web. Cột *P/N/B*: Positive / Negative / Boundary.

| REQ ID | Mô tả ngắn | Trạng thái REQ | Số TC | TC IDs | Đủ P/N/B? | Ghi chú |
|---|---|---|---|---|---|---|
| REQ-LOGIN-01 | Trang Login đủ thành phần | 🟢 | 2 | 001 · 064 | ✅ P | Trang tĩnh — không có N/B |
| REQ-LOGIN-02 | Ô Email được chọn sẵn | 🟢 | 2 | 001 · 062 | ✅ P | |
| REQ-LOGIN-03 | Ô Password che ký tự | 🟢 | 2 | 002 · 023 | ✅ P · N | |
| REQ-LOGIN-04 | Remember me mặc định không tích | 🟢 | 2 | 001 · 021 | ✅ P | |
| REQ-LOGIN-05 | Tiêu đề tab chứa "Login" | 🟢 | 1 | 001 | ✅ P | 001 gánh 4 REQ — chỉ REQ-05 không có TC khác (6b: đạt) |
| REQ-LOGIN-06 | Logo trỏ về "/" | 🟢 | 1 | 005 | ✅ P | `@NeedsVerify` |
| REQ-LOGIN-07 | Liên kết Forgot Password? | 🟢 | 1 | 004 | ✅ P | |
| REQ-LOGIN-08 | Email trống → bắt buộc | 🟢 | 2 | 009 · 012 | ✅ N · B | 012 khoảng trắng |
| REQ-LOGIN-09 | Password trống → bắt buộc | 🟢 | 2 | 010 · 013 | ✅ N · B | |
| REQ-LOGIN-10 | Cả hai trống → 2 thông báo | 🟢 | 1 | 011 | ✅ N | |
| REQ-LOGIN-11 | Email sai định dạng bị trình duyệt chặn | 🟢 | 2 | 014 · 065 | ✅ N | 6 biến thể + 2 trình duyệt |
| REQ-LOGIN-12 | Email không tồn tại → thông báo chung | 🟢 | 7 | 015 · 016 · 017 · 024 · 025 · 052 · 053 | ✅ N · B | Gồm biên độ dài, SQLi/XSS, bấm 2 lần, mất mạng |
| REQ-LOGIN-13 | Sai mật khẩu với email tồn tại | ⚪ | 1 | 035 | ✅ N | `@Skip` — `AMB-LOGIN-01` ⏭️ |
| REQ-LOGIN-14 | Không điền lại email sau lỗi | 🟢 | 2 | 010 · 018 | ✅ N | |
| REQ-LOGIN-15 | Xoá Password sau lỗi | 🟢 | 1 | 019 | ✅ N | |
| REQ-LOGIN-16 | Enter trong ô Password gửi form | 🟢 | 1 | 022 | ✅ P | |
| REQ-LOGIN-17 | Đăng nhập thành công → Dashboard | 🟢 | 3 | 006 · 020 · 065 | ✅ P · B | |
| REQ-LOGIN-18 | Luôn về Dashboard sau đăng nhập | 🟢 | 1 | 037 | ✅ P | |
| REQ-LOGIN-19 | Chưa đăng nhập bị đưa về Login | 🟢 | 1 | 049 | ✅ N | 4 đường dẫn |
| REQ-LOGIN-20 | Đã đăng nhập mở Login → Dashboard | 🟢 | 1 | 038 | ✅ P | |
| REQ-LOGIN-21 | Đã đăng nhập mở Forgot → Dashboard | 🟢 | 1 | 039 | ✅ P | |
| REQ-LOGIN-22 | Cookie phiên có cờ bảo vệ | 🟢 | 1 | 055 | ✅ P | `@TechCheck` |
| REQ-LOGIN-23 | Hạn cookie phiên trượt 8 giờ | 🟢 | 1 | 056 | ✅ P | `@TechCheck` |
| REQ-LOGIN-24 | Form Login có mã chống giả mạo | 🟢 | 2 | 057 · 061 | ✅ P · N | |
| REQ-LOGIN-25 | Từ chối mã chống giả mạo sai | ⚪ | 1 | 058 | ✅ N | `@NeedsVerify` — Dev đã chốt có kiểm (`AMB-LOGIN-16`) |
| REQ-LOGIN-26 | Tích Remember me tạo ghi nhớ | 🟢 | 1 | 040 | ✅ P | `@TechCheck` |
| REQ-LOGIN-27 | Ghi nhớ tự đăng nhập lại | 🟢 | 1 | 041 | ✅ P | `@TechCheck` |
| REQ-LOGIN-28 | Không tích → không ghi nhớ | ⚪ | 1 | 042 | ✅ N | Chạy được ngay — ⚪ chỉ vì recon chưa chạy đủ |
| REQ-LOGIN-29 | Đăng xuất vô hiệu ghi nhớ | ⚪ | 1 | 043 | ✅ N | Chạy được ngay |
| REQ-LOGIN-30 | Menu avatar có Logout | 🟢 | 1 | 007 | ✅ P | |
| REQ-LOGIN-31 | Logout về trang Login | 🟢 | 1 | 008 | ✅ P | |
| REQ-LOGIN-32 | Sau đăng xuất trang nội bộ bị chặn | 🟢 | 2 | 044 · 047 | ✅ N | 047 hai tab |
| REQ-LOGIN-33 | Sau đăng xuất Back không lộ trang | 🟢 | 1 | 045 | ✅ N | |
| REQ-LOGIN-34 | Đăng xuất cấp phiên mới | 🟢 | 1 | 060 | ✅ P | `@TechCheck` |
| REQ-LOGIN-35 | Logout khi có bộ đếm giờ → hỏi xác nhận | ⚪ | 1 | 046 | ✅ P | `@Skip` — `AMB-LOGIN-17` |
| REQ-LOGIN-36 | Trang Forgot Password đủ thành phần | 🟢 | 2 | 003 · 063 | ✅ P | |
| REQ-LOGIN-37 | Forgot trống → `Email not found` | 🟢 | 1 | 026 | ✅ N | |
| REQ-LOGIN-38 | Forgot sai định dạng bị chặn | 🟢 | 1 | 027 | ✅ N | 4 biến thể |
| REQ-LOGIN-39 | Forgot email không tồn tại → thông báo trung tính | 🟡 | 1 | 028 | ✅ N | `@KnownBug` — dự kiến FAIL |
| REQ-LOGIN-40 | Forgot giữ email sau khi gửi | 🟢 | 2 | 029 · 054 | ✅ P · B | |
| REQ-LOGIN-41 | Form Forgot có mã chống giả mạo | 🟢 | 1 | 059 | ✅ P | `@TechCheck` |
| REQ-LOGIN-42 | Email tồn tại → nhận email reset | ⚪ | 1 | 031 | ✅ P | `@Skip` — `AMB-LOGIN-01` ⏭️ |
| REQ-LOGIN-43 | Link reset dùng được | ⚪ | 1 | 032 | ✅ P · N | `@Skip` |
| REQ-LOGIN-44 | Contact đăng nhập Admin bị từ chối | ⚪ | 1 | 050 | ✅ N | `@Skip` |
| REQ-LOGIN-45 | Contact dùng Forgot → như không tồn tại | ⚪ | 1 | 033 | ✅ N | `@Skip` |
| REQ-LOGIN-46 | Khoá sau 5 lần sai liên tiếp | 🟡 | 1 | 036 | ✅ N · B | `@Skip` — chưa triển khai (`CRM-LOGIN-101`) · biên lần 5 / lần 6 · `@NeedsVerify` kiểu khung thông báo khoá |
| REQ-LOGIN-47 | Phiên hết hạn sau 8 giờ | ⚪ | 1 | 048 | ✅ N | `@PersonalOnly` |
| REQ-LOGIN-48 | Staff vô hiệu hoá không đăng nhập được | ⚪ | 1 | 051 | ✅ N | `@Skip` |
| REQ-LOGIN-49 | Email tồn tại → cùng thông báo trung tính | ⚪ | 1 | 030 | ✅ N | `@Skip` |
| REQ-LOGIN-50 | Staff vô hiệu hoá dùng Forgot | ⚪ | 1 | 034 | ✅ N | `@Skip` |
| REQ-LOGIN-51 | Thông báo khoá nguyên văn từ lần thử thứ 6 | ⚪ | 1 | 066 | ✅ N | `@Skip` — chưa triển khai (`CRM-LOGIN-101`) · `@NeedsVerify` kiểu khung |
| REQ-LOGIN-52 | Đang khoá, mật khẩu đúng vẫn bị từ chối | ⚪ | 1 | 067 | ✅ N | `@Skip` — chưa triển khai |
| REQ-LOGIN-53 | Hết 15 phút đăng nhập được lại | ⚪ | 1 | 068 | ✅ P | `@Skip` · `@PersonalOnly` — chờ 15 phút |
| REQ-LOGIN-54 | Đăng nhập thành công đặt lại bộ đếm | ⚪ | 1 | 069 | ✅ P · B | `@Skip` · biên 4 lần sai |
| REQ-LOGIN-55 | Email khác cùng IP không bị chặn | ⚪ | 1 | 070 | ✅ N | `@Skip` |
| REQ-LOGIN-56 | Email bị khoá vẫn bị khoá từ IP khác | ⚪ | 1 | 071 | ✅ N | `@Skip` · cần mạng thứ hai |
| REQ-LOGIN-57 | Email không tồn tại không bị khoá | ⚪ | 1 | 072 | ✅ N | `@Skip` · không khoá tài khoản test |
| REQ-LOGIN-58 | Bộ đếm không phân biệt hoa/thường | ⚪ | 1 | 073 | ✅ N | `@Skip` |
| REQ-LOGIN-59 | Thử lại khi đang khoá không dời mốc mở khoá | ⚪ | 1 | 074 | ✅ N | `@Skip` · `@PersonalOnly` — chờ 15 phút |
| REQ-LOGIN-60 | Hết khoá thì bộ đếm về 0 | ⚪ | 1 | 075 | ✅ P | `@Skip` · `@PersonalOnly` · 2 chu kỳ khoá (`ASM-LOGIN-13`) |
| REQ-LOGIN-61 | Bỏ trống Password không tính lần sai | ⚪ | 1 | 076 | ✅ N · B | `@Skip` · biên 4 sai + 2 bỏ trống + 1 sai |
| REQ-LOGIN-62 | Đang khoá vẫn dùng được Forgot Password | ⚪ | 1 | 077 | ✅ P | `@Skip` · cần hộp thư (`AMB-LOGIN-01` ⏭️) |
| REQ-LOGIN-63 | Đặt lại mật khẩu không mở khoá sớm | ⚪ | 1 | 078 | ✅ N | `@Skip` · cần hộp thư · `@NeedsVerify` trang đặt mật khẩu mới |
| REQ-LOGIN-64 | Khoá không đẩy phiên đang mở ra ngoài | ⚪ | 1 | 079 | ✅ P | `@Skip` |
| REQ-LOGIN-65 | Cookie ghi nhớ vẫn tự đăng nhập khi đang khoá | ⚪ | 1 | 080 | ✅ P | `@Skip` · `@TechCheck` |
| REQ-LOGIN-66 | Email contact không bị khoá | ⚪ | 1 | 081 | ✅ N | `@Skip` · cần tài khoản contact |
| REQ-LOGIN-67 | Staff vô hiệu hoá không bị khoá | ⚪ | 1 | 082 | ✅ N | `@Skip` · cần staff vô hiệu hoá |
| REQ-LOGIN-68 | Bộ đếm bỏ khoảng trắng đầu/cuối email | ⚪ | 1 | 083 | ✅ N | `@Skip` · `@NeedsVerify` — `ASM-LOGIN-15` |

**Tổng: 68/68 REQ có TC · 🔴 thiếu: 0.** REQ đang bị chặn chạy (`@Skip` / `@PersonalOnly`): 29 — 13 · 35 · 42 · 43 · 44 · 45 · 46 · 47 · 48 · 49 · 50 · 51 → 68.

**Phép thử chiều ngược (6b):** 066 → 083 mỗi TC gánh đúng 1 REQ. 3 TC gánh ≥ 2 REQ — `001` (01 · 02 · 04 · 05 → chỉ REQ-05 không có TC khác), `010` (09 · 14 → cả hai có TC khác), `065` (11 · 17 → cả hai có TC khác). **Không vi phạm.**

### Decision Table — kết quả bấm Login

> ≥ 3 điều kiện kết hợp quyết định một kết quả → bắt buộc lập bảng. Mỗi rule = 1 TC.

| Rule | Email | Password | Loại tài khoản | Trạng thái khoá | → Kết quả | TC |
|---|---|---|---|---|---|---|
| R1 | Trống | Trống | — | — | 2 khung đỏ bắt buộc | 011 |
| R2 | Trống | Có | — | — | `The Email Address field is required.` | 009 · 012 |
| R3 | Đúng định dạng | Trống | — | — | `The Password field is required.` | 010 · 013 |
| R4 | Sai định dạng | Có | — | — | Trình duyệt chặn, không gửi | 014 |
| R5 | Đúng định dạng, không tồn tại | Có | — | — | `Invalid email or password` | 015 · 016 · 017 |
| R6 | Tồn tại | Sai | Staff hoạt động | Lần sai 1 → 5 | `Invalid email or password` | 035 · 036 (bước 2 → 4) |
| R7 | Tồn tại | Đúng | Staff hoạt động | Không bị khoá | Vào Dashboard | 006 · 020 |
| R8 | Tồn tại | Đúng | Contact | — | `Invalid email or password` | 050 |
| R9 | Tồn tại | Đúng | Staff vô hiệu hoá | — | Bị chặn, khung đỏ (nguyên văn chưa biết) | 051 |
| R10 | Tồn tại | Sai | Staff hoạt động | Lần thử thứ 6 (đã sai 5 lần) | `Your account is locked. Please try again in 15 minutes.` | 036 (bước 5 → 6) · 066 · 073 · 083 |
| R11 | Tồn tại | Đúng | Staff hoạt động | Đang khoá | `Your account is locked. Please try again in 15 minutes.` | 067 · 071 |
| R12 | Đúng định dạng, không tồn tại | Có | — | Lần ≥ 6 | `Invalid email or password` — không khoá | 072 |
| R13 | Tồn tại | Sai | Contact | Lần ≥ 6 | Không thông báo khoá | 081 |
| R14 | Tồn tại | Sai | Staff vô hiệu hoá | Lần ≥ 6 | Không thông báo khoá | 082 |
| R15 | Tồn tại | Trống | Staff hoạt động | — | `The Password field is required.` — không tính lần sai | 076 |

### State Transition — trạng thái phiên đăng nhập

> Đối chiếu [Ma trận trạng thái phiên](../../requirements/login/REQUIREMENTS_LOGIN_SUMMARY.md#5-ma-trận-trạng-thái-phiên) của requirements.

| Từ \ Hành động | Đăng nhập đúng | Đăng nhập lỗi | Mở trang nội bộ | Mở Login/Forgot | Logout | Mất cookie phiên | 8 giờ không hoạt động | Hết 15 phút |
|---|---|---|---|---|---|---|---|---|
| **Chưa đăng nhập** | ✅ 006 · 040 · sau < 5 lần sai, bộ đếm về 0 — 069 | ✅ 009 → 017 · 035 · 050 · 051 · 🔒 sai lần thứ 5 → **Tài khoản bị khoá** — 036 | ❌ bị chặn — 049 | ✅ mở bình thường — 001 · 003 | — | — | — | — |
| **Đã đăng nhập (không ghi nhớ)** | — | — | ✅ 006 | ❌ bị chuyển — 038 · 039 | ✅ 008 · 044 · 045 · 060 · ⚪ 046 | ❌ về Login — 042 | ❌ về Login — 048 | — |
| **Đã đăng nhập + ghi nhớ** | — | — | ✅ 041 | — | ✅ 043 | ✅ tự đăng nhập lại — 041 | — | — |
| **Tài khoản bị khoá** | ❌ thông báo khoá — 067 · 071 (IP khác) | ❌ thông báo khoá — 036 · 066 · không dời mốc mở khoá — 074 | ❌ bị chặn như chưa đăng nhập — 049 | ✅ Forgot vẫn dùng được — 077 · đặt lại mật khẩu không mở khoá sớm — 078 | — | — | — | ✅ về **Chưa đăng nhập**, bộ đếm về 0 — 068 · 075 |

> Chuyển trạng thái của **phiên khác** khi tài khoản bị khoá: phiên đang mở vẫn dùng được — 079 (REQ-64) · cookie ghi nhớ vẫn tự đăng nhập — 080 (REQ-65) · email khác cùng IP không bị chặn — 070 (REQ-55).

## Bảng Đối Soát Evidence

> Đã mở **13/13** ảnh: 11 ảnh `requirements/login/web/evidence/` + 2 ảnh tầng khám phá `_discovery/evidence/` mà requirements dùng làm evidence. Mở lại đủ 13/13 ngày 23-10-2026 trước khi bổ sung 066 → 083: **không** ảnh nào chụp trạng thái khoá (tính năng chưa triển khai) — 066 → 083 dựa vào ảnh `login_wrong_credentials_viewport.png` (khung `Invalid email or password`) · `login_password_empty_viewport.png` (076) · `remember_me_autologin_clip.png` (080) · `login_forgot_password_viewport.png` (077 · 078); phần thông báo khoá chỉ đối chiếu chữ PO chốt.

| Ảnh evidence | Màn hình / trạng thái | TC dựa vào | Đầy đủ? |
|---|---|---|---|
| `_discovery/evidence/login_overview_viewport.png` | Login — mặc định, form trống, ô Email có viền chọn | 001 · 002 · 021 · 062 · 064 | ✅ viewport đủ form |
| `_discovery/evidence/login_forgot_password_viewport.png` | Forgot Password — mặc định, ô Email **không** có viền chọn | 003 · 004 · 063 | ✅ |
| `login/web/evidence/login_empty_submit_viewport.png` | Login — sau gửi trống cả 2 ô, 2 khung đỏ | 011 | ✅ |
| `login/web/evidence/login_password_empty_viewport.png` | Login — sau gửi thiếu Password, ô Email trống lại | 010 · 018-a | ✅ |
| `login/web/evidence/login_email_empty_viewport.png` | Login — sau gửi thiếu Email, 1 khung đỏ, ô Password trống | 009 · 019-b | ✅ |
| `login/web/evidence/login_email_invalid_format_viewport.png` | Login — email `abc`, không có khung đỏ, Password còn dấu chấm | 014-a · 002 | 🟡 bong bóng của trình duyệt không lên ảnh — căn cứ chính là số liệu DOM trong requirements |
| `login/web/evidence/login_wrong_credentials_viewport.png` | Login — email không tồn tại, `Invalid email or password`, 2 ô trống | 015 · 018-b · 019-a · 022 | ✅ |
| `login/web/evidence/guard_redirect_to_login_viewport.png` | Login — trang đích khi mở `/admin/profile` chưa đăng nhập, không khung đỏ | 049 | ✅ |
| `login/web/evidence/logout_menu_open_clip.png` | Header Dashboard — menu avatar đang mở, 5 mục | 007 · 008 | ✅ cắt góc phải trên |
| `login/web/evidence/logout_landing_heading_clip.png` | Login — trang đích sau Logout (chỉ logo + tiêu đề) | 008 · 044 | 🟡 đã cắt bỏ phần form (trình duyệt tự điền email thật) — đủ cho Expected "tiêu đề Login" |
| `login/web/evidence/remember_me_autologin_clip.png` | Header Dashboard sau tự đăng nhập bằng ghi nhớ | 006 · 041 | ✅ dải header |
| `login/web/evidence/forgot_empty_submit_viewport.png` | Forgot — gửi trống, `Email not found` | 026 | ✅ |
| `login/web/evidence/forgot_unknown_email_viewport.png` | Forgot — email không tồn tại, `Email not found`, email còn trong ô | 028 (hiện trạng lệch) · 029-a | ✅ — ảnh chứng minh **hiện trạng lỗi** của REQ-39 |

### Vùng chưa có evidence

| Màn hình / trạng thái | TC gắn `@NeedsVerify` | Đề xuất recon bổ sung |
|---|---|---|
| Trang đích khi bấm logo | 005 | Chụp viewport trang `/` sau khi bấm logo |
| Ô Email / Password chỉ khoảng trắng · email viết HOA / thừa khoảng trắng với tài khoản thật | 012 · 013 · 020 | Chạy thử, chụp khung thông báo / trang đích |
| Trang đặt mật khẩu mới từ link email | 032 | Cần tài khoản test có hộp thư (`AMB-LOGIN-01`) |
| Thông báo khoá tài khoản (kiểu khung, vị trí) — tính năng `CRM-LOGIN-101` chưa triển khai | 036 · 066 | Chụp viewport trang Login ở lần thử thứ 6 sau khi Dev triển khai |
| Trang đặt mật khẩu mới khi tài khoản đang khoá | 078 | Cần hộp thư của tài khoản Project Manager (`AMB-LOGIN-01`) |
| Email có khoảng trắng đầu/cuối — trình duyệt có bỏ trước khi gửi không | 083 | Đội Dev xác minh bằng request thủ công (`ASM-LOGIN-15`) |
| Hộp thoại xác nhận Logout khi có bộ đếm giờ | 046 | Cần được bật bộ đếm giờ trên tài khoản test (`AMB-LOGIN-17`) |
| Trang/thông báo từ chối mã chống giả mạo sai hoặc hết hạn | 058 · 061 | Chạy qua DevTools, chụp viewport trang trả về |
| Viền chọn khi điều hướng bằng Tab (Login) · thứ tự Tab trang Forgot | 062 · 063 | Chụp từng bước Tab |
| Kích thước 1920 · 1366 · 768 · 375 · phóng to 125% · Edge · Firefox | 064 · 065 | Chụp viewport từng kích thước / trình duyệt |

## Đối soát Field-Level Validation

| Field | Loại (bảng 15 loại) | Mục đã phủ | Mục không áp dụng (lý do) | Kết quả |
|---|---|---|---|---|
| Email Address (Login) | Email — 9 mục | Hợp lệ (006) · thiếu `@` (014-a) · thiếu tên miền (014-b) · tên miền không hợp lệ (014-f · 015-b) · nhiều `@` (014-d) · ký tự đặc biệt trước `@` (015-d · 052) · độ dài tối đa (016) · hoa/thường (015-c · 020-a) · email tồn tại / không tồn tại (006 · 015 · 035) | — (mục "email đã tồn tại" của form đăng ký được hiểu thành tồn tại / không tồn tại cho form đăng nhập) | ✅ 9/9 + bắt buộc (009 · 012) + khoảng trắng (012 · 020-b) + ký tự có dấu (014-e) |
| Password (Login) | Password — 7 mục | Độ dài min/max (017-a · b · c) · ký tự đặc biệt (017-d · e · 053) · copy-paste (023) · hiện/ẩn (002 — không có nút hiện) | Yêu cầu chữ hoa/thường · yêu cầu số · xác nhận mật khẩu — **➖** form đăng nhập chỉ so khớp, không đặt quy tắc độ mạnh, không có ô xác nhận | ✅ 7/7 (3 ➖ có lý do) + bắt buộc (010 · 013) |
| Remember me | Checkbox — 4 mục | Mặc định (001 · 021) · tích/bỏ tích (021) | Bắt buộc — **➖** ô tuỳ chọn · nhóm radio — **➖** không phải radio | ✅ 4/4 (2 ➖) |
| Email Address (Forgot) | Email — 9 mục | Bắt buộc (026) · thiếu `@` · thiếu tên miền · thiếu phần trước `@` · nhiều `@` (027) · ký tự đặc biệt (029-c · 054) · độ dài (029-d) · hoa/thường (029-b) · tồn tại / không tồn tại (028 · 030) | — | ✅ 9/9 |

## Đối soát loại kiểm thử (4 vòng)

| Vòng | Nhánh | Trạng thái | TC ID / Lý do |
|---|---|---|---|
| 1 | UI cơ bản | ✅ | 001 · 002 · 003 · 007 (4 TC · 13 mục kiểm) |
| 1 | Open form | ✅ | 004 · 005 (2 TC · 2 biến thể) — mở bằng đường dẫn trực tiếp có trong mọi TC. Đóng bằng X / ESC / bấm ra ngoài: ➖ Login và Forgot Password là trang riêng, không phải hộp thoại |
| 1 | Display | ✅ | 009 · 011 · 015 · 026 — khung thông báo đỏ phía trên form (1 · 2 khung). Không có tiền tệ / ngày / bảng dữ liệu để kiểm định dạng. Khung thông báo khoá tài khoản (`CRM-LOGIN-101`): 036 · 066 chỉ đối chiếu chữ — kiểu khung chờ evidence (`@NeedsVerify`) |
| 1 | Input valid data | ✅ | 006 (1 TC · 1 biến thể) — form chỉ có 2 ô bắt buộc + 1 ô tuỳ chọn: bộ tối thiểu = 006, bộ đầy đủ (có tích Remember me) = 040 |
| 1 | Save | ✅ | 006 — "lưu" của module là gửi form đăng nhập; 008 gửi đăng xuất |
| 1 | Verify data | ✅ | 006 · 008 — trang đích Dashboard / Login. Không có bản ghi để đối chiếu ở danh sách + chi tiết |
| 2 | UI Behavior | ✅ | 021 · 022 · 023 (3 TC · 3 biến thể) — tích/bỏ tích, Enter gửi form, dán/sao chép |
| 2 | Required | ✅ | 009 · 010 · 011 · 012 · 013 · 026 (6 TC · 6 biến thể) — bỏ trống từng ô + cả hai |
| 2 | Validation | ✅ | 014 → 020 · 027 · 029 (9 TC · 33 biến thể) · đủ 9/9 mục Email (Login) · 7/7 Password · 4/4 Checkbox · 9/9 Email (Forgot) |
| 2 | Equivalence Partitioning | ✅ | Email: sai định dạng (014) · đúng định dạng không tồn tại (015) · tồn tại + sai mật khẩu (035) · tồn tại + đúng (006) · contact (050) · vô hiệu hoá (051). Bộ đếm khoá theo loại email: không tồn tại (072) · contact (081) · staff vô hiệu hoá (082) · cùng email khác kiểu chữ (073) · cùng email thêm khoảng trắng (083) · email khác cùng máy (070) |
| 2 | Boundary Value Analysis | ✅ | 016 (4 biến thể: 254 · 255 · 256 · 1000 ký tự) · 017-a · b · c (1 · 256 · 1000 ký tự) · 029-d — giao diện không đặt min/max, biên kiểm là độ dài cực trị · 036 (biên số lần sai: lần 5 vẫn `Invalid email or password` / lần 6 thông báo khoá) · 069 (4 lần sai rồi đúng — dưới ngưỡng) · 076 (4 sai + 2 bỏ trống + 1 sai — bỏ trống không chạm ngưỡng) · 068 · 074 (biên thời gian T + 15 phút) |
| 2 | Business Rule | ✅ | 028 · 030 · 033 · 034 (thông báo trung tính) · 035 (thông báo chung) · 036 (khoá sau 5 lần sai — `CRM-LOGIN-101`) · 066 → 069 · 075 · 076 (thông báo khoá, chặn mật khẩu đúng, thời hạn 15 phút, đặt lại bộ đếm) · 037 (luôn về Dashboard) · 038 · 039 (đã đăng nhập bị chuyển) |
| 2 | Decision Table | ✅ | 15 rule R1 → R15 (thêm điều kiện "Trạng thái khoá" theo `CRM-LOGIN-101`) — bảng ở mục Bảng Đối Soát Coverage, mỗi rule ≥ 1 TC |
| 2 | State Transition | ✅ | Ma trận phiên **4 trạng thái** (thêm "Tài khoản bị khoá" theo `CRM-LOGIN-101`) — vào trạng thái khoá: 036 · 066; trong trạng thái khoá: 067 · 071 · 074 · 077 · 078; ra khỏi trạng thái khoá: 068 · 075; phiên khác: 070 · 079 · 080 (bảng ở trên) |
| 2 | Dependency | ✅ | 040 · 041 · 042 · 043 — Remember me ↔ tự đăng nhập lại ↔ đăng xuất · 080 (ghi nhớ đăng nhập ↔ khoá tài khoản) · 077 (khoá ↔ Forgot Password) |
| 2 | Use Case / Scenario | ✅ | 037 (mở trang nội bộ → bị chặn → đăng nhập → Dashboard) · 043 (đăng nhập ghi nhớ → đăng xuất → truy cập lại) · 032 (quên mật khẩu → email → đặt lại → đăng nhập) · 078 (bị khoá → quên mật khẩu → đặt lại → vẫn bị khoá) |
| 2 | Save / Edit / Delete | ➖ | Module không có bản ghi CRUD — chỉ có gửi form xác thực |
| 2 | Error Guessing | ✅ | 024 (bấm Login 2 lần) · 025 (mất mạng) · 047 (đăng xuất ở tab khác) · 045 (Back sau đăng xuất) · 079 (bị khoá khi đang có phiên ở trình duyệt khác) |
| 3 | Permission | ✅ | 049 · 050 · 051 · 033 · 034 · 038 · 039 · 081 · 082 (contact / staff vô hiệu hoá không bị khoá) — hệ thống chỉ có role Admin (`AMB-LOGIN-18`); phân quyền ở module này là **loại tài khoản**: chưa đăng nhập · staff hoạt động · staff vô hiệu hoá · contact |
| 3 | Security | ✅ | 028 · 030 · 035 (chống dò tài khoản) · 044 · 045 · 047 · 060 (huỷ phiên) · 052 · 053 · 054 (SQLi/XSS) · 055 (cookie) · 057 · 058 · 059 (mã chống giả mạo) · 036 · 067 · 070 → 074 · 078 · 083 (khoá chống dò mật khẩu — không vượt khoá bằng mật khẩu đúng, IP khác, kiểu chữ, khoảng trắng, thử lại, đặt lại mật khẩu) |
| 3 | API | ➖ | QA chưa xác nhận quyền gọi API — mặc định đội Dev xác minh (`docs/requirements/README.md`, 12-10-2026). REQ-25 kiểm qua DevTools ở 058 |
| 3 | Database | ➖ | QA chưa xác nhận quyền truy vấn CSDL — đội Dev xác minh |
| 3 | Integration | ✅ | 031 · 032 · 077 · 078 (gửi email đặt lại mật khẩu) — `@Skip` chờ hộp thư test (`AMB-LOGIN-01` ⏭️) |
| 3 | Logging / Audit | ➖ | Requirements không có yêu cầu ghi nhật ký đăng nhập; QA không xem được nhật ký hoạt động (đã đo 12-10-2026 — *Access denied*) |
| 4 | Compatibility | ✅ | 065 (1 TC · 2 biến thể: Edge · Firefox). Safari **không** kiểm — máy QA chạy Windows |
| 4 | Responsive / UI Stability | ✅ | 064 (1 TC · 5 biến thể: 1920 · 1366 · 768 · 375 · phóng to 125%) |
| 4 | Accessibility | ✅ | 062 · 063 (Tab, Space, Enter, viền chọn). Quét WCAG đầy đủ, văn bản thay thế của logo: ➖ cần công cụ quét riêng — chưa có người phụ trách, đề xuất đưa vào Master Test Plan |
| 4 | Performance | ➖ | Không có ngưỡng thời gian đã cam kết cho trang Login; đo tải thuộc đội Hạ tầng — chưa có kế hoạch |
| 4 | Regression | ➖ | Module chưa có bug nào được đóng (`docs/bugs/` chưa tồn tại) |
| 4 | E2E | ➖ | Login không chuyển dữ liệu sang module khác — chỉ là tiền đề; luồng đi tiếp nằm ở bộ TC của module đích |

## Rà soát đặc tính chất lượng (ISO/IEC 25010:2023)

| Đặc tính | Trạng thái | TC ID / Lý do |
|---|---|---|
| Functional Suitability | ✅ Có TC | 001 → 054 · 066 → 083 |
| Performance Efficiency | ➖ Ngoài phạm vi | Không có ngưỡng thời gian cam kết; đo tải — đội Hạ tầng, chưa có kế hoạch |
| Compatibility | ✅ Có TC | 065 (Edge · Firefox). Safari không kiểm — QA không có macOS |
| Interaction Capability | ✅ Có TC | 001 · 003 (nhãn) · 009 → 011 (thông báo lỗi dễ hiểu) · 014 (chặn thao tác sai) · 021 · 022 · 062 · 063 (bàn phím) |
| Reliability | ✅ Có TC | 024 (bấm 2 lần) · 025 (mất mạng rồi gửi lại) · 047 (2 tab) · 048 (hết phiên) · 061 (mã chống giả mạo hết hạn) · 068 · 075 (tự mở khoá sau 15 phút) · 079 · 080 (khoá không làm rơi phiên đang dùng) |
| Security | ✅ Có TC | 028 · 030 · 035 · 036 · 066 → 074 · 078 · 083 (cơ chế khoá chống dò mật khẩu — `CRM-LOGIN-101`) · 044 · 045 · 047 · 049 → 060. Pentest, quét header bảo mật (`RISK-LOGIN-05`): ➖ đội Dev / Security — báo theo `RISK-LOGIN-05` |
| Maintainability | ➖ Không áp dụng | Đặc tính của mã nguồn — đội Dev (code review), không kiểm bằng manual TC |
| Flexibility | ✅ Có TC | 064 (5 kích thước màn hình). Đổi ngôn ngữ: ➖ trang Login không có bộ chọn ngôn ngữ (`RISK-LOGIN-06`) — PO quyết khi có yêu cầu đa ngữ |
| Safety | ➖ Không áp dụng | App nghiệp vụ, lỗi đăng nhập không gây thiệt hại vật lý — PO xác nhận khi lập Master Test Plan |

## Đối soát cột Automation

| Nền tảng | Yes | Partial | No | ⏸️ Hoãn |
|---|---|---|---|---|
| Web | 54 | 29 | 0 | 31 (10 Yes · 21 Partial) |

### Điều kiện cần chuẩn bị

| # | Điều kiện | Ai cấp | Trạng thái | TC phụ thuộc |
|---|---|---|---|---|
| 1 | Tài khoản staff test riêng (không dùng chung `.env`) | PO / quản trị môi trường | ⏳ Chưa có — `AMB-LOGIN-01` ⏭️ | 030 · 035 |
| 2 | Hộp thư test đọc được qua API (Mailpit / MailHog…) gắn với tài khoản test | DevOps | ⏳ Chưa có | 031 · 032 · 033 · 034 · 077 · 078 (hộp thư của tài khoản Project Manager) |
| 3 | Tài khoản contact test · staff ở trạng thái vô hiệu hoá | PO / quản trị môi trường | ⏳ Chưa có | 033 · 034 · 050 · 051 · 081 · 082 |
| 4 | Được phép bật bộ đếm giờ chấm công trên tài khoản test | PO | ⏳ Chưa có — `AMB-LOGIN-17` | 046 |
| 5 | Thời hạn phiên rút ngắn trên môi trường test (thay cho chờ 8 giờ) | Dev backend | ⏳ Chưa có | 048 |
| 6 | Tài khoản Admin đăng nhập được (`.env`) | — | ✅ Đã có | 006 · 020 · 037 → 045 · 047 · 055 · 056 · 060 · 065 |
| 7 | Tài khoản Project Manager trong `.env` (`EMAIL_PM` · `PASSWORD_PM`) — dành riêng cho TC khoá tài khoản | User | ⏳ User cấp 22-10-2026, chờ thêm vào `.env` | 036 · 066 → 071 · 073 → 080 · 083 |
| 8 | Dev triển khai ticket `CRM-LOGIN-101` lên môi trường demo | Dev | ⏳ Chưa có | 036 · 066 → 083 |
| 9 | Thiết bị / mạng thứ hai có IP công khai khác (VD runner CI ở mạng khác, hoặc proxy) | DevOps | ⏳ Chưa có | 071 |
| 10 | Thời hạn khoá rút ngắn trên môi trường test (thay cho chờ 15 phút) | Dev backend | ⏳ Chưa có | 068 · 074 · 075 |

### TC Partial · No · Hoãn

| TC ID | Nền tảng | Automation | Trục chặn | Điều kiện · phần kiểm tay · lý do |
|---|---|---|---|---|
| CRM_LOGIN_TC_005 | Web | Yes · ⏸️ Hoãn | — | `@NeedsVerify` — chờ recon trang đích của logo |
| CRM_LOGIN_TC_012 | Web | Yes · ⏸️ Hoãn | — | `@NeedsVerify` — `ASM-LOGIN-01` |
| CRM_LOGIN_TC_013 | Web | Yes · ⏸️ Hoãn | — | `@NeedsVerify` — `ASM-LOGIN-02` |
| CRM_LOGIN_TC_020 | Web | Yes · ⏸️ Hoãn | — | `@NeedsVerify` — `ASM-LOGIN-03` |
| CRM_LOGIN_TC_030 | Web | Partial | 2 · tiền đề | Điều kiện #1 |
| CRM_LOGIN_TC_031 | Web | Partial | 1 · Email | Điều kiện #1 · #2 |
| CRM_LOGIN_TC_032 | Web | Partial · ⏸️ Hoãn | 1 · Email | Điều kiện #1 · #2 · `@NeedsVerify` trang đặt mật khẩu mới |
| CRM_LOGIN_TC_033 | Web | Partial | 1 · Email | Điều kiện #2 · #3 |
| CRM_LOGIN_TC_034 | Web | Partial | 1 · Email | Điều kiện #2 · #3 |
| CRM_LOGIN_TC_035 | Web | Partial | 2 · tiền đề · môi trường dùng chung | Điều kiện #1 — cấm thử sai trên tài khoản dùng chung |
| CRM_LOGIN_TC_036 | Web | Partial · ⏸️ Hoãn | 2 · khoá tài khoản dùng chung | Điều kiện #7 · #8 — tính năng chưa lên môi trường test · `@NeedsVerify` kiểu khung thông báo khoá · làm khoá tài khoản Project Manager 15 phút → chạy tuần tự |
| CRM_LOGIN_TC_046 | Web | Partial · ⏸️ Hoãn | 2 · tiền đề | Điều kiện #4 · `@NeedsVerify` hộp thoại |
| CRM_LOGIN_TC_048 | Web | Partial | 1 · phụ thuộc thời gian thật phía máy chủ | Điều kiện #5 — đồng hồ trình duyệt không tác động hết hạn phiên |
| CRM_LOGIN_TC_050 | Web | Partial | 2 · tiền đề | Điều kiện #3 |
| CRM_LOGIN_TC_051 | Web | Partial | 2 · tiền đề | Điều kiện #3 |
| CRM_LOGIN_TC_058 | Web | Yes · ⏸️ Hoãn | — | `@NeedsVerify` — `ASM-LOGIN-08` |
| CRM_LOGIN_TC_061 | Web | Yes · ⏸️ Hoãn | — | `@NeedsVerify` — `ASM-LOGIN-07` |
| CRM_LOGIN_TC_062 | Web | Yes · ⏸️ Hoãn | — | `@NeedsVerify` — `ASM-LOGIN-10` |
| CRM_LOGIN_TC_063 | Web | Yes · ⏸️ Hoãn | — | `@NeedsVerify` — `ASM-LOGIN-10` |
| CRM_LOGIN_TC_064 | Web | Partial · ⏸️ Hoãn | 2 · Expected cần mắt người | Phần tử hiện/ẩn theo kích thước: tự động được · "không đè chữ, không tràn" kiểm tay · `@NeedsVerify` |
| CRM_LOGIN_TC_065 | Web | Yes · ⏸️ Hoãn | — | `@NeedsVerify` — `ASM-LOGIN-11` |
| CRM_LOGIN_TC_066 | Web | Partial · ⏸️ Hoãn | 2 · khoá tài khoản dùng chung | Điều kiện #7 · #8 · `@NeedsVerify` kiểu khung thông báo khoá |
| CRM_LOGIN_TC_067 | Web | Partial · ⏸️ Hoãn | 2 · khoá tài khoản dùng chung | Điều kiện #7 · #8 |
| CRM_LOGIN_TC_068 | Web | Partial · ⏸️ Hoãn | 1 · phụ thuộc thời gian thật phía máy chủ | Điều kiện #7 · #8 · #10 |
| CRM_LOGIN_TC_069 | Web | Partial · ⏸️ Hoãn | 2 · khoá tài khoản dùng chung | Điều kiện #7 · #8 — lỗi hệ thống sẽ khoá tài khoản |
| CRM_LOGIN_TC_070 | Web | Partial · ⏸️ Hoãn | 2 · khoá tài khoản dùng chung | Điều kiện #7 · #8 |
| CRM_LOGIN_TC_071 | Web | Partial · ⏸️ Hoãn | 2 · tiền đề | Điều kiện #7 · #8 · #9 |
| CRM_LOGIN_TC_072 | Web | Yes · ⏸️ Hoãn | — | Điều kiện #8 — không khoá tài khoản test, tự động hoá được ngay khi triển khai |
| CRM_LOGIN_TC_073 | Web | Partial · ⏸️ Hoãn | 2 · khoá tài khoản dùng chung | Điều kiện #7 · #8 |
| CRM_LOGIN_TC_074 | Web | Partial · ⏸️ Hoãn | 1 · phụ thuộc thời gian thật phía máy chủ | Điều kiện #7 · #8 · #10 |
| CRM_LOGIN_TC_075 | Web | Partial · ⏸️ Hoãn | 1 · phụ thuộc thời gian thật phía máy chủ | Điều kiện #7 · #8 · #10 · 2 chu kỳ khoá (`ASM-LOGIN-13`) |
| CRM_LOGIN_TC_076 | Web | Partial · ⏸️ Hoãn | 2 · khoá tài khoản dùng chung | Điều kiện #7 · #8 |
| CRM_LOGIN_TC_077 | Web | Partial · ⏸️ Hoãn | 1 · Email | Điều kiện #2 · #7 · #8 |
| CRM_LOGIN_TC_078 | Web | Partial · ⏸️ Hoãn | 1 · Email | Điều kiện #2 · #7 · #8 · `@NeedsVerify` trang đặt mật khẩu mới · đổi `PASSWORD_PM` |
| CRM_LOGIN_TC_079 | Web | Partial · ⏸️ Hoãn | 2 · khoá tài khoản dùng chung | Điều kiện #7 · #8 — cần 2 context trình duyệt |
| CRM_LOGIN_TC_080 | Web | Partial · ⏸️ Hoãn | 2 · khoá tài khoản dùng chung | Điều kiện #7 · #8 — xoá cookie phiên làm được bằng script |
| CRM_LOGIN_TC_081 | Web | Partial · ⏸️ Hoãn | 2 · tiền đề | Điều kiện #3 · #8 |
| CRM_LOGIN_TC_082 | Web | Partial · ⏸️ Hoãn | 2 · tiền đề | Điều kiện #3 · #8 — nguyên văn thông báo chưa biết |
| CRM_LOGIN_TC_083 | Web | Partial · ⏸️ Hoãn | 2 · Expected không quan sát được qua giao diện | Điều kiện #7 · #8 · `ASM-LOGIN-15` — nên kiểm ở tầng API |

> `CRM_LOGIN_TC_028` (`@KnownBug`) **không** hoãn — vẫn automate, test FAIL chính là thứ phơi bug REQ-39.
>
> **Thứ tự ưu tiên automate:** `@Smoke` · `@CriticalPath` (001 → 008 · 049) → TC nhiều biến thể (014 · 015 · 016 · 017 · 027 · 029 · 052 · 053 · 054) → phần `@Regression` Priority High còn lại.

## Nhật ký thay đổi

| Ngày | Nguồn | TC ảnh hưởng | Loại | Tóm tắt | Mốc git |
|---|---|---|---|---|---|
| 20-10-2026 | `/generate-testcases-from-requirements` (QUICK · GỘP) | `CRM_LOGIN_TC_001` → `065` | 🟢 Thêm | Khởi tạo bộ TC web từ `requirements_login_web.md` bản 1.2 — 65 TC · 115 biến thể · 3 part · phủ 50/50 REQ | `e2ae96b` (trước khi tạo — thư mục `docs/testcases/` chưa tồn tại) |
| 23-10-2026 | `/update-testcases-from-impact` APPLY · ticket `CRM-LOGIN-101` | `CRM_LOGIN_TC_036` | ✏️ Sửa | V2 · Business Rule + BVA: đảo Expected theo REQ-LOGIN-46 🟡 — "không giới hạn số lần sai" → sai 5 lần liên tiếp bị khoá; lần 1 → 5 `Invalid email or password`, lần thử thứ 6 `Your account is locked. Please try again in 15 minutes.` Tài khoản phép thử đổi sang Project Manager (cấm Admin). Giữ `@Skip` (chưa triển khai), thêm `@NeedsVerify` (kiểu khung thông báo khoá chưa có evidence). Automation `Partial · ⏸️ Hoãn` | `web/parts/part_02_web_forgot_phien.md` @ `f8025be` |
| 23-10-2026 | ↑ | — (phần ngoài dòng TC) | ✏️ Biên tập | Header part_02 (tài khoản Project Manager, lý do `@Skip`) · metadata nguồn → bản 1.5 · Coverage: REQ-46 🟡 + 18 dòng REQ-51 → 68 chưa có TC (50/68) · Decision Table thêm điều kiện "Trạng thái khoá" (R10 → R15) · State Transition thêm trạng thái "Tài khoản bị khoá" + cột "Hết 15 phút" · 4 vòng (Display · BVA · Business Rule · Decision Table · State Transition) · ISO Security · Automation (Điều kiện #7 · #8) · `ASM-LOGIN-03` bỏ trỏ sai `AMB-LOGIN-22`. Delta TC List: [`impact/delta_tc_CRM-LOGIN-101.md`](impact/delta_tc_CRM-LOGIN-101.md) | `TEST_CASES_LOGIN_SUMMARY.md` @ `f8025be` |
| 23-10-2026 | `/generate-testcases-from-requirements` (QUICK · chế độ BỔ SUNG · GỘP) · ticket `CRM-LOGIN-101` | `CRM_LOGIN_TC_066` → `083` | 🟢 Thêm | 18 TC cho REQ-LOGIN-51 → 68 (STORY-LOGIN-07) vào part mới [`web/parts/part_04_web_khoa_tai_khoan.md`](web/parts/part_04_web_khoa_tai_khoan.md) — toàn bộ `@Skip` (chưa triển khai), tài khoản Project Manager. Index: Bản đồ tài liệu · nhãn · `ASM-LOGIN-13` → `15` · Coverage 68/68 · Decision Table R10 → R15 · State Transition · 4 vòng · ISO · Automation (Điều kiện #9 · #10). Header part_01 → 03 đổi số phần `/3` → `/4`. Không chạm dòng TC cũ | `f8025be` + thay đổi chưa commit của dòng trên (cùng ngày) · `part_04` tạo mới |
