# STORY-CUS-03 — Hồ sơ KH, sửa thông tin & Customer Admins

> ← [Index module `REQUIREMENTS_CUSTOMERS_SUMMARY.md`](../../REQUIREMENTS_CUSTOMERS_SUMMARY.md) · [File nền tảng Web](../requirements_customers_web.md)
>
> Phạm vi: `REQ-CUS-38` → `REQ-CUS-52` (15 REQ) · AMB liên quan: — · RISK liên quan: `RISK-CUS-07`

**Route:** `/admin/clients/client/<id>?group=<tab>` · Sửa: `POST /admin/clients/client/<id>` → `303` · Gán admin: `POST /admin/clients/assign_admins/<id>` · Gỡ admin: `GET /admin/clients/delete_customer_admin/<id KH>/<id staff>`.

## 1. Yêu cầu chức năng

| REQ ID | Tên yêu cầu | Mô tả | Acceptance Criteria | Trạng thái | Cập nhật lần cuối | Nguồn |
|---|---|---|---|---|---|---|
| REQ-CUS-38 | Hồ sơ KH có menu 19 tab, mỗi tab mở bằng tham số group | Điều hướng trong hồ sơ | Menu trái gồm 19 link theo thứ tự, href `?group=<giá trị>`: Profile `profile` · Contacts `contacts` · Notes `notes` · Statement `statement` · Invoices `invoices` · Payments `payments` · Proposals `proposals` · Credit Notes `credit_notes` · Estimates `estimates` · Subscriptions `subscriptions` · Expenses `expenses` · Contracts `contracts` · Projects `projects` · Tasks `tasks` · Tickets `tickets` · Files **`attachments`** · Vault `vault` · Reminders `reminders` · Map `map` | 🟢 | — | UI thực tế |
| REQ-CUS-39 | Tiêu đề hồ sơ hiển thị mã và tên KH | | Heading đầu hồ sơ **chứa** `#<id>` và tên Company (tên dài bị rút gọn bằng `...` trên giao diện — assert bằng `chứa` phần đầu tên hoặc đọc `title`); `document.title` = tên Company. Mũi tên cạnh heading mở menu chứa `Delete` | 🟢 | — | Kiểm chứng thực tế |
| REQ-CUS-40 | Tab Profile có 3 tab con | | Tab Profile của KH đã tạo có 3 tab con: `Customer Details` · `Billing & Shipping` · `Customer Admins` (form tạo mới chỉ có 2 tab đầu — REQ-CUS-17) | 🟢 | — | UI thực tế |
| REQ-CUS-41 | Sửa KH với Company trống bị chặn kèm thông báo bắt buộc | | Xoá trắng Company trên form sửa → `Save` → không gửi `POST`; hiện `p#company-error` nguyên văn `This field is required.` | 🟢 | — | Kiểm chứng thực tế |
| REQ-CUS-42 | Sửa KH hợp lệ lưu giá trị mới kèm thông báo thành công | | Đổi `Phone` của KH tự tạo → `Save` → `POST /admin/clients/client/<id>` trả `303` về chính hồ sơ; thông báo nổi nguyên văn `Customer updated successfully.`; ô Phone hiển thị giá trị mới sau khi tải lại | 🟢 | — | Kiểm chứng thực tế · API `POST /admin/clients/client/<id>` · 303 |
| REQ-CUS-43 | Form sửa KH chỉ có nút Save | Không có lối tắt tạo contact khi sửa | Form Profile của KH đã tạo có đúng 1 nút submit hiển thị `Save` (`button.only-save`); **không** có `Save and create contact` | 🟢 | — | UI thực tế |
| REQ-CUS-44 | Nút quả địa cầu cạnh Website mở website ở tab mới, tự thêm http:// | | KH có Website `auto.test` → cạnh ô Website có nút icon `fa-globe` là `<a href="http://auto.test" target="_blank">`. Chưa bấm thử | 🟢 | — | UI thực tế |
| REQ-CUS-45 | Hồ sơ KH không hoạt động hiện banner cảnh báo kèm link kích hoạt | | KH có Active tắt → mọi tab hồ sơ hiện `.alert` nguyên văn `This is inactive customer profile and some features may be disabled` kèm link `Mark as active` → `/admin/clients/mark_as_active/<id>` | 🟢 | — | Kiểm chứng thực tế |
| REQ-CUS-46 | Link Mark as active kích hoạt lại KH | | Chưa bấm thử — chưa biết có hỏi xác nhận không, banner có biến mất không | ⚪ | — | UI thực tế — chưa kiểm chứng |
| REQ-CUS-47 | Mở hồ sơ KH không tồn tại trả trang 404 | KH đã xoá | Mở `/admin/clients/client/<id của KH vừa xoá>` → HTTP `404`, `document.title` = `404 Page Not Found`, nội dung **chứa** `The page you requested was not found.` | 🟢 | — | Kiểm chứng thực tế |
| REQ-CUS-48 | Modal Assign Admin cho chọn nhiều staff | | Tab `Customer Admins` → `Assign Admin` → modal tiêu đề `Assign Admin` có `select[name="customer_admins[]"]` `multiple`, mặc định không chọn (`Nothing selected`); nút `Close` · `Save`. **Không** assert số/tên staff | 🟢 | — | Kiểm chứng thực tế |
| REQ-CUS-49 | Gán admin hiển thị staff trong bảng Customer Admins | | Chọn 1 staff → `Save` → `POST /admin/clients/assign_admins/<id>` → tải lại trang `?tab=customer_admins`; thông báo `Customer updated successfully.`; bảng (cột `Staff Member` · `Date Assigned` · `Options`) có 1 dòng, tên staff là link tới `/admin/profile/<id staff>` | 🟢 | — | Kiểm chứng thực tế |
| REQ-CUS-50 | Gỡ admin cần xác nhận rồi xoá khỏi bảng | | Bấm icon xoá ở dòng admin (href `/admin/clients/delete_customer_admin/<id KH>/<id staff>`, class `_delete`) → hộp `confirm` của trình duyệt (nội dung ứng dụng truyền vào: `Are you sure you want to perform this action?`) → đồng ý → bảng `No entries found` | 🟢 | — | Kiểm chứng thực tế |
| REQ-CUS-51 | Tab Invoices trong hồ sơ chỉ nạp hoá đơn của KH đang xem | Lọc chứng từ theo KH (`RISK-SYS-04`) | Mở `?group=invoices` → bảng nạp bằng `POST /admin/invoices/table/<id KH>` (id KH nằm trên đường dẫn). Nội dung cột, thao tác của bảng thuộc module `INV` | 🟢 | — | Kiểm chứng thực tế · API `POST /admin/invoices/table/<id>` · 200 |
| REQ-CUS-52 | Các tab chứng từ còn lại trong hồ sơ chỉ hiển thị dữ liệu của KH đang xem | Payments · Proposals · Credit Notes · Estimates · Subscriptions · Expenses · Contracts · Projects · Tasks · Tickets | Chưa mở từng tab — dự kiến cùng mẫu REQ-CUS-51; mỗi tab kiểm khi recon module chứng từ tương ứng | ⚪ | — | Suy theo mẫu REQ-CUS-51 — chưa kiểm chứng |

