# Customers — Checklist Smoke (19 mục · ~15 phút)

**Loại:** Smoke — xác nhận build chạy được, luồng sống còn của module Customers không vỡ; lấy mục từ **Vòng 1** (giao diện cơ bản · mở form · hiển thị · nhập hợp lệ · lưu · đối chiếu dữ liệu). Ngoại lệ có chủ ý: mục 19 (xoá KH) lấy từ Vòng 2 vì là luồng **mất dữ liệu không hồi lại được** — luôn có mặt dù loại checklist nào
**Nguồn:** TC-based — [`TEST_CASES_CUSTOMERS_SUMMARY.md`](../../../testcases/customers/TEST_CASES_CUSTOMERS_SUMMARY.md) → [`part_01_web_smoke.md`](../../../testcases/customers/web/parts/part_01_web_smoke.md) (`CRM_CUS_TC_001` → `019`) · mục 19 từ [`part_03_web_ho_so_active_bulk_import.md`](../../../testcases/customers/web/parts/part_03_web_ho_so_active_bulk_import.md) (`079` · `080`)
**Requirements:** [`REQUIREMENTS_CUSTOMERS_SUMMARY.md`](../../../requirements/customers/REQUIREMENTS_CUSTOMERS_SUMMARY.md) (bản 2.1)
**Scope:** Perfex CRM khu Admin · module `CUS` — Customers · nền tảng Web · role Admin · lập 12-11-2026
**Mã checklist:** `CL-smoke_20261112` — theo tên file `checklist_smoke_20261112.md`. Mã mục khi chạy = `CL-smoke_20261112-<số mục 2 chữ số>` (VD mục 5 → `CL-smoke_20261112-05`, mục 19 → `CL-smoke_20261112-19`)

## Chuẩn bị trước khi chạy

| Hạng mục | Yêu cầu |
|---|---|
| Trình duyệt | Google Chrome, cửa sổ desktop rộng ≥ 1600px, giao diện tiếng Anh (`RISK-CUS-09`) |
| Địa chỉ | Mọi đường dẫn tính trên `BASE_URL` trong `.env` |
| Tài khoản | Admin — `EMAIL_ADMIN` · `PASSWORD_ADMIN` trong `.env`, **đăng nhập sẵn** trước mục 1. 🔒 Tự nhập, không ghi giá trị vào báo cáo |
| Dữ liệu test | Tên KH dạng `auto_cus_smoke_20261112_<số mục>` — `20261112` là **ngày chạy** (`YYYYMMDD`); chạy ngày khác thì thay. Prefix chung `auto_cus_smoke_<ngày chạy>` dùng để dọn dữ liệu ở cuối |
| KH dùng chung giữa các mục | **KH A** = tạo ở mục 5 (chỉ có Company, chưa có contact · giao dịch · note · tệp · vault) — các mục 9 → 16 dùng lại KH này. **KH B** = tạo ở mục 6 (đủ 12 field) — mục 7 dùng |
| Môi trường dùng chung | Bộ đếm Summary và dữ liệu nền biến động theo người khác — **chỉ đối chiếu định dạng**, không đối chiếu con số tuyệt đối (`RISK-CUS-01`) |
| Thứ tự | Chạy **đúng thứ tự**: mục 5 tạo KH A **trước** mục 9 → 16; mục 16 (sửa KH A) chạy **sau** mục 15; mục 19 (xoá KH A) chạy **cuối cùng** |
| Việc cấm | Không bấm `Import` / `Simulate Import` / `Download Sample` (mục 17), không bấm `Save` ở hộp thoại nào ngoài mục 5 → 8, 14 → 16 đã nêu |

### Nhóm: Danh sách khách hàng

