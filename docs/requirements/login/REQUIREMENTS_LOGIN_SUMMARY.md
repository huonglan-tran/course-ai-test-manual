# Requirements — Login (`LOGIN`)

> **INDEX module — tên file bất biến.** Phần dùng chung mọi nền tảng + Bản đồ tài liệu. REQ chi tiết của từng nền tảng nằm ở file nền tảng (xem [Bản đồ tài liệu](#bản-đồ-tài-liệu)).
>
> Danh mục hệ thống: [`../README.md`](../README.md) · Bản đồ khám phá: [`../_discovery/modules/module_01_dang_nhap.md`](../_discovery/modules/module_01_dang_nhap.md)

## 1. Metadata

| Thuộc tính | Giá trị |
|---|---|
| **Hệ thống** | Perfex CRM — khu Admin |
| **Module** | Login · Forgot Password (gồm luồng đăng xuất và ghi nhớ đăng nhập) |
| **Prefix** | `LOGIN` — cấp bởi `/discover-system` 12-10-2026 |
| **Tiền tố TC ID** | `CRM_LOGIN_TC_<3 số>` |
| **Nền tảng** | Web ✅ (khảo sát 12-10-2026) · Mobile — chưa có · API — chưa có |
| **Phiên bản tài liệu** | 1.5 — 22-10-2026: chốt 7 AMB 🟡 còn lại của ticket `CRM-LOGIN-101` theo Assumption tạm · TC khoá dùng tài khoản Project Manager (1.4: chốt `AMB-LOGIN-22` · `23` · `27` · `30` · 1.3: ticket `CRM-LOGIN-101` · 1.2: chốt toàn bộ 21 AMB · 1.1: chốt AMB đợt 1 · 1.0: khởi tạo 12-10-2026) |
| **Dải mã đã dùng** | `REQ-LOGIN-01` → `REQ-LOGIN-68` (đợt 1 UI recon 12-10-2026: 01 → 43 · đợt 2 chốt AMB 19-10-2026: 44 → 50 · đợt 3 ticket `CRM-LOGIN-101` 22-10-2026: 51 → 56 · đợt 4 chốt AMB 🔴 của ticket 22-10-2026: 57 → 58 · đợt 5 chốt AMB 🟡 của ticket 22-10-2026: 59 → 68) · `AMB-LOGIN-01` → `AMB-LOGIN-33` · `RISK-LOGIN-01` → `RISK-LOGIN-09` · `STORY-LOGIN-01` → `STORY-LOGIN-07` |
| **Mã kế tiếp** | Đợt phân tích sau bắt đầu từ `REQ-LOGIN-69` · `AMB-LOGIN-34` · `RISK-LOGIN-10` · `STORY-LOGIN-08` — **KHÔNG đánh lại từ 01** |
| **Trạng thái REQ** | 🟢 35 · 🟡 2 · 🔴 0 · ⚪ 31 (chưa kiểm chứng được — trong đó REQ-51 → 68 là **chưa triển khai**) — tổng 68. REQ-46 🟡 cũng chưa triển khai: môi trường demo vẫn chạy hành vi cũ |

## 2. Tổng quan

Module `LOGIN` là cửa vào khu quản trị (Admin) của Perfex CRM dành cho **staff**. Người dùng đăng nhập bằng email + mật khẩu, tuỳ chọn ghi nhớ đăng nhập, khôi phục mật khẩu qua email và đăng xuất từ menu avatar trên header. Mọi trang `/admin/*` đều yêu cầu đã đăng nhập.

| Trong phạm vi | Ngoài phạm vi |
|---|---|
| Trang Login `/admin/authentication` · validation · xử lý sai thông tin | Đăng nhập cổng Customer/Contact (URL khác — sẽ bổ sung bằng `/discover-system` mode ADD, cùng prefix `LOGIN` nếu cùng nghiệp vụ) |
| Remember me (cookie ghi nhớ) | Đổi mật khẩu trong Edit Profile → module `PROF` |
| Chặn truy cập khi chưa đăng nhập · chuyển hướng khi đã đăng nhập | Two-Factor Authentication, cấu hình reCAPTCHA (nằm ở Settings — bị chặn, `AMB-SYS-01`) |
| Đăng xuất · huỷ phiên | Quản lý tài khoản staff, role (Setup — bị chặn) |
| Forgot Password `/admin/authentication/forgot_password` | |
| Khoá tài khoản khi đăng nhập sai nhiều lần (ticket `CRM-LOGIN-101`) | |

### 2.1. Bản đồ phủ tài liệu

Không có tài liệu — toàn bộ REQ đợt 1 sinh từ khảo sát UI thực tế. `docs/SRS_Login.md` (có trong git, đã xoá khỏi thư mục làm việc) **không** được dùng theo quyết định của user ngày 12-10-2026. REQ đợt 2 (44 → 50) và hành vi mong đợi của REQ-39 đến từ **câu trả lời AMB của user ngày 19-10-2026** — cột `Nguồn` ghi rõ, chưa kiểm chứng trên UI.

| Vùng chức năng | Tài liệu phủ | Mức phủ | REQ liên quan |
|---|---|---|---|
| Khoá tài khoản khi đăng nhập sai nhiều lần | Ticket `CRM-LOGIN-101` · AC#1 → AC#6 (PO chốt 21-10-2026) | 🟨 Một phần — ticket nêu luồng chính; user chốt toàn bộ 11 AMB của ticket ngày 22-10-2026 (`AMB-LOGIN-22` → `33`; 7 mã 🟡 chốt theo Assumption tạm). Không còn AMB treo | REQ-LOGIN-46 🟡 · 51 → 68 — **chưa kiểm chứng trên UI**; Dev **chưa triển khai** lên môi trường demo (user xác nhận 22-10-2026) |
| Các vùng còn lại | — | ⬜ Trắng — khảo sát UI | REQ-LOGIN-01 → 45 · 47 → 50 |

## Bản đồ tài liệu

| Nền tảng | File | Story | REQ bao phủ |
|---|---|---|---|
| Chung ≥ 2 nền tảng | chính file này | — | *(chưa có — module mới có 1 nền tảng)* |
| Web | [web/requirements_login_web.md](web/requirements_login_web.md) | STORY-LOGIN-01 → 07 | `REQ-LOGIN-01` → `REQ-LOGIN-68` (68 REQ) |

## 3. Yêu cầu dùng chung nhiều nền tảng

*Chưa có.* Module hiện chỉ khảo sát Web — theo skill mục 2.2, REQ chưa kiểm trên nền tảng thứ hai thì nằm ở file `web/`. Khi bổ sung mobile/API mà rule trùng, **chuyển dòng REQ** từ `web/` lên đây và sửa cột `Nền tảng` (giữ nguyên mã).

## 4. Ma trận phân quyền

> Màn hình Login là trước đăng nhập — "phân quyền" ở đây là **loại tài khoản nào dùng được luồng xác thực khu Admin**. Hành vi của người **chưa đăng nhập** (bị chặn khỏi `/admin/*`) đã kiểm chứng ở `REQ-LOGIN-19`.
>
> Ký hiệu `⚠️❌` trong bảng này = **theo xác nhận của user (19-10-2026), chưa đăng nhập thử** — mức bằng chứng thấp hơn `❌`, cao hơn `❔`.

| Hành động | Staff role Admin — tài khoản `.env` (Admin · Project Manager) | Staff — role khác | Staff bị vô hiệu hoá | Contact (khách hàng) |
|---|---|---|---|---|
| Đăng nhập khu Admin bằng thông tin đúng | ✅ | — | ⚠️❌ (REQ-48) | ⚠️❌ (REQ-44) |
| Ghi nhớ đăng nhập (Remember me) | ✅ | — | — | — |
| Đăng xuất | ✅ | — | — | — |
| Yêu cầu đặt lại mật khẩu (Forgot Password) | ❔ | — | ⚠️❌ không nhận email reset (REQ-50) | ⚠️❌ không nhận email reset (REQ-45) |

```
Tổng 16 ô = Đã kiểm chứng 3 · Suy diễn (theo xác nhận user) 4 · Chưa rõ 1 · Không áp dụng 8
8 ô "không áp dụng": cột [Staff — role khác] (4 ô) — hệ thống chỉ có role Admin (AMB-LOGIN-18 ✅) ·
  [Remember me / Đăng xuất] × [Staff vô hiệu hoá / Contact] (4 ô) — không đăng nhập được thì không có phiên để ghi nhớ hay đăng xuất.
1 ô "chưa rõ": [Forgot Password × tài khoản .env] — cố ý không thử, gửi email reset tới tài khoản dùng chung là cấm (AMB-LOGIN-01 ⏭️, chấp nhận rủi ro).
```

## 5. Ma trận trạng thái phiên

> Module không có entity nghiệp vụ mang status flow; bảng dưới là **trạng thái phiên đăng nhập** của trình duyệt.

| Trạng thái hiện tại | Hành động | Trạng thái kế tiếp | Ai thực hiện | REQ |
|---|---|---|---|---|
| Chưa đăng nhập | Đăng nhập đúng, không tick Remember me | Đã đăng nhập (cookie `sp_session`) | Staff role Admin | 17 · 28 ⚪ |
| Chưa đăng nhập | Đăng nhập đúng, có tick Remember me | Đã đăng nhập + ghi nhớ (thêm `autologin`) | Staff role Admin | 26 |
| Chưa đăng nhập | Đăng nhập lỗi, chưa đủ 5 lần sai mật khẩu liên tiếp với cùng email | Chưa đăng nhập (ở lại Login kèm alert) | Bất kỳ | 08 → 13 |
| Chưa đăng nhập | Email **không tồn tại** trong hệ thống, gửi sai bao nhiêu lần cũng được | Chưa đăng nhập — **không** khoá, luôn `Invalid email or password` | Bất kỳ | 57 ⚪ |
| Chưa đăng nhập (đã sai 4 lần liên tiếp) | Sai mật khẩu lần thứ 5 với cùng email (không phân biệt hoa/thường) | **Tài khoản bị khoá 15 phút** — lần này vẫn hiện `Invalid email or password`, thông báo khoá hiện từ lần thử thứ 6 | Bất kỳ | 46 🟡 · 58 ⚪ |
| Chưa đăng nhập (đã sai < 5 lần) | Đăng nhập đúng | Đã đăng nhập, bộ đếm lần sai về 0 | Staff role Admin | 54 ⚪ |
| Tài khoản bị khoá | Đăng nhập (kể cả đúng mật khẩu) trong vòng 15 phút, từ bất kỳ IP nào | Tài khoản bị khoá — thông báo khoá | Bất kỳ | 51 · 52 · 56 ⚪ |
| Tài khoản bị khoá | Email **khác** đăng nhập từ cùng máy/IP | Không bị ảnh hưởng | Bất kỳ | 55 ⚪ |
| Tài khoản bị khoá | Thử đăng nhập (đúng hoặc sai) trong 15 phút | Vẫn bị khoá — mốc mở khoá **không** dời | Bất kỳ | 59 ⚪ |
| Tài khoản bị khoá | Hết 15 phút (tính từ lần sai thứ 5) | Chưa đăng nhập, đăng nhập đúng được lại, bộ đếm lần sai về 0 | Hệ thống | 53 · 60 ⚪ |
| Tài khoản bị khoá | Dùng Forgot Password | Vẫn nhận email đặt lại; đặt lại mật khẩu **không** mở khoá sớm | Chủ tài khoản | 62 · 63 ⚪ |
| Đã đăng nhập (ở trình duyệt khác) | Tài khoản bị khoá từ nơi khác | Vẫn đăng nhập — phiên không bị đẩy ra | Hệ thống | 64 ⚪ |
| Đã đăng nhập + ghi nhớ | Mất cookie phiên trong lúc tài khoản bị khoá | Tự đăng nhập lại bằng cookie ghi nhớ | Hệ thống | 65 ⚪ |
| Chưa đăng nhập | Gửi form bỏ trống Password | Không cộng vào bộ đếm lần sai | Bất kỳ | 61 ⚪ |
| Chưa đăng nhập | Đăng nhập bằng tài khoản contact / staff vô hiệu hoá | Chưa đăng nhập (bị từ chối) | Contact · staff vô hiệu hoá | 44 ⚪ · 48 ⚪ |
| Chưa đăng nhập | Mở trang `/admin/*` | Chưa đăng nhập (bị đưa về Login) | Bất kỳ | 19 |
| Đã đăng nhập | Mở trang Login / Forgot Password | Đã đăng nhập (bị đưa về Dashboard) | Staff role Admin | 20 · 21 |
| Đã đăng nhập | Logout, không có timer chạy | Chưa đăng nhập (phiên mới, trang nội bộ bị chặn) | Staff role Admin | 31 · 32 · 33 · 34 |
| Đã đăng nhập | Logout khi có timer chạy | Đã đăng nhập + popup xác nhận → bấm `Logout` trong popup → Chưa đăng nhập | Staff role Admin | 35 ⚪ |
| Đã đăng nhập + ghi nhớ | Mất cookie phiên (đóng phiên trình duyệt / hết hạn) | Đã đăng nhập (tự đăng nhập lại) | Hệ thống | 27 |
| Đã đăng nhập + ghi nhớ | Logout | Chưa đăng nhập, `autologin` bị vô hiệu | Staff role Admin | 29 ⚪ |
| Đã đăng nhập (không ghi nhớ) | Không hoạt động 8 giờ | Chưa đăng nhập (phiên hết hạn) | Hệ thống | 23 · 47 ⚪ |

## 6. Điểm mơ hồ & Rủi ro

### 6.1. Ambiguities

> **19-10-2026:** user trả lời trực tiếp `AMB-LOGIN-02` · `03` · `18` · `20`; các AMB còn lại user chốt **lấy Assumption tạm làm câu trả lời**. Cột *Kết luận* ghi rõ nguồn từng dòng.
>
> **22-10-2026:** ticket `CRM-LOGIN-101` thay kết luận của `AMB-LOGIN-02` và mở `AMB-LOGIN-22` → `30`. Cùng ngày user trả lời `AMB-LOGIN-22` · `23` · `27` · `30` (cả 4 🔴) → mở thêm `AMB-LOGIN-31` · `32`; sau đó user chốt cả 7 AMB 🟡 (24 · 25 · 26 · 28 · 29 · 31 · 32) **theo Assumption tạm** và đổi tài khoản phép thử sang Project Manager → mở `AMB-LOGIN-33`, user trả lời ngay cùng ngày (cùng role Admin). **Module không còn AMB treo.**

| Mã | Câu hỏi | Nguy cơ | Mức độ | Assumption tạm | Trạng thái | Kết luận |
|---|---|---|---|---|---|---|
| AMB-LOGIN-01 | Xin **tài khoản staff test riêng có hộp thư nhận được email** để kiểm: sai mật khẩu với email tồn tại (REQ-13), gửi email reset (REQ-42), link reset (REQ-43). Thử trên tài khoản `.env` dùng chung là cấm | 3 REQ không có TC chạy được; luồng khôi phục mật khẩu chưa ai kiểm | 🔴 | REQ-13/42/43 viết TC trước, đánh `skip` | ⏭️ Bỏ qua 19-10-2026 | Chốt theo Assumption tạm: **không** cấp tài khoản test ở giai đoạn này. REQ-13 · 42 · 43 · 46 · 49 giữ ⚪, TC viết trước và đánh `skip`. Chấp nhận rủi ro luồng khôi phục mật khẩu chưa được kiểm |
| AMB-LOGIN-02 | Có giới hạn số lần đăng nhập sai / khoá tài khoản / CAPTCHA sau N lần sai không? UI không có CAPTCHA, không nói gì về khoá | Brute-force mật khẩu; hoặc ngược lại TC thử sai làm khoá tài khoản dùng chung | 🔴 | Không có giới hạn (theo UI hiện tại). Không thử sai lặp lại trên tài khoản thật | ✅ Đã trả lời 19-10-2026 | User: **không có giới hạn số lần đăng nhập sai** → sinh `REQ-LOGIN-46` (⚪). Rủi ro brute-force giữ ở `RISK-LOGIN-04`. ⚠️ **Bị thay thế 22-10-2026** bởi ticket `CRM-LOGIN-101` (PO chốt 21-10-2026): sai mật khẩu 5 lần liên tiếp với cùng email → khoá 15 phút → `REQ-LOGIN-46` 🟡 |
| AMB-LOGIN-03 | Forgot Password báo `Email not found` cho email không tồn tại (REQ-39) → người ngoài dò được email nào có tài khoản, trong khi trang Login dùng thông báo chung `Invalid email or password`. Cố ý hay lỗi? | Lộ danh sách email staff (account enumeration) | 🔴 | Hành vi hiện tại là thực tế; TC kiểm theo thông báo hiện có và báo như rủi ro bảo mật | ✅ Đã trả lời 19-10-2026 | User: là **lỗi, Dev sẽ sửa**. Sau khi sửa phải hiện nguyên văn `Nếu địa chỉ email này có tài khoản trong hệ thống, chúng tôi đã gửi liên kết đặt lại mật khẩu tới đó. Vui lòng kiểm tra hộp thư (và cả thư mục Spam).` → `REQ-LOGIN-39` 🟡 (đổi hành vi mong đợi) + sinh `REQ-LOGIN-49` (email tồn tại nhận cùng thông báo). **Khác Assumption tạm** → TC REQ-39 dự kiến FAIL tới khi sửa |
| AMB-LOGIN-04 | Bỏ trống email ở Forgot Password nhận `Email not found` thay vì thông báo bắt buộc như trang Login (`The Email Address field is required.`) — cố ý? | Thông báo gây hiểu nhầm; không nhất quán giữa 2 form | 🟡 | Theo hành vi hiện tại (REQ-37) | ✅ Đã trả lời 19-10-2026 | Chốt theo Assumption tạm: giữ hành vi hiện tại — REQ-37 không đổi |
| AMB-LOGIN-05 | Forgot Password giữ lại email sau lỗi (REQ-40) còn Login thì xoá (REQ-14) — chủ đích? | Không nhất quán trải nghiệm | 🟡 | Theo hành vi hiện tại của từng trang | ✅ Đã trả lời 19-10-2026 | Chốt theo Assumption tạm: giữ nguyên hành vi từng trang — REQ-14 · 40 không đổi |
| AMB-LOGIN-06 | Sau đăng nhập luôn về Dashboard, không quay lại trang người dùng định mở trước đó (REQ-18) — cố ý hay thiếu tính năng? | Link chia sẻ tới bản ghi cụ thể phải mở lại thủ công sau đăng nhập | 🟡 | Luôn về Dashboard | ✅ Đã trả lời 19-10-2026 | Chốt theo Assumption tạm: luôn về Dashboard là đúng thiết kế — REQ-18 không đổi |
| AMB-LOGIN-07 | Trang Forgot Password không có link quay lại trang Login — thiếu? | Người dùng nhớ lại mật khẩu phải tự sửa URL / bấm Back | 🟡 | Không có — TC không kiểm link quay lại | ✅ Đã trả lời 19-10-2026 | Chốt theo Assumption tạm: không có link quay lại là đúng thiết kế — TC không kiểm |
| AMB-LOGIN-08 | Tiêu đề tab trang Forgot Password là `… - Login`, trùng trang Login — thiếu tên trang? | Khó phân biệt tab; TC assert tiêu đề dễ nhầm trang | 🟡 | TC Forgot Password **không** assert tiêu đề tab, assert heading `Forgot Password` | ✅ Đã trả lời 19-10-2026 | Chốt theo Assumption tạm: TC Forgot Password assert heading, không assert tiêu đề tab |
| AMB-LOGIN-09 | Mọi chuyển hướng xác thực quan sát được trả `307` (chưa đăng nhập → Login, đã đăng nhập → Dashboard, Logout → Login), lệch quy ước thường dùng `302/303`. Chưa loại trừ khả năng do cách công cụ đo ghi nhận | TC assert mã HTTP có thể đỏ oan | 🟡 | **Không** assert mã chuyển hướng; assert URL đích + nội dung trang (áp nhất quán cho REQ-19 · 20 · 21 · 31 · 32) | ✅ Đã trả lời 19-10-2026 | Chốt theo Assumption tạm: không assert mã chuyển hướng, áp cho REQ-19 · 20 · 21 · 31 · 32 |
| AMB-LOGIN-10 | Mở `/admin/authentication/reset_password` không kèm tham số trả **`500`** (lỗi máy chủ) thay vì 404 / thông báo link không hợp lệ | Lộ lỗi máy chủ; trải nghiệm khi link reset hỏng/thiếu | 🟡 | Ghi nhận là nghi lỗi; chưa sinh REQ cho hành vi link reset sai | ✅ Đã trả lời 19-10-2026 | Chốt theo Assumption tạm: là **lỗi**, chưa sinh REQ — ứng viên bug report (`/create-bug-report`) |
| AMB-LOGIN-11 | Cookie `autologin` **không** có cờ `HttpOnly` (REQ-26) trong khi `sp_session` có — cố ý? | Script độc hại (XSS) đọc được cookie ghi nhớ 62 ngày | 🟡 | Theo hiện trạng; báo như rủi ro bảo mật | ✅ Đã trả lời 19-10-2026 | Chốt theo Assumption tạm: giữ hiện trạng — REQ-26 không đổi; ghi nhận là rủi ro bảo mật |
| AMB-LOGIN-12 | Đăng xuất có vô hiệu cookie `autologin` không (REQ-29)? Lượt Logout đã thử chạy khi phiên chưa tick Remember me | Máy dùng chung: người sau tự đăng nhập bằng tài khoản người trước | 🟡 | Logout vô hiệu `autologin` — cần kiểm ở lượt sau (tốn thêm 1 lần đăng nhập thật) | ✅ Đã trả lời 19-10-2026 | Chốt theo Assumption tạm: Logout **vô hiệu** `autologin` — REQ-29 là hành vi mong đợi, vẫn ⚪ tới khi kiểm |
| AMB-LOGIN-13 | Cookie phiên hạn 8 giờ, gia hạn theo request (REQ-23). Phiên phía server có hết hạn thật sau 8 giờ không hoạt động không? | TC timeout không biết chờ bao lâu | 🟡 | Hết hạn sau 8 giờ không hoạt động — chưa kiểm | ✅ Đã trả lời 19-10-2026 | Chốt theo Assumption tạm: phiên hết hạn sau 8 giờ không hoạt động → sinh `REQ-LOGIN-47` (⚪) |
| AMB-LOGIN-14 | Checkbox Remember me có `value="estimate"` — giá trị lạ (thường là `1`/`on`). Có ảnh hưởng gì không? | Thấp — Remember me vẫn hoạt động (REQ-27) | 🟢 | Không ảnh hưởng | ✅ Đã trả lời 19-10-2026 | Chốt theo Assumption tạm: không ảnh hưởng — không sinh REQ |
| AMB-LOGIN-15 | Bỏ trống cả 2 ô, thông báo Password hiện **trước** Email (ngược thứ tự field) — có quy định thứ tự không? | TC assert thứ tự đỏ oan | 🟢 | Không quy định — TC không assert thứ tự | ✅ Đã trả lời 19-10-2026 | Chốt theo Assumption tạm: không quy định thứ tự — REQ-10 không assert thứ tự |
| AMB-LOGIN-16 | Server có thực thi kiểm CSRF token không (REQ-25)? Kiểm cần dựng request thủ công | Form có token nhưng server không kiểm → CSRF | 🟡 | Có kiểm — đội Dev xác minh (QA chưa xác nhận quyền gọi API) | ✅ Đã trả lời 19-10-2026 | Chốt theo Assumption tạm: server **có** kiểm, đội Dev xác minh — REQ-25 giữ ⚪ ở phía QA |
| AMB-LOGIN-17 | Cảnh báo Logout khi có timer chạy (REQ-35) chỉ đọc từ mã JS — được phép bật timer chấm công trên tài khoản dùng chung để kiểm không? | REQ-35 không có TC chạy được | 🟡 | Hành vi đúng như mã JS | ✅ Đã trả lời 19-10-2026 | Chốt theo Assumption tạm: hành vi đúng như mã JS — REQ-35 là hành vi mong đợi; vẫn ⚪ vì chưa được bật timer để chạy thật |
| AMB-LOGIN-18 | Hệ thống có những role staff nào? Chưa đọc được (Setup bị chặn — `AMB-SYS-01`), chưa có account role nào khác ngoài tài khoản `.env` | 4 ô phân quyền ❔ | 🔴 | Mọi role staff dùng chung luồng đăng nhập | ✅ Đã trả lời 19-10-2026 | User xác nhận: hệ thống **chỉ có role Admin** → cột "Staff — role khác" của Ma trận phân quyền chuyển sang *Không áp dụng*; không cần TC phân quyền theo role khác. Không sinh REQ mới |
| AMB-LOGIN-19 | Staff bị vô hiệu hoá (inactive) đăng nhập thì sao — bị chặn với thông báo gì? Chưa có account test | 4 ô phân quyền ❔; không biết thông báo | 🔴 | Bị chặn | ✅ Đã trả lời 19-10-2026 | Chốt theo Assumption tạm: **bị chặn** đăng nhập → sinh `REQ-LOGIN-48` (⚪, nguyên văn thông báo chưa biết). Câu trả lời không nói về Forgot Password → mở `AMB-LOGIN-21` |
| AMB-LOGIN-20 | Contact (tài khoản cổng khách hàng) đăng nhập vào khu Admin thì sao? Chưa có account contact | 4 ô phân quyền ❔ | 🔴 | Bị từ chối như email không tồn tại | ✅ Đã trả lời 19-10-2026 | User: **bị từ chối như email không tồn tại** → sinh `REQ-LOGIN-44` (Login) và `REQ-LOGIN-45` (Forgot Password — áp cùng nguyên tắc "như email không tồn tại" cho cả luồng xác thực khu Admin) |
| AMB-LOGIN-21 | Staff bị vô hiệu hoá dùng Forgot Password khu Admin thì sao — có nhận email đặt lại mật khẩu không? (`AMB-LOGIN-19` chỉ chốt phần đăng nhập) | Ô phân quyền [Forgot Password × staff vô hiệu hoá] ❔; tài khoản đã khoá vẫn có thể nhận link reset | 🔴 | Không nhận email reset; thông báo trên trang giống REQ-39 | ✅ Đã trả lời 19-10-2026 | User xác nhận đúng Assumption tạm: **không nhận email reset, thông báo giống REQ-39** → sinh `REQ-LOGIN-50` (⚪) |
| AMB-LOGIN-22 | Email **không tồn tại** (kể cả email của contact — REQ-44, staff vô hiệu hoá — REQ-48) bị nhập sai 5 lần liên tiếp thì có hiện thông báo khoá (REQ-51) không? Ticket chỉ nói "sai mật khẩu … với cùng một email" | Nếu chỉ email tồn tại mới hiện thông báo khoá → thử 5 lần là biết email nào có tài khoản, phá thông báo chung của REQ-13 và chống dò tài khoản (REQ-39 · 49). Ngược lại, TC của REQ-12 lặp nhiều lần trên cùng email `@auto.test` sẽ gặp thông báo khoá → FAIL giả | 🔴 | Xử lý như nhau: email không tồn tại sai 5 lần liên tiếp cũng nhận thông báo khoá | ✅ Đã trả lời 22-10-2026 | User: **không khoá** email không có trong hệ thống → sinh `REQ-LOGIN-57`. **Khác Assumption tạm** → TC REQ-12 lặp trên cùng email `@auto.test` không còn nguy cơ FAIL giả. Hệ quả: email tồn tại sai 6 lần thì thấy thông báo khoá, email không tồn tại thì không bao giờ → phân biệt được email có tài khoản (`RISK-LOGIN-04`). Câu trả lời không nói về email của contact / staff vô hiệu hoá → mở `AMB-LOGIN-31` |
| AMB-LOGIN-23 | Lần sai **thứ 5** hiển thị gì — đã là thông báo khoá (REQ-51), hay vẫn `Invalid email or password` và lần thử **thứ 6** mới thấy thông báo khoá? | Expected của TC biên 4 / 5 / 6 lần đổi hoàn toàn | 🔴 | Ngay lần sai thứ 5 đã hiện thông báo khoá | ✅ Đã trả lời 22-10-2026 | User: lần sai thứ 5 vẫn hiện `Invalid email or password`, lần thử **thứ 6** mới hiện thông báo khoá. **Khác Assumption tạm** → sửa AC `REQ-LOGIN-46` (🟡). Tài khoản bị khoá ngay sau lần sai thứ 5 (ticket AC#1), nên lần thử thứ 6 dù đúng mật khẩu cũng nhận thông báo khoá (REQ-52) |
| AMB-LOGIN-24 | Mốc 15 phút tính từ lúc nào, và thử đăng nhập (đúng hoặc sai) **trong lúc đang khoá** có tính lại 15 phút từ đầu không? | TC của REQ-53 không biết chờ bao lâu; người dùng thật thử lại liên tục có thể bị khoá kéo dài | 🟡 | Tính từ lần sai thứ 5; thử lại trong lúc khoá **không** kéo dài thời gian khoá | ✅ Đã trả lời 22-10-2026 | Chốt theo Assumption tạm: mốc tính từ lần sai thứ 5 (ghi vào AC REQ-53); thử lại trong lúc khoá không kéo dài → sinh `REQ-LOGIN-59` |
| AMB-LOGIN-25 | Hết 15 phút khoá, bộ đếm lần sai về 0 hay giữ nguyên (sai thêm 1 lần là khoá lại ngay)? | TC sau khi mở khoá sai kỳ vọng | 🟡 | Về 0 | ✅ Đã trả lời 22-10-2026 | Chốt theo Assumption tạm: hết khoá thì bộ đếm về 0 → sinh `REQ-LOGIN-60` |
| AMB-LOGIN-26 | Lần gửi form bị từ chối vì **bỏ trống** Password (REQ-09) có tính vào bộ đếm lần sai của email đã nhập không? | TC validation chạy lặp trên tài khoản Project Manager có thể khoá tài khoản ngoài ý muốn | 🟡 | Không tính — chỉ lần gửi đủ email + mật khẩu mà sai mật khẩu mới tính | ✅ Đã trả lời 22-10-2026 | Chốt theo Assumption tạm: gửi form bỏ trống Password không tính vào bộ đếm → sinh `REQ-LOGIN-61` |
| AMB-LOGIN-27 | Bộ đếm khoá có coi email khác **hoa/thường** hoặc có **khoảng trắng đầu/cuối** (VD `PM@Example.com` và `pm@example.com`) là cùng một email không? | Nếu đăng nhập không phân biệt hoa/thường (giả định `ASM-LOGIN-03` của bộ TC) mà bộ đếm lại phân biệt → đổi kiểu chữ là vượt khoá, brute-force không bị cản | 🔴 | Cùng một email — không phân biệt hoa/thường, bỏ khoảng trắng đầu/cuối | ✅ Đã trả lời 22-10-2026 | User: **không phân biệt hoa/thường** → sinh `REQ-LOGIN-58`. Trùng Assumption tạm ở phần hoa/thường. Phần khoảng trắng đầu/cuối chưa được trả lời → mở `AMB-LOGIN-32` |
| AMB-LOGIN-28 | Tài khoản **đang bị khoá** dùng Forgot Password có nhận email đặt lại mật khẩu không? Đặt lại mật khẩu xong có mở khoá sớm không? | Không có kỳ vọng cho lối thoát tự nhiên nhất của người dùng bị khoá | 🟡 | Forgot Password vẫn dùng được; đặt lại mật khẩu **không** mở khoá sớm | ✅ Đã trả lời 22-10-2026 | Chốt theo Assumption tạm → sinh `REQ-LOGIN-62` (Forgot Password dùng được khi đang khoá) · `REQ-LOGIN-63` (đặt lại mật khẩu không mở khoá sớm). Cả hai cần hộp thư của tài khoản test — chưa có (`AMB-LOGIN-01` ⏭️) |
| AMB-LOGIN-29 | Khoá tài khoản có ảnh hưởng **phiên đang mở** và cookie ghi nhớ `autologin` (REQ-27) của chính tài khoản đó không? | Không biết người đang làm việc có bị đẩy ra khi bị kẻ khác khoá tài khoản; hoặc ngược lại `autologin` vượt được khoá | 🟡 | Khoá chỉ chặn đăng nhập qua form; phiên đang mở và `autologin` không bị ảnh hưởng | ✅ Đã trả lời 22-10-2026 | Chốt theo Assumption tạm → sinh `REQ-LOGIN-64` (phiên đang mở không bị đẩy ra) · `REQ-LOGIN-65` (cookie ghi nhớ vẫn tự đăng nhập khi đang khoá) |
| AMB-LOGIN-30 | Ticket yêu cầu TC dùng **tài khoản Project Manager**, không dùng tài khoản Admin. Nhưng: (1) user đã xác nhận hệ thống **chỉ có role Admin** (`AMB-LOGIN-18` ✅ 19-10-2026); (2) `.env` hiện chỉ có tài khoản Admin; (3) `AMB-LOGIN-01` ⏭️ chốt chưa cấp tài khoản test. "Project Manager" là **role mới**, hay một tài khoản staff role Admin đặt tên Project Manager? Xin cấp tài khoản (lưu `.env`, không ghi vào `docs/`) | Toàn bộ TC của REQ-46 · 51 → 56 không chạy được. Nếu là role mới thì Ma trận phân quyền (mục 4) và thuộc tính "Role của hệ thống" ở danh mục đang sai | 🔴 | Là **một tài khoản staff riêng**, không phải role mới; chưa được cấp → TC viết trước, đánh `@Skip`. Ma trận phân quyền chưa sửa | ✅ Đã trả lời 22-10-2026 | User: TC khoá dùng **tài khoản Admin dùng chung trong `.env`** (`EMAIL_ADMIN` / `PASSWORD_ADMIN`). Khi được hỏi lại về mâu thuẫn, user chọn **bỏ ràng buộc AC#6** của ticket — quyết định của user 22-10-2026 đè lên ticket 21-10-2026. **Khác Assumption tạm.** ⚠️ **Cập nhật lần 2 — 22-10-2026:** user đổi quyết định, TC khoá dùng **tài khoản Project Manager** do user cấp (lưu `.env`: `EMAIL_PM` / `PASSWORD_PM` — không ghi giá trị vào tài liệu), **không** dùng tài khoản Admin → ràng buộc AC#6 của ticket **có hiệu lực trở lại**. Tài khoản Admin không còn bị khoá khi chạy TC. Tài khoản này có phải role riêng không → `AMB-LOGIN-33` |
| AMB-LOGIN-33 | Tài khoản **Project Manager** (user cấp 22-10-2026) có role khác Admin không? Danh mục đang ghi hệ thống **chỉ có role Admin** (`AMB-LOGIN-18` ✅ 19-10-2026) | Nếu là role riêng: Ma trận phân quyền (mục 4) thiếu một cột, thuộc tính "Role của hệ thống" ở danh mục sai, và mọi module cần xét lại phân quyền | 🟡 | Là một tài khoản staff **cùng role Admin**, chỉ khác người dùng — thuộc tính "chỉ có role Admin" giữ nguyên | ✅ Đã trả lời 22-10-2026 | User: tài khoản Project Manager **cùng role Admin**, hệ thống vẫn chỉ có role Admin. Trùng Assumption tạm → Ma trận phân quyền và thuộc tính danh mục không đổi, không sinh REQ |
| AMB-LOGIN-31 | Email của **contact** (REQ-44) và của **staff bị vô hiệu hoá** (REQ-48) sai 5 lần liên tiếp thì có bị khoá / hiện thông báo khoá không? `AMB-LOGIN-22` chỉ chốt email không có trong hệ thống | Không biết kỳ vọng ở lần thử thứ 6 cho 2 loại tài khoản này; có thể lộ loại tài khoản | 🟡 | Không khoá — contact xử lý như email không tồn tại (REQ-44); staff vô hiệu hoá đã bị chặn bởi REQ-48 | ✅ Đã trả lời 22-10-2026 | Chốt theo Assumption tạm → sinh `REQ-LOGIN-66` (contact không bị khoá) · `REQ-LOGIN-67` (staff vô hiệu hoá không bị khoá). Chưa có 2 loại tài khoản này |
| AMB-LOGIN-32 | Bộ đếm khoá có bỏ **khoảng trắng đầu/cuối** của email không (`␣admin@…␣` có cùng bộ đếm với `admin@…`)? `AMB-LOGIN-27` chỉ chốt phần hoa/thường | Thêm khoảng trắng có thể vượt khoá nếu đăng nhập bỏ khoảng trắng mà bộ đếm thì không (liên quan `ASM-LOGIN-03` của bộ TC) | 🟡 | Có bỏ — cùng một bộ đếm | ✅ Đã trả lời 22-10-2026 | Chốt theo Assumption tạm → sinh `REQ-LOGIN-68`. ⚠️ Ô Email là `type="email"` — trình duyệt có thể tự cắt khoảng trắng trước khi gửi (`ASM-LOGIN-01` của bộ TC), khi đó REQ này không kiểm được qua giao diện |

### 6.2. Risks

| Mã | Rủi ro | Mô tả | Mitigation |
|---|---|---|---|
| RISK-LOGIN-01 | Môi trường & tài khoản dùng chung | Thử sai mật khẩu, Forgot Password, đổi mật khẩu, Logout trên tài khoản `.env` có thể khoá tài khoản / gửi email / làm phiền người khác dùng chung | Phép thử lỗi chỉ dùng email `@auto.test` không tồn tại; không dùng tài khoản `.env` cho TC phá huỷ. Không có tài khoản test riêng (`AMB-LOGIN-01` ⏭️). **Từ 22-10-2026:** TC khoá tài khoản của ticket `CRM-LOGIN-101` dùng **tài khoản Project Manager** (`EMAIL_PM` / `PASSWORD_PM` trong `.env`), **cấm** dùng tài khoản Admin — đúng ràng buộc AC#6 của ticket (`AMB-LOGIN-30` ✅ lần 2 · `RISK-LOGIN-09`). Tài khoản Project Manager chỉ cấp cho STORY-LOGIN-07; TC khác muốn dùng phải được user đồng ý |
| RISK-LOGIN-02 | Trình duyệt tự điền (autofill) | Profile trình duyệt có mật khẩu đã lưu tự điền Email/Password — TC assert ô trống (REQ-14/15) đỏ oan. Đã xảy ra trong lượt recon: sau Logout, profile Playwright tự điền email tài khoản thật | Chạy TC trong context sạch, không lưu mật khẩu; cấm assert ô trống trên profile có autofill |
| RISK-LOGIN-03 | Thông báo của trình duyệt | Validation `type="email"` (REQ-11/38) hiện chuỗi do trình duyệt sinh, đổi theo trình duyệt/ngôn ngữ | Assert `checkValidity() === false` + không có request POST; cấm assert chuỗi |
| RISK-LOGIN-04 | Dò tài khoản & mật khẩu | Ticket `CRM-LOGIN-101` đổi REQ-46 sang **khoá 15 phút sau 5 lần sai liên tiếp** — brute-force bị cản **khi Dev triển khai xong** (chưa kiểm chứng; quan sát 12-10-2026 vẫn không giới hạn, không CAPTCHA). Bộ đếm không phân biệt hoa/thường (`AMB-LOGIN-27` ✅) — không vượt khoá bằng đổi kiểu chữ. ⚠️ **Lộ email tồn tại qua thông báo khoá:** email không có trong hệ thống không bao giờ bị khoá (`AMB-LOGIN-22` ✅, REQ-57), còn email tồn tại sai đến lần thử thứ 6 thì thấy `Your account is locked…` → người ngoài phân biệt được email nào có tài khoản, đi ngược thông báo chung của REQ-13. Đây là quyết định của user, ghi nhận như rủi ro bảo mật cần báo PO. Forgot Password hiện vẫn lộ email tồn tại cho tới khi Dev sửa REQ-39 (`AMB-LOGIN-03`) | TC bảo mật ở Vòng 4; báo PO/Dev; không chạy dò hàng loạt trên môi trường dùng chung |
| RISK-LOGIN-05 | Thiếu security header | Trang Login không có `X-Frame-Options` / `Content-Security-Policy` / `Strict-Transport-Security` / `X-Content-Type-Options` → có thể bị nhúng iframe (clickjacking) | Báo Dev; TC phi chức năng kiểm header response |
| RISK-LOGIN-06 | Ngôn ngữ giao diện | Thông báo lỗi tiếng Anh theo ngôn ngữ mặc định hệ thống — đổi ngôn ngữ (25 ngôn ngữ, `RISK-SYS-03`) làm hỏng assert nguyên văn. Riêng thông báo mong đợi của REQ-39 / 49 do PO chốt bằng **tiếng Việt** trong khi các thông báo khác của trang là tiếng Anh | Locator theo `id`/`name`; chốt ngôn ngữ English ở bước chuẩn bị TC; khi Dev sửa REQ-39, xác nhận lại thông báo hiển thị theo ngôn ngữ nào |
| RISK-LOGIN-07 | Cookie ghi nhớ còn lại sau recon | Lượt recon 12-10-2026 đăng nhập có tick Remember me → profile Playwright trên máy QA giữ cookie `autologin` hạn ≈ 62 ngày | Xoá cookie / profile Playwright khi không cần; không chia sẻ thư mục profile |
| RISK-LOGIN-08 | Khoá tài khoản bị lợi dụng để chặn người dùng thật | Khoá **theo email, không theo IP** (REQ-55 · 56): ai biết email của staff đều khoá được tài khoản đó 15 phút, lặp lại không giới hạn — kể cả tài khoản Admin. Ticket không nêu ngoại lệ, cách mở khoá thủ công hay báo cho chủ tài khoản | Báo PO như rủi ro sản phẩm; **không** viết TC khoá lặp lại nhiều chu kỳ — mỗi TC khoá tài khoản Project Manager đúng 1 chu kỳ, theo `RISK-LOGIN-09`; **không** bao giờ thử khoá tài khoản Admin |
| RISK-LOGIN-09 | TC khoá tài khoản trên môi trường dùng chung | TC khoá dùng **tài khoản Project Manager** (`AMB-LOGIN-30` ✅ lần 2): mỗi TC khoá tài khoản này 15 phút → TC khoá kế tiếp và người khác dùng chung tài khoản Project Manager (nếu có) bị chặn theo; chạy lại liền nhau (Definition of Done: PASS 2 lần liên tiếp) hoặc song song (framework mặc định 5 luồng) trên cùng tài khoản sẽ đỏ dây chuyền. Tài khoản Admin **không** bị ảnh hưởng → smoke và TC module khác chạy bình thường. REQ-59 · 60 · 53 phải chờ 15 phút thật; REQ-63 đổi mật khẩu tài khoản Project Manager | TC khoá chạy **tuần tự**, cách nhau ≥ 15 phút, không chạy song song với nhau; không chạy cùng lúc TC nào khác dùng tài khoản Project Manager; sau REQ-63 cập nhật `PASSWORD_PM` trong `.env`; báo trước nếu tài khoản Project Manager có người khác dùng |

## 7. Phân rã Epic / Story

**Epic:** Xác thực khu Admin (`LOGIN`)

| Story ID | Tên Story | REQ bao phủ | Số REQ | AMB / RISK liên quan | Ghi chú phạm vi |
|---|---|---|---|---|---|
| STORY-LOGIN-01 | Giao diện trang Login | REQ-LOGIN-01 → 07 | 7 | — | Thành phần form, focus, che mật khẩu, tiêu đề, logo, link Forgot |
| STORY-LOGIN-02 | Validation & xử lý lỗi đăng nhập | REQ-LOGIN-08 → 16 · 44 · 48 | 11 | AMB-LOGIN-15 · RISK-LOGIN-02 · 03 | REQ-13 · 44 · 48 ⚪. REQ-46 chuyển sang STORY-07 ngày 22-10-2026 |
| STORY-LOGIN-03 | Đăng nhập thành công & chặn truy cập | REQ-LOGIN-17 → 25 · 47 | 10 | AMB-LOGIN-06 · 09 · 13 · 16 · RISK-LOGIN-05 | Gồm cookie phiên + CSRF + hết hạn phiên; REQ-25 · 47 ⚪ |
| STORY-LOGIN-04 | Remember me | REQ-LOGIN-26 → 29 | 4 | AMB-LOGIN-11 · 12 · 14 · RISK-LOGIN-07 | REQ-28 · 29 ⚪ |
| STORY-LOGIN-05 | Đăng xuất | REQ-LOGIN-30 → 35 | 6 | AMB-LOGIN-17 | REQ-35 ⚪ |
| STORY-LOGIN-06 | Forgot Password | REQ-LOGIN-36 → 43 · 45 · 49 · 50 | 11 | AMB-LOGIN-03 · 04 · 05 · 07 · 08 · 10 · RISK-LOGIN-04 | REQ-39 🟡 · REQ-42 · 43 · 45 · 49 · 50 ⚪ |
| STORY-LOGIN-07 | Khoá tài khoản khi đăng nhập sai nhiều lần (ticket `CRM-LOGIN-101`) | REQ-LOGIN-46 · 51 → 68 | 19 | AMB-LOGIN-02 · 22 → 33 · RISK-LOGIN-08 · 09 | REQ-46 🟡 · 51 → 68 ⚪ — dùng tài khoản Project Manager (`AMB-LOGIN-30` ✅ lần 2). **BLOCKED:** Dev chưa triển khai lên môi trường demo (user xác nhận 22-10-2026) |

**Tổng: 7 Story / 68 REQ — mọi REQ thuộc đúng một Story, không mồ côi, không trùng:** `7 + 11 + 10 + 4 + 6 + 11 + 19 = 68 ✔`

**Đối chiếu AMB / RISK:**

| Nhóm | Mã | Nằm ở đâu |
|---|---|---|
| AMB thuộc Story | AMB-LOGIN-02 → 17 · 22 → 33 (28 mã) | phân bổ ở bảng Story |
| AMB cấp Epic | AMB-LOGIN-01 · 18 · 19 · 20 · 21 (5 mã) | 01: ảnh hưởng REQ của STORY-02 và STORY-06 · 18/19/20/21: Ma trận phân quyền |
| RISK thuộc Story | RISK-LOGIN-02 · 03 · 04 · 05 · 07 · 08 · 09 (7 mã) | phân bổ ở bảng Story |
| RISK cấp Epic | RISK-LOGIN-01 · 06 (2 mã) | rủi ro mức module: môi trường dùng chung · ngôn ngữ |

`AMB: 28 + 5 = 33 ✔ (AMB-LOGIN-01 → 33)` · `RISK: 7 + 2 = 9 ✔ (RISK-LOGIN-01 → 09)` · Còn treo: **0**

**Hạng mục cấp Epic** (cố ý không gán vào Story):
- Ma trận phân quyền (mục 4) — cắt ngang mọi Story; hệ thống chỉ có role Admin; còn 1 ô ❔ (cố ý không thử, `AMB-LOGIN-01` ⏭️)
- Ma trận trạng thái phiên (mục 5) — nối STORY-02/03/04/05
- Yêu cầu phi chức năng (file web mục 5) — header bảo mật, HTTPS, CAPTCHA, trình duyệt
- AMB-LOGIN-01 · RISK-LOGIN-01 · RISK-LOGIN-06 — ảnh hưởng nhiều Story

**Thứ tự triển khai đề xuất:**
1. **STORY-LOGIN-03** — đăng nhập thành công + chặn truy cập là điều kiện cho mọi module khác (smoke). REQ-47 cần chờ 8 giờ — tách khỏi bộ smoke
2. **STORY-LOGIN-02** — validation & lỗi (không cần tài khoản thật, trừ REQ-13 · 44 · 46 · 48 — `skip` theo `AMB-LOGIN-01` ⏭️)
3. **STORY-LOGIN-01** — giao diện
4. **STORY-LOGIN-06** — Forgot Password: REQ-39 viết TC theo **hành vi mong đợi**, dự kiến FAIL tới khi Dev sửa → nên chạy sớm để theo dõi bug; REQ-42 · 43 · 45 · 49 · 50 `skip`
5. **STORY-LOGIN-05** — đăng xuất (REQ-35 `skip`: chưa được bật timer trên tài khoản dùng chung)
6. **STORY-LOGIN-04** — Remember me (REQ-29 cần thêm 1 lần đăng nhập thật có tick Remember me)
7. **STORY-LOGIN-07** — Khoá tài khoản: **BLOCKED** — Dev chưa triển khai ticket lên môi trường demo (user xác nhận 22-10-2026). Viết TC trước, đánh `@Skip`; gỡ khi Dev báo đã triển khai (ghi build/ngày vào Nhật ký). Dùng tài khoản Project Manager (`AMB-LOGIN-30` ✅ lần 2) → TC khoá chạy tuần tự, cách nhau ≥ 15 phút (`RISK-LOGIN-09`); không chặn TC dùng tài khoản Admin. REQ-57 · 66 · 67 không khoá tài khoản test nào. REQ-53 · 59 · 60 phải chờ 15 phút · REQ-56 cần mạng thứ hai · REQ-62 · 63 cần hộp thư · REQ-66 · 67 cần tài khoản contact / staff vô hiệu hoá

## 8. Nhật ký thay đổi

| Ngày | Nguồn | REQ ảnh hưởng | Loại | Tóm tắt thay đổi | TC cần xử lý |
|---|---|---|---|---|---|
| 22-10-2026 | User trả lời `AMB-LOGIN-33` | — (không REQ nào đổi) | ✏️ Biên tập | Tài khoản Project Manager cùng role Admin — hệ thống vẫn chỉ có role Admin. `AMB-LOGIN-33` ✅, module không còn AMB treo. Ma trận phân quyền không đổi | Không cần sửa TC |
| 22-10-2026 | User chốt 7 AMB 🟡 theo Assumption tạm (`AMB-LOGIN-24` · `25` · `26` · `28` · `29` · `31` · `32`) | REQ-LOGIN-59 → 68 | 🟢 Thêm | 59 thử lại trong lúc khoá không kéo dài khoá · 60 hết khoá bộ đếm về 0 · 61 bỏ trống Password không tính vào bộ đếm · 62 Forgot Password dùng được khi đang khoá · 63 đặt lại mật khẩu không mở khoá sớm · 64 phiên đang mở không bị đẩy ra · 65 cookie ghi nhớ vẫn tự đăng nhập khi đang khoá · 66 contact không bị khoá · 67 staff vô hiệu hoá không bị khoá · 68 bộ đếm bỏ khoảng trắng email. Tất cả ⚪ chưa build. Trùng Assumption tạm → TC hiện có không đổi | Viết mới, đánh `@Skip` |
| 22-10-2026 | User đổi tài khoản phép thử (`AMB-LOGIN-30` lần 2) | REQ-LOGIN-46 · 51 → 58 | ✏️ Biên tập | Tài khoản phép thử đổi từ Admin `.env` sang **Project Manager** (`EMAIL_PM` / `PASSWORD_PM`) — ràng buộc AC#6 có hiệu lực trở lại. REQ-53 ghi mốc 15 phút tính từ lần sai thứ 5 (`AMB-LOGIN-24` ✅). Cập nhật `RISK-LOGIN-01` · `08` · `09`. Mở `AMB-LOGIN-33` 🟡 (Project Manager có phải role riêng) | `CRM_LOGIN_TC_036` → tiền điều kiện dùng tài khoản Project Manager; TC dùng tài khoản Admin không còn bị chặn bởi TC khoá |
| 22-10-2026 | User xác nhận Dev chưa triển khai `CRM-LOGIN-101` | REQ-LOGIN-46 · 51 → 58 | ✏️ Biên tập | Ghi rõ tính năng khoá **chưa triển khai** lên môi trường demo — hệ thống hiện vẫn **không giới hạn** số lần đăng nhập sai (hành vi cũ của REQ-46). REQ-51 → 58 thuộc nhóm ⚪ chưa build. STORY-07 BLOCKED. Không đổi hành vi mong đợi | `CRM_LOGIN_TC_036` và mọi TC mới của STORY-07 → `@Skip`, lý do "chưa triển khai"; **không** chạy TC khoá trên demo |
| 22-10-2026 | User trả lời `AMB-LOGIN-22` · `27` | REQ-LOGIN-57 · 58 | 🟢 Thêm | 57 email không có trong hệ thống không bị khoá dù sai nhiều lần (`AMB-LOGIN-22`, khác Assumption tạm) · 58 bộ đếm khoá không phân biệt hoa/thường của email (`AMB-LOGIN-27`). Cả hai ⚪. Mở `AMB-LOGIN-31` (contact / staff vô hiệu hoá) · `32` (khoảng trắng email) cho phần chưa trả lời | Viết mới · `CRM_LOGIN_TC_017` · `053` hết nguy cơ FAIL giả → không cần sửa |
| 22-10-2026 | User trả lời `AMB-LOGIN-23` | REQ-LOGIN-46 | 🟡 Sửa | Biên khoá: lần sai 1 → 5 đều hiện `Invalid email or password`, thông báo khoá hiện từ lần thử **thứ 6** (trước đó giả định hiện ngay lần thứ 5) | `CRM_LOGIN_TC_036` → review: Expected biên lần 5 / lần 6 |
| 22-10-2026 | User trả lời `AMB-LOGIN-30` | REQ-LOGIN-46 · 51 → 56 | ✏️ Biên tập | Tài khoản phép thử đổi từ "Project Manager" sang tài khoản Admin `.env` — user bỏ ràng buộc AC#6 của ticket. Không có role mới. Cập nhật `RISK-LOGIN-01` · `04` · `08` · `09`; STORY-07 hết BLOCKED vì thiếu tài khoản | Tiền điều kiện TC mới + `CRM_LOGIN_TC_036` dùng tài khoản Admin `.env`, chạy cuối bộ |
| 22-10-2026 | Ticket `CRM-LOGIN-101` (PO chốt 21-10-2026) | REQ-LOGIN-46 | 🟡 Sửa | Đảo hành vi: "không giới hạn số lần đăng nhập sai" → **sai mật khẩu 5 lần liên tiếp với cùng email thì tài khoản bị khoá**. Kết luận `AMB-LOGIN-02` (19-10-2026) bị thay thế. REQ-46 chuyển từ STORY-02 sang STORY-07 mới | `CRM_LOGIN_TC_036` → **review**: Expected đảo ngược, tiền điều kiện đổi sang tài khoản Project Manager |
| 22-10-2026 | Ticket `CRM-LOGIN-101` (PO chốt 21-10-2026) | REQ-LOGIN-51 → 56 | 🟢 Thêm | 51 thông báo khoá nguyên văn · 52 đúng mật khẩu vẫn bị từ chối khi đang khoá · 53 hết 15 phút đăng nhập được lại · 54 đăng nhập thành công đặt lại bộ đếm · 55 khoá email không chặn email khác cùng IP · 56 email bị khoá vẫn bị khoá từ IP khác. Tất cả ⚪ — chưa kiểm chứng UI. Thêm STORY-LOGIN-07 | Viết mới, đánh `@Skip` (`AMB-LOGIN-30`) |
| 22-10-2026 | Ticket `CRM-LOGIN-101` (PO chốt 21-10-2026) | — (không REQ nào đổi hành vi) | ✏️ Biên tập | Mở `AMB-LOGIN-22` → `30` (4 🔴). Thêm `RISK-LOGIN-08` (khoá bị lợi dụng chặn người dùng) · `09` (TC khoá trên môi trường dùng chung); cập nhật `RISK-LOGIN-01` (ràng buộc tài khoản Project Manager, cấm Admin) · `04`. Ma trận trạng thái phiên thêm trạng thái *Tài khoản bị khoá*. Ràng buộc AC#6 là ràng buộc kiểm thử — không cấp REQ | `CRM_LOGIN_TC_035` → review tiền điều kiện · `CRM_LOGIN_TC_017` · `053` → review có điều kiện (`AMB-LOGIN-22`) — chi tiết ở [Impact Report](impact/impact_CRM-LOGIN-101.md) |
| 19-10-2026 | User trả lời `AMB-LOGIN-21` | REQ-LOGIN-50 | 🟢 Thêm | Staff bị vô hiệu hoá dùng Forgot Password: không nhận email reset, thông báo giống REQ-39 (⚪). `AMB-LOGIN-21` ✅ → module không còn AMB treo. Ma trận phân quyền `3 · 4 · 1 · 8` | Viết mới, đánh `skip` |
| 19-10-2026 | User chốt AMB đợt 1 (`AMB-LOGIN-03`) | REQ-LOGIN-39 | 🟡 Sửa | Đổi hành vi mong đợi: email không tồn tại ở Forgot Password phải nhận thông báo trung tính `Nếu địa chỉ email này có tài khoản trong hệ thống, chúng tôi đã gửi liên kết đặt lại mật khẩu tới đó. Vui lòng kiểm tra hộp thư (và cả thư mục Spam).` thay cho `Email not found` (hiện trạng là lỗi, Dev sẽ sửa) | TC của REQ-39 → **review**: expected đổi sang thông báo mới, dự kiến FAIL tới khi Dev sửa |
| 19-10-2026 | User chốt AMB đợt 1 (`AMB-LOGIN-02` · `03` · `13` · `19` · `20`) | REQ-LOGIN-44 → 49 | 🟢 Thêm | 44 contact bị từ chối đăng nhập · 45 contact dùng Forgot Password như email không tồn tại · 46 không giới hạn số lần đăng nhập sai · 47 phiên hết hạn sau 8 giờ không hoạt động · 48 staff vô hiệu hoá không đăng nhập được · 49 email tồn tại nhận cùng thông báo trung tính. Tất cả ⚪ — chưa kiểm chứng UI | Viết mới, đánh `skip` |
| 19-10-2026 | User chốt AMB đợt 1 (các AMB còn lại theo Assumption tạm) | REQ-LOGIN-23 · 25 · 29 · 42 | ✏️ Biên tập | AMB-01 ⏭️ · AMB-04 → 17 và 19 ✅. Sửa câu chữ: REQ-23 trỏ sang REQ-47 · REQ-25 ghi Dev xác minh · REQ-29 ghi nguồn đã chốt · REQ-42 trỏ thông báo sang REQ-49. Mở `AMB-LOGIN-21` (Forgot Password × staff vô hiệu hoá). Ma trận phân quyền `3 · 3 · 2 · 8`; ma trận trạng thái phiên cập nhật | Không đổi hành vi — không cần sửa TC |
| 19-10-2026 | User trả lời `AMB-LOGIN-18` | — (không REQ nào đổi) | ✏️ Biên tập | Hệ thống chỉ có role Admin → `AMB-LOGIN-18` ✅; Ma trận phân quyền: cột "Staff — role khác" sang *Không áp dụng*, dòng tổng `3 · 0 · 9 · 4` | Không viết TC phân quyền cho role khác |
| 12-10-2026 | UI recon (`/generate-requirements-from-website login`) | REQ-LOGIN-01 → 43 | 🟢 Thêm | Khởi tạo tài liệu từ khảo sát UI thực tế mặt Web — 43 REQ (🟢 36 · ⚪ 7), 20 AMB, 7 RISK, 6 Story | — (viết TC mới) |
