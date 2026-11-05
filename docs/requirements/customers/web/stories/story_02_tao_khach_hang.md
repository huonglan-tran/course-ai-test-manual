# STORY-CUS-02 — Tạo khách hàng

> ← [Index module `REQUIREMENTS_CUSTOMERS_SUMMARY.md`](../../REQUIREMENTS_CUSTOMERS_SUMMARY.md) · [File nền tảng Web](../requirements_customers_web.md)
>
> Phạm vi: `REQ-CUS-17` → `REQ-CUS-37` (21 REQ) · AMB liên quan: `AMB-CUS-01` · `02` · `03` · `04` · `17` · RISK liên quan: `RISK-CUS-08`

**Route:** `/admin/clients/client` (tiêu đề tab `Add new customer`) · Submit: `POST /admin/clients/client` → `303` · Kiểm trùng tên: `POST /admin/clients/check_duplicate_customer_name`.

## 1. Yêu cầu chức năng

| REQ ID | Tên yêu cầu | Mô tả | Acceptance Criteria | Trạng thái | Cập nhật lần cuối | Nguồn |
|---|---|---|---|---|---|---|
| REQ-CUS-17 | Form tạo KH có 2 tab Customer Details và Billing & Shipping | Bố cục form tạo mới | Mở `/admin/clients/client` → `document.title` = `Add new customer`; 2 tab: `Customer Details` (`#contact_info`, active) · `Billing & Shipping` (`#billing_and_shipping`); cuối form 2 nút `Save and create contact` · `Save` | 🟢 | — | UI thực tế |
| REQ-CUS-18 | Ô Company được focus sẵn khi mở form tạo | Nhập được ngay | Mở form → `document.activeElement.id === "company"` | 🟢 | — | UI thực tế |
| REQ-CUS-19 | Bỏ trống Company bị chặn tại trình duyệt kèm thông báo bắt buộc | Company là trường bắt buộc (nhãn có `*`) | Để trống Company, bấm `Save` → **không** có request `POST` nào được gửi; xuất hiện `p#company-error` nguyên văn `This field is required.`; nhóm field mang class `has-error`. Ô **không** có thuộc tính `required` (form `novalidate`) — validation do JS ứng dụng | 🟢 | — | Kiểm chứng thực tế (đọc DOM — ảnh chụp lệch vị trí cuộn, đã loại) |
| REQ-CUS-20 | Company chỉ gồm khoảng trắng: máy chủ cắt khoảng trắng và vẫn tạo KH | Hành vi **hiện tại** — nghi lỗi | Nhập `"   "` (3 dấu cách) vào Company, bấm `Save` → **không** có thông báo lỗi; `POST /admin/clients/client` → `303` tới `/admin/clients/client/<id>`; ô Company của hồ sơ có giá trị `""`; heading hồ sơ chỉ hiện `#<id>`; `document.title` không chứa tên KH. ⚠️ Mô tả hành vi quan sát, **chưa** phải hành vi mong đợi — TC viết theo kết luận `AMB-CUS-01` 🔴 | 🟢 | — | Kiểm chứng thực tế · API `POST /admin/clients/client` · 303 |
| REQ-CUS-21 | Company dài hơn 191 ký tự bị cắt còn 191 ký tự, không báo lỗi | Hành vi **hiện tại** — nghi lỗi | Ô Company **không** có `maxlength` (nhập đủ 300 ký tự). Lưu Company 300 ký tự → `303` tạo KH; giá trị Company của hồ sơ dài **191** ký tự (300 ký tự đầu bị cắt phần đuôi); không có thông báo nào. TC viết theo kết luận `AMB-CUS-02` 🔴 | 🟢 | — | Kiểm chứng thực tế · API `POST /admin/clients/client` · 303 |
| REQ-CUS-22 | Rời ô Company với tên đã tồn tại hiện cảnh báo trùng tên | Gợi ý tránh tạo trùng KH | Nhập tên **trùng** một KH đã có (dùng KH tự tạo trước đó) rồi chuyển focus sang ô khác → `POST /admin/clients/check_duplicate_customer_name` (body `company`) trả JSON `exists: true`; dưới ô Company hiện `.alert.alert-info` nguyên văn `It looks that a customer with name <tên> already exists, if you still want to create the customer you can ignore this message.` | 🟢 | — | Kiểm chứng thực tế · API `POST .../check_duplicate_customer_name` · 200 |
| REQ-CUS-23 | Kiểm trùng tên không phân biệt hoa thường | | Tên KH đã có viết **IN HOA** toàn bộ → response `exists: true`, cảnh báo REQ-CUS-22 hiện | 🟢 | — | Kiểm chứng thực tế |
| REQ-CUS-24 | Kiểm trùng tên bỏ qua khoảng trắng đầu và cuối | | Tên KH đã có, thêm 2 dấu cách ở đầu và 2 ở cuối → response `exists: true`, cảnh báo hiện | 🟢 | — | Kiểm chứng thực tế |
| REQ-CUS-25 | Tên chưa tồn tại không hiện cảnh báo trùng | | Tên mới dạng `auto_cus_unique_<timestamp>` → response `exists: false`; không còn `.alert.alert-info` nào hiển thị (cảnh báo của lần nhập trước biến mất) | 🟢 | — | Kiểm chứng thực tế |
| REQ-CUS-26 | Cảnh báo trùng tên không chặn việc lưu KH | Theo câu chữ của cảnh báo | Chưa lưu thử với tên trùng — chỉ suy từ câu *"if you still want to create the customer you can ignore this message"* (`AMB-CUS-03`) | ⚪ | — | UI thực tế — chưa kiểm chứng |
| REQ-CUS-27 | Ô Groups chọn nhiều nhóm, có tìm kiếm và chọn/bỏ tất cả | | `select#groups_in[]` `multiple`, bootstrap-select có ô tìm kiếm (`data-live-search=true`) và 2 nút `Select All` · `Deselect All`; mặc định không chọn nhóm nào (nút hiện `Nothing selected`). **Không** assert số option (master data dùng chung — `RISK-CUS-08`) | 🟢 | — | UI thực tế |
| REQ-CUS-28 | Nút + cạnh Groups mở modal tạo nhóm, Name là bắt buộc | Tạo nhóm KH ngay trên form | Bấm nút `+` (`data-target=#customer_group_modal`) → modal `Add New Customer Group` có 1 ô `* Name` và nút `Close` · `Save`. Bấm `Save` khi trống → không gửi request, hiện `This field is required.`. Chưa tạo nhóm thật (master data dùng chung) | 🟢 | — | Kiểm chứng thực tế |
| REQ-CUS-29 | Currency có 3 lựa chọn, mặc định System Default, kèm cảnh báo đổi tiền tệ | | `select#default_currency` 3 option value:text: `:` (hiển thị `System Default`) · `1:USD` · `2:EUR`; mặc định option rỗng. Icon `?` cạnh nhãn có tooltip nguyên văn `If the customer use other currency then the base currency make sure you select the appropriate currency for this customer. Changing the currency is not possible after transactions are recorded.` | 🟢 | — | UI thực tế |
| REQ-CUS-30 | Default Language có 27 lựa chọn, mặc định System Default | | `select#default_language` 27 option, option đầu value rỗng text `System Default` đang `selected`; 26 ngôn ngữ còn lại value chữ thường (VD `english`, `vietnamese`) | 🟢 | — | UI thực tế |
| REQ-CUS-31 | Ba ô Country có 251 lựa chọn, mặc định chưa chọn | Country của địa chỉ chính, Billing, Shipping | `#country` · `#billing_country` · `#shipping_country` mỗi ô 251 option (option đầu value rỗng); mặc định hiển thị `Nothing selected`; có ô tìm kiếm | 🟢 | — | UI thực tế |
| REQ-CUS-32 | Link Same as Customer Info chép địa chỉ chính sang Billing | | Điền Address · City · State · Zip Code · Country ở tab Customer Details → tab Billing & Shipping → bấm `Same as Customer Info` → 5 ô Billing (Street · City · State · Zip Code · Country) có đúng 5 giá trị đó | 🟢 | — | Kiểm chứng thực tế |
| REQ-CUS-33 | Link Copy Billing Address chép Billing sang Shipping | | Sau REQ-CUS-32, bấm `Copy Billing Address` → 5 ô Shipping bằng đúng 5 ô Billing | 🟢 | — | Kiểm chứng thực tế |
| REQ-CUS-34 | Lưu KH hợp lệ chuyển tới hồ sơ KH kèm thông báo thành công | | Company hợp lệ `auto_cus_<timestamp>` → `Save` → `POST /admin/clients/client` trả `303`, `Location` = `/admin/clients/client/<id mới>`; trang hồ sơ có `document.title` = tên Company; thông báo nổi nguyên văn `Customer added successfully.` | 🟢 | — | Kiểm chứng thực tế · API `POST /admin/clients/client` · 303 |
| REQ-CUS-35 | KH mới tạo mặc định ở trạng thái Active | | Tạo KH → tìm KH đó ở danh sách → switch `Active` của dòng đang `checked`; bộ đếm `Active Customers` tăng | 🟢 | — | Kiểm chứng thực tế |
| REQ-CUS-36 | Save and create contact tạo KH rồi mở form thêm contact | | Company hợp lệ → `Save and create contact` → payload có thêm `save_and_add_contact=true`; `303` tới `/admin/clients/client/<id>?group=contacts&new_contact=true`; modal `#contact` tiêu đề `Add new contact` tự mở. Nội dung form contact thuộc module `CONT` | 🟢 | — | Kiểm chứng thực tế · API `POST /admin/clients/client` · 303 (ảnh chụp giữa hiệu ứng mở modal, đã loại) |
| REQ-CUS-37 | Rời form tạo KH đang nhập dở bị trình duyệt hỏi xác nhận | Tránh mất dữ liệu đang nhập | Nhập ít nhất 1 ô rồi điều hướng sang URL khác → xuất hiện hộp thoại loại `beforeunload`. Nội dung hộp thoại do **trình duyệt** sinh (Chrome để trống message) — **cấm** assert câu chữ, chỉ assert loại hộp thoại; huỷ thì ở lại form | 🟢 | — | Kiểm chứng thực tế |

