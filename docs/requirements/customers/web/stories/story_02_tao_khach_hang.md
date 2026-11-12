# STORY-CUS-02 — Tạo khách hàng

> ← [Index module](../../REQUIREMENTS_CUSTOMERS_SUMMARY.md) · [File web](../requirements_customers_web.md) · REQ `REQ-CUS-28` → `54` (27 REQ). AMB/RISK, phân quyền, trạng thái: xem index.

**User story:** Là staff, tôi muốn tạo khách hàng mới với thông tin công ty, nhóm, tiền tệ, ngôn ngữ và địa chỉ để lập chứng từ cho họ.

## 1. Yêu cầu chức năng

| REQ ID | Tên yêu cầu | Mô tả | Acceptance Criteria | Trạng thái | Cập nhật lần cuối | Nguồn |
|---|---|---|---|---|---|---|
| REQ-CUS-28 | Form tạo mới có 2 tab | `/admin/clients/client` | Tiêu đề tab trình duyệt `Add new customer`; form có 2 tab `Customer Details` (`#contact_info`, mở mặc định) · `Billing & Shipping` (`#billing_and_shipping`); chân form 2 nút `Save and create contact` · `Save` | 🟢 | — | Kiểm chứng thực tế |
| REQ-CUS-29 | Tab Customer Details có 12 field hiển thị | — | Theo thứ tự: `* Company` · `VAT Number` · `Phone` · `Website` · `Groups` (+ nút `+`) · `Currency` · `Default Language` · `Address` · `City` · `State` · `Zip Code` · `Country` — chi tiết ở mục 2 | 🟢 | — | Kiểm chứng thực tế (DOM) |
| REQ-CUS-30 | Company bắt buộc | — | Để trống Company → `Save` → dưới ô hiện `This field is required.` (`#company-error`), ô viền đỏ, **không** có request gửi đi, URL giữ `/admin/clients/client` | 🟢 | — | Kiểm chứng thực tế |
| REQ-CUS-31 | Company chỉ gồm khoảng trắng bị chặn như bỏ trống | Khoảng trắng không phải tên — cùng quy tắc với REQ-CUS-30 | Company = 3 dấu cách → `Save` → **không** tạo KH, ở lại `/admin/clients/client`, dưới ô Company hiện `This field is required.`; danh sách KH không có thêm KH tên rỗng. ⚠️ **Hiện trạng 11-11-2026 lệch:** tạo được KH mới, chuyển `/admin/clients/client/<id>`, Company lưu là chuỗi rỗng, tiêu đề hồ sơ chỉ `#<id>`, tiêu đề tab `Perfex CRM \| Anh Tester Demo` → TC theo REQ này **dự kiến FAIL cho tới khi Dev sửa** — ứng viên bug (`AMB-CUS-01` ✅) | 🟡 | 11-11-2026 · `AMB-CUS-01` | Kiểm chứng thực tế (hiện trạng) + User chốt `AMB-CUS-01` (hành vi mong đợi) |
| REQ-CUS-32 | Company tối đa 191 ký tự, vượt thì bị chặn kèm thông báo | Không cắt dữ liệu âm thầm | (1) Company đúng **191** ký tự → `Save` → tạo KH, giá trị lưu đủ 191 ký tự, không mất ký tự nào · (2) Company **192** ký tự → `Save` → **không** tạo KH, ở lại form, dưới ô Company hiện nguyên văn `Please enter no more than 191 characters.` (câu do QA đề xuất theo uỷ quyền của user 11-11-2026 — cùng giọng với thông báo kiểm hợp lệ sẵn có `This field is required.`). ⚠️ **Hiện trạng 11-11-2026 lệch:** ô không có `maxlength`; nhập 206 ký tự → `Save` thành công, không báo lỗi, giá trị lưu bị cắt còn 191 ký tự đầu → TC vế (2) **dự kiến FAIL cho tới khi Dev sửa** — ứng viên bug (`AMB-CUS-02` ✅) | 🟡 | 11-11-2026 · `AMB-CUS-02` | Kiểm chứng thực tế (hiện trạng) + User chốt `AMB-CUS-02` (giới hạn 191, UI chặn) |
| REQ-CUS-33 | Cảnh báo khi tên công ty đã tồn tại | Kiểm khi rời ô Company | Nhập tên trùng KH có sẵn → rời ô (Tab) → gửi `POST /admin/clients/check_duplicate_customer_name`; dưới ô hiện khung `alert-info` (`#company_exists_info`) nguyên văn `It looks that a customer with name <tên> already exists, if you still want to create the customer you can ignore this message.` | 🟢 | — | Kiểm chứng thực tế + API |
| REQ-CUS-34 | Kiểm trùng không phân biệt hoa thường | — | Tên có sẵn `auto_cus_x` → nhập `AUTO_CUS_X` → rời ô → vẫn hiện cảnh báo (tên trong cảnh báo là chuỗi vừa nhập) | 🟢 | — | Kiểm chứng thực tế |
| REQ-CUS-35 | Tên không trùng không hiện cảnh báo | — | Đổi sang tên chưa tồn tại → rời ô → `#company_exists_info` có class `hide` (không hiển thị) | 🟢 | — | Kiểm chứng thực tế |
| REQ-CUS-36 | Cảnh báo trùng không chặn lưu | — | Đang hiện cảnh báo trùng → `Save` → tạo KH mới thành công (2 KH cùng tên trong danh sách). Cố ý — cảnh báo chỉ để nhắc, không chặn (`AMB-CUS-03` ✅) | 🟢 | 11-11-2026 · ✏️ biên tập | Kiểm chứng thực tế |
| REQ-CUS-37 | VAT Number không kiểm định dạng | — | VAT = `VAT-abc-!@#` → `Save` → lưu nguyên giá trị, không lỗi — cố ý, chấp nhận mọi chuỗi (`AMB-CUS-04` ✅) | 🟢 | 11-11-2026 · ✏️ biên tập | Kiểm chứng thực tế — nhập trên form **sửa** (cùng field với form tạo) |
| REQ-CUS-38 | Phone không kiểm định dạng | — | Phone = `abc-phone` → lưu nguyên giá trị, không lỗi — cố ý, chấp nhận mọi chuỗi (`AMB-CUS-04` ✅) | 🟢 | 11-11-2026 · ✏️ biên tập | Kiểm chứng thực tế — nhập trên form **sửa** (cùng field với form tạo) |
| REQ-CUS-39 | Website không kiểm định dạng | — | Website = `not a url` → lưu nguyên giá trị, không lỗi — cố ý, chấp nhận mọi chuỗi (`AMB-CUS-04` ✅) | 🟢 | 11-11-2026 · ✏️ biên tập | Kiểm chứng thực tế — nhập trên form **sửa** (cùng field với form tạo) |
| REQ-CUS-40 | Groups chọn nhiều nhóm, mặc định trống | — | `select#groups_in[]` có `multiple`; mặc định không option nào được chọn, nút hiển thị `Nothing selected`; option lấy từ danh sách nhóm KH (11-11-2026: 317 option — **biến động**, `RISK-CUS-03`) | 🟢 | — | UI thực tế (DOM) |
| REQ-CUS-41 | Nút `+` mở modal thêm nhóm | — | Bấm `+` cạnh Groups → modal `Add New Customer Group` có 1 field `* Name` (`input#name`) và nút `Close` · `Save`; form gửi tới `/admin/clients/group` | 🟢 | — | Kiểm chứng thực tế |
| REQ-CUS-42 | Tên nhóm bắt buộc | — | Modal nhóm: để trống Name → `Save` → `This field is required.` (`#name-error`), modal vẫn mở | 🟢 | — | Kiểm chứng thực tế |
| REQ-CUS-43 | Tạo nhóm mới từ modal | Tác tạo của REQ-CUS-41 | Nhập Name → `Save` → nhóm mới có trong dropdown Groups và được chọn được. **Chưa kiểm** — tạo master data dùng chung không xoá được (Setup bị chặn — `RISK-CUS-03`) | ⚪ | — | Chưa kiểm chứng |
| REQ-CUS-44 | Currency có 3 lựa chọn, mặc định System Default | — | `select#default_currency` có 3 option `value` `""` · `1` · `2`; nút hiển thị lần lượt `System Default` · `USD` · `EUR`; mặc định `""` (`System Default`). Icon `?` có tooltip nguyên văn `If the customer use other currency then the base currency make sure you select the appropriate currency for this customer. Changing the currency is not possible after transactions are recorded.` | 🟢 | — | UI thực tế (DOM) |
| REQ-CUS-45 | Không đổi được Currency khi KH đã có giao dịch | Theo tooltip REQ-CUS-44 | KH đã có hoá đơn/thanh toán → mở hồ sơ → ô Currency **bị khoá** (không chọn được giá trị khác), giá trị đang lưu giữ nguyên. **Chưa kiểm** — KH test không có giao dịch | ⚪ | 11-11-2026 · ✏️ biên tập | Chưa kiểm chứng — cách chặn chốt theo `AMB-CUS-26` ✅ |
| REQ-CUS-46 | Default Language có 27 lựa chọn | — | `select#default_language` có 27 option: `""` (`System Default`, mặc định) + 26 ngôn ngữ (`english` … `romanian`, có `vietnamese`) | 🟢 | — | UI thực tế (DOM) |
| REQ-CUS-47 | Country có 251 lựa chọn | — | `select#country` có 251 option: 1 option rỗng (mặc định, hiển thị `Nothing selected`) + 250 quốc gia (`1`=`Afghanistan` …; `Vietnam`=`243`) | 🟢 | — | UI thực tế (DOM) |
| REQ-CUS-48 | Save tạo KH và mở hồ sơ | — | Nhập Company hợp lệ → `Save` → chuyển `/admin/clients/client/<id>`, toast nguyên văn `Customer added successfully.`, tiêu đề tab = Company | 🟢 | — | Kiểm chứng thực tế |
| REQ-CUS-49 | Save and create contact mở modal thêm contact | — | Nhập Company → `Save and create contact` → toast `Customer added successfully.`, chuyển `/admin/clients/client/<id>?group=contacts&new_contact=true`, modal `Add new contact` tự mở (nội dung modal thuộc module `CONT`) | 🟢 | — | Kiểm chứng thực tế |
| REQ-CUS-50 | KH mới tạo ở trạng thái Active | — | Tạo KH → tìm trong danh sách → switch Active đang bật (`checked=true`) | 🟢 | — | Kiểm chứng thực tế |
| REQ-CUS-51 | Tab Billing & Shipping gồm 2 khối địa chỉ | — | Khối `Billing Address` và `Shipping Address`, mỗi khối: `Street` (textarea) · `City` · `State` · `Zip Code` · `Country` (251 option). Tiêu đề Shipping có icon `?` tooltip nguyên văn `Do not fill shipping address information if you won't use shipping address on customer invoices` | 🟢 | — | Kiểm chứng thực tế |
| REQ-CUS-52 | Same as Customer Info chép địa chỉ sang Billing | Link cạnh tiêu đề Billing | Điền Address/City/State/Zip/Country ở tab Customer Details → `Same as Customer Info` → Billing Street/City/State/Zip/Country nhận đúng 5 giá trị đó; Shipping không đổi | 🟢 | — | Kiểm chứng thực tế — thao tác trên form **sửa** (hồ sơ KH); form tạo có cùng 2 link (DOM) |
| REQ-CUS-53 | Copy Billing Address chép Billing sang Shipping | Link cạnh tiêu đề Shipping | `Copy Billing Address` → 5 field Shipping nhận đúng giá trị Billing | 🟢 | — | Kiểm chứng thực tế — thao tác trên form **sửa** (hồ sơ KH); form tạo có cùng 2 link (DOM) |
| REQ-CUS-54 | Địa chỉ Billing/Shipping được lưu | — | Sau REQ-CUS-52/53 → `Save` → tải lại hồ sơ → 10 field Billing/Shipping giữ đúng giá trị | 🟢 | — | Kiểm chứng thực tế — thao tác trên form **sửa** (hồ sơ KH); form tạo có cùng 2 link (DOM) |

