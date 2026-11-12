# STORY-CUS-08 — Vault & Map

> ← [Index module](../../REQUIREMENTS_CUSTOMERS_SUMMARY.md) · [File web](../requirements_customers_web.md) · REQ `REQ-CUS-123` → `138` (16 REQ). AMB/RISK, phân quyền, trạng thái: xem index.

**User story:** Là staff, tôi muốn lưu thông tin truy cập hệ thống của khách hàng một cách an toàn (Vault) và ghi toạ độ địa lý của khách hàng (Map).

## 1. Yêu cầu chức năng — Vault

| REQ ID | Tên yêu cầu | Mô tả | Acceptance Criteria | Trạng thái | Cập nhật lần cuối | Nguồn |
|---|---|---|---|---|---|---|
| REQ-CUS-123 | Vault rỗng có thông báo | `?group=vault` | Tiêu đề `Vault`, nút `New Vault Entry`; chưa có entry → nguyên văn `Vault entries not found for this customer.` | 🟢 | — | Kiểm chứng thực tế |
| REQ-CUS-124 | Modal Vault Entry | — | `New Vault Entry` → modal `Vault Entry`: `* Server Address` · `Port` (`type=number`) · `* Username` · `* Password` (`type=password`) · `Short Description` (textarea) · 3 radio `visibility`: `Visible to all staff member who have access to this customer` (`1`, mặc định chọn) · `Visible only to administrators` (`2`) · `Visible only to me (administrators are not excluded)` (`3`) · checkbox `Share this vault entry in projects with project members` (mặc định không tick) · `Close` · `Save`. Ghi chú trình duyệt: Chrome 157 **tự điền email đăng nhập vào `Username`** khi mở modal — chốt là lỗi nhẹ, ứng viên bug (`AMB-CUS-24` ✅); TC phải xoá trắng ô trước khi nhập, **cấm** assert ô rỗng lúc mở | 🟢 | 11-11-2026 · ✏️ biên tập | Kiểm chứng thực tế |
| REQ-CUS-125 | 3 trường bắt buộc | — | Để trống Server Address, Username, Password → `Save` → 3 lỗi `This field is required.` (`#server_address-error` · `#username-error` · `#password-error`), modal vẫn mở | 🟢 | — | Kiểm chứng thực tế |
| REQ-CUS-126 | Port chỉ nhận số trong khoảng 1–65535 | Port TCP/UDP hợp lệ | Port = `70000` → `Save` → **không** lưu, modal vẫn mở, ô Port có thông báo lỗi — nguyên văn: `AMB-CUS-28`. Biên: `1` · `65535` lưu được; `0` · `65536` bị chặn. Port để trống vẫn lưu được (không bắt buộc — REQ-CUS-125). ⚠️ **Hiện trạng 11-11-2026 lệch:** `70000` lưu thành công, thẻ hiện `Port: 70000` → TC **dự kiến FAIL cho tới khi Dev sửa** — ứng viên bug (`AMB-CUS-19` ✅) | 🟡 | 11-11-2026 · `AMB-CUS-19` | Kiểm chứng thực tế (hiện trạng) + User chốt `AMB-CUS-19` theo Assumption tạm |
| REQ-CUS-127 | Tạo vault entry | — | Điền đủ → `Save` → modal đóng, xuất hiện thẻ tiêu đề = Server Address, nội dung `Server Address:` · `Port:` · `Username:` · `Password:` · `Short Description:` và dòng **chứa** `This vault entry is created by <staff>` (thời điểm tương đối `Just now` là động); form gửi `/admin/clients/vault_entry_create/<id>` | 🟢 | — | Kiểm chứng thực tế |
| REQ-CUS-128 | Mật khẩu hiển thị dạng che cố định | — | Dòng `Password:` hiện 10 dấu `•` bất kể độ dài mật khẩu (mật khẩu 22 ký tự vẫn hiện 10 dấu), kèm icon khoá (tooltip `View Password`) | 🟢 | — | Kiểm chứng thực tế |
| REQ-CUS-129 | Sửa entry: điền sẵn, Password không bắt buộc | — | Icon sửa (`edit_vault_entry(<entry_id>)`) → modal điền sẵn Server Address, Port, Username, Short Description, visibility; ô Password **rỗng** và nhãn `Password` **không** có dấu `*`; form gửi `/admin/clients/vault_entry_update/<entry_id>` | 🟢 | — | Kiểm chứng thực tế |
| REQ-CUS-130 | Lưu khi sửa | — | Sửa Short Description, để trống Password → `Save` → toast nguyên văn `Vault Entry updated successfully.`, thẻ hiện mô tả mới, thêm dòng **chứa** `Last updated by <staff>` | 🟢 | — | Kiểm chứng thực tế |
| REQ-CUS-131 | Xoá vault entry | — | Icon xoá (`/admin/clients/vault_entry_delete/<entry_id>`) → confirm `Are you sure you want to perform this action?` → OK → về trạng thái rỗng REQ-CUS-123 | 🟢 | — | Kiểm chứng thực tế |
| REQ-CUS-132 | Xem mật khẩu phải nhập lại mật khẩu đăng nhập | — | Icon khoá → modal `View Password`, dòng nguyên văn `For security reasons please enter your password below`, 1 ô `type=password` (`#user_password`) · `Close` · `Confirm`; để trống → `Confirm` → `This field is required.` (`#user_password-error`) | 🟢 | — | Kiểm chứng thực tế |
| REQ-CUS-133 | Nhập đúng mật khẩu đăng nhập thì hiện mật khẩu vault | Tác tạo của REQ-CUS-132 | Nhập mật khẩu đăng nhập đúng → `Confirm` → dòng Password hiện giá trị thật. **Chưa kiểm** — agent không nhập mật khẩu thật; TC này **user tự chạy thủ công** (`AMB-CUS-22` ✅) | ⚪ | 11-11-2026 · ✏️ biên tập | Chưa kiểm chứng |
| REQ-CUS-134 | Visibility giới hạn staff được xem entry | — | Entry `Visible only to me` do staff A tạo → staff B (không phải admin) không thấy entry. **Chưa kiểm** — cần đăng nhập tài khoản thứ hai; chấp nhận để ⚪ ở giai đoạn này (`AMB-CUS-23` ⏭️) | ⚪ | 11-11-2026 · ✏️ biên tập | Chưa kiểm chứng |

