# STORY-CUS-01 — Danh sách, tìm kiếm & bộ lọc

> ← [Index module `REQUIREMENTS_CUSTOMERS_SUMMARY.md`](../../REQUIREMENTS_CUSTOMERS_SUMMARY.md) · [File nền tảng Web](../requirements_customers_web.md)
>
> Phân quyền, trạng thái, AMB/RISK, Nhật ký nằm ở **index** — file này chỉ **tham chiếu** mã.
> Phạm vi: `REQ-CUS-01` → `REQ-CUS-16` (16 REQ) · AMB liên quan: `AMB-CUS-13` · RISK liên quan: `RISK-CUS-01`

**Route:** `/admin/clients` · Danh sách nạp bằng DataTables phía server: `POST /admin/clients/table` (response `draw` · `iTotalRecords` · `iTotalDisplayRecords` · `aaData` — mỗi dòng 9 ô + `DT_RowClass`).

## 1. Yêu cầu chức năng

| REQ ID | Tên yêu cầu | Mô tả | Acceptance Criteria | Trạng thái | Cập nhật lần cuối | Nguồn |
|---|---|---|---|---|---|---|
| REQ-CUS-01 | Thanh công cụ trang danh sách có 3 nút điều hướng | Từ danh sách đi được tới tạo mới, import, danh sách contact | Mở `/admin/clients` → 3 nút hiển thị: `New Customer` (href `/admin/clients/client`) · `Import Customers` (href `/admin/clients/import`) · `Contacts` (href `/admin/clients/all_contacts`) | 🟢 | — | UI thực tế |
| REQ-CUS-02 | Khối Customers Summary có 6 bộ đếm | Tổng quan số lượng KH và contact | Khối tiêu đề `Customers Summary` hiển thị 6 nhãn theo thứ tự: `Total Customers` · `Active Customers` · `Inactive Customers` · `Active Contacts` · `Inactive Contacts` · `Contacts Logged In Today`, mỗi nhãn kèm một số nguyên. **Không** assert giá trị tuyệt đối (`RISK-CUS-01`) | 🟢 | — | UI thực tế |
| REQ-CUS-03 | Bảng danh sách có 9 cột theo đúng thứ tự | Cấu trúc bảng ổn định cho người dùng và automation | `table.dataTable thead th` gồm 9 ô: (checkbox chọn tất cả `#mass_select_all`) · `#` · `Company` · `Primary Contact` · `Primary Email` · `Phone` · `Active` · `Groups` · `Date Created` | 🟢 | — | UI thực tế |
| REQ-CUS-04 | Danh sách mặc định sắp xếp theo Company tăng dần | Thứ tự mặc định khi mở trang | Mở trang (chưa bấm tiêu đề cột nào) → `th` của cột `Company` mang class `sorting_asc`; 7 cột còn lại (trừ checkbox) mang class `sorting` | 🟢 | — | UI thực tế |
| REQ-CUS-05 | Ô số dòng mỗi trang có 5 lựa chọn, mặc định 25 | Người dùng đổi số dòng hiển thị | `select[name=clients_length]` có 5 option theo thứ tự value:text — `10:10` · `25:25` · `50:50` · `100:100` · `-1:All`; option `25` đang `selected` | 🟢 | — | UI thực tế |
| REQ-CUS-06 | Ô tìm kiếm của bảng lọc phía máy chủ | Tìm KH theo từ khoá | Nhập từ khoá vào ô `Search...` của bảng (`input[aria-controls=clients]` — **khác** ô tìm kiếm toàn cục `#search_input` trên header) → gửi `POST /admin/clients/table` với `search[value]=<từ khoá>`; dòng thông tin **chứa** `filtered from` và số tổng (VD tìm theo tên KH tự tạo → `Showing 1 to 1 of 1 entries (filtered from <N> total entries)`) | 🟢 | — | Kiểm chứng thực tế · API `POST /admin/clients/table` · 200 |
| REQ-CUS-07 | Mỗi dòng có 3 thao tác nhanh dưới tên Company | Mở hồ sơ, mở tab contact, xoá | Ô `Company` của dòng chứa `.row-options` với 3 link: `View` → `/admin/clients/client/<id>` · `Contacts` → `/admin/clients/client/<id>?group=contacts` · `Delete` → `/admin/clients/delete/<id>` (class `_delete`). Đo trên KH tự tạo; dòng KH có primary contact quan sát được chỉ 2 link `View` (→ `?group=contacts`) · `Delete` — xem ghi chú dưới bảng | 🟢 | — | Kiểm chứng thực tế |
| REQ-CUS-08 | Cột Date Created hiển thị ngày giờ dạng DD-MM-YYYY HH:mm:ss | Định dạng ngày giờ nhất quán | Ô `Date Created` của KH tự tạo có dạng `<DD>-<MM>-<YYYY> <HH>:<mm>:<ss>` (regex `^\d{2}-\d{2}-\d{4} \d{2}:\d{2}:\d{2}$`), bằng thời điểm tạo theo giờ máy chủ | 🟢 | — | Kiểm chứng thực tế |
| REQ-CUS-09 | Nút Export có 4 định dạng xuất | Xuất danh sách đang lọc | Bấm `Export` → menu `.dt-button-collection` có 4 mục: `Excel` · `CSV` · `PDF` · `Print`. Chưa bấm thử từng mục (không kiểm nội dung tệp xuất) | 🟢 | — | UI thực tế (đọc DOM — ảnh chụp hụt menu, đã loại) |
| REQ-CUS-10 | Bộ đếm Inactive Customers bằng số KH lọc được với Active = No | Bộ đếm khớp dữ liệu | Đọc số của `Inactive Customers` → áp bộ lọc `Active = No` (REQ-CUS-15) → `iTotalDisplayRecords` bằng đúng số đó (đo 04-11-2026: 9 = 9). Đọc hai giá trị **liền nhau** trong cùng phiên — môi trường dùng chung (`RISK-CUS-01`) | 🟢 | — | Kiểm chứng thực tế · API `POST /admin/clients/table` |
| REQ-CUS-11 | Bộ đếm Summary cập nhật theo thao tác trên KH | Tạo, tắt Active, xoá phản ánh ngay vào Summary | So **chênh lệch** trước/sau, không so giá trị tuyệt đối: tạo 4 KH → `Total Customers` +4 · tắt Active 1 KH → `Inactive Customers` +1, `Active Customers` −1 · xoá 4 KH → `Total Customers` −4 và `Inactive Customers` về giá trị ban đầu (đo 04-11-2026: 2911 → 2915 → 2911; 9 → 10 → 9) | 🟢 | — | Kiểm chứng thực tế |
| REQ-CUS-12 | Menu lọc (icon phễu) có New Filter và danh sách bộ lọc đã lưu | Truy cập bộ lọc nâng cao | Bấm nút icon `fa-filter` cạnh thanh công cụ → menu mở (`aria-expanded=true`) gồm dòng link `New Filter` (link `Clear Filter` · `Edit` đang ẩn `display:none` khi chưa áp bộ lọc) và danh sách bộ lọc đã lưu (tên do người dùng đặt). **Không** assert tên/số bộ lọc đã lưu — dữ liệu do người khác tạo (`AMB-CUS-13`) | 🟢 | — | Kiểm chứng thực tế |
| REQ-CUS-13 | Modal Create Filter có 15 điều kiện lọc | Danh sách tiêu chí lọc nâng cao | `New Filter` → modal tiêu đề `Create Filter`, combobox `#clientsRules` có 15 option value:text: `phonenumber:Phone` · `active:Active` · `invoice_statuses:Invoices` · `estimate_statuses:Estimates` · `proposal_statuses:Proposals` · `project_statuses:Projects` · `contracts_types:Contracts Types` · `city:City` · `zip:Zip Code` · `state:State` · `country:Country` · `customer_admins:Responsible admin` · `groups:Groups` · `my_customers:Customers assigned to me` · `requires_confirmation:Requires Registration Confirmation` | 🟢 | — | UI thực tế |
| REQ-CUS-14 | Chọn điều kiện Active sinh dòng rule Yes/No, Apply chỉ bật khi đã chọn giá trị | Ngăn áp bộ lọc rỗng | Chọn `Active` trong `#clientsRules` → xuất hiện dòng rule nhãn `Active` với 2 radio `Yes` · `No` (chưa radio nào được chọn) và link xoá rule; nút `Apply` `disabled`. Chọn `No` → `Apply` enabled | 🟢 | — | Kiểm chứng thực tế |
| REQ-CUS-15 | Áp bộ lọc Active = No chỉ trả về KH không hoạt động | Lọc theo trạng thái | Apply với `Active = No` → modal đóng, `POST /admin/clients/table` gửi `filters[match_type]=or` · `filters[rules][0][id]=active` · `filters[rules][0][value]=0` · `filters[rules][0][type]=BooleanRule`; mọi dòng trả về có switch `Active` **không** `checked` | 🟢 | — | Kiểm chứng thực tế · API `POST /admin/clients/table` · 200 |
| REQ-CUS-16 | Lưu bộ lọc bằng Save Filter để dùng lại | Bộ lọc dùng lại được | Modal có nhãn `Save Filter` kèm công tắc. Chưa bật/lưu thử — không biết bộ lọc lưu riêng người tạo hay dùng chung (`AMB-CUS-13`) | ⚪ | — | UI thực tế — chưa kiểm chứng |

