# STORY-CUS-01 — Danh sách, Summary, tìm kiếm, bộ lọc, Export

> ← [Index module](../../REQUIREMENTS_CUSTOMERS_SUMMARY.md) · [File web](../requirements_customers_web.md) · REQ `REQ-CUS-01` → `27` (27 REQ). AMB/RISK, phân quyền, trạng thái: xem index.

**User story:** Là staff, tôi muốn xem danh sách khách hàng kèm số liệu tổng hợp, tìm và lọc nhanh để tìm đúng khách hàng cần làm việc.

## 1. Yêu cầu chức năng

| REQ ID | Tên yêu cầu | Mô tả | Acceptance Criteria | Trạng thái | Cập nhật lần cuối | Nguồn |
|---|---|---|---|---|---|---|
| REQ-CUS-01 | Summary hiển thị 6 bộ đếm | Khối `Customers Summary` đầu trang | Mở `/admin/clients` → thấy 6 bộ đếm, mỗi bộ 1 số + nhãn nguyên văn theo thứ tự: `Total Customers` · `Active Customers` · `Inactive Customers` · `Active Contacts` · `Inactive Contacts` · `Contacts Logged In Today` | 🟢 | — | Kiểm chứng thực tế |
| REQ-CUS-02 | Total Customers bằng tổng bản ghi của bảng | Bộ đếm khớp bảng khi không lọc | Không tìm/lọc: số ở `Total Customers` = N trong `Showing 1 to 25 of N entries` (11-11-2026: 2942 = `2,942`). **Không** assert số tuyệt đối (`RISK-CUS-01`) | 🟢 | — | Kiểm chứng thực tế |
| REQ-CUS-03 | Total Customers thay đổi theo tạo/xoá | Bộ đếm cập nhật khi tải lại trang | Ghi Total trước = T → tạo k KH → tải lại → Total = T + k; xoá k KH đó → Total = T (đo 11-11-2026: 2942 → 2945 → 2942) | 🟢 | — | Kiểm chứng thực tế |
| REQ-CUS-04 | Inactive Customers tăng khi tắt Active | — | Ghi Inactive trước = I → tắt Active 1 KH (REQ-CUS-66) → tải lại → Inactive = I + 1, Active Customers giảm 1 (đo: 9 → 10) | 🟢 | — | Kiểm chứng thực tế |
| REQ-CUS-05 | Thanh công cụ 3 nút + nút lọc | — | Đầu trang có link `New Customer` → `/admin/clients/client` · `Import Customers` → `/admin/clients/import` · `Contacts` → `/admin/clients/all_contacts` · và 1 nút icon phễu (mở menu lọc, REQ-CUS-22) | 🟢 | — | Kiểm chứng thực tế (DOM `href`) |
| REQ-CUS-06 | Bảng có 9 cột theo thứ tự cố định | Bảng `table#clients` | Tiêu đề cột theo thứ tự: (checkbox chọn tất cả `#mass_select_all`) · `#` · `Company` · `Primary Contact` · `Primary Email` · `Phone` · `Active` · `Groups` · `Date Created` | 🟢 | — | UI thực tế (DOM) |
| REQ-CUS-07 | 8 cột sắp xếp được | — | Cột checkbox có class `sorting_disabled`; 8 cột còn lại có class `sorting` (bấm tiêu đề để sắp xếp) | 🟢 | — | UI thực tế (DOM) |
| REQ-CUS-08 | Sắp xếp mặc định theo Company tăng dần | — | Mở trang lần đầu: tiêu đề `Company` có class `sorting_asc`; request `POST /admin/clients/table` mang `order[0][column]=2&order[0][dir]=asc` | 🟢 | — | API · `POST /admin/clients/table` |
| REQ-CUS-09 | Số dòng mỗi trang: 10 / 25 / 50 / 100 / All, mặc định 25 | — | Dropdown page size có đúng 5 option, `value` lần lượt `10` · `25` · `50` · `100` · `-1` (nhãn `All`); option `25` được chọn mặc định; request mang `length=25` | 🟢 | — | UI thực tế (DOM) + API |
| REQ-CUS-10 | Phân trang Previous / số trang / Next + ô nhảy trang | — | Chân bảng có link `Previous` · các số trang (có `…` khi nhiều trang) · `Next`, kèm 1 dropdown chọn số trang. Trang đầu: `Previous` không bấm được | 🟢 | — | UI thực tế (DOM) |
| REQ-CUS-11 | Dòng thông tin số bản ghi | — | Không tìm: `Showing <a> to <b> of <N> entries`. Đang tìm: thêm hậu tố ` (filtered from <N> total entries)` (VD `Showing 1 to 2 of 2 entries (filtered from 2,945 total entries)`). Số có dấu phẩy ngăn nghìn | 🟢 | — | Kiểm chứng thực tế |
| REQ-CUS-12 | Tìm kiếm theo Company | Ô `Search...` của bảng | Nhập đúng tên Company duy nhất do test tạo → bảng chỉ còn các KH có tên đó | 🟢 | — | Kiểm chứng thực tế |
| REQ-CUS-13 | Tìm kiếm theo Phone | — | Nhập giá trị Phone duy nhất của KH test → bảng trả đúng 1 dòng KH đó | 🟢 | — | Kiểm chứng thực tế |
| REQ-CUS-14 | Tìm không khớp hiển thị trạng thái rỗng | — | Nhập chuỗi không tồn tại → thân bảng 1 dòng `No matching records found`, info `Showing 0 to 0 of 0 entries (filtered from <N> total entries)` | 🟢 | — | Kiểm chứng thực tế |
| REQ-CUS-15 | Dòng KH công ty có 3 thao tác | Cột Company | Tên Company là link `/admin/clients/client/<id>`; dưới tên có 3 link `View` → `/admin/clients/client/<id>` · `Contacts` → `…/<id>?group=contacts` · `Delete` → `/admin/clients/delete/<id>` | 🟢 | — | UI thực tế (DOM) |
| REQ-CUS-16 | Dòng KH không có Company hiển thị `Person - View Profile` | KH cá nhân (Company rỗng, có contact) | Cột Company hiện link `Person - View Profile` → `/admin/clients/client/<id>?contactid=<contact_id>`; thao tác chỉ có `View` (→ `?group=contacts`) · `Delete` — **không** có `Contacts` | 🟢 | — | UI thực tế (DOM — bản ghi có sẵn trên môi trường) |
| REQ-CUS-17 | Cột Active là switch, tiêu đề có tooltip | — | Mỗi dòng có checkbox `input.onoffswitch-checkbox` (`id` = id KH, `data-switch-url=/admin/clients/change_client_status`); tiêu đề cột Active có tooltip nguyên văn `Won't be shown in dropdowns when creating new records` | 🟢 | — | UI thực tế (DOM) |
| REQ-CUS-18 | Cột Groups hiển thị nhãn nhóm | — | KH thuộc nhóm → cột Groups hiện nhãn tên nhóm (VD `customer group b`); không thuộc nhóm → ô trống | 🟢 | — | Kiểm chứng thực tế |
| REQ-CUS-19 | Date Created hiển thị ngày giờ đầy đủ | — | Định dạng `DD-MM-YYYY HH:mm:ss` theo giờ máy chủ (VD `11-11-2026 14:18:49`) | 🟢 | — | Kiểm chứng thực tế |
| REQ-CUS-20 | Menu Export có 4 lựa chọn | Nút `Export` | Bấm `Export` → menu `Excel` · `CSV` · `PDF` · `Print` | 🟢 | — | Kiểm chứng thực tế |
| REQ-CUS-21 | Export tạo tệp đúng dữ liệu đang hiển thị | Tác tạo của REQ-CUS-20 (skill 4.3.8) | Lọc về KH test → Export CSV → tệp tải về chỉ chứa các KH đang lọc, đủ các cột hiển thị (trừ cột checkbox `not-export`). **Chưa kiểm** — tải tệp chưa được phép (`AMB-CUS-12` ⏭️) | ⚪ | 11-11-2026 · ✏️ biên tập | Chưa kiểm chứng |
| REQ-CUS-22 | Menu lọc gồm New Filter + bộ lọc đã lưu | Nút icon phễu | Bấm → menu có mục `New Filter` và danh sách bộ lọc đã lưu, **kể cả bộ lọc do staff khác tạo** — bộ lọc lưu dùng chung toàn hệ thống (`AMB-CUS-21` ✅). Không assert danh sách bộ lọc cố định | 🟢 | 11-11-2026 · ✏️ biên tập | Kiểm chứng thực tế |
| REQ-CUS-23 | Create Filter có 15 rule | Modal `Create Filter` → dropdown `Add Rule` | Dropdown có đúng 15 rule theo thứ tự: `Phone` · `Active` · `Invoices` · `Estimates` · `Proposals` · `Projects` · `Contracts Types` · `City` · `Zip Code` · `State` · `Country` · `Responsible admin` · `Groups` · `Customers assigned to me` · `Requires Registration Confirmation` | 🟢 | — | UI thực tế (DOM) |
| REQ-CUS-24 | Apply chỉ bấm được khi rule có giá trị | — | Thêm rule `Active` (2 radio `Yes` / `No`, chưa chọn) → nút `Apply` có thuộc tính `disabled`; chọn `No` → `Apply` hết `disabled` | 🟢 | — | Kiểm chứng thực tế |
| REQ-CUS-25 | Lọc Active = No trả đúng KH Inactive | — | Apply rule Active = `No` → mọi dòng có switch Active tắt; số dòng (info `of N entries`) = bộ đếm `Inactive Customers` (đo: 9 = 9). Request mang `filters[match_type]=or&filters[rules][0][id]=active&filters[rules][0][value]=0&filters[rules][0][type]=BooleanRule` | 🟢 | — | Kiểm chứng thực tế + API |
| REQ-CUS-26 | Bộ lọc chưa lưu không còn sau khi tải lại | `Save Filter` để tắt | Apply bộ lọc (Save Filter tắt) → tải lại `/admin/clients` → bảng về đầy đủ (info `of <Total> entries`) | 🟢 | — | Kiểm chứng thực tế |
| REQ-CUS-27 | Lưu bộ lọc bằng Save Filter | Toggle `Save Filter` trong modal | Bật `Save Filter` + Apply → bộ lọc xuất hiện trong menu lọc, chọn lại cho cùng kết quả. **Chưa kiểm** — bộ lọc lưu dùng chung toàn hệ thống, tránh tạo rác (`AMB-CUS-21` ✅) | ⚪ | 11-11-2026 · ✏️ biên tập | Chưa kiểm chứng |