| # | Trạng thái | Hạng mục kiểm tra | Kết quả kỳ vọng | Priority | REQ ID | TC ID liên quan |
|---|---|---|---|---|---|---|
| 1 | ✅ | Bấm `Customers` ở menu trái, đối chiếu khối Summary và thanh nút đầu trang | Mở `/admin/clients`; khối `Customers Summary` có 6 số theo thứ tự `Total Customers` · `Active Customers` · `Inactive Customers` · `Active Contacts` · `Inactive Contacts` · `Contacts Logged In Today`, số **không** có dấu phẩy ngăn nghìn; `Inactive Customers` và `Inactive Contacts` chữ đỏ; đầu trang 3 nút `New Customer` (xanh, dấu `+`) · `Import Customers` (xanh) · `Contacts` (trắng), góc phải nút phễu | P1 | REQ-CUS-01, REQ-CUS-05 | CRM_CUS_TC_001 |
| 2 | ✅ | Đối chiếu thanh công cụ và bảng KH, không gõ gì | Thanh trên bảng: ô số dòng `25` · `Export` · `Bulk Actions` · nút tải lại · ô tìm kiếm chữ mờ `Search...`; cột theo thứ tự: ô tích · `#` · `Company` · `Primary Contact` · `Primary Email` · `Phone` · `Active` · `Groups` · `Date Created`; `Company` có mũi tên sắp xếp hướng lên, tên xếp tăng dần; 25 dòng; dưới bảng `Showing 1 to 25 of <N> entries` (N có dấu phẩy, không có `filtered from`); `Previous` mờ, trang `1` tô xanh, có `Next` | P2 | REQ-CUS-06, REQ-CUS-08, REQ-CUS-09, REQ-CUS-10, REQ-CUS-11 | CRM_CUS_TC_002 |

### Nhóm: Form tạo khách hàng

| # | Trạng thái | Hạng mục kiểm tra | Kết quả kỳ vọng | Priority | REQ ID | TC ID liên quan |
|---|---|---|---|---|---|---|
| 3 | ✅ | Bấm `New Customer`, đối chiếu tab `Customer Details`, không gõ gì | Mở `/admin/clients/client`, tiêu đề tab trình duyệt `Add new customer`; 2 tab `Customer Details` (đang chọn) · `Billing & Shipping`; từ trên xuống `* Company` (dấu `*` đỏ) · `VAT Number` · `Phone` · `Website` · `Groups` (nút `+`) · `Currency` (icon `?`) và `Default Language` cùng hàng · `Address` · `City` · `State` · `Zip Code` · `Country`; con trỏ nằm sẵn trong ô Company, mọi ô trống; mặc định `Nothing selected` (Groups, Country), `System Default` (Currency, Default Language); chân form `Save and create contact` (trắng) · `Save` (xanh) | P2 | REQ-CUS-28, REQ-CUS-29, REQ-CUS-40, REQ-CUS-44, REQ-CUS-46, REQ-CUS-47 | CRM_CUS_TC_003 |
| 4 | ✅ | Bấm tab `Billing & Shipping`, rê chuột lên icon `?` cạnh `Shipping Address` | Tab chữ xanh, gạch chân; tooltip nguyên văn `Do not fill shipping address information if you won't use shipping address on customer invoices`; 2 cột `Billing Address` (liên kết `Same as Customer Info`) và `Shipping Address` (liên kết `Copy Billing Address`), mỗi cột `Street` · `City` · `State` · `Zip Code` · `Country`. ⚠️ Chưa có evidence trạng thái rỗng của form tạo — ghi lại nếu 8 ô chữ không trống hoặc Country không hiện `Nothing selected` (`@NeedsVerify`) | P3 | REQ-CUS-51 | CRM_CUS_TC_004 |

### Nhóm: Tạo, lưu và đối chiếu khách hàng