## 2. Đặc tả trường dữ liệu

| Field (Label) | Loại UI · `name` | Required | Ràng buộc / mặc định | REQ liên quan | Ghi chú |
|---|---|---|---|---|---|
| Company | text · `company` | ✅ (client) | **Mong đợi:** tối đa 191 ký tự, vượt → chặn (REQ-32); chỉ khoảng trắng = trống (REQ-31). **Hiện trạng:** không `maxlength`, server cắt còn 191; trim → khoảng trắng thành rỗng và vẫn lưu | 30 → 36 | Kiểm trùng khi rời ô |
| VAT Number | text · `vat` | — | Không ràng buộc | 37 | |
| Phone | text · `phonenumber` | — | Không ràng buộc | 38 | |
| Website | text · `website` | — | Không ràng buộc | 39 | |
| Groups | multi-select · `groups_in[]` | — | Mặc định rỗng | 40 → 43 | Nút `+` → modal nhóm |
| Currency | select · `default_currency` | — | `""`=System Default* · `1`=USD · `2`=EUR | 44 · 45 | |
| Default Language | select · `default_language` | — | 27 option, `""`=System Default* | 46 | |
| Address | textarea · `address` | — | — | 52 | |
| City · State · Zip Code | text · `city` · `state` · `zip` | — | — | 52 | |
| Country | select · `country` | — | 251 option, mặc định rỗng | 47 | |
| Billing Street/City/State/Zip/Country | `billing_street` · `billing_city` · `billing_state` · `billing_zip` · `billing_country` | — | — | 51 → 54 | |
| Shipping Street/City/State/Zip/Country | `shipping_*` tương ứng | — | — | 51 → 54 | |
| *(ẩn)* Show primary contact full name on Invoices, Estimates, Payments, Credit Notes | checkbox · `show_primary_contact`, `value=1` | — | Có trong DOM, **không hiển thị** ở KH chưa có contact | — | Chỉ hiện khi KH có primary contact — kiểm khi recon `CONT` (`AMB-CUS-05` ✅) |
| Modal nhóm — Name | text · `name` | ✅ (client) | — | 41 · 42 | |

## 3. Validation messages

| REQ ID | Rule / Trigger | Thông báo nguyên văn |
|---|---|---|
| REQ-CUS-30 | Company trống | `This field is required.` |
| REQ-CUS-31 | Company chỉ gồm khoảng trắng | `This field is required.` — **mong đợi**, hiện trạng không báo gì và vẫn lưu |
| REQ-CUS-32 | Company > 191 ký tự | `Please enter no more than 191 characters.` — **mong đợi** (QA đề xuất, user uỷ quyền 11-11-2026), hiện trạng không báo gì và cắt dữ liệu |
| REQ-CUS-33 | Company trùng (rời ô) | `It looks that a customer with name <tên> already exists, if you still want to create the customer you can ignore this message.` |
| REQ-CUS-42 | Tên nhóm trống | `This field is required.` |
| REQ-CUS-48 · 49 | Tạo thành công | `Customer added successfully.` |

## 4. Luồng chính

1. `New Customer` → nhập Company (+ field tuỳ chọn) → (tuỳ chọn) tab Billing & Shipping → `Same as Customer Info` → `Copy Billing Address`
2. `Save` → hồ sơ KH · hoặc `Save and create contact` → tab Contacts + modal thêm contact
