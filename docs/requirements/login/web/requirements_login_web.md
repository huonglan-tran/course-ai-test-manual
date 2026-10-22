# Requirements — Login (`LOGIN`) · Web

> ← [Index module `REQUIREMENTS_LOGIN_SUMMARY.md`](../REQUIREMENTS_LOGIN_SUMMARY.md) — phân quyền, trạng thái phiên, AMB/RISK, Story, Nhật ký nằm ở index.
>
> File này chứa **REQ chỉ áp nền tảng Web** của module `LOGIN`: `REQ-LOGIN-01` → `REQ-LOGIN-68`.

## Bảng metadata khảo sát

| Thuộc tính | Giá trị |
|---|---|
| **Nền tảng** | Web — khu Admin |
| **Route** | `/admin/authentication` (Login) · `/admin/authentication/forgot_password` (Forgot Password) · `/admin/authentication/logout` (Đăng xuất) · `/admin/authentication/reset_password` (đặt lại mật khẩu — chỉ quan sát mã lỗi, xem `AMB-LOGIN-10`) |
| **Ngày khảo sát** | 12-10-2026 |
| **Trình duyệt khảo sát** | Google Chrome (Playwright MCP, headed), viewport `1600×780` (đo bằng `innerWidth × innerHeight`, cửa sổ 1618×915 — không bị cắt). Mọi AC dựa trên thông báo mặc định của trình duyệt **chỉ đúng với trình duyệt này** |
| **Ngữ cảnh đo** | Phép thử khi **chưa đăng nhập** chạy trong **context trình duyệt mới, không cookie, không mật khẩu đã lưu** (không có autofill). Phép thử khi **đã đăng nhập** chạy trên phiên Playwright chính — người dùng tự nhập thông tin đăng nhập, agent chỉ quan sát |
| **Tài khoản** | Tài khoản staff "Admin" trong `.env` — agent **không** nhập mật khẩu thật. Phép thử lỗi dùng email **không tồn tại** do agent sinh: `auto_login_<timestamp>@auto.test` · `auto_forgot_<timestamp>@auto.test`, mật khẩu là chuỗi ngẫu nhiên `Auto_<timestamp>_x` |
| **Tầng network** | Quan sát thụ động request `document` do form/điều hướng tự phát sinh (method · status · `Location`). Ghi chú: 1 request `GET /admin/authentication/reset_password` được gọi trực tiếp trong nhánh bắt lỗi của script đo — chỉ đọc, kết quả bị loại bỏ, **không** dùng làm căn cứ REQ |
| **Bản đồ phủ tài liệu** | Không có tài liệu — toàn bộ REQ sinh từ khảo sát UI thực tế. (`docs/SRS_Login.md` cũ trong git **không** được dùng — user chốt 12-10-2026) |

## 1. Yêu cầu chức năng — Web

> **Quy ước trạng thái trong tài liệu này:** ⚪ = **chưa kiểm chứng được** trên UI (thiếu tài khoản test / không được phép ghi dữ liệu / nằm ở tầng server / **chưa được triển khai** — REQ-51 → 68) — REQ vẫn được cấp theo skill mục 4.3.8 để không mất độ phủ; TC viết trước và đánh `skip` tới khi gỡ được AMB tương ứng.

### STORY-LOGIN-01 — Giao diện trang Login

| REQ ID | Tên yêu cầu | Mô tả | Acceptance Criteria | Trạng thái | Cập nhật lần cuối | Nguồn |
|---|---|---|---|---|---|---|
| REQ-LOGIN-01 | Trang Login có đủ thành phần form đăng nhập | Là staff, tôi thấy form đăng nhập khi mở trang Login | Mở `/admin/authentication` khi chưa đăng nhập → heading `Login` · ô `Email Address` · ô `Password` · checkbox `Remember me` · nút `Login` · link `Forgot Password?` — cả 6 phần tử render (`offsetParent ≠ null`, kích thước > 0) ở viewport 1600×780 | 🟢 | — | UI thực tế |
| REQ-LOGIN-02 | Ô Email Address được focus sẵn khi mở trang | Người dùng gõ được ngay không cần bấm vào ô | Mở trang Login → `document.activeElement` là `#email` (ô có thuộc tính `autofocus`) | 🟢 | — | UI thực tế |
| REQ-LOGIN-03 | Ô Password che ký tự nhập | Mật khẩu không hiển thị dạng rõ | `#password` có `type="password"`; không có nút hiện/ẩn mật khẩu | 🟢 | — | UI thực tế |
| REQ-LOGIN-04 | Checkbox Remember me mặc định không được tick | Ghi nhớ đăng nhập là lựa chọn chủ động | Mở trang Login → `#remember.checked === false` | 🟢 | — | UI thực tế |
| REQ-LOGIN-05 | Tiêu đề tab trang Login chứa "Login" | Phân biệt trang Login trên tab trình duyệt | `document.title` **chứa** `Login` (quan sát: `Perfex CRM \| Anh Tester Demo - Login` — phần trước `- Login` là tên công ty cấu hình được, **không** assert khớp tuyệt đối) | 🟢 | — | UI thực tế |
| REQ-LOGIN-06 | Logo trên trang Login liên kết về trang chủ "/" | Logo là liên kết | Logo là thẻ `<a href="/">` (trang chủ phía khách hàng, không phải `/admin/`). Chưa bấm thử | 🟢 | — | UI thực tế |
| REQ-LOGIN-07 | Link "Forgot Password?" mở trang Forgot Password | Người quên mật khẩu đi tới trang khôi phục | Bấm `Forgot Password?` → URL `/admin/authentication/forgot_password`, heading `Forgot Password` | 🟢 | — | Kiểm chứng thực tế |

### STORY-LOGIN-02 — Validation & xử lý lỗi đăng nhập