| # | Trạng thái | Hạng mục kiểm tra | Kết quả kỳ vọng | Priority | REQ ID | TC ID liên quan |
|---|---|---|---|---|---|---|
| 5 | ✅ | Tạo **KH A**: mở form tạo, chỉ nhập Company `auto_cus_smoke_20261112_05`, bấm `Save` | Chuyển sang `/admin/clients/client/<id>` (id là số); thông báo nguyên văn `Customer added successfully.`; tiêu đề tab trình duyệt `auto_cus_smoke_20261112_05`; tiêu đề trái `#<id> auto_cus_smoke_20261112_05`. *Ghi lại `<id>` để dùng ở mục 19* | P1 | REQ-CUS-48 | CRM_CUS_TC_006 |
| 6 | ✅ | Tạo **KH B**: mở form tạo, nhập 12 field theo ghi chú dưới bảng, bấm `Save` | Chuyển sang `/admin/clients/client/<id>`, thông báo `Customer added successfully.`; ở tab `Profile` → `Customer Details`, cả 12 field hiện đúng giá trị đã nhập, không mất ký tự nào | P1 | REQ-CUS-48, REQ-CUS-29 | CRM_CUS_TC_007 |
| 7 | ✅ | Bấm `Customers` ở menu trái, gõ `auto_cus_smoke_20261112_06` vào ô `Search...`, đối chiếu dòng KH B | Còn đúng 1 dòng; dưới bảng `Showing 1 to 1 of 1 entries (filtered from <N> total entries)`; `Company` là liên kết chữ xanh; `Active` là công tắc **bật**; `Groups` nhãn `customer group b`; `Phone` `0912345678`; `Date Created` dạng `DD-MM-YYYY HH:mm:ss` đúng ngày chạy (VD `12-11-2026 14:18:49`) | P1 | REQ-CUS-50, REQ-CUS-12, REQ-CUS-18, REQ-CUS-19 | CRM_CUS_TC_008 |
| 8 | ✅ | Mở form tạo, nhập Company `auto_cus_smoke_20261112_08`, bấm `Save and create contact`, rồi đóng hộp thoại bằng `×` | Thông báo `Customer added successfully.`; thanh địa chỉ `/admin/clients/client/<id>?group=contacts&new_contact=true`; menu trái `Contacts` đang chọn; hộp thoại `Add new contact` tự mở (không đối chiếu nội dung hộp thoại); sau `×` hộp đóng, KH vẫn còn (tiêu đề trái `#<id> auto_cus_smoke_20261112_08`) | P2 | REQ-CUS-49 | CRM_CUS_TC_009 |

> **Dữ liệu mục 6 — KH B:** Company `auto_cus_smoke_20261112_06` · VAT Number `0101234567` · Phone `0912345678` · Website `https://auto-cus-smoke.test` · Groups `customer group b` (không có → chọn nhóm khác, ghi tên nhóm vào báo cáo, `ASM-CUS-07`) · Currency `USD` · Default Language `Vietnamese` · Address `12 Auto Street` · City `AutoCity` · State `AutoState` · Zip Code `70000` · Country `Vietnam`. *Nếu đổi nhóm ở mục 6, mục 7 đối chiếu nhãn nhóm đã chọn.*

### Nhóm: Hồ sơ khách hàng và các tab con (dùng KH A)

