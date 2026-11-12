# Test Cases — Customers (tổng 169 TC · 272 biến thể · 1 nền tảng)

> **INDEX module — tên file bất biến.** Không chứa dòng TC — TC chi tiết nằm ở các part trong [Bản đồ tài liệu](#bản-đồ-tài-liệu).
>
> Danh mục: [`../README.md`](../README.md) · Requirements: [`REQUIREMENTS_CUSTOMERS_SUMMARY.md`](../../requirements/customers/REQUIREMENTS_CUSTOMERS_SUMMARY.md) → [`web/requirements_customers_web.md`](../../requirements/customers/web/requirements_customers_web.md)

| Thông tin | Nội dung |
|---|---|
| **Hệ thống · module** | Perfex CRM khu Admin · `CUS` — Customers: danh sách, Summary, tìm kiếm, bộ lọc, tạo/sửa/xoá KH, Active, Bulk Actions, Import, hồ sơ KH (Profile · Customer Admins · Notes · Statement · Files · Vault · Map) |
| **Nguồn requirement** | [`REQUIREMENTS_CUSTOMERS_SUMMARY.md`](../../requirements/customers/REQUIREMENTS_CUSTOMERS_SUMMARY.md) bản 2.1 (11-11-2026) — `REQ-CUS-01` → `REQ-CUS-141` (🟢 113 · 🟡 9 · ⚪ 19), 8 Story |
| **Phạm vi REQ** | 141/141 REQ có TC — không REQ nào bị loại khỏi phạm vi viết TC. 14 REQ chỉ có TC `@Skip` (chưa được phép chạy trên môi trường dùng chung) · 9 REQ 🟡 có TC `@KnownBug` |
| **Nền tảng** | Web ✅ 169 TC · Mobile — chưa có requirements · API — chưa có requirements |
| **Dải TC ID đã dùng** | `CRM_CUS_TC_001` → `CRM_CUS_TC_169` — chung mọi nền tảng. TC kế tiếp: `CRM_CUS_TC_170` |
| **Mode · độ hạt** | QUICK (`/generate-testcases-from-requirements`) · **GỘP** — biến thể cùng trường, cùng loại phản hồi chung 1 TC, mỗi biến thể có mã `a`, `b`… (báo FAIL ghi `CRM_CUS_TC_045-c`); quan sát tĩnh gộp thành Bảng kiểm `1`, `2`… |
| **Mức rủi ro · độ sâu** | `Cao` → **Đầy đủ** — V1 đủ 6 nhánh, V2 đủ mọi nhánh có điều kiện kích hoạt, V3 · V4 chấm từng nhánh.<br>Căn cứ chấm Cao: chạm **dữ liệu cá nhân / thông tin liên lạc của khách hàng** · có thao tác **không hồi lại được** (xoá KH, Mass Delete, Bulk ghi đè nhóm) · lưu **thông tin truy cập bí mật** (Vault) · là **cổng vào** của mọi chứng từ Sales, dự án, hợp đồng (hỏng thì chặn cả đợt kiểm thử).<br>**Rà lại độ sâu khi:** có thêm role thứ hai ngoài Admin (mở lại nhánh Permission) · được phép Import thật / gửi email / tải tệp trên môi trường test (gỡ `@Skip`) · mở cổng Customer dùng chung prefix `CUS` · có thêm nền tảng Mobile / API |
| **Ngày sinh** | 11-11-2026 |

## Bản đồ tài liệu

| Nền tảng | File | Nhóm chức năng | Số TC | TC ID | REQ bao phủ |
|---|---|---|---|---|---|
| Web | [web/parts/part_01_web_smoke.md](web/parts/part_01_web_smoke.md) | V1 Smoke — giao diện 8 màn hình, mở/đóng hộp thoại, tạo · lưu · đối chiếu · sửa KH | 19 | 001–019 | 01 · 05 · 06 · 08 → 12 · 18 · 19 · 28 · 29 · 40 · 41 · 44 · 46 → 51 · 55 → 60 · 62 · 75 · 83 → 85 · 96 · 97 · 103 · 109 · 113 · 114 · 118 · 120 · 123 · 124 · 135 |
| Web | [web/parts/part_02_web_danh_sach_form.md](web/parts/part_02_web_danh_sach_form.md) | V2a danh sách · Summary · sắp xếp · phân trang · tìm · Export · bộ lọc · validation form tạo/sửa · nhóm · tiền tệ · ngôn ngữ · quốc gia · địa chỉ | 47 | 020–066 | 02 → 04 · 07 · 09 → 17 · 20 → 27 · 30 → 48 · 52 → 54 · 59 |
| Web | [web/parts/part_03_web_ho_so_active_bulk_import.md](web/parts/part_03_web_ho_so_active_bulk_import.md) | V2b Customer Admins · Active · xoá KH · Bulk Actions · ảnh hưởng sang form chứng từ · Import | 36 | 067–101 · 169 | 18 · 50 · 56 · 57 · 61 → 82 · 85 → 95 · 139 → 141 |
| Web | [web/parts/part_04_web_tab_ho_so.md](web/parts/part_04_web_tab_ho_so.md) | V2c Notes · Files · Statement | 28 | 102–129 | 97 → 122 |
| Web | [web/parts/part_05_web_vault_map_error.md](web/parts/part_05_web_vault_map_error.md) | V2d Vault · Map · Error Guessing (bấm 2 lần, F5/Back, 2 tab cùng sửa) | 25 | 130–154 | 48 · 59 · 66 · 98 · 124 → 138 |
| Web | [web/parts/part_06_web_technical_nonfunc.md](web/parts/part_06_web_technical_nonfunc.md) | V3 chưa đăng nhập, chèn mã script / câu lệnh CSDL, che mật khẩu Vault, mất phiên, mất mạng · V4 responsive, bàn phím, Edge/Firefox | 14 | 155–168 | 01 · 06 · 12 · 28 · 29 · 48 · 71 · 83 · 98 · 123 · 124 · 127 · 128 |

> Tách 6 part vì file nền tảng web (169 TC) vượt ngưỡng 50 TC của độ hạt GỘP — cắt tại ranh giới vòng / nhóm chức năng. `CRM_CUS_TC_169` sinh sau cùng (phủ đủ Decision Table Bulk Actions) nên nằm ở part 3 theo nhóm chức năng, ID nối tiếp dải — không chèn số giữa.

### Nhãn đặc biệt trong bộ TC

| Tag | Số TC | TC ID | Ý nghĩa khi chạy |
|---|---|---|---|
| `@Skip` | 14 | 034 · 040 · 058 · 081 · 098 · 099 · 100 · 101 · 114 · 117 · 126 · 129 · 143 · 148 | REQ ⚪ chưa đủ điều kiện chạy: tải tệp / Import thật / mở PDF (`AMB-CUS-12` ⏭️) · gửi email (`AMB-CUS-25` ⏭️) · tạo bộ lọc / nhóm dùng chung (`AMB-CUS-21`, `RISK-CUS-03`) · xoá KH có chứng từ · cổng khách hàng · staff thứ hai (`AMB-CUS-23` ⏭️) · Google API key (`AMB-SYS-01`). `/execute-test-cases` chấm **BLOCKED**, không tính FAIL |
| `@KnownBug` | 10 | 042 · 044 · 090 · 103 · 116 · 122 · 128 · 135 · 146 · 147 | Viết theo hành vi **mong đợi** đã chốt ở AMB cho 9 REQ 🟡 (mục 6.3 của requirements) — **dự kiến FAIL** cho tới khi Dev sửa. FAIL → `/create-bug-report`; `042` user đã yêu cầu mở bug (`AMB-CUS-01`). 090 · 122 · 135 · 146 · 147 **không** đối chiếu nguyên văn thông báo (`AMB-CUS-28` 🟡) |
| `@PersonalOnly` | 1 | 142 | Phải gõ mật khẩu đăng nhập thật để xem mật khẩu Vault — QA tự chạy thủ công (`AMB-CUS-22`), `/execute-test-cases` bỏ qua |
| `@TechCheck` | 4 | 062 · 086 · 161 · 163 | Có dòng `🔧 Ghi chú kỹ thuật` cần DevTools (QA có quyền). Che dòng đó đi TC vẫn chấm được |
| `@NeedsVerify` | 41 | xem [Vùng chưa có evidence](#vùng-chưa-có-evidence) | Expected chưa có ảnh / lần đọc DOM chống lưng — chạy lần đầu là lượt xác nhận; lệch thì sửa TC, **không** mở bug ngay |
| `@Smoke` | 19 | 001 → 019 | Chạy đầu tiên mỗi build — Vòng 1 FAIL thì dừng, mọi TC Vòng 2 coi như BLOCKED |

## Assumptions

| Mã | Điểm chưa rõ | Giả định đã áp dụng | TC bị ảnh hưởng |
|---|---|---|---|
| ASM-CUS-01 | 🚩 **Xung đột tài liệu ↔ ảnh:** `REQ-CUS-103` ghi tab Files có nút `Choose from Google Drive`; ảnh `customer_files_uploaded_png_visible_viewport.png` **không** thấy nút này (khoảng giữa vùng kéo thả và bảng trống) | Viết theo ảnh — TC **không** đối chiếu nút này. Có thể nút chỉ hiện khi đã cấu hình Google API (Setup bị chặn). **Cần recon lại** để sửa REQ-103 | 015 |
| ASM-CUS-02 | Tên dữ liệu test phải duy nhất trên môi trường dùng chung | Test Data ghi hậu tố `_20261111`; khi chạy **thay bằng ngày chạy** `YYYYMMDD` — vẫn truy ngược được TC qua `tc<NNN>` | Mọi TC tạo dữ liệu |
| ASM-CUS-03 | `AMB-CUS-28` 🟡 chưa chốt hình thức và nguyên văn thông báo chặn của REQ-82 · 117 · 126 · 137 | Theo Assumption tạm của AMB: REQ-82 **chặn**; cả 4 REQ assert hành vi chặn (không gỡ / không nạp / không lưu) + **có** thông báo nhìn thấy được, **không** assert nguyên văn | 090 · 122 · 135 · 146 · 147 |
| ASM-CUS-04 | Note chỉ gồm khoảng trắng — REQ-99 chỉ nói "để trống" | Coi như trống, cùng nguyên tắc `REQ-CUS-31` (Company chỉ khoảng trắng = trống) | 103-b |
| ASM-CUS-05 | Address · City · State · Zip Code (và VAT · Phone · Website) chưa khảo sát giới hạn độ dài; requirements không nêu ràng buộc định dạng của Address · City · State · Zip | Kỳ vọng lưu nguyên giá trị như `AMB-CUS-04` (cố ý chấp nhận mọi chuỗi); **không** sinh BVA độ dài cho các ô này — giá trị test ≤ 50 ký tự. Rà lại khi recon bổ sung đọc được `maxlength` | 049 → 055 |
| ASM-CUS-06 | Mở URL của module khi chưa đăng nhập — không có evidence trong module `CUS` | Theo hành vi chung của khu Admin đã kiểm ở module `LOGIN`: chuyển về `/admin/authentication` | 155 |
| ASM-CUS-07 | Không tạo được nhóm KH riêng cho test (`RISK-CUS-03`, Setup bị chặn) | Dùng nhóm có sẵn `customer group b` (thấy ở evidence 11-11-2026) và "nhóm thứ hai" bất kỳ khác tên — người chạy ghi tên nhóm thứ hai vào báo cáo. Không có `customer group b` → chọn nhóm khác, ghi lại | 007 · 008 · 056 · 088 · 089 · 090 · 169 |
| ASM-CUS-08 | Form **sửa** báo lỗi Company trống như form tạo? (chỉ có ảnh lỗi trên form tạo) | Cùng field, cùng thông báo `This field is required.` | 066 |
| ASM-CUS-09 | Mục 3 hướng dẫn Import: tài liệu ghi `Setup->Settings->Customers`, ảnh hiển thị mũi tên `→` (phông chữ ghép `->` thành mũi tên) | TC đối chiếu từ khoá `Setup` · `Settings` · `Customers` nối bằng mũi tên, không đối chiếu ký tự nối | 013 |
| ASM-CUS-10 | Đóng hộp thoại bằng phím Esc chưa khảo sát | Esc đóng hộp thoại như nút `×` | 019 |
| ASM-CUS-11 | Kiểm khoá Currency cần KH có giao dịch — không tạo giao dịch trên môi trường dùng chung | Dùng KH có sẵn đã có hoá đơn, **chỉ xem, không lưu** | 060 |
| ASM-CUS-12 | Tên hiển thị của staff đang đăng nhập | `Admin Example` — thấy ở cột `Added From` (Notes), dòng `created by` (Vault), hộp thoại Assign Admin | 068 · 069 · 070 · 071 · 102 · 136 · 139 |
| ASM-CUS-13 | Khoảng ngày của Statement tính theo ngày chạy | Test Data tính theo ngày `11-11-2026`; chạy ngày khác thì tính lại `This Month` / `Last Month` / `This Year`… | 016 · 120 · 125 |

## Bảng Đối Soát Coverage (toàn module)

> Một nền tảng (Web) — mọi REQ nằm ở tầng `web/`, không có REQ dùng chung ≥ 2 nền tảng. `P` Positive · `N` Negative · `B` Boundary.

| REQ ID | Mô tả ngắn | Trạng thái REQ | Số TC | TC IDs | Đủ Positive/Negative/Boundary? | Ghi chú |
|---|---|---|---|---|---|---|
| REQ-CUS-01 | Summary hiển thị 6 bộ đếm | 🟢 | 2 | 001 · 155 | ✅ P | `@NeedsVerify` 155 |
| REQ-CUS-02 | Total Customers bằng tổng bản ghi của bảng | 🟢 | 1 | 020 | ✅ P | — |
| REQ-CUS-03 | Total Customers thay đổi theo tạo/xoá | 🟢 | 1 | 021 | ✅ P | — |
| REQ-CUS-04 | Inactive Customers tăng khi tắt Active | 🟢 | 1 | 022 | ✅ P | — |
| REQ-CUS-05 | Thanh công cụ 3 nút + nút lọc | 🟢 | 2 | 001 · 005 | ✅ P | — |
| REQ-CUS-06 | Bảng có 9 cột theo thứ tự cố định | 🟢 | 2 | 002 · 164 | ✅ P | `@NeedsVerify` 164 |
| REQ-CUS-07 | 8 cột sắp xếp được | 🟢 | 2 | 023 · 024 | ✅ P | — |
| REQ-CUS-08 | Sắp xếp mặc định theo Company tăng dần | 🟢 | 2 | 002 · 023 | ✅ P | — |
| REQ-CUS-09 | Số dòng mỗi trang: 10 / 25 / 50 / 100 / All, mặc định 25 | 🟢 | 2 | 002 · 025 | ✅ P | — |
| REQ-CUS-10 | Phân trang Previous / số trang / Next + ô nhảy trang | 🟢 | 2 | 002 · 026 | ✅ P | — |
| REQ-CUS-11 | Dòng thông tin số bản ghi | 🟢 | 2 | 002 · 027 | ✅ P | — |
| REQ-CUS-12 | Tìm kiếm theo Company | 🟢 | 4 | 008 · 027 · 160 · 168 | ✅ P đủ tên · một phần · N chèn mã | `@NeedsVerify` 168 |
| REQ-CUS-13 | Tìm kiếm theo Phone | 🟢 | 1 | 028 | ✅ P | — |
| REQ-CUS-14 | Tìm không khớp hiển thị trạng thái rỗng | 🟢 | 1 | 029 | ✅ N | — |
| REQ-CUS-15 | Dòng KH công ty có 3 thao tác | 🟢 | 1 | 030 | ✅ P | — |
| REQ-CUS-16 | Dòng KH không có Company hiển thị `Person - View Profile` | 🟢 | 1 | 031 | ✅ P | — |
| REQ-CUS-17 | Cột Active là switch, tiêu đề có tooltip | 🟢 | 1 | 032 | ✅ P | — |
| REQ-CUS-18 | Cột Groups hiển thị nhãn nhóm | 🟢 | 2 | 008 · 088 | ✅ P | — |
| REQ-CUS-19 | Date Created hiển thị ngày giờ đầy đủ | 🟢 | 1 | 008 | ✅ P | chỉ có TC đa REQ này — FAIL vẫn quy được về REQ (6b ✅) |
| REQ-CUS-20 | Menu Export có 4 lựa chọn | 🟢 | 1 | 033 | ✅ P | — |
| REQ-CUS-21 | Export tạo tệp đúng dữ liệu đang hiển thị | ⚪ | 1 | 034 | ✅ P | `@Skip` 034 |
| REQ-CUS-22 | Menu lọc gồm New Filter + bộ lọc đã lưu | 🟢 | 1 | 035 | ✅ P | — |
| REQ-CUS-23 | Create Filter có 15 rule | 🟢 | 1 | 036 | ✅ P | — |
| REQ-CUS-24 | Apply chỉ bấm được khi rule có giá trị | 🟢 | 1 | 037 | ✅ P | — |
| REQ-CUS-25 | Lọc Active = No trả đúng KH Inactive | 🟢 | 1 | 038 | ✅ P | — |
| REQ-CUS-26 | Bộ lọc chưa lưu không còn sau khi tải lại | 🟢 | 1 | 039 | ✅ P | — |
| REQ-CUS-27 | Lưu bộ lọc bằng Save Filter | ⚪ | 1 | 040 | ✅ P | `@Skip` 040 |
| REQ-CUS-28 | Form tạo mới có 2 tab | 🟢 | 3 | 003 · 005 · 155 | ✅ P | `@NeedsVerify` 155 |
| REQ-CUS-29 | Tab Customer Details có 12 field hiển thị | 🟢 | 4 | 003 · 007 · 165 · 166 | ✅ P | `@NeedsVerify` 165 · 166 |
| REQ-CUS-30 | Company bắt buộc | 🟢 | 2 | 041 · 066 | ✅ N (041 · 066) | `@NeedsVerify` 066 |
| REQ-CUS-31 | Company chỉ gồm khoảng trắng bị chặn như bỏ trống | 🟡 | 1 | 042 | ✅ N | `@KnownBug` 042 |
| REQ-CUS-32 | Company tối đa 191 ký tự, vượt thì bị chặn kèm thông báo | 🟡 | 2 | 043 · 044 | ✅ B 190 · 191 · 192 · 206 | `@KnownBug` 044 |
| REQ-CUS-33 | Cảnh báo khi tên công ty đã tồn tại | 🟢 | 2 | 046 · 048 | ✅ P | — |
| REQ-CUS-34 | Kiểm trùng không phân biệt hoa thường | 🟢 | 1 | 046 | ✅ P | chỉ có TC đa REQ này — FAIL vẫn quy được về REQ (6b ✅) |
| REQ-CUS-35 | Tên không trùng không hiện cảnh báo | 🟢 | 1 | 047 | ✅ P | — |
| REQ-CUS-36 | Cảnh báo trùng không chặn lưu | 🟢 | 1 | 048 | ✅ P | chỉ có TC đa REQ này — FAIL vẫn quy được về REQ (6b ✅) |
| REQ-CUS-37 | VAT Number không kiểm định dạng | 🟢 | 1 | 049 | ✅ P | — |
| REQ-CUS-38 | Phone không kiểm định dạng | 🟢 | 1 | 050 | ✅ P | — |
| REQ-CUS-39 | Website không kiểm định dạng | 🟢 | 1 | 051 | ✅ P | — |
| REQ-CUS-40 | Groups chọn nhiều nhóm, mặc định trống | 🟢 | 2 | 003 · 056 | ✅ P | `@NeedsVerify` 056 |
| REQ-CUS-41 | Nút `+` mở modal thêm nhóm | 🟢 | 2 | 019 · 057 | ✅ P | `@NeedsVerify` 019 |
| REQ-CUS-42 | Tên nhóm bắt buộc | 🟢 | 1 | 057 | ✅ N | chỉ có TC đa REQ này — FAIL vẫn quy được về REQ (6b ✅) |
| REQ-CUS-43 | Tạo nhóm mới từ modal | ⚪ | 1 | 058 | ✅ P | `@Skip` 058 |
| REQ-CUS-44 | Currency có 3 lựa chọn, mặc định System Default | 🟢 | 2 | 003 · 059 | ✅ P | — |
| REQ-CUS-45 | Không đổi được Currency khi KH đã có giao dịch | ⚪ | 1 | 060 | ✅ P | `@NeedsVerify` 060 |
| REQ-CUS-46 | Default Language có 27 lựa chọn | 🟢 | 2 | 003 · 061 | ✅ P | — |
| REQ-CUS-47 | Country có 251 lựa chọn | 🟢 | 2 | 003 · 062 | ✅ P | — |
| REQ-CUS-48 | Save tạo KH và mở hồ sơ | 🟢 | 9 | 006 · 007 · 045 · 149 · 150 · 156 · 162 · 163 · 168 | ✅ P · N chèn mã (156) · lỗi thao tác (149 · 150 · 162 · 163) | `@NeedsVerify` 045 · 149 · 150 · 156 · 162 · 163 · 168 |
| REQ-CUS-49 | Save and create contact mở modal thêm contact | 🟢 | 1 | 009 | ✅ P | — |
| REQ-CUS-50 | KH mới tạo ở trạng thái Active | 🟢 | 2 | 008 · 072 | ✅ P | — |
| REQ-CUS-51 | Tab Billing & Shipping gồm 2 khối địa chỉ | 🟢 | 1 | 004 | ✅ P | `@NeedsVerify` 004 |
| REQ-CUS-52 | Same as Customer Info chép địa chỉ sang Billing | 🟢 | 1 | 063 | ✅ P | `@NeedsVerify` 063 |
| REQ-CUS-53 | Copy Billing Address chép Billing sang Shipping | 🟢 | 1 | 064 | ✅ P | — |
| REQ-CUS-54 | Địa chỉ Billing/Shipping được lưu | 🟢 | 1 | 065 | ✅ P | — |
| REQ-CUS-55 | Hồ sơ KH có menu 19 tab | 🟢 | 1 | 010 | ✅ P | chỉ có TC đa REQ này — FAIL vẫn quy được về REQ (6b ✅) |
| REQ-CUS-56 | Tiêu đề hồ sơ `#<id> <Company>` kèm menu chỉ có Delete | 🟢 | 2 | 010 · 080 | ✅ P | — |
| REQ-CUS-57 | Tab Profile có 3 tab con | 🟢 | 2 | 011 · 067 | ✅ P | — |
| REQ-CUS-58 | Form sửa chỉ có nút Save | 🟢 | 1 | 011 | ✅ P | chỉ có TC đa REQ này — FAIL vẫn quy được về REQ (6b ✅) |
| REQ-CUS-59 | Sửa KH lưu đủ field | 🟢 | 7 | 012 · 052 · 053 · 054 · 055 · 066 · 151 | ✅ P 11 field + từng field tự do · N Company trống | `@NeedsVerify` 052 · 053 · 054 · 055 · 066 · 151 |
| REQ-CUS-60 | Tiêu đề tab trình duyệt của hồ sơ là tên Company | 🟢 | 2 | 010 · 012 | ✅ P | — |
| REQ-CUS-61 | Bảng Customer Admins | 🟢 | 1 | 067 | ✅ P | chỉ có TC đa REQ này — FAIL vẫn quy được về REQ (6b ✅) |
| REQ-CUS-62 | Modal Assign Admin chọn nhiều staff | 🟢 | 2 | 019 · 068 | ✅ P | `@NeedsVerify` 019 |
| REQ-CUS-63 | Gán Customer Admin | 🟢 | 1 | 069 | ✅ P | — |
| REQ-CUS-64 | Gỡ Customer Admin | 🟢 | 1 | 070 | ✅ P | — |
| REQ-CUS-65 | Customer Admin có tác dụng ở bộ lọc "Customers assigned to me" | ⚪ | 1 | 071 | ✅ P | `@NeedsVerify` 071 |
| REQ-CUS-66 | Tắt Active ngay trên danh sách | 🟢 | 3 | 072 · 075 · 153 | ✅ P 2 chiều · bấm 2 lần | `@NeedsVerify` 075 · 153 |
| REQ-CUS-67 | Hồ sơ KH Inactive có banner cảnh báo | 🟢 | 1 | 073 | ✅ P | — |
| REQ-CUS-68 | Mark as active kích hoạt lại KH | 🟢 | 1 | 074 | ✅ P | — |
| REQ-CUS-69 | KH Inactive không có trong ô chọn khách hàng của Invoice | 🟢 | 1 | 076 | ✅ P | — |
| REQ-CUS-70 | Xoá KH phải xác nhận | 🟢 | 1 | 079 | ✅ Cancel · OK | — |
| REQ-CUS-71 | Xoá KH thành công | 🟢 | 2 | 080 · 168 | ✅ P | `@NeedsVerify` 168 |
| REQ-CUS-72 | Hồ sơ KH đã xoá trả 404 | 🟢 | 2 | 080 · 086 | ✅ P | — |
| REQ-CUS-73 | Xoá KH đang có chứng từ | ⚪ | 1 | 081 | ✅ N (chặn) | `@Skip` 081 |
| REQ-CUS-74 | Chọn tất cả dòng đang hiển thị | 🟢 | 1 | 082 | ✅ P | — |
| REQ-CUS-75 | Modal Bulk Actions | 🟢 | 2 | 019 · 083 | ✅ P | `@NeedsVerify` 019 |
| REQ-CUS-76 | Tick Mass Delete ẩn ô chọn nhóm | 🟢 | 1 | 084 | ✅ P | `@NeedsVerify` 084 |
| REQ-CUS-77 | Confirm Bulk Actions phải xác nhận | 🟢 | 1 | 085 | ✅ P | — |
| REQ-CUS-78 | Mass Delete xoá các KH đã chọn | 🟢 | 1 | 086 | ✅ P | chỉ có TC đa REQ này — FAIL vẫn quy được về REQ (6b ✅) |
| REQ-CUS-79 | Mass Delete khi chưa chọn KH nào | 🟢 | 1 | 087 | ✅ biên 0 KH | — |
| REQ-CUS-80 | Bulk gán nhóm cho KH đã chọn | 🟢 | 2 | 088 · 169 | ✅ P | `@NeedsVerify` 169 |
| REQ-CUS-81 | Bulk gán nhóm ghi đè nhóm cũ | 🟢 | 1 | 089 | ✅ P | — |
| REQ-CUS-82 | Bulk Confirm không chọn nhóm không được gỡ nhóm của KH âm thầm | 🟡 | 1 | 090 | ✅ N | `@KnownBug` 090 |
| REQ-CUS-83 | Trang Import có 4 hướng dẫn | 🟢 | 2 | 013 · 155 | ✅ P | `@NeedsVerify` 155 |
| REQ-CUS-84 | Bảng mẫu 27 cột, 4 cột bắt buộc | 🟢 | 1 | 013 | ✅ P | chỉ có TC đa REQ này — FAIL vẫn quy được về REQ (6b ✅) |
| REQ-CUS-85 | Form Import | 🟢 | 2 | 013 · 097 | ✅ P | `@NeedsVerify` 097 |
| REQ-CUS-86 | Chưa chọn file thì báo bắt buộc | 🟢 | 1 | 091 | ✅ N | — |
| REQ-CUS-87 | File CSV rỗng báo không đủ dòng | 🟢 | 1 | 092 | ✅ N | — |
| REQ-CUS-88 | Simulate hiển thị dữ liệu sẽ nhập | 🟢 | 2 | 093 · 095 | ✅ P | — |
| REQ-CUS-89 | Simulate không tạo khách hàng | 🟢 | 1 | 094 | ✅ P | — |
| REQ-CUS-90 | Simulate không đánh dấu dòng lỗi | 🟢 | 1 | 095 | ✅ P | chỉ có TC đa REQ này — FAIL vẫn quy được về REQ (6b ✅) |
| REQ-CUS-91 | Simulate hiển thị Country = 0 khi để trống | 🟢 | 1 | 096 | ✅ P | — |
| REQ-CUS-92 | Sau Simulate phải chọn lại file | 🟢 | 1 | 093 | ✅ P | chỉ có TC đa REQ này — FAIL vẫn quy được về REQ (6b ✅) |
| REQ-CUS-93 | Import thật tạo KH và contact chính | ⚪ | 1 | 098 | ✅ P | `@Skip` 098 |
| REQ-CUS-94 | Dòng trùng email không được nhập | ⚪ | 1 | 099 | ✅ P | `@Skip` 099 |
| REQ-CUS-95 | Download Sample tải file CSV mẫu | ⚪ | 1 | 100 | ✅ P | `@Skip` 100 |
| REQ-CUS-96 | Tab Notes có nút New Note và bảng ghi chú | 🟢 | 1 | 014 | ✅ P | chỉ có TC đa REQ này — FAIL vẫn quy được về REQ (6b ✅) |
| REQ-CUS-97 | New Note mở ô nhập | 🟢 | 2 | 014 · 102 | ✅ P | — |
| REQ-CUS-98 | Thêm note | 🟢 | 4 | 102 · 104 · 152 · 157 | ✅ P | `@NeedsVerify` 104 · 152 · 157 |
| REQ-CUS-99 | Note trống bị chặn | 🟡 | 1 | 103 | ✅ N | `@KnownBug` 103 |
| REQ-CUS-100 | Menu tab Notes có badge đếm số note | 🟢 | 2 | 102 · 106 | ✅ P | — |
| REQ-CUS-101 | Sửa note tại chỗ | 🟢 | 1 | 105 | ✅ P | — |
| REQ-CUS-102 | Xoá note | 🟢 | 1 | 106 | ✅ P | chỉ có TC đa REQ này — FAIL vẫn quy được về REQ (6b ✅) |
| REQ-CUS-103 | Tab Files có vùng upload và bảng tệp | 🟢 | 1 | 015 | ✅ P | — |
| REQ-CUS-104 | Chỉ nhận 10 đuôi tệp, tối đa 128 MB | 🟢 | 4 | 108 · 109 · 111 · 112 | ✅ P 9 đuôi · B 129 MB · nhiều tệp | `@NeedsVerify` 111 · 112 |
| REQ-CUS-105 | Tệp sai đuôi bị từ chối | 🟢 | 1 | 110 | ✅ N 5 đuôi | — |
| REQ-CUS-106 | Upload tệp hợp lệ | 🟢 | 1 | 107 | ✅ P | — |
| REQ-CUS-107 | Show to customers area mặc định tắt, bật được | 🟢 | 1 | 113 | ✅ P | — |
| REQ-CUS-108 | Tệp bật hiển thị xem được ở cổng khách hàng | ⚪ | 1 | 114 | ✅ P | `@Skip` 114 |
| REQ-CUS-109 | Modal gửi tệp qua email | 🟢 | 2 | 019 · 115 | ✅ P | `@NeedsVerify` 019 |
| REQ-CUS-110 | Send File để trống báo Email Address bắt buộc | 🟡 | 1 | 116 | ✅ N | `@KnownBug` 116 |
| REQ-CUS-111 | Gửi tệp qua email | ⚪ | 1 | 117 | ✅ P | `@Skip` 117 |
| REQ-CUS-112 | Xoá tệp | 🟢 | 1 | 118 | ✅ P | — |
| REQ-CUS-113 | Chọn khoảng thời gian có 7 lựa chọn, mặc định This Month | 🟢 | 2 | 016 · 119 | ✅ P | — |
| REQ-CUS-114 | Nội dung sao kê | 🟢 | 1 | 016 | ✅ P | chỉ có TC đa REQ này — FAIL vẫn quy được về REQ (6b ✅) |
| REQ-CUS-115 | Đổi khoảng thời gian nạp lại sao kê | 🟢 | 1 | 120 | ✅ P | — |
| REQ-CUS-116 | Chọn Period hiện 2 ô ngày | 🟢 | 2 | 121 · 124 | ✅ P · N ngày không hợp lệ · năm nhuận | `@NeedsVerify` 124 |
| REQ-CUS-117 | Khoảng ngày đảo ngược bị chặn | 🟡 | 2 | 122 · 123 | ✅ B From = To · From > To | `@KnownBug` 122 |
| REQ-CUS-118 | Nút Print và View PDF theo khoảng đang chọn | 🟢 | 2 | 016 · 125 | ✅ P | — |
| REQ-CUS-119 | PDF sao kê đúng nội dung | ⚪ | 1 | 126 | ✅ P | `@Skip` 126 |
| REQ-CUS-120 | Modal gửi sao kê qua email | 🟢 | 2 | 019 · 127 | ✅ P | `@NeedsVerify` 019 |
| REQ-CUS-121 | Gửi sao kê bắt buộc chọn người nhận | 🟡 | 1 | 128 | ✅ N | `@KnownBug` 128 |
| REQ-CUS-122 | Gửi sao kê thành công | ⚪ | 1 | 129 | ✅ P | `@Skip` 129 |
| REQ-CUS-123 | Vault rỗng có thông báo | 🟢 | 2 | 017 · 155 | ✅ P | `@NeedsVerify` 155 |
| REQ-CUS-124 | Modal Vault Entry | 🟢 | 3 | 017 · 019 · 167 | ✅ P | `@NeedsVerify` 019 · 167 |
| REQ-CUS-125 | 3 trường bắt buộc | 🟢 | 4 | 130 · 131 · 132 · 133 | ✅ N từng trường + tất cả | — |
| REQ-CUS-126 | Port chỉ nhận số trong khoảng 1–65535 | 🟡 | 2 | 134 · 135 | ✅ B 0 · 1 · 65535 · 65536 · 70000 · −1 | `@KnownBug` 135 |
| REQ-CUS-127 | Tạo vault entry | 🟢 | 4 | 136 · 154 · 158 · 159 | ✅ P | `@NeedsVerify` 154 · 158 · 159 |
| REQ-CUS-128 | Mật khẩu hiển thị dạng che cố định | 🟢 | 2 | 137 · 161 | ✅ P · bảo mật (161) | — |
| REQ-CUS-129 | Sửa entry: điền sẵn, Password không bắt buộc | 🟢 | 2 | 138 · 144 | ✅ P | — |
| REQ-CUS-130 | Lưu khi sửa | 🟢 | 1 | 139 | ✅ P | — |
| REQ-CUS-131 | Xoá vault entry | 🟢 | 1 | 140 | ✅ P | — |
| REQ-CUS-132 | Xem mật khẩu phải nhập lại mật khẩu đăng nhập | 🟢 | 1 | 141 | ✅ P | — |
| REQ-CUS-133 | Nhập đúng mật khẩu đăng nhập thì hiện mật khẩu vault | ⚪ | 1 | 142 | ✅ P | `@PersonalOnly` 142 |
| REQ-CUS-134 | Visibility giới hạn staff được xem entry | ⚪ | 1 | 143 | ✅ P | `@Skip` 143 |
| REQ-CUS-135 | Tab Map có 2 ô toạ độ | 🟢 | 1 | 018 | ✅ P | `@NeedsVerify` 018 |
| REQ-CUS-136 | Lưu toạ độ | 🟢 | 1 | 145 | ✅ P | chỉ có TC đa REQ này — FAIL vẫn quy được về REQ (6b ✅) |
| REQ-CUS-137 | Toạ độ phải là số trong khoảng hợp lệ | 🟡 | 3 | 145 · 146 · 147 | ✅ B ±90 · ±91 · ±180 · ±181 · chữ | `@KnownBug` 146 · 147 |
| REQ-CUS-138 | Hiển thị bản đồ khi có Google API key | ⚪ | 1 | 148 | ✅ P | `@Skip` 148 |
| REQ-CUS-139 | KH Inactive không có trong ô chọn khách hàng khi tạo mới các chứng từ khác | ⚪ | 1 | 077 | ✅ P | `@NeedsVerify` 077 |
| REQ-CUS-140 | Chứng từ đã lập trước khi KH chuyển Inactive giữ nguyên KH | ⚪ | 1 | 078 | ✅ P | `@NeedsVerify` 078 |
| REQ-CUS-141 | Import thật bỏ qua dòng lỗi và báo số dòng bị bỏ | ⚪ | 1 | 101 | ✅ P | `@Skip` 101 |

**Tổng kết:** 141/141 REQ có ≥ 1 TC · 0 REQ 🔴 thiếu · 0 REQ ngoài phạm vi viết TC.
- REQ chỉ có TC `@Skip` (14): 21 · 27 · 43 · 73 · 93 · 94 · 95 · 108 · 111 · 119 · 122 · 134 · 138 · 141 — đã có TC viết trước, chờ được phép chạy
- REQ 🟡 có TC `@KnownBug` (9): 31 · 32 · 82 · 99 · 110 · 117 · 121 · 126 · 137
- **Phép thử chiều ngược (6b):** 0 vi phạm. 15 TC đa REQ có đúng 1 REQ chỉ được kiểm ở chính TC đó (008:19 · 010:55 · 011:58 · 013:84 · 014:96 · 016:114 · 046:34 · 048:36 · 057:42 · 067:61 · 086:78 · 093:92 · 095:90 · 106:102 · 145:136) — được phép, FAIL vẫn quy được về REQ. Lần chạy đầu phát hiện 4 TC vi phạm (008 · 014 · 046 · 086) → đã gắn REQ vào TC thật sự kiểm REQ đó (048 · 072 · 080 · 088 · 102), không tạo TC mới

### Decision Table — Bulk Actions (3 điều kiện → 1 kết quả)

| Rule | Tích Mass Delete | Chọn nhóm | Có KH được tích | → Kết quả | TC |
|---|---|---|---|---|---|
| R1 | Có | — (ô nhóm bị ẩn) | Có | Xoá các KH đã tích, `Total customers deleted: <n>` | 086 |
| R2 | Có | — | Không | `Total customers deleted: 0`, không xoá gì | 087 |
| R3 | Không | Có | Có | Chuyển KH sang nhóm đã chọn (ghi đè nhóm cũ), không có thông báo | 088 · 089 |
| R4 | Không | Không | Có | **Mong đợi:** chặn / cảnh báo riêng, nhóm giữ nguyên (REQ-82 🟡) | 090 `@KnownBug` |
| R5 | Không | Có | Không | Không KH nào đổi nhóm | 169-a |
| R6 | Không | Không | Không | Không KH nào đổi | 169-b |

### State Transition — trạng thái KH

| Từ \ Hành động | Save / Save and create contact | Tắt công tắc Active | Bật công tắc Active | `Mark as active` | Delete / Mass Delete | Mở hồ sơ |
|---|---|---|---|---|---|---|
| *(chưa tồn tại)* | ✅ → Active — 006 · 009 · 072 | — | — | — | — | — |
| Active | — | ✅ → Inactive — 072 · 022 | ➖ đã bật | ➖ không có banner — 075 bước 4 | ✅ → Đã xoá — 080-a · 080-b · 086 | ✅ không banner — 075 |
| Inactive | — | ➖ đã tắt | ✅ → Active — 075 | ✅ → Active — 074 | ✅ → Đã xoá — 080-c | ✅ có banner — 073 |
| Có chứng từ (Active / Inactive) | — | ✅ chứng từ cũ giữ KH — 078 | — | — | ⛔ **Bị chặn** — 081 `@Skip` | — |
| Đã xoá | — | — | — | — | — | ⛔ trang `404 Page Not Found` — 080 · 086 |

## Bảng Đối Soát Evidence

| Ảnh evidence | Màn hình / trạng thái | TC dựa vào | Đầy đủ? |
|---|---|---|---|
| `customers_list_own_records_export_menu_viewport.png` | Danh sách lọc 3 KH test · Summary · menu Export · công tắc Active · nhãn nhóm | 001 · 002 · 008 · 033 · 072 · 088 | ✅ viewport |
| `customers_list_search_no_match_viewport.png` | Tìm không khớp | 029 | ✅ |
| `customer_new_form_details_default_fullpage.png` | Form tạo — tab Customer Details mặc định | 003 · 005 · 165 · 166 | ✅ full-page |
| `customer_new_form_company_required_viewport.png` | Lỗi Company trống | 041 | ✅ |
| `customer_profile_blank_company_saved_viewport.png` | Hồ sơ KH Company rỗng (hiện trạng lỗi REQ-31) · menu hồ sơ · 3 tab con | 010 · 011 · 042 (hiện trạng) | 🟡 viewport cắt ở mục `Tickets` — 4 mục cuối menu lấy từ ảnh Billing |
| `customer_new_form_duplicate_name_info_viewport.png` | Cảnh báo trùng tên, con trỏ ở VAT Number | 046 · 048 | ✅ |
| `customer_billing_shipping_copied_fullpage.png` | Billing & Shipping sau khi chép · đủ 19 mục menu · chỉ nút Save | 004 · 010 · 011 · 063 · 064 | ✅ full-page — chụp trên form **sửa**; trạng thái rỗng của form tạo chưa có (004 `@NeedsVerify`) |
| `customer_group_modal_required_viewport.png` | Hộp thoại nhóm — lỗi Name | 019-a · 057 | ✅ |
| `customer_admins_assign_modal_viewport.png` | Hộp thoại Assign Admin · bảng rỗng phía sau | 019-b · 067 · 068 | ✅ |
| `customer_profile_inactive_banner_viewport.png` | Banner KH Inactive | 073 · 074 | ✅ |
| `invoice_customer_dropdown_inactive_hidden_viewport.png` | Ô Customer của Invoice — KH Inactive bị ẩn | 076 · 077 (tham chiếu) | ✅ |
| `customers_bulk_actions_mass_delete_checked_viewport.png` | Bulk Actions — đã tích Mass Delete, ô nhóm ẩn | 019-c · 084 · 087 | 🟡 chưa có ảnh trạng thái mặc định (083 dựa vào lần đọc DOM ở REQ-75) |
| `customers_import_page_default_viewport.png` | Trang Import mặc định | 013 · 091 | ✅ |
| `customers_import_simulate_result_viewport.png` | Kết quả Simulate 5 dòng · Country `0` · ô tệp trống | 093 · 095 · 096 | ✅ |
| `customer_notes_list_empty_and_text_viewport.png` | Bảng Notes 3 dòng (2 rỗng — hiện trạng lỗi REQ-99) · số đếm `3` | 014 · 102 · 103 (hiện trạng) · 106 | ✅ |
| `customer_files_uploaded_png_visible_viewport.png` | Tab Files — 1 PNG đã upload, công tắc bật | 015 · 107 · 113 | 🚩 Không thấy nút `Choose from Google Drive` — `ASM-CUS-01` |
| `customer_statement_period_reversed_fullpage.png` | Statement — Period đảo ngược (hiện trạng lỗi REQ-117) · lịch chọn ngày | 016 · 121 · 122 (hiện trạng) | ✅ full-page |
| `customer_statement_send_modal_no_contact_viewport.png` | Hộp thoại Account Summary — KH không có contact | 019-f · 127 | 🟡 cắt ở nội dung mẫu — chân hộp thoại (`Close` · `Send`) lấy từ DOM |
| `customer_vault_entry_required_viewport.png` | Hộp thoại Vault Entry — 3 lỗi bắt buộc · lựa chọn mặc định | 017 · 130 | 🟡 cắt mất tiêu đề hộp thoại phía trên |
| `customer_vault_entry_created_masked_viewport.png` | Thẻ entry · Port 70000 (hiện trạng lỗi REQ-126) · mật khẩu che | 135 (hiện trạng) · 136 · 137 | ✅ |
| `customer_vault_view_password_modal_viewport.png` | Hộp thoại View Password | 141 | ✅ |
| `customer_map_invalid_coords_saved_viewport.png` | Tab Map — toạ độ `abc` / `999` đã lưu (hiện trạng lỗi REQ-137) | 018 · 146 · 147 (hiện trạng) | ✅ |
| *(không có ảnh — đọc DOM khi khảo sát)* | Tooltip, `href`, số lựa chọn dropdown, 15 rule bộ lọc, cấu hình upload | 023 · 025 · 030 · 032 · 036 · 059 · 061 · 062 · 104 · 119 · 125 | ✅ theo AC ghi số liệu DOM |

> **Đã mở 22/22 ảnh** trước khi ghi dòng TC đầu tiên.

### Vùng chưa có evidence

| Màn hình / trạng thái | TC gắn `@NeedsVerify` | Đề xuất recon bổ sung |
|---|---|---|
| Form tạo — tab Billing & Shipping lúc mới mở · tab Map lúc chưa lưu toạ độ | 004 · 018 | Chụp full-page form tạo tab Billing; chụp tab Map của KH mới |
| Đóng hộp thoại bằng Esc | 019 | Thử Esc trên 6 hộp thoại |
| Ký tự ngoài bảng Latin · xuống dòng · nội dung dài ở Company / Address / City / State / Zip / Note | 045 · 052 → 055 · 104 | Đọc `maxlength` của các ô; chụp hồ sơ sau khi lưu |
| Groups chọn 2 nhóm · Currency bị khoá · lỗi Company trên form sửa | 056 · 060 · 066 | Chụp nút Groups khi chọn nhiều; mở hồ sơ một KH có hoá đơn |
| Bộ lọc `Customers assigned to me` · bật Active từ danh sách · bỏ tích Mass Delete · Bulk khi chưa tích KH | 071 · 075 · 084 · 169 | Chạy lại trên KH test, chụp viewport |
| Form tạo Estimate / Proposal / Project · sửa hoá đơn của KH Inactive | 077 · 078 | Gộp vào lượt recon các module chứng từ (`INV` · `EST` · `PROP` · `PRJ`) |
| Tệp không phải CSV khi Import · tệp > 128 MB · chọn nhiều tệp · ngày Period gõ sai | 097 · 111 · 112 · 124 | Thử trên KH test, chụp thông báo |
| Bấm 2 lần · F5/Back · 2 tab cùng sửa · mất phiên · mất mạng | 149 → 154 · 162 · 163 | Chạy thử một lượt, chụp kết quả |
| Chưa đăng nhập · chèn mã script / câu lệnh CSDL | 155 → 159 | Chạy thử một lượt trên dữ liệu do TC tạo |
| Responsive · thứ tự Tab · Edge / Firefox | 164 → 168 | Chụp ở 1366 · 1024 · 768; ghi thứ tự Tab thật |

## Đối soát Field-Level Validation

| Field (màn hình) | Loại | Mục bảng đã phủ | Mục không áp dụng / chưa phủ — lý do |
|---|---|---|---|
| Company (form tạo/sửa) | Text | **8/8** — Required 041 · 066 · Min/Max 043 · 044 · Chỉ khoảng trắng 042 · Ký tự đặc biệt 045-d · XSS · SQLi 156 · Unicode/Emoji 045-a,b,c · Khoảng trắng đầu/cuối 045-e | — |
| VAT Number · Phone · Website | Text · Phone · Text | Giá trị tự do + để trống 049 · 050 · 051 (Phone: chỉ số, `+84`, chữ cái, `-` `.` khoảng trắng, ngoặc) | Định dạng / mã vùng / độ dài: **không áp dụng** — cố ý chấp nhận mọi chuỗi (`AMB-CUS-04`); độ dài tối đa chưa khảo sát (`ASM-CUS-05`); XSS đại diện ở Company |
| Address | Textarea | Xuống dòng · tiếng Việt 052 | Max length, bộ đếm ký tự: chưa có ràng buộc được khảo sát (`ASM-CUS-05`); HTML đại diện ở Note 157 |
| City · State · Zip Code | Text | Có dấu, ký tự đặc biệt, chữ số 053 · 054 · 055 | Như Address |
| Groups (form KH) · Assign Admin · Email to | Multi-select | Mặc định 003 · 068 · 127 · chọn nhiều / bỏ chọn 056 · 068 · bắt buộc (Email to) 128 | Giới hạn số lượng, tag trùng: không áp dụng — chọn từ danh sách cố định |
| Currency · Default Language · Country · khoảng thời gian Statement | Dropdown | Mặc định 003 · 119 · đủ lựa chọn 059 · 061 · 062 · 119 · đổi lựa chọn 059 · 061 · 062 · 120 · lựa chọn bị khoá 060 | Required: không áp dụng — đều có giá trị mặc định |
| Mass Delete · Share in projects · Visibility · Save Filter · công tắc Active / Show to customers area | Checkbox / Radio | Mặc định 017 · 083 · trạng thái thay đổi 039 · 072 · 075 · 084 · 113 · 144 · nhóm radio chỉ chọn 1 144 | Required: không áp dụng |
| Tệp ở tab Files | File Upload | Hợp lệ 10 đuôi 107 → 109 · sai đuôi 110 · quá 128 MB 111 · nhiều tệp 112 | Tệp 0 KB, tên tệp có ký tự đặc biệt, kéo-thả: **chưa phủ** — chưa khảo sát hành vi; kéo-thả cho cùng đường tải lên như bấm chọn. Đề xuất bổ sung ở lượt recon sau |
| Choose CSV File (Import) | File Upload | Bắt buộc 091 · tệp rỗng 092 · sai loại 097 | Max size, nhiều tệp: không áp dụng — không đặt giới hạn, ô nhận 1 tệp |
| Server Address · Username (Vault) | Text | Bắt buộc 130 → 132 · XSS 158 | Độ dài, ký tự đặc biệt: không có ràng buộc được khảo sát |
| Port (Vault) | Number | Min/Max · 0 · âm · tràn 134 · 135 · để trống 134-d | Số thập phân, chữ cái: ô số của trình duyệt không cho gõ chữ; thập phân REQ-126 chưa nêu — ghi nhận để hỏi khi chốt `AMB-CUS-28` |
| Password (Vault) | Password | Bắt buộc khi tạo 133 · không bắt buộc khi sửa 138 · 139 · che ký tự 017 · 137 · không lộ khi sao chép 161 · xem lại cần xác thực 141 · 142 | Độ dài / độ phức tạp: không áp dụng — ô lưu mật khẩu của hệ thống khác, không phải mật khẩu tài khoản |
| Short Description · Note description · Message (Send File) | Textarea | Note: trống 103 · tiếng Việt/emoji/nhiều dòng/2.000 ký tự 104 · HTML 157 · Short Description: HTML 159 | Message (Send File): không gửi email trên môi trường dùng chung (`AMB-CUS-25` ⏭️) |
| Latitude · Longitude (Map) | Number | Thập phân, biên ±90/±180, 0, âm 145 · ngoài khoảng, chữ 146 · 147 | — |
| Period From · To (Statement) | Date Range | Định dạng 121 · năm nhuận 121-b · ngày không tồn tại, chữ 124 · From > To 122 · From = To 123 | Giới hạn độ dài khoảng: không có ràng buộc |
| Ô tìm kiếm danh sách | Text | Khớp đủ / một phần 027 · số điện thoại 028 · không khớp 029 · chèn mã 160 | — |
| Preview Email Template (Statement) | Rich Text | Hiển thị nội dung mẫu 127 | Sanitize / dán định dạng: không gửi email trên môi trường dùng chung (`AMB-CUS-25` ⏭️) |
| Default password for all contacts (Import) | Text | Chỉ dùng ở Import thật 098 `@Skip` | Import thật chưa được phép (`AMB-CUS-12` ⏭️) |

## Đối soát loại kiểm thử (4 vòng)

| Vòng | Nhánh | Trạng thái | TC ID / Lý do |
|---|---|---|---|
| 1 | UI cơ bản | ✅ | 001 · 002 · 003 · 004 · 010 · 011 · 013 → 018 (12 TC · Bảng kiểm, không biến thể) |
| 1 | Open form | ✅ | 005 · 019 (2 TC · 8 biến thể) |
| 1 | Display | ✅ | Số Summary 001 · ngày giờ 008 · tiền tệ sao kê 016 · ảnh xem trước tệp 107 |
| 1 | Input valid data · Save · Verify data | ✅ | Tối thiểu 006 · đầy đủ 007 · đối chiếu danh sách 008 · Save and create contact 009 · sửa 012 (5 TC) |
| 2 | UI Behavior | ✅ | 037 · 047 · 084 · 121 · 014 bước 3–4 |
| 2 | Required | ✅ | 041 · 042 · 057 · 066 · 091 · 103 · 116 · 128 · 130 → 133 · 141 (13 TC · 15 biến thể) |
| 2 | Validation | ✅ | 043 · 044 · 045 · 049 → 055 · 092 · 097 · 104 · 108 → 112 · 124 · 134 · 135 · 137 · 146 · 147 (24 TC · 75 biến thể) — đối soát từng field ở [bảng trên](#đối-soát-field-level-validation) |
| 2 | Equivalence Partitioning | ✅ | Chuỗi tự do VAT/Phone/Website 049 → 051 · đuôi tệp hợp lệ / không hợp lệ 107 → 110 · KH Active / Inactive 076 |
| 2 | Boundary Value Analysis | ✅ | 043 · 044 · 111 · 122 · 123 · 134 · 135 · 145 · 146 · 147 (10 TC) |
| 2 | Business Rule | ✅ | Trùng tên chỉ cảnh báo 046 → 048 · ghi đè nhóm 089 · KH Inactive ẩn khỏi chứng từ 076 → 078 · khoá Currency 060 · chặn xoá KH có chứng từ 081 · Simulate không tạo KH 094 · mật khẩu che 137 |
| 2 | Decision Table | ✅ | Bulk Actions 6 rule — 086 · 087 · 088 · 089 · 090 · 169 |
| 2 | State Transition | ✅ | Bảng State Transition — 072 · 073 · 074 · 075 · 078 · 080 · 081 · 086 |
| 2 | Dependency | ✅ | Same as Customer Info / Copy Billing 063 · 064 · Active → ô chọn KH của chứng từ 076 → 078 · Customer Admin → bộ lọc 071 · Mass Delete ẩn ô nhóm 084 |
| 2 | Use Case / Scenario | ✅ | Tạo → đếm → xoá 021 · vòng đời note 102 · 105 · 106 · tạo → tìm → xoá 168 |
| 2 | Save / Edit / Delete | ✅ | KH 006 · 012 · 079 · 080 · 086 · admin 069 · 070 · note 102 · 105 · 106 · tệp 107 · 118 · vault 136 · 139 · 140 · toạ độ 145 |
| 2 | Error Guessing | ✅ | 149 → 154 (bấm 2 lần, F5/Back, 2 tab cùng sửa — 6 TC) |
| 3 | Permission | ➖ | Hệ thống **chỉ có role Admin** (user xác nhận 19-10-2026) — không có ma trận role × hành động để kiểm. Phần phân quyền theo đối tượng: visibility Vault 143 `@Skip` (`AMB-CUS-23` ⏭️, user chấp nhận 11-11-2026). Rà lại khi có role thứ hai / tài khoản staff không phải admin |
| 3 | Security | ✅ | Chưa đăng nhập 155 · chèn mã script / câu lệnh CSDL 156 → 160 · che & không lộ mật khẩu Vault 137 · 138 · 141 · 142 · 161 · visibility 143 `@Skip` · chặn đuôi tệp nguy hiểm 110 (13 TC · 28 biến thể). Đường dẫn tuyệt đối lộ ở tab Files (`RISK-CUS-04`) **không** kiểm — chuyển Dev/bảo mật đánh giá; pentest ngoài phạm vi QA chức năng |
| 3 | API | ➖ | Quyền gọi API của QA chưa xác nhận — mặc định **đội Dev xác minh** (`docs/requirements/README.md`, chốt 12-10-2026) |
| 3 | Database | ➖ | Không có quyền truy vấn CSDL — **đội Dev xác minh** (xoá mềm/cứng, dữ liệu rác sau Simulate) |
| 3 | Integration | ⏭️ | Email (gửi tệp 117, sao kê 129), Google Maps 148, cổng khách hàng 114 — có tích hợp nhưng **không** kiểm trên môi trường dùng chung. **Quyết định: user, 11-11-2026** (`AMB-CUS-25` ⏭️, `AMB-SYS-01`). Rà lại khi có hộp thư test / Google API key / tài khoản contact |
| 3 | Logging / Audit | ✅ | Dấu vết hiển thị trong app: `Added From` 102 · `created by` 136 · `Last updated by` 139 · `Date Assigned` 069. Nhật ký hoạt động hệ thống: đã đo **Access denied** (`AMB-SYS-01`) — chờ tài khoản Admin đầy đủ |
| 4 | Compatibility | ✅ | 168 (Edge · Firefox — 2 biến thể). Safari không kiểm — môi trường QA là Windows |
| 4 | Responsive / UI Stability | ✅ | 164 · 165 (1366×768 · 1024×768 · 768×1024 — 6 biến thể) |
| 4 | Accessibility | ✅ | Thứ tự Tab, viền focus 166 · thao tác hộp thoại bằng bàn phím 167 · Esc 019. WCAG đầy đủ / trình đọc màn hình: ngoài phạm vi — cần công cụ riêng |
| 4 | Performance | ➖ | Không có ngưỡng thời gian được cam kết — chỉ quan sát thô trang không treo với `All` ~2.900 KH (025-d). Đo tải thuộc đội Hạ tầng |
| 4 | Regression | ➖ | Module chưa có bug nào được đóng (`docs/bugs/customers/` chưa tồn tại) |
| 4 | E2E | ✅ | Customers → Invoice / Estimate / Proposal / Project 076 → 078. Luồng xuyên module sâu hơn → `/generate-cross-module-test-plan` |

## Rà soát đặc tính chất lượng (ISO/IEC 25010:2023)

| Đặc tính | Trạng thái | TC ID / Lý do |
|---|---|---|
| Functional Suitability | ✅ Có TC | 001 → 154 · 169 |
| Performance Efficiency | ➖ Ngoài phạm vi | Chỉ quan sát thô (025-d). Đo thời gian phản hồi / tải — **đội Hạ tầng**, chưa có công cụ |
| Compatibility | ✅ Có TC | Edge · Firefox 168 · liên thông với module chứng từ 076 → 078 · nhập CSV 091 → 097 |
| Interaction Capability | ✅ Có TC | Thông báo lỗi đúng ô 041 · 057 · 130 → 133 · chặn thao tác sai 079 · 085 · 090 · trạng thái rỗng 014 · 015 · 017 · 029 · tooltip 004 · 032 · 059 · bàn phím 166 · 167 |
| Reliability | ✅ Có TC | Bấm 2 lần / F5 / Back / 2 tab 149 → 154 · mất phiên 162 · mất mạng 163 |
| Security | ✅ Có TC | 110 · 137 · 141 · 142 · 155 → 161 · 143 `@Skip`. Pentest / quét lỗ hổng, đánh giá `RISK-CUS-04` — **đội Dev/bảo mật** |
| Maintainability | ➖ Không áp dụng | Đặc tính của mã nguồn — code review / static analysis của **đội Dev**, không kiểm bằng manual TC |
| Flexibility | ✅ Có TC | Responsive 164 · 165. Đổi ngôn ngữ giao diện (25 ngôn ngữ, `RISK-SYS-03`) **không** kiểm: đổi ngôn ngữ tài khoản dùng chung ảnh hưởng người khác — chờ tài khoản riêng, **QA** chịu trách nhiệm rà lại |
| Safety | ✅ Có TC | Cảnh báo trước thao tác không hồi lại: xác nhận xoá KH 079 · Bulk 085 · gỡ admin 070 · xoá note 106 · tệp 118 · vault 140 |

## Đối soát cột Automation

| Nền tảng | Yes | Partial | No | ⏸️ Hoãn |
|---|---|---|---|---|
| Web | 157 | 12 | 0 | 55 (41 `@NeedsVerify` · 14 `@Skip`) |

> `@KnownBug` (10 TC) **không** tính Hoãn — test FAIL chính là thứ phơi bug, vẫn automate. Thứ tự ưu tiên automate: `@Smoke` 001 → 019 → `@CriticalPath` → TC nhiều biến thể ở Validation/BVA (045 · 049 · 050 · 108 · 110 · 134 · 135 · 145 · 147) → phần còn lại.

### Điều kiện cần chuẩn bị

| # | Điều kiện | Ai cấp | Trạng thái | TC phụ thuộc |
|---|---|---|---|---|
| 1 | Môi trường / dữ liệu cô lập để bộ đếm Summary không bị người khác làm lệch (`RISK-CUS-01`) | DevOps | ⏳ Chưa có | 020 · 021 · 022 · 038 |
| 2 | Dữ liệu nền cố định: KH cá nhân có contact · KH đã có hoá đơn | QA / Dev seed | ⏳ Chưa có — đang dùng dữ liệu có sẵn | 031 · 060 |
| 3 | Môi trường riêng được tạo nhóm KH, lưu bộ lọc, Import thật, tải tệp, mở PDF, xoá KH có chứng từ | PO / DevOps | ⏳ Chưa có (`AMB-CUS-12` ⏭️) | 034 · 040 · 058 · 081 · 098 → 101 · 126 |
| 4 | Hộp thư test đọc được qua API | DevOps | ⏳ Chưa có (`AMB-CUS-25` ⏭️) | 117 · 129 |
| 5 | Google API key trên môi trường test | Admin hệ thống | ⏳ Chưa có (`AMB-SYS-01`) | 148 |
| 6 | Tài khoản contact cổng khách hàng · staff thứ hai không phải admin | PO / Admin hệ thống | ⏳ Chưa có | 114 · 143 |

### TC Partial · No · Hoãn

| TC ID | Nền tảng | Automation | Trục chặn | Điều kiện · phần kiểm tay · lý do |
|---|---|---|---|---|
| CRM_CUS_TC_020 · 021 · 022 · 038 | Web | Partial | 2 · chạy lại không cùng kết quả | Bộ đếm dùng chung — assert chênh lệch, cần điều kiện #1 để ổn định |
| CRM_CUS_TC_031 · 060 | Web | Partial | 2 · tiền đề không tự dựng | Dựa vào dữ liệu có sẵn — điều kiện #2 |
| CRM_CUS_TC_058 | Web | Partial | 2 · môi trường dùng chung | Tạo master data không xoá được — điều kiện #3 |
| CRM_CUS_TC_117 · 129 | Web | Partial | 1 · Email | Điều kiện #4 |
| CRM_CUS_TC_148 | Web | Partial | 1 · Map | Kiểm ghim trên bản đồ bằng mắt; điều kiện #5 |
| CRM_CUS_TC_164 · 165 | Web | Partial | 2 · Expected cần mắt người | Phần tử hiện/bấm được → tự động; "không đè chữ" kiểm tay |
| 14 TC `@Skip` | Web | Yes / Partial · ⏸️ Hoãn | — | Chờ điều kiện #3 → #6 — xem [Nhãn đặc biệt](#nhãn-đặc-biệt-trong-bộ-tc) |
| 41 TC `@NeedsVerify` | Web | Yes / Partial · ⏸️ Hoãn | — | Chờ recon xong — xem [Vùng chưa có evidence](#vùng-chưa-có-evidence) |
| CRM_CUS_TC_142 | Web | Yes · `@PersonalOnly` | — | Script dùng `PASSWORD_ADMIN` từ `.env` được; lượt chạy tay do QA tự làm (`AMB-CUS-22`) |

## Nhật ký thay đổi

| Ngày | Nguồn | Thay đổi | Mốc git trước khi ghi |
|---|---|---|---|
| 11-11-2026 | User yêu cầu sửa 2 TC theo [báo cáo review](review/testcase_review_report_web_20261111.md) (`/review-testcases` — chỉ 2 TC, không chạy Mode FIX đầy đủ) | `CRM_CUS_TC_156-d`: thay chuỗi `DROP TABLE` bằng lệnh xoá **chỉ khớp tên KH của chính biến thể** (`… WHERE company LIKE 'auto_cus_tc156d_%'`); thêm bước ghi Total trước/sau và bước dọn — đếm đúng 1 dòng rồi xoá bằng liên kết `Delete`. `CRM_CUS_TC_021`: tách bước 4 cũ thành 4 → 6, thêm chốt *"đúng 2 dòng, đúng tên KH test — khác thì dừng"* trước Mass Delete. Không đổi TC ID, số TC, số biến thể | Không có mốc git — thư mục `docs/testcases/customers/` chưa từng được commit (user chọn sửa không mốc) |
| 11-11-2026 | `/generate-testcases-from-requirements` (QUICK · GỘP) | Khởi tạo bộ TC web: 169 TC · 272 biến thể · 6 part · phủ 141/141 REQ theo requirements bản 2.1. 14 `@Skip` · 10 `@KnownBug` · 41 `@NeedsVerify` · 1 `@PersonalOnly`. Ghi nhận 1 xung đột tài liệu ↔ ảnh (`ASM-CUS-01` — nút Google Drive ở tab Files) | `c816e8a` — chưa có bộ TC `customers` nào trong git |
