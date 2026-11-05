# STORY-CUS-08 — Vault & Map

> ← [Index module `REQUIREMENTS_CUSTOMERS_SUMMARY.md`](../../REQUIREMENTS_CUSTOMERS_SUMMARY.md) · [File nền tảng Web](../requirements_customers_web.md)
>
> Phạm vi: `REQ-CUS-88` → `REQ-CUS-101` (14 REQ) · AMB liên quan: `AMB-CUS-12` · `15` · `16` · `18` · RISK liên quan: `RISK-CUS-09`

**Route:** Vault `?group=vault` · tạo `POST /admin/clients/vault_entry_create/<id KH>` · xoá `/admin/clients/vault_entry_delete/<id entry>`. Map `?group=map` · lưu `POST /admin/clients/save_longitude_and_latitude/<id KH>`.

## 1. Yêu cầu chức năng

| REQ ID | Tên yêu cầu | Mô tả | Acceptance Criteria | Trạng thái | Cập nhật lần cuối | Nguồn |
|---|---|---|---|---|---|---|
| REQ-CUS-88 | Vault trống hiển thị thông báo không có mục nào | | KH chưa có vault entry → tab có nút `New Vault Entry` và câu nguyên văn `Vault entries not found for this customer.` | 🟢 | — | Kiểm chứng thực tế |
| REQ-CUS-89 | Modal Vault Entry có đủ trường và mặc định quyền xem | | `New Vault Entry` → modal `#entryModal` tiêu đề `Vault Entry`: `* Server Address` · `Port` (`type=number`) · `* Username` · `* Password` (`type=password`) · `Short Description`; 3 radio `visibility` — `Visible to all staff member who have access to this customer` (value `1`, **mặc định chọn**) · `Visible only to administrators` (`2`) · `Visible only to me (administrators are not excluded)` (`3`); checkbox `Share this vault entry in projects with project members` không tick | 🟢 | — | Kiểm chứng thực tế |
| REQ-CUS-90 | Bỏ trống 3 trường bắt buộc của Vault bị chặn kèm thông báo | | Để trống Server Address · Username · Password → `Save` → không gửi `POST`; 3 thông báo `#server_address-error` · `#username-error` · `#password-error` đều nguyên văn `This field is required.` (Username phải xoá trắng trước — xem `AMB-CUS-16`) | 🟢 | — | Kiểm chứng thực tế |
| REQ-CUS-91 | Tạo vault entry hiển thị thông tin với mật khẩu bị che | | Điền đủ (giá trị test sinh ngẫu nhiên) → `Save` → `POST /admin/clients/vault_entry_create/<id>` `303`; thông báo `Vault Entry added successfully.`; panel tiêu đề = Server Address, hiển thị `Server Address:` · `Port:` · `Username:` · `Password: ••••••••••` (10 ký tự che, cố định — **không** phản ánh độ dài thật) · `Short Description:` và câu **chứa** `This vault entry is created by <tên staff> - ` kèm thời gian tương đối | 🟢 | — | Kiểm chứng thực tế · API `POST .../vault_entry_create/<id>` · 303 |
| REQ-CUS-92 | Mật khẩu vault không có trong mã HTML của trang | | Phép thử trạng thái sạch: (1) sinh mật khẩu mới duy nhất `Auto_<ts>_x` (chưa từng tồn tại) → (2) xác nhận trang Vault trước khi tạo không chứa chuỗi đó → (3) tạo entry → (4) `document.documentElement.innerHTML` **không** chứa chuỗi mật khẩu; ô `.vault-password-encrypted` rỗng | 🟢 | — | Kiểm chứng thực tế |
| REQ-CUS-93 | Xem mật khẩu vault yêu cầu nhập lại mật khẩu đăng nhập | | Icon khoá `.vault-view-password` → modal `#vaultConfirmPassword` tiêu đề `View Password`, câu nguyên văn `For security reasons please enter your password below`, ô `user_password` `type=password`, nút `Close` · `Confirm` | 🟢 | — | Kiểm chứng thực tế |
| REQ-CUS-94 | Nhập đúng mật khẩu đăng nhập thì hiện mật khẩu vault | | Agent không nhập mật khẩu tài khoản thật (`AMB-CUS-15`) | ⚪ | — | Chưa kiểm chứng |
| REQ-CUS-95 | Tuỳ chọn visibility giới hạn staff xem được vault entry | 3 mức quyền xem | Cần 2 tài khoản staff cùng đăng nhập để so sánh (`AMB-CUS-18`) | ⚪ | — | Chưa kiểm chứng |
| REQ-CUS-96 | Xoá vault entry cần xác nhận | | Icon `×` đỏ (href `/admin/clients/vault_entry_delete/<id>`, class `_delete`) → hộp `confirm` → đồng ý → entry biến mất (mở lại tab không còn Server Address đã tạo) | 🟢 | — | Kiểm chứng thực tế |
| REQ-CUS-97 | Tab Map có 2 ô toạ độ và báo thiếu Google API key | | Tab có ô `Latitude (Google Maps)` (`latitude`, kèm nút `G`) · `Longitude (Google Maps)` (`longitude`) · nút `Save` · `Open in Google Map`; khi hệ thống chưa cấu hình key hiện câu nguyên văn `Setup google api key in order to view to customer map` và không có bản đồ (`#map` không tồn tại) | 🟢 | — | Kiểm chứng thực tế |
| REQ-CUS-98 | Lưu toạ độ hợp lệ báo thành công | | Nhập `10.7769` / `106.7009` → `Save` → `POST /admin/clients/save_longitude_and_latitude/<id>` trả `200`, body `success` (text/html); thông báo nổi `Customer updated successfully.`; không tải lại trang | 🟢 | — | Kiểm chứng thực tế · API `POST .../save_longitude_and_latitude/<id>` · 200 |
| REQ-CUS-99 | Toạ độ không hợp lệ vẫn được lưu | Hành vi **hiện tại** — nghi lỗi | Nhập Latitude `abc` · Longitude `999` → `Save` → `200`, không thông báo lỗi; tải lại tab → 2 ô vẫn là `abc` / `999`. TC viết theo kết luận `AMB-CUS-12` 🔴 | 🟢 | — | Kiểm chứng thực tế |
| REQ-CUS-100 | Nút Open in Google Map mở vị trí của KH | | Chưa bấm | ⚪ | — | UI thực tế — chưa kiểm chứng |
| REQ-CUS-101 | Nút G lấy toạ độ từ địa chỉ KH | | Chưa bấm — mã trang có thông báo `The address couldn't be found, please try again` cho trường hợp không tìm thấy, chưa quan sát trên UI | ⚪ | — | UI thực tế — chưa kiểm chứng |