| # | Trạng thái | Hạng mục kiểm tra | Kết quả kỳ vọng | Priority | REQ ID | TC ID liên quan |
|---|---|---|---|---|---|---|
| 9 | ✅ | Mở hồ sơ KH A từ danh sách, đối chiếu menu trái và tiêu đề; bấm mũi tên `⌄` cạnh tiêu đề | `Profile` đang chọn (chữ xanh); menu trái đủ 19 mục theo thứ tự: `Profile` · `Contacts` · `Notes` · `Statement` · `Invoices` · `Payments` · `Proposals` · `Credit Notes` · `Estimates` · `Subscriptions` · `Expenses` · `Contracts` · `Projects` · `Tasks` · `Tickets` · `Files` · `Vault` · `Reminders` · `Map`; tiêu đề trái `#<id> auto_cus_smoke_20261112_05`, tab trình duyệt cùng tên; menu thả xuống chỉ có đúng 1 mục `Delete` (bấm ra ngoài để đóng, **không** chọn) | P2 | REQ-CUS-55, REQ-CUS-56, REQ-CUS-60 | CRM_CUS_TC_010 |
| 10 | ✅ | Ở tab `Profile`, đếm tab con và nhìn chân form | Đúng 3 tab con theo thứ tự `Customer Details` (đang chọn) · `Billing & Shipping` · `Customer Admins`; chân form chỉ có 1 nút xanh `Save`, **không** có `Save and create contact` | P2 | REQ-CUS-57, REQ-CUS-58 | CRM_CUS_TC_011 |
| 11 | ✅ | Bấm `Notes` ở menu trái, đối chiếu trang, rồi bấm `New Note` hai lần | Thanh địa chỉ có `?group=notes`, tiêu đề `Notes`, nút xanh lá `+ New Note`; bảng cột `Description` · `Added From` · `Date Added` · `Options`, thân bảng `No entries found`, mục `Notes` ở menu **không** có số đếm; bấm lần 1 hiện ô nhiều dòng `Note description` và nút `Save`; bấm lần 2 ô nhập và nút Save ẩn đi (không bấm `Save`) | P3 | REQ-CUS-96, REQ-CUS-97 | CRM_CUS_TC_014 |
| 12 | ✅ | Bấm `Files` ở menu trái, đối chiếu trang | Thanh địa chỉ có `?group=attachments`, tiêu đề `Files`; dòng chữ xanh nguyên văn `Files from projects and tasks linked to the customer are not shown on this table.`; khung viền đứt nét `Drop files here to upload`; bảng cột `File` · `Show to customers area` · `Date Uploaded` · `Options`, thân bảng `No entries found`. ⚠️ Không đối chiếu nút `Choose from Google Drive` (`ASM-CUS-01`) | P3 | REQ-CUS-103 | CRM_CUS_TC_015 |
| 13 | ✅ | Bấm `Statement` ở menu trái, đối chiếu sao kê hiện ngay | Tiêu đề `Statement`; ô khoảng thời gian `This Month`; góc phải 3 nút biểu tượng máy in · PDF · phong bì; tiêu đề sao kê chứa `Customer Statement For auto_cus_smoke_20261112_05`; `Account Summary` + `01-11-2026 To 30-11-2026` (tính theo tháng của ngày chạy); `Beginning Balance:` · `Invoiced Amount:` · `Amount Paid:` · `Balance Due:` đều `$0.00`; bảng cột `Date` · `Details` · `Amount` · `Payments` · `Balance`, dòng đầu `Beginning Balance` `0.00`, dòng cuối `Balance Due` `$0.00` | P2 | REQ-CUS-113, REQ-CUS-114, REQ-CUS-118 | CRM_CUS_TC_016 |
| 14 | ✅ | Bấm `Vault` ở menu trái, rồi bấm `New Vault Entry`, đối chiếu hộp thoại | Trang Vault: tiêu đề `Vault`, nút xanh `+ New Vault Entry`, khung thông báo nguyên văn `Vault entries not found for this customer.`. Hộp thoại `Vault Entry`: `* Server Address` · `Port` · `* Username` · `* Password` · `Short Description`; 3 nút chọn, **đang chọn** `Visible to all staff member who have access to this customer`; ô `Share this vault entry in projects with project members` chưa tích; chân `Close` · `Save`. ⚠️ Không đối chiếu ô Username lúc mở — Chrome có thể tự điền email đăng nhập (`AMB-CUS-24`). Bấm `Close`, **không** `Save` | P2 | REQ-CUS-123, REQ-CUS-124 | CRM_CUS_TC_017 |
| 15 | ❌ | Bấm `Map` ở menu trái, đối chiếu trang | Thanh địa chỉ có `?group=map`; cùng một hàng: ô `Latitude (Google Maps)` (nút chữ `G` bên phải ô) · ô `Longitude (Google Maps)` · nút xanh `Save` · nút `Open in Google Map`; dưới hàng có dòng nguyên văn `Setup google api key in order to view to customer map`. ⚠️ Chưa có evidence 2 ô toạ độ trống với KH chưa lưu toạ độ — ghi lại thực tế (`@NeedsVerify`) | P3 | REQ-CUS-135 | CRM_CUS_TC_018 |
| 16 | ✅ | Ở `Profile` → `Customer Details`, sửa 11 field theo ghi chú dưới bảng, bấm `Save`, nhấn F5 | Thông báo nguyên văn `Customer updated successfully.`, thanh địa chỉ vẫn `/admin/clients/client/<id>`; sau F5 cả 11 field mang giá trị mới; tiêu đề tab trình duyệt và tiêu đề trái đổi theo Company mới `#<id> auto_cus_smoke_20261112_16e` | P1 | REQ-CUS-59, REQ-CUS-60 | CRM_CUS_TC_012 |