## 2. Đặc tả trường dữ liệu — form tạo / sửa KH

> Đọc DOM 04-11-2026. Không ô nào có `required` / `maxlength` / `pattern` ở mức HTML — validation duy nhất là Company bắt buộc (JS). Cột `name` là tên field gửi đi trong payload `POST /admin/clients/client`.

| Field (Label) | `name` | Tab | Loại UI | Required | Ràng buộc / mặc định | REQ liên quan | Ghi chú |
|---|---|---|---|---|---|---|---|
| Company | `company` | Customer Details | text | ✅ (`*`) | Không `maxlength`; máy chủ cắt còn 191 ký tự; cắt khoảng trắng đầu/cuối | 18 · 19 · 20 · 21 · 22 | `AMB-CUS-01` · `02` |
| VAT Number | `vat` | Customer Details | text | — | Không ràng buộc định dạng | — | `AMB-CUS-17` |
| Phone | `phonenumber` | Customer Details | text | — | Không ràng buộc định dạng | — | `AMB-CUS-17` |
| Website | `website` | Customer Details | text | — | Không ràng buộc định dạng; lưu `auto.test` được | 44 | `AMB-CUS-17` |
| Groups | `groups_in[]` | Customer Details | multi-select | — | Không chọn | 27 · 28 | |
| Currency | `default_currency` | Customer Details | select | — | `""` = System Default · `1` USD · `2` EUR | 29 | Master data ở Setup (`AMB-SYS-01`) |
| Default Language | `default_language` | Customer Details | select | — | `""` = System Default, 27 option | 30 | |
| Address | `address` | Customer Details | textarea | — | — | 32 | |
| City · State · Zip Code | `city` · `state` · `zip` | Customer Details | text | — | — | 32 | |
| Country | `country` | Customer Details | select | — | 251 option, mặc định rỗng | 31 · 32 | |
| Street · City · State · Zip Code · Country | `billing_street` · `billing_city` · `billing_state` · `billing_zip` · `billing_country` | Billing & Shipping | textarea/text/select | — | — | 31 · 32 · 33 | |
| Street · City · State · Zip Code · Country | `shipping_street` · `shipping_city` · `shipping_state` · `shipping_zip` · `shipping_country` | Billing & Shipping | textarea/text/select | — | — | 31 · 33 | Icon `?` cạnh tiêu đề Shipping Address |
| Show primary contact full name on Invoices, Estimates, Payments, Credit Notes | `show_primary_contact` | Customer Details | checkbox | — | Không tick; **ẩn** (`offsetParent = null`) ở form tạo và form sửa của KH chưa có contact | — | `AMB-CUS-04` |
| *(ẩn)* | `csrf_token_name` | — | hidden | — | `<32 ký tự hex>` | — | Không ghi giá trị |
| *(ẩn, chỉ khi bấm Save and create contact)* | `save_and_add_contact` | — | — | — | `true` | 36 | |