## 2. Đặc tả trường dữ liệu (thành phần danh sách)

| Thành phần | Loại UI | Ràng buộc / mặc định | REQ |
|---|---|---|---|
| Page size | `select[name$=_length]` | `10` · `25`* · `50` · `100` · `-1`=All | 09 |
| Ô tìm kiếm | `input[type=search]` `.dataTables_filter input`, placeholder `Search...` | Tìm trên server, gõ là lọc | 12 → 14 |
| Chọn tất cả | checkbox `#mass_select_all` | Chọn mọi dòng **đang hiển thị** | 74 (Story 04) |
| Switch Active | `input.onoffswitch-checkbox#<id>` + `label[for=<id>]` | Bấm `label` để đổi | 17 · 66 |
| Create Filter — Add Rule | bootstrap-select 15 option | Rule Active: radio `Yes`/`No` | 23 → 25 |
| Create Filter — Save Filter | toggle | Mặc định tắt | 26 · 27 |

## 3. Thông báo

| REQ ID | Trigger | Nội dung nguyên văn |
|---|---|---|
| REQ-CUS-14 | Tìm không khớp | `No matching records found` |
| REQ-CUS-17 | Hover tiêu đề cột Active | `Won't be shown in dropdowns when creating new records` |

## 4. Luồng chính

1. Mở `/admin/clients` → Summary + bảng 25 dòng, sắp theo Company tăng dần
2. Gõ từ khoá vào `Search...` → bảng lọc, info thêm `(filtered from … total entries)`
3. Icon phễu → `New Filter` → `Add Rule` → chọn giá trị → `Apply`
4. `Export` → chọn định dạng