> **Dữ liệu mục 16 — sửa KH A:** Company `auto_cus_smoke_20261112_16e` · VAT Number `VN0312345678` · Phone `0283822123` · Website `https://auto-cus-smoke-edit.test` · Currency `EUR` · Default Language `English` · Address `34 Edit Street` · City `EditCity` · State `EditState` · Zip Code `71000` · Country `Japan`.

### Nhóm: Import, hộp thoại và xoá

| # | Trạng thái | Hạng mục kiểm tra | Kết quả kỳ vọng | Priority | REQ ID | TC ID liên quan |
|---|---|---|---|---|---|---|
| 17 | ✅ | Ở danh sách bấm `Import Customers`, đối chiếu trang, **không** bấm nút nào ở cuối form | Mở `/admin/clients/import`, tiêu đề tab `Import`; khung nền xanh nhạt 4 mục đánh số: mục 1 nhắc dòng đầu CSV là tiêu đề cột và file UTF-8 · mục 2 `formatted in format Y-m-d (<ngày hôm nay>)` dạng `YYYY-MM-DD` (VD `2026-11-12`) · mục 3 có `Setup`, `Settings`, `Customers` nối bằng mũi tên · mục 4 **chữ đỏ** `Duplicate email rows won't be imported.`; tiêu đề `Import Customers`, nút xanh lá `Download Sample`; bảng mẫu cuộn ngang 27 cột, dấu `*` đỏ ở `Firstname` · `Lastname` · `Email` · `Company`, dòng mẫu `Sample Data`; form: `* Choose CSV File` (`No file chosen`) · `Groups` (`Nothing selected`, nút `+`) · `Default password for all contacts` (trống) · nút `Import` · `Simulate Import` | P2 | REQ-CUS-83, REQ-CUS-84, REQ-CUS-85 | CRM_CUS_TC_013 |
| 18 | ✅ | Mở hộp thoại `Bulk Actions` (danh sách) và `Add New Customer Group` (nút `+` cạnh Groups ở form tạo): đóng bằng `×`, mở lại rồi đóng bằng `Close` | Hộp thoại mở giữa màn hình, nền phía sau tối mờ; bấm `×` hoặc `Close` đều đóng hộp, trang phía sau giữ nguyên, **không** có thông báo nào. ⚠️ Chưa có evidence phím `Esc` và trạng thái mặc định của Bulk Actions — thử `Esc` và ghi kết quả (`@NeedsVerify`); không bấm `Confirm` / `Save` | P2 | REQ-CUS-41, REQ-CUS-75 | CRM_CUS_TC_019 |
| 19 | ⚠️ | **Cuối cùng:** mở hồ sơ KH A, bấm `⌄` cạnh tiêu đề → `Delete` → `OK` ở hộp xác nhận | Hộp xác nhận nội dung nguyên văn `Are you sure you want to perform this action?`; sau `OK` chuyển về `/admin/clients`, hiện thông báo nguyên văn `Customer deleted`. *Kết thúc: KH A đã xoá* | P1 | REQ-CUS-70, REQ-CUS-71 | CRM_CUS_TC_079, CRM_CUS_TC_080 |