| REQ ID | Tên yêu cầu | Mô tả | Acceptance Criteria | Trạng thái | Cập nhật lần cuối | Nguồn |
|---|---|---|---|---|---|---|
| REQ-LOGIN-08 | Bỏ trống Email Address bị máy chủ từ chối kèm thông báo bắt buộc | Validation nằm ở server — ô không có `required` | Để trống Email, nhập Password bất kỳ, bấm `Login` → form **được gửi** (`POST /admin/authentication`), trang ở lại `/admin/authentication`, hiện alert đỏ (`.alert.alert-danger`) nguyên văn `The Email Address field is required.` | 🟢 | — | Kiểm chứng thực tế · API `POST /admin/authentication` · 200 |
| REQ-LOGIN-09 | Bỏ trống Password bị máy chủ từ chối kèm thông báo bắt buộc | | Nhập Email đúng định dạng, để trống Password, bấm `Login` → ở lại `/admin/authentication`, alert đỏ nguyên văn `The Password field is required.` | 🟢 | — | Kiểm chứng thực tế · API `POST /admin/authentication` · 200 |
| REQ-LOGIN-10 | Bỏ trống cả hai ô hiển thị đồng thời cả hai thông báo bắt buộc | | Bấm `Login` khi cả 2 ô trống → hiện **2** alert đỏ: `The Password field is required.` và `The Email Address field is required.` Quan sát thứ tự: Password trước, Email sau — **không** assert thứ tự (`AMB-LOGIN-15`) | 🟢 | — | Kiểm chứng thực tế |
| REQ-LOGIN-11 | Email sai định dạng bị trình duyệt chặn, không gửi request | Ô `type="email"` | Nhập `abc` vào Email, Password bất kỳ, bấm `Login` → `#email.checkValidity() === false` · **không** có request `POST /admin/authentication` · không có alert của ứng dụng. Chuỗi Chrome quan sát được (`Please include an '@' in the email address. 'abc' is missing an '@'.`) **cấm dùng làm assertion** — đổi theo trình duyệt/ngôn ngữ | 🟢 | — | Kiểm chứng thực tế |
| REQ-LOGIN-12 | Email không tồn tại bị từ chối với thông báo chung | | Nhập email không tồn tại (`auto_login_<timestamp>@auto.test`) + mật khẩu bất kỳ, bấm `Login` → quay về `/admin/authentication`, alert đỏ nguyên văn `Invalid email or password`. Quan sát network: `POST /admin/authentication` → `303` → `GET /admin/authentication` | 🟢 | — | Kiểm chứng thực tế · API `POST /admin/authentication` · 303 |
| REQ-LOGIN-13 | Sai mật khẩu với email tồn tại nhận cùng thông báo chung | Không để lộ email nào có tài khoản qua trang Login | Email **tồn tại** + mật khẩu sai → thông báo **giống hệt** REQ-LOGIN-12 (`Invalid email or password`). Chưa thử — không có tài khoản test riêng, thử sai trên tài khoản dùng chung có thể gây khoá (`AMB-LOGIN-01`, `AMB-LOGIN-02`) | ⚪ | — | Chưa kiểm chứng |
| REQ-LOGIN-14 | Máy chủ không điền lại email đã nhập sau khi đăng nhập lỗi | Đo ở tầng HTML, không phải màn hình người dùng | Trong context **không có mật khẩu đã lưu**: submit lỗi theo REQ-08/09/12 → sau khi trang tải lại, `#email.value === ''` và **không** có thuộc tính `value` trên `#email`. ⚠️ Trình duyệt có mật khẩu đã lưu sẽ **tự điền** ô này — TC **cấm** assert ô trống khi chạy trên profile có autofill (`RISK-LOGIN-02`) | 🟢 | — | Kiểm chứng thực tế |
| REQ-LOGIN-15 | Ô Password được xoá trống sau khi đăng nhập lỗi | | Trong context không có autofill: submit lỗi theo REQ-LOGIN-12 → `#password.value === ''` | 🟢 | — | Kiểm chứng thực tế |
| REQ-LOGIN-16 | Nhấn Enter trong ô Password gửi form đăng nhập | | Điền Email + Password, đặt con trỏ ở ô Password, nhấn `Enter` → có request `POST /admin/authentication` (kết quả giống bấm nút `Login`) | 🟢 | — | Kiểm chứng thực tế |
| REQ-LOGIN-44 | Contact đăng nhập khu Admin bị từ chối như email không tồn tại | Tài khoản cổng khách hàng không dùng được cho khu Admin | Nhập email + mật khẩu đúng của một **contact** vào trang Login khu Admin → không vào `/admin/`, ở lại `/admin/authentication`, alert nguyên văn `Invalid email or password` (giống REQ-LOGIN-12). Chưa kiểm — chưa có account contact | ⚪ | — | User xác nhận `AMB-LOGIN-20` (19-10-2026) — chưa kiểm chứng UI |
| REQ-LOGIN-48 | Staff bị vô hiệu hoá không đăng nhập được | | Tài khoản staff ở trạng thái vô hiệu hoá + thông tin đúng → không vào `/admin/`, ở lại trang Login kèm alert (nguyên văn chưa biết — ghi lại khi kiểm). Chưa kiểm — chưa có account | ⚪ | — | User xác nhận `AMB-LOGIN-19` (19-10-2026) — chưa kiểm chứng UI |

### STORY-LOGIN-03 — Đăng nhập thành công & chặn truy cập

| REQ ID | Tên yêu cầu | Mô tả | Acceptance Criteria | Trạng thái | Cập nhật lần cuối | Nguồn |
|---|---|---|---|---|---|---|
| REQ-LOGIN-17 | Đăng nhập thành công chuyển tới Dashboard | | Đăng nhập bằng thông tin đúng → URL `/admin/`, tiêu đề tab `Dashboard`, sidebar `#side-menu` hiển thị | 🟢 | — | Kiểm chứng thực tế (user tự nhập thông tin, agent quan sát trang đích) |
| REQ-LOGIN-18 | Sau đăng nhập không quay lại trang đã định truy cập | Luôn về Dashboard | Khi chưa đăng nhập, mở `/admin/clients` → bị đưa về Login → đăng nhập đúng → trang đích là `/admin/` (Dashboard), **không** phải `/admin/clients` (`AMB-LOGIN-06`) | 🟢 | — | Kiểm chứng thực tế (user tự nhập thông tin) |
| REQ-LOGIN-19 | Chưa đăng nhập truy cập trang nội bộ bị chuyển về trang Login | | Context chưa đăng nhập, mở lần lượt `/admin/` · `/admin/clients` · `/admin/invoices/invoice` · `/admin/profile` → đều dừng ở `/admin/authentication`, heading `Login`, **không** kèm alert. Quan sát mã chuyển hướng `307` — **không** assert mã (`AMB-LOGIN-09`) | 🟢 | — | Kiểm chứng thực tế |
| REQ-LOGIN-20 | Đã đăng nhập mở trang Login bị chuyển về Dashboard | | Phiên đã đăng nhập, mở `/admin/authentication` → dừng ở `/admin/`, tiêu đề `Dashboard` | 🟢 | — | Kiểm chứng thực tế |
| REQ-LOGIN-21 | Đã đăng nhập mở trang Forgot Password bị chuyển về Dashboard | | Phiên đã đăng nhập, mở `/admin/authentication/forgot_password` → dừng ở `/admin/`, tiêu đề `Dashboard` | 🟢 | — | Kiểm chứng thực tế |
| REQ-LOGIN-22 | Cookie phiên được bảo vệ bằng cờ HttpOnly, Secure, SameSite=Lax | | Cookie `sp_session` (chuỗi 40 ký tự) có `HttpOnly = true` · `Secure = true` · `SameSite = Lax`. Ghi **hình thái**, cấm chép giá trị | 🟢 | — | UI thực tế (đọc cookie trình duyệt) |
| REQ-LOGIN-23 | Hạn cookie phiên luôn tính từ lần tải trang gần nhất cộng 8 giờ | Hạn trượt (sliding) — một rule duy nhất | Sau mỗi lần tải trang, `expires` của `sp_session` ≈ thời điểm tải + 8 giờ (quan sát 3 lần trong ngày 12-10-2026, mốc dời theo lần tải). Chỉ khẳng định **hình thái cookie** — rule hết hạn phiên phía server tách thành REQ-LOGIN-47 | 🟢 | 19-10-2026 · ✏️ biên tập | UI thực tế (đọc cookie trình duyệt) |
| REQ-LOGIN-24 | Form Login gửi kèm trường ẩn CSRF token | | Form có `<input type="hidden" name="csrf_token_name">` — giá trị 32 ký tự hex, đổi theo phiên (cấm chép giá trị) · kèm cookie `csrf_cookie_name` (32 ký tự, `HttpOnly`, `Secure`, `SameSite=Lax`, hạn ≈ 1 giờ) | 🟢 | — | UI thực tế |
| REQ-LOGIN-25 | Máy chủ từ chối submit Login thiếu hoặc sai CSRF token | Tác tạo REQ-24 phải có tác dụng (skill 4.3.8) | Submit form với token bị xoá/sửa → bị từ chối. Chưa kiểm — cần dựng request thủ công, ngoài phạm vi recon UI; QA chưa xác nhận quyền gọi API. Đã chốt: server **có** kiểm — đội Dev xác minh (`AMB-LOGIN-16` ✅) | ⚪ | 19-10-2026 · ✏️ biên tập | User chốt `AMB-LOGIN-16` — Dev xác minh |
| REQ-LOGIN-47 | Phiên hết hạn sau 8 giờ không hoạt động | Rule phía server — REQ-23 chỉ đo hình thái cookie | Đăng nhập **không** tick Remember me → không tải trang nào trong 8 giờ → mở `/admin/` → bị đưa về trang Login. (Có tick Remember me thì REQ-LOGIN-27 tự đăng nhập lại — không áp dụng phép thử này.) Chưa kiểm — cần chờ 8 giờ | ⚪ | — | User xác nhận `AMB-LOGIN-13` (19-10-2026) — chưa kiểm chứng UI |