## 2. Đặc tả trường dữ liệu

| Field (Label) | `name` | Loại UI | Required | Ràng buộc / mặc định | REQ liên quan | Ghi chú |
|---|---|---|---|---|---|---|
| Server Address | `server_address` | text | ✅ | — | 89 · 90 | |
| Port | `port` | number | — | Không `min`/`max`/`step` | 89 | |
| Username | `username` | text | ✅ | Không `autocomplete=off` | 89 · 90 | Chrome tự điền email đăng nhập (`AMB-CUS-16`) |
| Password | `password` | password | ✅ | — | 89 · 90 · 92 | |
| Short Description | `description` | textarea | — | — | 89 | |
| Visibility | `visibility` | radio 1/2/3 | — | Mặc định `1` | 89 · 95 | |
| Share in projects | `share_in_projects` | checkbox | — | Không tick | 89 | |
| *(chống autofill)* | `fakeusernameremembered` · `fakepasswordremembered` | text/password ẩn | — | — | — | Không ngăn được autofill trên Chrome |
| Latitude (Google Maps) | `latitude` | text | — | Không validate | 97 · 98 · 99 | |
| Longitude (Google Maps) | `longitude` | text | — | Không validate | 97 · 98 · 99 | |

## 3. Validation & thông báo

| REQ ID | Rule / Trigger | Thông báo (nguyên văn) |
|---|---|---|
| REQ-CUS-88 | Vault trống | `Vault entries not found for this customer.` |
| REQ-CUS-90 | Thiếu Server Address / Username / Password | `This field is required.` (mỗi ô) |
| REQ-CUS-91 | Tạo thành công | `Vault Entry added successfully.` |
| REQ-CUS-93 | Xem mật khẩu | `For security reasons please enter your password below` |
| REQ-CUS-97 | Thiếu Google API key | `Setup google api key in order to view to customer map` |
| REQ-CUS-98 | Lưu toạ độ | `Customer updated successfully.` |
| REQ-CUS-99 | Toạ độ sai | *(không có thông báo)* |