### Dọn dữ liệu sau khi chạy

KH B (`_06`) và KH tạo ở mục 8 (`_08`) còn lại trên môi trường dùng chung (`RISK-CUS-02`): ở danh sách gõ `auto_cus_smoke_20261112` vào ô `Search...` → **đối chiếu chỉ còn đúng 2 dòng, đúng tên KH test** → `Bulk Actions` → tích `Mass Delete` → `Confirm`. Có dòng lạ (không thuộc prefix này) → **dừng, không xoá**.

### Không áp dụng / ngoài phạm vi checklist này

- **Data Table / List — thao tác:** sắp xếp từng cột, phân trang, đổi số dòng, tìm không khớp, bộ lọc — thuộc Vòng 2, chạy bằng bộ TC (`CRM_CUS_TC_023` → `040`). Smoke chỉ kiểm bảng **hiển thị đúng** (mục 1, 2, 7).
- **CRUD Lifecycle:** Create, View, Edit có (mục 5 → 8, 16); Delete có mục 19. Xoá từ danh sách, xoá KH Inactive, Cancel hộp xác nhận, Mass Delete, tạo trùng tên — Vòng 2 (`079` → `087`, `046` → `048`).
- **Permission / Role** — không áp dụng: hệ thống **chỉ có role Admin** (user xác nhận 19-10-2026); rà lại khi có role thứ hai.
- **Status Flow:** chỉ kiểm trạng thái mặc định **Active** khi tạo mới (mục 7). Tắt/bật Active, banner KH Inactive, `Mark as active` — Vòng 2 (`072` → `075`).
- **Notification / Toast:** có ở mục 5 → 8 (`Customer added successfully.`), 16 (`Customer updated successfully.`), 19 (`Customer deleted`); thông báo lỗi từng field — Vòng 2.
- **Modal / Dialog:** mục 18 kiểm 2 hộp thoại đại diện. 4 hộp còn lại (`Assign Admin`, `Vault Entry` — mục 14 chỉ mở, `Send File`, `Account Summary`) — chạy `CRM_CUS_TC_019` đủ 6 biến thể bằng bộ TC; `Send File` cần KH đã có 1 tệp tải lên.
- **Mở form tạo bằng URL trực tiếp** (`CRM_CUS_TC_005-b`) — không đưa vào Smoke, cùng kết quả với mục 3.
- **Race condition / Session & Network / UTF-8 / A11y bàn phím** — Vòng 3, 4: `CRM_CUS_TC_149` → `168`, không thuộc Smoke.
- **Chờ điều kiện:** TC `@Skip` (Import thật, tải tệp, gửi email, Google API key — `AMB-CUS-12` ⏭️, `AMB-CUS-25` ⏭️, `AMB-SYS-01`) và TC `@KnownBug` (`042` · `044` · `090`…) **không** có trong checklist này.

### Kết quả lần chạy 12-11-2026

17/19 mục ✅ PASS · mục **15** ❌ FAIL (thiếu nút `Open in Google Map`) · mục **19** ⚠️ BLOCKED (không bắt được thông báo `Customer deleted`). Cột `Trạng thái` ghi ✅ PASS · ❌ FAIL · ⚠️ BLOCKED. Chi tiết: [`execution_report.md`](../../../executions/customers/web/run_1794468325/execution_report.md).

### Ký nhận

| Môi trường | Build / Version | Người thực hiện | Ngày | Kết quả (Pass/Fail/Blocked) |
|---|---|---|---|---|
|  |  |  |  |  |
