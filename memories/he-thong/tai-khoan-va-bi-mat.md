---
name: tai-khoan-va-bi-mat
description: Tài khoản test, khoá trong .env, ai gõ mật khẩu, quy tắc không ghi bí mật vào tài liệu, bẫy autofill/cookie
type: project
updated: 12-11-2026
---

# Tài khoản test & bí mật

## `.env` (đã `.gitignore`) — chỉ ghi TÊN khoá, KHÔNG ghi giá trị

| Khoá | Dùng cho |
|---|---|
| `BASE_URL` | Địa chỉ hệ thống |
| `EMAIL_ADMIN` · `PASSWORD_ADMIN` | Tài khoản "Admin" (hiển thị `Admin Example`) — dùng chung cho gần như mọi TC |
| `EMAIL_PM` · `PASSWORD_PM` | Tài khoản Project Manager — user cấp 22-10-2026, lưu vào `.env` 23-10-2026, **chỉ** dùng cho TC khoá tài khoản (`AMB-LOGIN-30`/`33`) |

- `CLAUDE.md` cho phép Claude đọc `.env` để lấy tài khoản; **chỉ đọc để dùng**, không ghi lại, không hiển thị ra log/report.
- 🔒 Không ghi mật khẩu/token/cookie/session id vào `docs/` hay `memories/` (lịch sử git không xoá được). Tài liệu chỉ mô tả *hình thái*. Báo cáo ghi "Admin (🔒 không ghi giá trị)".

## Ai gõ mật khẩu? — thực tế đã diễn ra ở repo này

- Các lần chạy `/execute-test-cases` (21-10, 23-10, 12-11-2026): **agent không tự gõ mật khẩu đăng nhập**. Khi tới bước cần đăng nhập, agent dừng, báo tester đăng nhập thủ công trên cửa sổ Playwright; tester trả lời "Đã đăng nhập…" thì agent chạy tiếp từ phiên đó.
- Tester cũng tự đóng hộp thoại "lưu mật khẩu" của Chrome rồi báo lại.
- Quy tắc này khớp skill `skills-manual-test-executor` / `execute-test-cases`. Đừng tự gõ mật khẩu trừ khi workflow và user cho phép rõ.
- Hai TC không chạy được qua agent: `CRM_CUS_TC_142` (xem mật khẩu Vault cần gõ mật khẩu đăng nhập — user tự chạy tay, `AMB-CUS-22`) và các TC `@PersonalOnly` của LOGIN (chờ 8 giờ / 15 phút khoá thật).

## Dữ liệu cho phép thử lỗi

- Phép thử sai/Forgot Password chỉ dùng email **`@auto.test` không tồn tại**. **Cấm** thử sai mật khẩu lặp lại trên tài khoản thật (môi trường dùng chung — `RISK-LOGIN-01`).
- TC khoá tài khoản (REQ-LOGIN-51 → 68) dùng tài khoản PM: mỗi TC khoá tài khoản này **15 phút** → không chạy liền nhau / song song (`RISK-LOGIN-09`). Tính năng khoá **chưa được Dev đưa lên demo** (user xác nhận 22-10-2026) → các TC này `@Skip`/BLOCKED.

## Bẫy trình duyệt

- Profile Playwright mặc định có **mật khẩu đã lưu** → ô Email/Password bị `:-webkit-autofill`, làm TC "ô trống" đỏ oan (`RISK-LOGIN-02`). Luôn chạy trong **context cách ly mới** (không cookie, không mật khẩu lưu); đầu buổi xoá cookie còn sót.
- Tick *Remember me* tạo cookie `autologin` sống ≈ 62 ngày trong profile → xoá cookie/profile khi không cần (`RISK-LOGIN-07`).
- Ô `Username` của Vault bị Chrome điền email đăng nhập → TC luôn xoá trắng ô trước khi nhập (`AMB-CUS-24`).
- Hộp thoại `confirm()` gốc (xoá KH, note, tệp, vault, Bulk Confirm) → đăng ký xử lý dialog **trước** khi bấm (`RISK-CUS-06`).