> **Ghi chú REQ-CUS-07:** dòng mẫu quan sát được trước khi lọc (KH của người khác, có primary contact) có `View` trỏ `?group=contacts` và **không** có link `Contacts`; KH tự tạo (chưa có contact) có đủ 3 link. Chưa xác định được quy tắc → TC chỉ assert trên KH tự tạo, không suy cho KH có contact.

## 2. Đặc tả trường dữ liệu — điều khiển danh sách

| Field (Label) | Loại UI | Required | Ràng buộc / mặc định | REQ liên quan | Ghi chú |
|---|---|---|---|---|---|
| Search... (bảng) | `input[type=search]` `aria-controls=clients` | — | Rỗng | 06 | Ô `#search_input` trên header là tìm kiếm toàn cục, **không** lọc bảng |
| Số dòng/trang | `select[name=clients_length]` | — | 5 option, mặc định `25` | 05 | |
| Chọn tất cả | `checkbox#mass_select_all` | — | Không tick | 03 | Dùng cho Bulk Actions (STORY-CUS-04) |
| Chọn dòng | `checkbox` trong ô đầu, `value=<id KH>` | — | Không tick | 03 | **Không** nhầm với switch Active (`input.onoffswitch-checkbox`, cùng `type=checkbox`, `value=on`) |
| Active (cột) | Switch `input.onoffswitch-checkbox` `id=<id KH>` `data-switch-url=/admin/clients/change_client_status` | — | `checked` = Active | 53 | Xem STORY-CUS-04 |
| Điều kiện lọc | `select#clientsRules` (name `tax`) | — | 15 option | 13 | |
| Giá trị rule Active | 2 radio `Yes` / `No` | — | Chưa chọn | 14 | |

## 3. Validation & thông báo

| REQ ID | Rule / Trigger | Thông báo (nguyên văn) |
|---|---|---|
| REQ-CUS-14 | Rule đã thêm nhưng chưa chọn giá trị | *(không có thông báo — nút `Apply` bị `disabled`)* |

## 4. Luồng xử lý

```
Mở /admin/clients ─→ POST /admin/clients/table (draw 1)
   ├─ Gõ ô Search của bảng ─→ POST .../table với search[value]           (REQ-06)
   ├─ Icon phễu ─→ New Filter ─→ chọn rule ─→ chọn giá trị ─→ Apply       (REQ-12 → 15)
   └─ Export ─→ Excel / CSV / PDF / Print                                 (REQ-09)
```
