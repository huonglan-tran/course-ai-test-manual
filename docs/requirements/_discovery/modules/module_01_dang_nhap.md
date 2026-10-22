# Module 01 — Login (`LOGIN`)

> ← [Bản đồ hệ thống](../system_map.md) · Trạng thái recon: [`README.md`](../../README.md) · Tầng khám phá — **không** chứa mã REQ.
>
> ✅ **Đã có tài liệu requirements** (12-10-2026): [`login/REQUIREMENTS_LOGIN_SUMMARY.md`](../../login/REQUIREMENTS_LOGIN_SUMMARY.md) — các "vùng chưa xác minh" bên dưới đã được recon; phần còn treo nằm ở AMB của tài liệu module.

| Thuộc tính | Giá trị |
|---|---|
| Tên UI | Login · Forgot Password |
| Route | Đăng nhập `/admin/authentication` · quên mật khẩu `/admin/authentication/forgot_password` · đăng xuất `/admin/authentication/logout` |
| Loại màn hình | 2 form đơn (Login · Forgot Password) |
| CRUD | Không |
| Status flow | Không |
| Luồng liên quan | Đăng nhập thành công → `/admin/` (Dashboard) · link `Forgot Password?` trên trang Login → trang Forgot Password · đăng xuất từ menu người dùng |
| Risk | 🔴 Cao — cửa vào của mọi module; liên quan bảo mật (mật khẩu, khoá tài khoản, quên mật khẩu, ghi nhớ phiên) |
| Ước REQ | 15–20 |

## Trang Login

| Thuộc tính | Giá trị |
|---|---|
| Route | `/admin/authentication` |
| Tiêu đề tab | `Perfex CRM \| Anh Tester Demo - Login` |
| Thành phần | Ô `Email Address` · ô `Password` · checkbox `Remember me` · nút `Login` · link `Forgot Password?` (`href` = `/admin/authentication/forgot_password`) |

## Trang Forgot Password

> Bổ sung 12-10-2026 (mode ADD, user chỉ ra lượt trước chưa mở trang này). Mở bằng **context trình duyệt riêng, chưa đăng nhập** — bấm link `Forgot Password?` trên trang Login. **Không** submit form (submit sẽ gửi email đặt lại mật khẩu thật).

| Thuộc tính | Giá trị (đọc DOM 12-10-2026) |
|---|---|
| Route | `/admin/authentication/forgot_password` |
| Tiêu đề trang (heading) | `Forgot Password` |
| Tiêu đề tab | `Perfex CRM \| Anh Tester Demo - Login` — **trùng** tiêu đề tab trang Login, không nêu "Forgot Password" |
| Form | 1 form · `method=post` · `action=/admin/authentication/forgot_password` · không có `novalidate` |
| Field | 1 field: `Email Address` — `<input type="email" name="email" id="email">`, **không** có thuộc tính `required`, không placeholder, không autofocus |
| Nút | `Confirm` |
| Link khác trên trang | **Không có** — không có link quay lại trang Login |
| CAPTCHA | Không thấy (không có phần tử reCAPTCHA) |

## Vùng chưa xác minh

| Vùng | Lý do | Xử lý ở recon module |
|---|---|---|
| Validation & message lỗi đăng nhập | Khám phá không submit form | Trigger từng validation |
| Forgot Password — validation email rỗng / sai định dạng / email không tồn tại · message thành công | Không submit khi khám phá. Field không có `required` → bỏ trống có thể chỉ bị chặn ở server; `type=email` → trình duyệt tự chặn sai định dạng (message do trình duyệt sinh, không phải của ứng dụng) | Trigger từng trường hợp, đọc message nguyên văn + tầng network |
| Forgot Password — email đặt lại mật khẩu, link reset, thời hạn link | Kiểm tầng tích hợp ❔ — cần hộp thư nhận được email | Chỉ thử với email của tài khoản test riêng — **không** dùng email của tài khoản dùng chung |
| Không có đường quay lại trang Login từ Forgot Password | Quan sát được, chưa rõ cố ý hay thiếu | Hỏi PO khi recon module (ứng viên `AMB-LOGIN-…`) |
| Khoá tài khoản / CAPTCHA sau nhiều lần sai | Chưa thử — môi trường dùng chung, thử sai có thể khoá tài khoản đang dùng chung | Hỏi user trước khi thử; dùng tài khoản riêng nếu có |
| Hành vi `Remember me` | Chưa thử | Kiểm cookie phiên sau khi đóng trình duyệt |
| Đăng nhập cổng Customer | Ngoài phạm vi (URL khác) | Mode ADD — cùng prefix `LOGIN` nếu cùng nghiệp vụ |

## Evidence

> Ảnh chụp 12-10-2026 (Playwright MCP, viewport 1600×780), đã mở lại xác nhận.

| Ảnh | Màn hình | Trạng thái chụp | Phạm vi |
|---|---|---|---|
| [login_overview_viewport.png](../evidence/login_overview_viewport.png) | `/admin/authentication` | Chưa đăng nhập, form trống — thấy đủ Email Address · Password · Remember me · Login · Forgot Password? | Viewport — form nằm trọn |
| [login_forgot_password_viewport.png](../evidence/login_forgot_password_viewport.png) | `/admin/authentication/forgot_password` | Chưa đăng nhập, form trống — heading `Forgot Password` · ô Email Address · nút Confirm | Viewport — form nằm trọn |