### STORY-LOGIN-04 — Remember me

| REQ ID | Tên yêu cầu | Mô tả | Acceptance Criteria | Trạng thái | Cập nhật lần cuối | Nguồn |
|---|---|---|---|---|---|---|
| REQ-LOGIN-26 | Tick Remember me khi đăng nhập tạo cookie ghi nhớ | Tác tạo — vế "được tạo đúng hình thái" | Đăng nhập thành công **có** tick `Remember me` → xuất hiện cookie `autologin`: chuỗi không rỗng (quan sát 110 ký tự — độ dài có thể đổi theo id người dùng, **không** assert độ dài) · `Secure = true` · `SameSite = Lax` · `HttpOnly = false` (`AMB-LOGIN-11`) · hạn ≈ 62 ngày (quan sát 1.488 giờ) | 🟢 | — | Kiểm chứng thực tế (user tự đăng nhập có tick) |
| REQ-LOGIN-27 | Cookie ghi nhớ tự đăng nhập lại khi mất cookie phiên | Tác tạo — vế "dùng được" | Phép thử trạng thái sạch: (1) xoá cookie `sp_session`, giữ `autologin` → (2) xác nhận chỉ còn `csrf_cookie_name` + `autologin` → (3) mở `/admin/` → (4) trả `200`, vào thẳng Dashboard (có `#side-menu`), không qua trang Login, và trình duyệt được cấp `sp_session` mới | 🟢 | — | Kiểm chứng thực tế |
| REQ-LOGIN-28 | Không tick Remember me thì không tạo cookie ghi nhớ | Khẳng định phủ định — cần phép thử trạng thái sạch | (1) xoá sạch cookie → (2) xác nhận không còn `autologin` → (3) đăng nhập **không** tick → (4) không có cookie `autologin`. Chưa chạy đủ 4 bước — mới chỉ thấy một phiên đăng nhập trước đó không có `autologin`, chưa biết lúc đó có tick hay không | ⚪ | — | Chưa kiểm chứng |
| REQ-LOGIN-29 | Đăng xuất vô hiệu cookie ghi nhớ | Nếu không, người khác dùng chung máy vẫn tự đăng nhập được | Đăng nhập có tick → Logout → mở `/admin/` → bị đưa về Login (không tự đăng nhập lại bằng `autologin`). Chưa kiểm — lượt Logout thực hiện khi phiên **chưa** có `autologin`. Hành vi mong đợi đã được chốt (`AMB-LOGIN-12` ✅) | ⚪ | 19-10-2026 · ✏️ biên tập | User xác nhận `AMB-LOGIN-12` (19-10-2026) — chưa kiểm chứng UI |

### STORY-LOGIN-05 — Đăng xuất

| REQ ID | Tên yêu cầu | Mô tả | Acceptance Criteria | Trạng thái | Cập nhật lần cuối | Nguồn |
|---|---|---|---|---|---|---|
| REQ-LOGIN-30 | Menu avatar trên header có mục Logout | Lối đăng xuất dùng được ở desktop | Viewport 1600×780: bấm avatar (`li.header-user-profile > a.dropdown-toggle`) → dropdown hiện 5 mục `My Profile` · `My Timesheets` · `Edit Profile` · `Language` · `Logout`; mục `Logout` render 160×32. DOM còn 2 phần tử `Logout` khác **không** dùng được ở viewport này: 1 trong `div.mobile-navbar` (ẩn ở desktop), 1 nút trong popup cảnh báo timer (ẩn bằng `div.hide`) — locator phải bám `li.header-user-profile li.header-logout > a` | 🟢 | — | Kiểm chứng thực tế |
| REQ-LOGIN-31 | Bấm Logout đưa người dùng về trang Login | | Không có timer chấm công đang chạy → bấm `Logout` → `GET /admin/authentication/logout` → dừng ở `/admin/authentication`, heading `Login`, **không** kèm alert. Mã chuyển hướng quan sát `307` — không assert (`AMB-LOGIN-09`) | 🟢 | — | Kiểm chứng thực tế |
| REQ-LOGIN-32 | Sau đăng xuất, trang nội bộ bị chặn | Phiên bị huỷ thật, không chỉ chuyển trang | Ngay sau REQ-31, mở `/admin/clients` → bị đưa về `/admin/authentication` | 🟢 | — | Kiểm chứng thực tế |
| REQ-LOGIN-33 | Sau đăng xuất, nút Back không hiển thị lại trang nội bộ | Không lộ dữ liệu từ cache trình duyệt | Sau REQ-32, bấm Back của trình duyệt 2 lần → trang được tải lại từ server, dừng ở `/admin/authentication`; không có sidebar `#side-menu` | 🟢 | — | Kiểm chứng thực tế |
| REQ-LOGIN-34 | Đăng xuất cấp cookie phiên mới | Chống tái sử dụng phiên cũ | Giá trị `sp_session` sau Logout **khác** giá trị trước Logout (so sánh bằng nhau/khác nhau, không chép giá trị) | 🟢 | — | Kiểm chứng thực tế |
| REQ-LOGIN-35 | Có timer chấm công đang chạy thì Logout hiện cảnh báo xác nhận | | Khi có timer (`.started-timers-top li.timer`) → bấm `Logout` **không** đăng xuất ngay, hiện popup nội dung `Started tasks timers found!` / `Are you sure you want to logout without stopping the timers?` kèm nút `Logout` trỏ `/admin/authentication/logout`. Căn cứ: mã hàm `logout()` phía client + template `#timers-logout-template-warning` trong DOM. Chưa chạy thật — cần bật timer, tức ghi dữ liệu chấm công trên tài khoản dùng chung (`AMB-LOGIN-17`) | ⚪ | — | Mã JS phía client (chưa kiểm chứng hành vi) |

### STORY-LOGIN-06 — Forgot Password