## 2. Yêu cầu chức năng — Map

| REQ ID | Tên yêu cầu | Mô tả | Acceptance Criteria | Trạng thái | Cập nhật lần cuối | Nguồn |
|---|---|---|---|---|---|---|
| REQ-CUS-135 | Tab Map có 2 ô toạ độ | `?group=map` | `Latitude (Google Maps)` (`#latitude`, kèm nút icon `G`) · `Longitude (Google Maps)` (`#longitude`) · nút `Save` · `Open in Google Map`; chưa cấu hình API key → dòng nguyên văn `Setup google api key in order to view to customer map` | 🟢 | — | Kiểm chứng thực tế |
| REQ-CUS-136 | Lưu toạ độ | — | Nhập 2 giá trị → `Save` → `POST /admin/clients/save_longitude_and_latitude/<id>` (200), trang tải lại, 2 ô giữ giá trị vừa nhập; không có toast | 🟢 | — | Kiểm chứng thực tế + API |
| REQ-CUS-137 | Toạ độ phải là số trong khoảng hợp lệ | Vĩ độ [-90, 90], kinh độ [-180, 180] | Latitude = `abc`, Longitude = `999` → `Save` → **không** lưu (sau tải lại 2 ô giữ giá trị cũ), có thông báo lỗi — nguyên văn: `AMB-CUS-28`. Biên: Latitude `90` · `-90`, Longitude `180` · `-180` lưu được; Latitude `91`, Longitude `181` bị chặn. ⚠️ **Hiện trạng 11-11-2026 lệch:** sau tải lại 2 ô vẫn là `abc` / `999`, không báo lỗi → TC **dự kiến FAIL cho tới khi Dev sửa** — ứng viên bug (`AMB-CUS-20` ✅) | 🟡 | 11-11-2026 · `AMB-CUS-20` | Kiểm chứng thực tế (hiện trạng) + User chốt `AMB-CUS-20` theo Assumption tạm |
| REQ-CUS-138 | Hiển thị bản đồ khi có Google API key | Tác tạo của REQ-CUS-136 | Cấu hình API key → tab Map hiện bản đồ có ghim tại toạ độ đã lưu. **Chưa kiểm** — Setup bị chặn (`AMB-SYS-01`) | ⚪ | — | Chưa kiểm chứng |

## 3. Đặc tả trường dữ liệu

| Field | Loại UI · `name` | Required | Ràng buộc | REQ |
|---|---|---|---|---|
| Server Address | text · `server_address` | ✅ (client) | — | 125 |
| Port | number · `port` | — | **Mong đợi:** 1–65535. Hiện trạng: không giới hạn khoảng | 126 |
| Username | text · `username` | ✅ (client) | Bị trình duyệt tự điền | 124 · 125 |
| Password | password · `password` | ✅ khi tạo · ❌ khi sửa | Để trống khi sửa = giữ mật khẩu cũ (giả định — chưa xem được mật khẩu để đối chiếu) | 125 · 129 |
| Short Description | textarea · `description` | — | — | 127 · 130 |
| Visibility | radio · `visibility` | — | `1`* · `2` · `3` | 124 · 134 |
| Share in projects | checkbox · `share_in_projects` | — | Mặc định tắt | 124 |
| *(ẩn, chống autofill)* | `fakeusernameremembered` · `fakepasswordremembered` | — | Trường mồi — chỉ chặn được autofill mật khẩu | `AMB-CUS-24` ✅ — lỗi nhẹ |
| View Password | password · `user_password` | ✅ (client) | — | 132 |
| Latitude · Longitude | text · `latitude` · `longitude` | — | **Mong đợi:** số, vĩ độ [-90, 90] · kinh độ [-180, 180]. Hiện trạng: không validate | 135 → 137 |

## 4. Validation messages

| REQ ID | Rule / Trigger | Thông báo nguyên văn |
|---|---|---|
| REQ-CUS-123 | Chưa có entry | `Vault entries not found for this customer.` |
| REQ-CUS-125 | Server Address / Username / Password trống | `This field is required.` |
| REQ-CUS-126 | Port ngoài 1–65535 | **Mong đợi:** có thông báo lỗi — nguyên văn chờ `AMB-CUS-28`. Hiện trạng: lưu, không báo |
| REQ-CUS-130 | Sửa thành công | `Vault Entry updated successfully.` |
| REQ-CUS-132 | View Password — trống | `This field is required.` |
| REQ-CUS-135 | Chưa có Google API key | `Setup google api key in order to view to customer map` |
| REQ-CUS-137 | Toạ độ không phải số / ngoài khoảng | **Mong đợi:** có thông báo lỗi — nguyên văn chờ `AMB-CUS-28`. Hiện trạng: lưu, không báo |