## 3. Validation & thông báo

| REQ ID | Rule / Trigger | Thông báo (nguyên văn) |
|---|---|---|
| REQ-CUS-19 | Company trống, bấm Save | `This field is required.` |
| REQ-CUS-20 | Company chỉ khoảng trắng | *(không có thông báo — KH vẫn được tạo)* |
| REQ-CUS-21 | Company > 191 ký tự | *(không có thông báo — giá trị bị cắt)* |
| REQ-CUS-22 | Company trùng tên KH đã có (blur) | `It looks that a customer with name <tên> already exists, if you still want to create the customer you can ignore this message.` |
| REQ-CUS-28 | Name của Customer Group trống | `This field is required.` |
| REQ-CUS-34 | Lưu thành công | `Customer added successfully.` |

## 4. Luồng xử lý

```
New Customer ─→ /admin/clients/client (focus Company)
   ├─ blur Company ─→ POST check_duplicate_customer_name ─→ cảnh báo (không chặn)
   ├─ tab Billing & Shipping ─→ Same as Customer Info ─→ Copy Billing Address
   ├─ Save ──────────────────────→ 303 /admin/clients/client/<id>                         (REQ-34)
   └─ Save and create contact ──→ 303 /admin/clients/client/<id>?group=contacts&new_contact=true (REQ-36)
```