| REQ ID | Tên yêu cầu | Mô tả | Acceptance Criteria | Trạng thái | Cập nhật lần cuối | Nguồn |
|---|---|---|---|---|---|---|
| REQ-LOGIN-36 | Trang Forgot Password có đủ thành phần form | | Mở `/admin/authentication/forgot_password` khi chưa đăng nhập → heading `Forgot Password` · ô `Email Address` (`type="email"`, không `required`) · nút `Confirm`. Không có link khác trên trang (`AMB-LOGIN-07`) | 🟢 | — | UI thực tế |
| REQ-LOGIN-37 | Bỏ trống email ở Forgot Password nhận thông báo "Email not found" | Không có thông báo "bắt buộc" riêng | Để trống, bấm `Confirm` → form được gửi (`POST /admin/authentication/forgot_password` → 200), ở lại trang, alert đỏ nguyên văn `Email not found` (`AMB-LOGIN-04`) | 🟢 | — | Kiểm chứng thực tế · API `POST /admin/authentication/forgot_password` · 200 |
| REQ-LOGIN-38 | Email sai định dạng ở Forgot Password bị trình duyệt chặn | | Nhập `abc`, bấm `Confirm` → `#email.checkValidity() === false` · **không** có request POST · không có alert của ứng dụng. Chuỗi của Chrome cấm assert | 🟢 | — | Kiểm chứng thực tế |
| REQ-LOGIN-39 | Email không tồn tại ở Forgot Password nhận thông báo trung tính, không tiết lộ email có tài khoản hay không | Chống dò tài khoản (account enumeration) | Nhập `auto_forgot_<timestamp>@auto.test`, bấm `Confirm` → ở lại trang, hiện thông báo nguyên văn: `Nếu địa chỉ email này có tài khoản trong hệ thống, chúng tôi đã gửi liên kết đặt lại mật khẩu tới đó. Vui lòng kiểm tra hộp thư (và cả thư mục Spam).` — nguyên văn do PO chốt 19-10-2026; **không** assert màu/kiểu alert (chưa có bản đã sửa để quan sát). ⚠️ **Hiện trạng 12-10-2026 lệch:** hệ thống báo `Email not found` (`POST /admin/authentication/forgot_password` → 200) → TC theo REQ này **dự kiến FAIL cho tới khi Dev sửa** (`AMB-LOGIN-03` ✅) | 🟡 | 19-10-2026 · `AMB-LOGIN-03` | Kiểm chứng thực tế (hiện trạng) + User chốt `AMB-LOGIN-03` (hành vi mong đợi) |
| REQ-LOGIN-40 | Email đã nhập được giữ lại sau khi Forgot Password báo lỗi | Ngược với trang Login (REQ-14) | Context không có autofill: sau REQ-39 → `#email.value` vẫn chứa email vừa nhập (`AMB-LOGIN-05`) | 🟢 | — | Kiểm chứng thực tế |
| REQ-LOGIN-41 | Form Forgot Password gửi kèm trường ẩn CSRF token | | Form có `<input type="hidden" name="csrf_token_name">` (32 ký tự hex, cấm chép giá trị) | 🟢 | — | UI thực tế |
| REQ-LOGIN-42 | Email tồn tại thì hệ thống gửi email đặt lại mật khẩu | Tác tạo — vế "được tạo" | Nhập email của tài khoản **tồn tại**, bấm `Confirm` → hộp thư đó nhận đúng 1 email đặt lại mật khẩu chứa link. Thông báo hiển thị trên trang: xem REQ-LOGIN-49. Chưa kiểm — không cấp tài khoản test có hộp thư; **cấm** thử bằng email tài khoản dùng chung (`AMB-LOGIN-01` ⏭️) | ⚪ | 19-10-2026 · ✏️ biên tập | Chưa kiểm chứng |
| REQ-LOGIN-43 | Link trong email đặt lại mật khẩu dùng được để đặt mật khẩu mới | Tác tạo — vế "dùng được" | Mở link trong email REQ-42 → trang đặt mật khẩu mới → đặt xong đăng nhập được bằng mật khẩu mới, mật khẩu cũ bị từ chối. Chưa kiểm (`AMB-LOGIN-01`). Ghi chú: mở `/admin/authentication/reset_password` **không kèm** tham số trả `500` (`AMB-LOGIN-10`) | ⚪ | — | Chưa kiểm chứng |
| REQ-LOGIN-45 | Contact dùng Forgot Password khu Admin được xử lý như email không tồn tại | | Nhập email của một **contact** vào Forgot Password khu Admin, bấm `Confirm` → **không** gửi email đặt lại mật khẩu khu Admin; thông báo trên trang giống hệt REQ-LOGIN-39. Chưa kiểm — chưa có account contact có hộp thư | ⚪ | — | User xác nhận `AMB-LOGIN-20` (19-10-2026), áp cho cả luồng Forgot Password — chưa kiểm chứng UI |
| REQ-LOGIN-49 | Email tồn tại ở Forgot Password nhận cùng thông báo trung tính như email không tồn tại | Vế còn lại của chống dò tài khoản — tách khỏi REQ-39 vì khác lớp input | Nhập email của tài khoản **tồn tại**, bấm `Confirm` → thông báo trên trang **giống hệt** nguyên văn của REQ-LOGIN-39 → người ngoài không phân biệt được email có tài khoản hay không. Chưa kiểm — không cấp tài khoản test (`AMB-LOGIN-01` ⏭️) | ⚪ | — | User chốt `AMB-LOGIN-03` (19-10-2026) — chưa kiểm chứng UI |
| REQ-LOGIN-50 | Staff bị vô hiệu hoá dùng Forgot Password được xử lý như email không tồn tại | Tài khoản đã khoá không lấy lại được quyền truy cập qua link reset | Nhập email của một **staff bị vô hiệu hoá**, bấm `Confirm` → **không** gửi email đặt lại mật khẩu; thông báo trên trang giống hệt REQ-LOGIN-39. Chưa kiểm — chưa có account staff vô hiệu hoá có hộp thư | ⚪ | — | User xác nhận `AMB-LOGIN-21` (19-10-2026) — chưa kiểm chứng UI |

### STORY-LOGIN-07 — Khoá tài khoản khi đăng nhập sai nhiều lần (ticket `CRM-LOGIN-101`)

> **Tài khoản phép thử:** tài khoản **Project Manager** do user cấp 22-10-2026 (`.env`: `EMAIL_PM` / `PASSWORD_PM` — không ghi giá trị vào tài liệu) — `AMB-LOGIN-30` ✅ lần 2, đúng ràng buộc AC#6 của ticket: **cấm** dùng tài khoản Admin. Mỗi phép thử khoá làm tài khoản Project Manager bị khoá 15 phút → các TC khoá chạy tuần tự, cách nhau ≥ 15 phút; TC dùng tài khoản Admin không bị ảnh hưởng (`RISK-LOGIN-09`). Toàn bộ Story **chưa kiểm chứng trên UI** và **chưa được triển khai** lên môi trường demo (user xác nhận 22-10-2026) — demo vẫn chạy hành vi cũ: không giới hạn số lần đăng nhập sai. TC của Story này đánh `@Skip` tới khi Dev báo đã triển khai.