## 2. Đặc tả trường dữ liệu

Form sửa dùng **cùng bộ field** với form tạo — xem [STORY-CUS-02 mục 2](story_02_tao_khach_hang.md#2-đặc-tả-trường-dữ-liệu--form-tạo--sửa-kh). Khác biệt: có thêm tab con `Customer Admins`; chỉ có nút `Save`.

| Field (Label) | Loại UI | Required | Ràng buộc / mặc định | REQ liên quan |
|---|---|---|---|---|
| Assign Admin — staff | `select[name="customer_admins[]"]` multiple | — | Không chọn | 48 · 49 |

## 3. Validation & thông báo

| REQ ID | Rule / Trigger | Thông báo (nguyên văn) |
|---|---|---|
| REQ-CUS-41 | Company trống khi sửa | `This field is required.` |
| REQ-CUS-42 · 49 | Sửa / gán admin thành công | `Customer updated successfully.` |
| REQ-CUS-45 | KH không hoạt động | `This is inactive customer profile and some features may be disabled` · link `Mark as active` |
| REQ-CUS-47 | Hồ sơ không tồn tại | `404 Page Not Found` · `The page you requested was not found.` |
| REQ-CUS-50 | Gỡ admin | `Are you sure you want to perform this action?` (hộp `confirm` gốc của trình duyệt — chỉ assert có hộp thoại) |

## 4. Luồng xử lý

```
Hồ sơ /admin/clients/client/<id>
   ├─ Profile ─→ Customer Details / Billing & Shipping ─→ Save ─→ 303 + "Customer updated successfully."
   ├─ Profile ─→ Customer Admins ─→ Assign Admin ─→ Save ─→ bảng admin ─→ xoá (confirm)
   └─ Tab chứng từ ─→ POST /admin/<chứng từ>/table/<id KH>
```