| REQ ID | Tên yêu cầu | Mô tả | Acceptance Criteria | Trạng thái | Cập nhật lần cuối | Nguồn |
|---|---|---|---|---|---|---|
| REQ-LOGIN-46 | Đăng nhập sai mật khẩu 5 lần liên tiếp với cùng một email thì tài khoản bị khoá | Chống dò mật khẩu (`RISK-LOGIN-04`). **Thay** hành vi cũ "không giới hạn số lần đăng nhập sai" (`AMB-LOGIN-02` ✅ 19-10-2026) | (1) Bộ đếm sạch: tài khoản Project Manager vừa đăng nhập thành công rồi Logout (REQ-54) → (2) nhập đúng email + mật khẩu sai, bấm `Login`, lặp **5** lần → **cả 5 lần** ở lại `/admin/authentication` kèm alert `Invalid email or password` (lần thứ 5 **chưa** hiện thông báo khoá) → (3) lần thử **thứ 6** (mật khẩu sai) → hiện thông báo khoá của REQ-LOGIN-51. Mỗi lần sai dùng một chuỗi mật khẩu khác nhau. Chưa kiểm chứng. ⚠️ **Hiện trạng: chưa triển khai** (user xác nhận 22-10-2026) — demo vẫn không giới hạn số lần sai; chạy TC theo REQ này trước khi triển khai sẽ FAIL | 🟡 | 22-10-2026 · CRM-LOGIN-101 · `AMB-LOGIN-23` | Ticket CRM-LOGIN-101 · AC#1 (PO chốt 21-10-2026) + User chốt `AMB-LOGIN-23` (22-10-2026) — chưa kiểm chứng UI |
| REQ-LOGIN-51 | Tài khoản đang bị khoá thì trang Login hiển thị thông báo khoá | | Tài khoản Project Manager đang bị khoá theo REQ-46 → gửi form Login (từ lần thử thứ 6 trở đi) → ở lại `/admin/authentication`, hiển thị nguyên văn `Your account is locked. Please try again in 15 minutes.` Câu chữ cố định do PO chốt — **không** assert màu/kiểu alert (chưa quan sát). Chưa kiểm chứng | ⚪ | — | Ticket CRM-LOGIN-101 · AC#2 (PO chốt 21-10-2026) |
| REQ-LOGIN-52 | Đang bị khoá thì nhập đúng mật khẩu vẫn không đăng nhập được | Khoá chặn cả thông tin đúng | Sau lần sai thứ 5 (REQ-46), trong vòng 15 phút: nhập email + mật khẩu **đúng** của tài khoản Project Manager, bấm `Login` → **không** vào `/admin/`, ở lại `/admin/authentication`, hiện thông báo của REQ-LOGIN-51. Chưa kiểm chứng | ⚪ | — | Ticket CRM-LOGIN-101 · AC#3 (PO chốt 21-10-2026) |
| REQ-LOGIN-53 | Hết 15 phút khoá thì đăng nhập đúng thành công trở lại | Khoá có thời hạn | Tài khoản Project Manager bị khoá ở mốc T = lúc gửi lần sai thứ 5 (`AMB-LOGIN-24` ✅) → **không** thử đăng nhập trong khoảng T → T + 15 phút → sau T + 15 phút đăng nhập đúng → vào Dashboard `/admin/`. Phải chờ 15 phút. Chưa kiểm chứng | ⚪ | — | Ticket CRM-LOGIN-101 · AC#1 "khoá 15 phút" (PO chốt 21-10-2026) |
| REQ-LOGIN-54 | Đăng nhập thành công trước khi đủ 5 lần sai đặt lại bộ đếm về 0 | | (1) Tài khoản Project Manager, bộ đếm sạch → (2) sai mật khẩu 4 lần liên tiếp → (3) đăng nhập **đúng** → vào Dashboard → Logout → (4) sai thêm 4 lần liên tiếp → (5) đăng nhập đúng → vẫn vào Dashboard, **không** có thông báo khoá (tổng 8 lần sai nhưng không lần nào đủ 5 liên tiếp). Chưa kiểm chứng | ⚪ | — | Ticket CRM-LOGIN-101 · AC#4 (PO chốt 21-10-2026) |
| REQ-LOGIN-55 | Khoá một email không chặn email khác đăng nhập từ cùng máy, cùng IP | Không khoá theo IP | Tài khoản Project Manager đang bị khoá → cùng trình duyệt, cùng mạng: gửi form với email **khác** (`auto_login_<timestamp>@auto.test`, không tồn tại) + mật khẩu bất kỳ → alert `Invalid email or password` (REQ-12), **không** phải thông báo khoá. **Không** dùng tài khoản Admin làm tài khoản thứ hai (ticket AC#6). Chưa kiểm chứng | ⚪ | — | Ticket CRM-LOGIN-101 · AC#5 (PO chốt 21-10-2026) |
| REQ-LOGIN-56 | Email đang bị khoá vẫn bị khoá khi đăng nhập từ IP khác | Khoá theo email | Khoá tài khoản Project Manager từ mạng thứ nhất → trong vòng 15 phút, từ máy/mạng **khác** (VD mạng di động, IP công khai khác) đăng nhập đúng tài khoản đó → bị từ chối, hiện thông báo của REQ-LOGIN-51. Cần mạng thứ hai. Chưa kiểm chứng | ⚪ | — | Ticket CRM-LOGIN-101 · AC#5 (PO chốt 21-10-2026) |
| REQ-LOGIN-57 | Email không có trong hệ thống không bị khoá dù gửi sai nhiều lần | Khoá chỉ áp cho email có tài khoản | Gửi form Login **6** lần liên tiếp với cùng một email không tồn tại (`auto_login_<timestamp>@auto.test`) + mật khẩu bất kỳ → **cả 6 lần** ở lại `/admin/authentication` kèm alert `Invalid email or password`; **không** lần nào hiện thông báo khoá của REQ-LOGIN-51. Không làm khoá tài khoản nào — chạy được ở bất kỳ vị trí nào trong bộ. Chưa kiểm chứng | ⚪ | — | User chốt `AMB-LOGIN-22` (22-10-2026) — chưa kiểm chứng UI |
| REQ-LOGIN-58 | Bộ đếm lần sai không phân biệt hoa/thường của email | Đổi kiểu chữ không vượt được khoá | (1) Tài khoản Project Manager, bộ đếm sạch → (2) sai mật khẩu 3 lần với email viết **thường** → (3) sai thêm 2 lần với cùng email viết **HOA** toàn bộ → (4) lần thử thứ 6 (email viết thường) → hiện thông báo khoá của REQ-LOGIN-51 (5 lần sai cộng chung một bộ đếm). Chưa kiểm chứng | ⚪ | — | User chốt `AMB-LOGIN-27` (22-10-2026) — chưa kiểm chứng UI |
| REQ-LOGIN-59 | Thử đăng nhập trong lúc đang bị khoá không kéo dài thời gian khoá | Mốc mở khoá cố định | Tài khoản Project Manager bị khoá ở mốc T (lần sai thứ 5) → khoảng T + 5 phút gửi form với mật khẩu sai, khoảng T + 10 phút gửi form với mật khẩu đúng → cả hai lần nhận thông báo khoá của REQ-LOGIN-51 → ngay sau T + 15 phút đăng nhập đúng → vào Dashboard (mốc mở khoá **không** dời theo các lần thử). Phải chờ 15 phút. Chưa kiểm chứng | ⚪ | — | User chốt `AMB-LOGIN-24` theo Assumption tạm (22-10-2026) — chưa kiểm chứng UI |
| REQ-LOGIN-60 | Hết thời gian khoá thì bộ đếm lần sai về 0 | | Tài khoản Project Manager bị khoá → chờ hết 15 phút, không thử gì → sai mật khẩu 5 lần liên tiếp → **cả 5 lần** `Invalid email or password` → lần thử thứ 6 mới nhận thông báo khoá. (Nếu bộ đếm không về 0, thông báo khoá xuất hiện ngay từ lần sai đầu tiên sau mở khoá.) Phải chờ 15 phút. Chưa kiểm chứng | ⚪ | — | User chốt `AMB-LOGIN-25` theo Assumption tạm (22-10-2026) — chưa kiểm chứng UI |
| REQ-LOGIN-61 | Gửi form bỏ trống Password không tính vào bộ đếm lần sai | | (1) Tài khoản Project Manager, bộ đếm sạch → (2) sai mật khẩu 4 lần liên tiếp → (3) gửi form với email Project Manager và **bỏ trống** Password 2 lần → mỗi lần nhận `The Password field is required.` → (4) sai mật khẩu thêm 1 lần (lần sai thứ 5) → vẫn `Invalid email or password` → (5) lần thử kế tiếp → thông báo khoá. (Nếu bỏ trống được tính, bước 4 đã nhận thông báo khoá.) Chưa kiểm chứng | ⚪ | — | User chốt `AMB-LOGIN-26` theo Assumption tạm (22-10-2026) — chưa kiểm chứng UI |
| REQ-LOGIN-62 | Tài khoản đang bị khoá vẫn dùng được Forgot Password | Lối thoát của người dùng bị khoá | Tài khoản Project Manager đang bị khoá → mở Forgot Password, nhập email Project Manager, bấm `Confirm` → trang hiện thông báo trung tính của REQ-LOGIN-39 và hộp thư của tài khoản nhận email đặt lại mật khẩu (như REQ-LOGIN-42). Cần hộp thư của tài khoản — chưa có (`AMB-LOGIN-01` ⏭️). Chưa kiểm chứng | ⚪ | — | User chốt `AMB-LOGIN-28` theo Assumption tạm (22-10-2026) — chưa kiểm chứng UI |
| REQ-LOGIN-63 | Đặt lại mật khẩu không mở khoá tài khoản sớm | | Tài khoản Project Manager đang bị khoá → đặt mật khẩu mới qua link email (REQ-LOGIN-43) → trong 15 phút kể từ khi bị khoá, đăng nhập bằng mật khẩu **mới** → vẫn nhận thông báo khoá. ⚠️ Phép thử đổi mật khẩu tài khoản Project Manager — phải cập nhật `PASSWORD_PM` trong `.env` ngay sau đó. Cần hộp thư. Chưa kiểm chứng | ⚪ | — | User chốt `AMB-LOGIN-28` theo Assumption tạm (22-10-2026) — chưa kiểm chứng UI |
| REQ-LOGIN-64 | Khoá tài khoản không đẩy phiên đang mở ra ngoài | | Trình duyệt A: tài khoản Project Manager đã đăng nhập, đang ở Dashboard → trình duyệt B (cửa sổ ẩn danh khác): khoá tài khoản theo REQ-46 (thấy thông báo khoá ở lần thử thứ 6) → quay lại trình duyệt A, mở `/admin/clients` → vẫn vào được, **không** bị đưa về trang Login. Chưa kiểm chứng | ⚪ | — | User chốt `AMB-LOGIN-29` theo Assumption tạm (22-10-2026) — chưa kiểm chứng UI |
| REQ-LOGIN-65 | Cookie ghi nhớ vẫn tự đăng nhập khi tài khoản đang bị khoá | | Trình duyệt A: đăng nhập tài khoản Project Manager **có** tick `Remember me` → xoá cookie phiên, giữ cookie ghi nhớ (như phép thử REQ-LOGIN-27) → trình duyệt B: khoá tài khoản theo REQ-46 → trình duyệt A mở `/admin/` → vào thẳng Dashboard, không qua trang Login. Chưa kiểm chứng | ⚪ | — | User chốt `AMB-LOGIN-29` theo Assumption tạm (22-10-2026) — chưa kiểm chứng UI |
| REQ-LOGIN-66 | Email của contact không bị khoá dù gửi sai nhiều lần | Contact xử lý như email không tồn tại (REQ-44) | Gửi form Login **6** lần liên tiếp với email của một **contact** + mật khẩu sai → cả 6 lần `Invalid email or password`, **không** lần nào hiện thông báo khoá. Chưa có tài khoản contact (`AMB-LOGIN-20`). Chưa kiểm chứng | ⚪ | — | User chốt `AMB-LOGIN-31` theo Assumption tạm (22-10-2026) — chưa kiểm chứng UI |
| REQ-LOGIN-67 | Staff bị vô hiệu hoá không bị khoá dù gửi sai nhiều lần | Đã bị chặn bởi REQ-48 | Gửi form Login **6** lần liên tiếp với email của một **staff bị vô hiệu hoá** + mật khẩu sai → **không** lần nào hiện thông báo khoá của REQ-LOGIN-51 (nguyên văn thông báo nhận được chưa biết — ghi lại khi kiểm). Chưa có tài khoản (`AMB-LOGIN-19`). Chưa kiểm chứng | ⚪ | — | User chốt `AMB-LOGIN-31` theo Assumption tạm (22-10-2026) — chưa kiểm chứng UI |
| REQ-LOGIN-68 | Bộ đếm lần sai bỏ khoảng trắng đầu/cuối của email | Thêm khoảng trắng không vượt được khoá | (1) Tài khoản Project Manager, bộ đếm sạch → (2) sai mật khẩu 3 lần với email không có khoảng trắng → (3) sai thêm 2 lần với cùng email có thêm khoảng trắng ở đầu và cuối → (4) lần thử thứ 6 → thông báo khoá. ⚠️ Ô Email là `type="email"`: trình duyệt có thể tự cắt khoảng trắng trước khi gửi → khi đó phép thử qua giao diện không chứng minh được rule này, cần gửi request thủ công (đội Dev — QA chưa xác nhận quyền gọi API). Chưa kiểm chứng | ⚪ | — | User chốt `AMB-LOGIN-32` theo Assumption tạm (22-10-2026) — chưa kiểm chứng UI |

**Tổng: 68 REQ** — 🟢 35 · 🟡 2 (`REQ-LOGIN-39` · `46`) · ⚪ 31 (`REQ-LOGIN-13` · `25` · `28` · `29` · `35` · `42` · `43` · `44` · `45` · `47` · `48` · `49` · `50` · `51` → `68`).

> Thứ tự dòng theo Story, nên mã không liền mạch trong từng bảng (VD STORY-02 có 08 → 16 rồi 44 · 48; STORY-07 có 46 rồi 51 → 68). Mã REQ gắn với hành vi, **không** đánh lại số theo vị trí.

## 2. Đặc tả trường dữ liệu

### 2.1. Form Login — `POST /admin/authentication`

| Field (Label) | Loại UI | Required | Ràng buộc (đọc DOM 12-10-2026) | REQ liên quan | Ghi chú |
|---|---|---|---|---|---|
| Email Address | `input` · `type="email"` · `name="email"` · `id="email"` | Có — **chỉ ở server** (không có thuộc tính `required`) | Không `maxlength`/`minlength`/`pattern`/`placeholder`; có `autofocus`; trình duyệt chặn sai định dạng email | 02 · 08 · 10 · 11 · 12 · 14 · 46 · 51 → 68 | Không được điền lại sau lỗi. Là khoá của bộ đếm lần sai (khoá theo email — REQ-55 · 56, không phân biệt hoa/thường — REQ-58, bỏ khoảng trắng — REQ-68) |
| Password | `input` · `type="password"` · `name="password"` · `id="password"` | Có — **chỉ ở server** | Không `maxlength`/`minlength`; không nút hiện/ẩn | 03 · 09 · 10 · 15 · 16 · 46 · 52 · 54 · 61 | Bị xoá sau lỗi. Bỏ trống không tính vào bộ đếm lần sai (REQ-61) |
| Remember me | `input` · `type="checkbox"` · `name="remember"` · `id="remember"` | Không | Mặc định không tick; `value="estimate"` (giá trị lạ — `AMB-LOGIN-14`) | 04 · 26 · 27 · 28 | |
| (ẩn) CSRF token | `input type="hidden"` · `name="csrf_token_name"` | — | 32 ký tự hex, đổi theo phiên | 24 · 25 | Không render |
| Login | `button type="submit"` | — | Nhãn `Login` | 08 → 16 | Enter trong ô Password cũng gửi form |
| Forgot Password? | `a href="/admin/authentication/forgot_password"` | — | | 07 | |
| Logo | `a href="/"` | — | | 06 | Trỏ trang chủ phía khách hàng |

Thứ tự Tab: Email Address → Password → Remember me → Login (mọi phần tử `tabIndex = 0`, theo thứ tự DOM). Không có CAPTCHA, không có bộ chọn ngôn ngữ trên trang.

### 2.2. Form Forgot Password — `POST /admin/authentication/forgot_password`

| Field (Label) | Loại UI | Required | Ràng buộc | REQ liên quan | Ghi chú |
|---|---|---|---|---|---|
| Email Address | `input` · `type="email"` · `name="email"` · `id="email"` | Không có `required`; server báo `Email not found` khi trống | Không placeholder, không autofocus | 36 · 37 · 38 · 39 · 40 | Được giữ lại sau lỗi |
| (ẩn) CSRF token | `input type="hidden"` · `name="csrf_token_name"` | — | 32 ký tự hex | 41 | |
| Confirm | `button type="submit"` | — | Nhãn `Confirm` | 37 → 39 · 42 | |

### 2.3. Cookie liên quan đăng nhập

| Cookie | Hình thái (không chép giá trị) | HttpOnly | Secure | SameSite | Hạn | REQ |
|---|---|---|---|---|---|---|
| `sp_session` | 40 ký tự | ✅ | ✅ | Lax | ≈ 8 giờ, dời theo mỗi lần tải trang | 22 · 23 · 34 |
| `csrf_cookie_name` | 32 ký tự | ✅ | ✅ | Lax | ≈ 1 giờ | 24 |
| `autologin` | chuỗi không rỗng (quan sát 110 ký tự) | ❌ | ✅ | Lax | ≈ 62 ngày | 26 · 27 |

## 3. Business Rules & Validation Messages

| REQ ID | Rule / Trigger | Thông báo (nguyên văn) | Nơi hiển thị |
|---|---|---|---|
| REQ-LOGIN-08 | Login — Email trống | `The Email Address field is required.` | Alert `.alert.alert-danger` trên form |
| REQ-LOGIN-09 | Login — Password trống | `The Password field is required.` | Alert `.alert.alert-danger` trên form |
| REQ-LOGIN-10 | Login — cả hai trống | 2 alert: `The Password field is required.` · `The Email Address field is required.` | 2 alert riêng |
| REQ-LOGIN-12 | Login — email không tồn tại | `Invalid email or password` | Alert `.alert.alert-danger` |
| REQ-LOGIN-46 (lần 1 → 5) · 57 · 60 · 66 | Login — sai mật khẩu lần 1 → 5 với cùng email · email không tồn tại / email contact sai bao nhiêu lần | `Invalid email or password` | Alert `.alert.alert-danger` |
| REQ-LOGIN-46 (lần 6) · 51 · 52 · 56 · 58 · 59 · 63 | Login — tài khoản đang bị khoá (từ lần thử thứ 6) | `Your account is locked. Please try again in 15 minutes.` | Chưa quan sát — nguyên văn do PO chốt 21-10-2026 (ticket `CRM-LOGIN-101` · AC#2) |
| REQ-LOGIN-11 · 38 | Email sai định dạng (cả 2 form) | *(chuỗi của trình duyệt, Chrome)* `Please include an '@' in the email address. 'abc' is missing an '@'.` — **cấm assert** | Bong bóng validation của trình duyệt |
| REQ-LOGIN-37 | Forgot — email trống | `Email not found` | Alert `.alert.alert-danger` |
| REQ-LOGIN-39 | Forgot — email không tồn tại | **Mong đợi (PO chốt 19-10-2026):** `Nếu địa chỉ email này có tài khoản trong hệ thống, chúng tôi đã gửi liên kết đặt lại mật khẩu tới đó. Vui lòng kiểm tra hộp thư (và cả thư mục Spam).` · **Hiện trạng (lệch, chờ Dev sửa):** `Email not found` | Hiện trạng: alert `.alert.alert-danger`. Sau khi sửa: chưa quan sát |
| REQ-LOGIN-49 · 45 · 50 | Forgot — email tồn tại · email của contact · email của staff bị vô hiệu hoá | Giống hệt nguyên văn mong đợi của REQ-LOGIN-39 | Chưa quan sát |
| REQ-LOGIN-44 | Login — email + mật khẩu của contact | `Invalid email or password` (giống REQ-LOGIN-12) | Chưa quan sát — theo xác nhận của user |
| REQ-LOGIN-35 | Logout khi có timer chạy | `Started tasks timers found!` · `Are you sure you want to logout without stopping the timers?` · nút `Logout` | Popup (theo template DOM — chưa chạy thật) |

## 4. Luồng xử lý

**F1 — Đăng nhập thành công:** mở `/admin/authentication` → nhập Email + Password (tuỳ chọn tick Remember me) → `Login` hoặc `Enter` → `/admin/` Dashboard (REQ-17). Đi từ một trang nội bộ khi chưa đăng nhập vẫn về Dashboard (REQ-18).

**F2 — Đăng nhập lỗi:** submit → lỗi trống: trang trả lại ngay (200) kèm alert bắt buộc (REQ-08/09/10) · lỗi thông tin: `303` về trang Login kèm `Invalid email or password` (REQ-12) · cả hai trường hợp ô Email và Password đều trống lại (REQ-14/15). Sai mật khẩu 5 lần liên tiếp với cùng email → tài khoản bị khoá (F7) · contact và staff bị vô hiệu hoá bị từ chối (REQ-44 · 48, ⚪).

**F3 — Chặn truy cập:** chưa đăng nhập mở trang nội bộ → về Login (REQ-19) · đã đăng nhập mở trang Login/Forgot → về Dashboard (REQ-20/21). Không hoạt động 8 giờ → phiên hết hạn (REQ-47, ⚪).

**F4 — Remember me:** đăng nhập có tick → cookie `autologin` (REQ-26) → mất cookie phiên vẫn vào thẳng (REQ-27).

**F5 — Đăng xuất:** avatar → `Logout` (REQ-30) → [có timer → popup xác nhận (REQ-35, ⚪)] → Login (REQ-31) → phiên huỷ (REQ-32/33), cookie phiên mới (REQ-34).

**F6 — Quên mật khẩu:** Login → `Forgot Password?` (REQ-07) → nhập email → `Confirm` → trống: `Email not found` (REQ-37) · email không tồn tại, email tồn tại, email của contact, email của staff bị vô hiệu hoá: **cùng một** thông báo trung tính (REQ-39 🟡 · 49 · 45 · 50 — hiện trạng email không tồn tại vẫn báo `Email not found`, chờ Dev sửa) · email tồn tại: gửi email reset (REQ-42, ⚪) → link reset (REQ-43, ⚪). Trang không có link quay lại Login (đã chốt, `AMB-LOGIN-07` ✅).

**F7 — Khoá tài khoản (ticket `CRM-LOGIN-101`, chưa kiểm chứng):** sai mật khẩu lần 1 → 5 với cùng email (không phân biệt hoa/thường — REQ-58): `Invalid email or password` → sau lần thứ 5 tài khoản bị khoá 15 phút (REQ-46 🟡) → từ lần thử thứ 6, mọi lần đăng nhập trong 15 phút, kể cả đúng mật khẩu và từ IP khác, bị từ chối với `Your account is locked. Please try again in 15 minutes.` (REQ-51 · 52 · 56) → email khác không bị ảnh hưởng (REQ-55) → hết 15 phút đăng nhập đúng được lại (REQ-53). Đăng nhập thành công trước khi đủ 5 lần sai → bộ đếm về 0 (REQ-54). Email không có trong hệ thống, email contact, staff vô hiệu hoá không bao giờ bị khoá (REQ-57 · 66 · 67). Bỏ trống Password không tính vào bộ đếm (REQ-61); bộ đếm không phân biệt hoa/thường, bỏ khoảng trắng (REQ-58 · 68). Thử lại trong lúc khoá không dời mốc mở khoá (REQ-59); hết khoá bộ đếm về 0 (REQ-60). Đang khoá vẫn dùng được Forgot Password, đặt lại mật khẩu không mở khoá sớm (REQ-62 · 63). Phiên đang mở và cookie ghi nhớ không bị khoá ảnh hưởng (REQ-64 · 65).

## 5. Yêu cầu phi chức năng quan sát được

| Hạng mục | Quan sát 12-10-2026 | Liên quan |
|---|---|---|
| HTTPS | Cookie đều có cờ `Secure` | REQ-22 · 24 · 26 |
| Security header của trang Login | Response `GET /admin/authentication` **không** có `X-Frame-Options` · `Content-Security-Policy` · `Strict-Transport-Security` · `X-Content-Type-Options` | `RISK-LOGIN-05` |
| CAPTCHA / chống dò mật khẩu | Không có CAPTCHA trên cả 2 form. Giới hạn số lần sai: lúc khảo sát chưa rõ — ticket `CRM-LOGIN-101` (21-10-2026) chốt khoá 15 phút sau 5 lần sai liên tiếp, **chưa triển khai** lên demo (22-10-2026) | `AMB-LOGIN-02` · `RISK-LOGIN-04` · REQ-46 · 51 → 68 |
| Ngôn ngữ | `<html lang="en">`; không có bộ chọn ngôn ngữ trên trang Login | `RISK-LOGIN-06` |
| Trình duyệt | Chỉ khảo sát Google Chrome, viewport 1600×780. Viewport khác / mobile: chưa khảo sát | — |

## 6. Danh mục Evidence

> Ảnh chụp 12-10-2026 bằng Playwright MCP, đã **mở lại xác nhận** từng ảnh. Email trong ảnh (nếu có) đều là email giả `@auto.test` do agent sinh.

| Tệp | Màn hình | Trạng thái | REQ |
|---|---|---|---|
| [../../_discovery/evidence/login_overview_viewport.png](../../_discovery/evidence/login_overview_viewport.png) | Login | Mặc định, form trống (ảnh của tầng khám phá) | 01 · 04 |
| [evidence/login_empty_submit_viewport.png](evidence/login_empty_submit_viewport.png) | Login | Sau submit trống cả 2 ô — 2 alert đỏ | 08 · 09 · 10 |
| [evidence/login_password_empty_viewport.png](evidence/login_password_empty_viewport.png) | Login | Sau submit thiếu Password — 1 alert, ô Email đã trống lại | 09 · 14 |
| [evidence/login_email_empty_viewport.png](evidence/login_email_empty_viewport.png) | Login | Sau submit thiếu Email — 1 alert | 08 |
| [evidence/login_email_invalid_format_viewport.png](evidence/login_email_invalid_format_viewport.png) | Login | Email `abc` sau khi bấm Login — **không** có alert ứng dụng, form chưa gửi. Bong bóng của trình duyệt **không** xuất hiện trong ảnh chụp; bằng chứng chính là số liệu DOM ở AC | 11 |
| [evidence/login_wrong_credentials_viewport.png](evidence/login_wrong_credentials_viewport.png) | Login | Sau submit email không tồn tại — alert `Invalid email or password`, 2 ô trống | 12 · 14 · 15 |
| [evidence/guard_redirect_to_login_viewport.png](evidence/guard_redirect_to_login_viewport.png) | Login | Trang đích sau khi mở `/admin/profile` khi chưa đăng nhập — không có alert | 19 |
| [evidence/logout_menu_open_clip.png](evidence/logout_menu_open_clip.png) | Header Dashboard | Dropdown avatar **đang mở** (đã chờ hết hiệu ứng fade, `opacity = 1`) — 5 mục, có `Logout`. Cắt góc phải trên, không kéo dữ liệu Dashboard | 30 |
| [evidence/logout_landing_heading_clip.png](evidence/logout_landing_heading_clip.png) | Login | Trang đích sau Logout — **chỉ giữ phần logo + heading `Login`**: bản viewport gốc có ô Email bị trình duyệt tự điền email tài khoản thật nên đã cắt bỏ và xoá ảnh gốc | 31 |
| [evidence/remember_me_autologin_clip.png](evidence/remember_me_autologin_clip.png) | Header Dashboard | Sau bước (3) của phép thử REQ-27 — header của trang đã đăng nhập (avatar), không qua Login. Chỉ cắt dải header | 27 |
| [../../_discovery/evidence/login_forgot_password_viewport.png](../../_discovery/evidence/login_forgot_password_viewport.png) | Forgot Password | Mặc định, form trống (ảnh của tầng khám phá) | 07 · 36 |
| [evidence/forgot_empty_submit_viewport.png](evidence/forgot_empty_submit_viewport.png) | Forgot Password | Sau submit trống — alert `Email not found` | 37 |
| [evidence/forgot_unknown_email_viewport.png](evidence/forgot_unknown_email_viewport.png) | Forgot Password | Sau submit email `@auto.test` không tồn tại — alert `Email not found`, email vẫn còn trong ô | 39 · 40 |

REQ 🟢 không có ảnh riêng — căn cứ là **số liệu DOM / cookie / network ghi trong AC**: 02 · 03 · 05 · 06 · 16 · 17 · 18 · 20 · 21 · 22 · 23 · 24 · 26 · 32 · 33 · 34 · 38 · 41.
