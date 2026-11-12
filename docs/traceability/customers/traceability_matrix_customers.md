# Ma Trận Truy Vết (RTM) — Customers (`CUS`)

> **Tóm tắt:** 141/141 REQ đã có TC · **0/169 TC có script automation** · lần chạy gần nhất 127 PASS / 19 FAIL, 23 TC chưa có kết quả · **17 REQ đang có TC FAIL** (9 là ứng viên bug đã biết) và 22 REQ chưa có bằng chứng chạy — độ phủ trên giấy đủ, độ phủ đã kiểm chứng mới 99/141

| Thông tin | Nội dung |
|---|---|
| Ngày sinh | 12-11-2026 |
| Phạm vi | Module `CUS` · web — mục đích: báo cáo coverage trước release |
| Mốc git | `c816e8a` — TC/script sửa sau mốc này thì RTM đã cũ |
| Nguồn đã quét | REQ: `requirements/customers/` — index + 8 file Story (141 REQ: 🟢 113 · 🟡 9 · ⚪ 19) · TC: `testcases/customers/web/parts/` (6 part · 169 TC · 272 biến thể) · Automation: **không tìm thấy script** (`*.spec.*`, `*Test.java`, `test_*.py`, chuỗi `CRM_CUS_TC` trong mã nguồn) — ẩn cột `Automation` · Checklist: `checklist_smoke_20261112.md` (19 mục, TC-based) · Kết quả chạy: `run_1794391198` (bộ TC, 169/169 TC) — lần chạy checklist `run_1794468325` không tính · Bug: `docs/bugs/README.md` — **chưa có bug nào của `CUS`** |
| Tổng hợp | [../traceability_matrix.md](../traceability_matrix.md) — ⬛ module ngoài tầm nhìn của RTM nằm ở đó |

## 1. Tổng quan

| Chỉ số | Tiến độ | Giá trị |
|---|---|---|
| Requirement Coverage | `▰▰▰▰▰▰▰▰▰▰` | **141/141** (100%) REQ |
| Platform Coverage · web | `▰▰▰▰▰▰▰▰▰▰` | 141/141 cặp REQ × nền tảng |
| Automation Coverage | `▱▱▱▱▱▱▱▱▱▱` | **0/169** TC (0%) |
| Pass rate lần chạy gần nhất | `▰▰▰▰▰▰▰▰▰▱` | 127/146 (87%) · ❌ 19 FAIL · 23 TC chưa có kết quả (18 BLOCKED · 5 SKIPPED) |
| REQ đã kiểm chứng đủ (mọi TC PASS) | `▰▰▰▰▰▰▰▱▱▱` | 99/141 · 17 REQ có TC FAIL · 3 REQ mới PASS một phần, phần còn lại chưa chạy · 22 REQ chưa có kết quả nào |

> Pass rate ở đây = PASS / (PASS + FAIL), cùng cách tính với RTM `LOGIN`. Execution report `run_1794391198` ghi **77.4%** vì tính cả BLOCKED vào mẫu số (127/164) — hai con số không mâu thuẫn, chỉ khác mẫu số.

**Phân bổ REQ theo trạng thái:**

| ✅ Đã automate | 🟣 Automate một phần | 🟡 Thủ công | 🟠 Chỉ checklist | 🔴 Chưa có TC | ⏭️ Loại trừ |
|:-:|:-:|:-:|:-:|:-:|:-:|
| 0 | 0 | 141 | 0 | 0 | 0 |

*Không có REQ ⏭️: index TC không có bảng "REQ cố ý KHÔNG có TC" — REQ của 3 AMB ⏭️ (`AMB-CUS-12` · `23` · `25`) vẫn được viết TC, đánh `@Skip` và nằm trong mẫu số.*

## 2. Việc cần làm

1. ❌ **19 TC FAIL, chưa TC nào có bug** (`docs/bugs/` chưa có file `BUG_*` của `CUS`):
   - **10 TC `@KnownBug`** — 042 · 044 · 090 · 103 · 116 · 122 · 128 · 135 · 146 · 147 — FAIL đúng dự kiến, phủ đủ 9 REQ 🟡 ứng viên bug (REQ-CUS-31 · 32 · 82 · 99 · 110 · 117 · 121 · 126 · 137). `REQ-CUS-31` user đã yêu cầu mở bug; 8 REQ còn lại chờ user quyết → `/create-bug-report`. REQ-82 · 117 · 126 · 137 còn treo nguyên văn thông báo (`AMB-CUS-28` 🟡)
   - **7 TC `@NeedsVerify`** — 018 · 045 · 124 · 153 · 156 · 157 · 164 — chưa rõ lỗi hệ thống hay TC/REQ lệch UI; 156 · 157 (chuỗi `<img …>` làm mất chữ, để lại thẻ thật) và 153 (bấm đúp công tắc Active lưu sai 3/10 lần) nghiêng về lỗi thật → đối chiếu REQ/TC (`/review-testcases` mode FIX) rồi mới `/create-bug-report`
   - **2 TC không gắn tag** — 032 (vị trí tooltip cột Active) · 059 (`System Default` trong danh sách Currency hiển thị trống) — nghi lệch TC ↔ UI → đối chiếu như trên
2. ▶️ **23 TC chưa có kết quả → 22 REQ chưa có bằng chứng chạy:**
   - 14 TC `@Skip` (034 · 040 · 058 · 081 · 098 · 099 · 100 · 101 · 114 · 117 · 126 · 129 · 143 · 148) — chờ môi trường riêng / PO cho phép Import thật, tải tệp, gửi email, cổng khách hàng, staff thứ hai, Google API key
   - 4 TC Mass Delete bị auto-skip trên môi trường dùng chung (021 · 085 · 086 · 087) — trong đó **086 là Critical `@CriticalPath`** → chạy có giám sát hoặc trên môi trường riêng bằng `/execute-test-cases`
   - 2 TC thiếu bước (071 · 078) → `/review-testcases` mode FIX bổ sung bước, rồi chạy lại
   - 142 `@PersonalOnly` — user tự chạy tay (xem mật khẩu Vault, `AMB-CUS-22`) · 110 biến thể `c` (tệp `.exe`) chạy tay có giám sát · 168 cần Edge + Firefox
3. 🟡 **Bắt đầu automate:** 37 TC Critical/High `Automation = Yes` đã PASS và không bị hoãn (bỏ `@Skip` · `@NeedsVerify` · `@PersonalOnly`) — 14 TC trong đó là `@Smoke` / `@CriticalPath`, làm trước → `/generate-automation-from-testcases`

## 3. Ma trận chi tiết

### 3.1. Danh sách, Summary, tìm kiếm, bộ lọc, Export (STORY-CUS-01)

`▰▰▰▰▰` 27/27 REQ có TC · `▰▰▰▰▱` 21/27 REQ mọi TC PASS

| REQ ID | Yêu cầu | TC IDs | Kết quả gần nhất | Trạng thái |
|---|---|---|:-:|---|
| REQ-CUS-01 | Summary hiển thị 6 bộ đếm | CRM_CUS_TC_001, CRM_CUS_TC_155 | ✅ 2 | 🟡 Thủ công |
| REQ-CUS-02 | Total Customers bằng tổng bản ghi của bảng | CRM_CUS_TC_020 | ✅ 1 | 🟡 Thủ công |
| REQ-CUS-03 | Total Customers thay đổi theo tạo/xoá | CRM_CUS_TC_021 | ⏩ 1 | 🟡 Thủ công |
| REQ-CUS-04 | Inactive Customers tăng khi tắt Active | CRM_CUS_TC_022 | ✅ 1 | 🟡 Thủ công |
| REQ-CUS-05 | Thanh công cụ 3 nút + nút lọc | CRM_CUS_TC_001, CRM_CUS_TC_005 | ✅ 2 | 🟡 Thủ công |
| REQ-CUS-06 | Bảng có 9 cột theo thứ tự cố định | CRM_CUS_TC_002, CRM_CUS_TC_164 | ✅ 1 · ❌ 1 | 🟡 Thủ công |
| REQ-CUS-07 | 8 cột sắp xếp được | CRM_CUS_TC_023, CRM_CUS_TC_024 | ✅ 2 | 🟡 Thủ công |
| REQ-CUS-08 | Sắp xếp mặc định theo Company tăng dần | CRM_CUS_TC_002, CRM_CUS_TC_023 | ✅ 2 | 🟡 Thủ công |
| REQ-CUS-09 | Số dòng mỗi trang: 10 / 25 / 50 / 100 / All, mặc định 25 | CRM_CUS_TC_002, CRM_CUS_TC_025 | ✅ 2 | 🟡 Thủ công |
| REQ-CUS-10 | Phân trang Previous / số trang / Next + ô nhảy trang | CRM_CUS_TC_002, CRM_CUS_TC_026 | ✅ 2 | 🟡 Thủ công |
| REQ-CUS-11 | Dòng thông tin số bản ghi | CRM_CUS_TC_002, CRM_CUS_TC_027 | ✅ 2 | 🟡 Thủ công |
| REQ-CUS-12 | Tìm kiếm theo Company | CRM_CUS_TC_008, CRM_CUS_TC_027, CRM_CUS_TC_160, CRM_CUS_TC_168 | ✅ 3 · 🚫 1 | 🟡 Thủ công |
| REQ-CUS-13 | Tìm kiếm theo Phone | CRM_CUS_TC_028 | ✅ 1 | 🟡 Thủ công |
| REQ-CUS-14 | Tìm không khớp hiển thị trạng thái rỗng | CRM_CUS_TC_029 | ✅ 1 | 🟡 Thủ công |
| REQ-CUS-15 | Dòng KH công ty có 3 thao tác | CRM_CUS_TC_030 | ✅ 1 | 🟡 Thủ công |
| REQ-CUS-16 | Dòng KH không có Company hiển thị `Person - View Profile` | CRM_CUS_TC_031 | ✅ 1 | 🟡 Thủ công |
| REQ-CUS-17 | Cột Active là switch, tiêu đề có tooltip | CRM_CUS_TC_032 | ❌ 1 | 🟡 Thủ công |
| REQ-CUS-18 | Cột Groups hiển thị nhãn nhóm | CRM_CUS_TC_008, CRM_CUS_TC_088 | ✅ 2 | 🟡 Thủ công |
| REQ-CUS-19 | Date Created hiển thị ngày giờ đầy đủ | CRM_CUS_TC_008 | ✅ 1 | 🟡 Thủ công |
| REQ-CUS-20 | Menu Export có 4 lựa chọn | CRM_CUS_TC_033 | ✅ 1 | 🟡 Thủ công |
| REQ-CUS-21 | Export tạo tệp đúng dữ liệu đang hiển thị | CRM_CUS_TC_034 | 🚫 1 | 🟡 Thủ công · ⚪ REQ chưa kiểm chứng |
| REQ-CUS-22 | Menu lọc gồm New Filter + bộ lọc đã lưu | CRM_CUS_TC_035 | ✅ 1 | 🟡 Thủ công |
| REQ-CUS-23 | Create Filter có 15 rule | CRM_CUS_TC_036 | ✅ 1 | 🟡 Thủ công |
| REQ-CUS-24 | Apply chỉ bấm được khi rule có giá trị | CRM_CUS_TC_037 | ✅ 1 | 🟡 Thủ công |
| REQ-CUS-25 | Lọc Active = No trả đúng KH Inactive | CRM_CUS_TC_038 | ✅ 1 | 🟡 Thủ công |
| REQ-CUS-26 | Bộ lọc chưa lưu không còn sau khi tải lại | CRM_CUS_TC_039 | ✅ 1 | 🟡 Thủ công |
| REQ-CUS-27 | Lưu bộ lọc bằng Save Filter | CRM_CUS_TC_040 | 🚫 1 | 🟡 Thủ công · ⚪ REQ chưa kiểm chứng |

### 3.2. Tạo khách hàng (STORY-CUS-02)

`▰▰▰▰▰` 27/27 REQ có TC · `▰▰▰▰▱` 22/27 REQ mọi TC PASS

| REQ ID | Yêu cầu | TC IDs | Kết quả gần nhất | Trạng thái |
|---|---|---|:-:|---|
| REQ-CUS-28 | Form tạo mới có 2 tab | CRM_CUS_TC_003, CRM_CUS_TC_005, CRM_CUS_TC_155 | ✅ 3 | 🟡 Thủ công |
| REQ-CUS-29 | Tab Customer Details có 12 field hiển thị | CRM_CUS_TC_003, CRM_CUS_TC_007, CRM_CUS_TC_165, CRM_CUS_TC_166 | ✅ 4 | 🟡 Thủ công |
| REQ-CUS-30 | Company bắt buộc | CRM_CUS_TC_041, CRM_CUS_TC_066 | ✅ 2 | 🟡 Thủ công |
| REQ-CUS-31 | Company chỉ gồm khoảng trắng bị chặn như bỏ trống | CRM_CUS_TC_042 | ❌ 1 | 🟡 Thủ công · 🟡 ứng viên bug |
| REQ-CUS-32 | Company tối đa 191 ký tự, vượt thì bị chặn kèm thông báo | CRM_CUS_TC_043, CRM_CUS_TC_044 | ✅ 1 · ❌ 1 | 🟡 Thủ công · 🟡 ứng viên bug |
| REQ-CUS-33 | Cảnh báo khi tên công ty đã tồn tại | CRM_CUS_TC_046, CRM_CUS_TC_048 | ✅ 2 | 🟡 Thủ công |
| REQ-CUS-34 | Kiểm trùng không phân biệt hoa thường | CRM_CUS_TC_046 | ✅ 1 | 🟡 Thủ công |
| REQ-CUS-35 | Tên không trùng không hiện cảnh báo | CRM_CUS_TC_047 | ✅ 1 | 🟡 Thủ công |
| REQ-CUS-36 | Cảnh báo trùng không chặn lưu | CRM_CUS_TC_048 | ✅ 1 | 🟡 Thủ công |
| REQ-CUS-37 | VAT Number không kiểm định dạng | CRM_CUS_TC_049 | ✅ 1 | 🟡 Thủ công |
| REQ-CUS-38 | Phone không kiểm định dạng | CRM_CUS_TC_050 | ✅ 1 | 🟡 Thủ công |
| REQ-CUS-39 | Website không kiểm định dạng | CRM_CUS_TC_051 | ✅ 1 | 🟡 Thủ công |
| REQ-CUS-40 | Groups chọn nhiều nhóm, mặc định trống | CRM_CUS_TC_003, CRM_CUS_TC_056 | ✅ 2 | 🟡 Thủ công |
| REQ-CUS-41 | Nút `+` mở modal thêm nhóm | CRM_CUS_TC_019, CRM_CUS_TC_057 | ✅ 2 | 🟡 Thủ công |
| REQ-CUS-42 | Tên nhóm bắt buộc | CRM_CUS_TC_057 | ✅ 1 | 🟡 Thủ công |
| REQ-CUS-43 | Tạo nhóm mới từ modal | CRM_CUS_TC_058 | 🚫 1 | 🟡 Thủ công · ⚪ REQ chưa kiểm chứng |
| REQ-CUS-44 | Currency có 3 lựa chọn, mặc định System Default | CRM_CUS_TC_003, CRM_CUS_TC_059 | ✅ 1 · ❌ 1 | 🟡 Thủ công |
| REQ-CUS-45 | Không đổi được Currency khi KH đã có giao dịch | CRM_CUS_TC_060 | ✅ 1 | 🟡 Thủ công · ⚪ REQ chưa kiểm chứng |
| REQ-CUS-46 | Default Language có 27 lựa chọn | CRM_CUS_TC_003, CRM_CUS_TC_061 | ✅ 2 | 🟡 Thủ công |
| REQ-CUS-47 | Country có 251 lựa chọn | CRM_CUS_TC_003, CRM_CUS_TC_062 | ✅ 2 | 🟡 Thủ công |
| REQ-CUS-48 | Save tạo KH và mở hồ sơ | CRM_CUS_TC_006, CRM_CUS_TC_007, CRM_CUS_TC_045, CRM_CUS_TC_149, CRM_CUS_TC_150, CRM_CUS_TC_156, CRM_CUS_TC_162, CRM_CUS_TC_163, CRM_CUS_TC_168 | ✅ 6 · ❌ 2 · 🚫 1 | 🟡 Thủ công |
| REQ-CUS-49 | Save and create contact mở modal thêm contact | CRM_CUS_TC_009 | ✅ 1 | 🟡 Thủ công |
| REQ-CUS-50 | KH mới tạo ở trạng thái Active | CRM_CUS_TC_008, CRM_CUS_TC_072 | ✅ 2 | 🟡 Thủ công |
| REQ-CUS-51 | Tab Billing & Shipping gồm 2 khối địa chỉ | CRM_CUS_TC_004 | ✅ 1 | 🟡 Thủ công |
| REQ-CUS-52 | Same as Customer Info chép địa chỉ sang Billing | CRM_CUS_TC_063 | ✅ 1 | 🟡 Thủ công |
| REQ-CUS-53 | Copy Billing Address chép Billing sang Shipping | CRM_CUS_TC_064 | ✅ 1 | 🟡 Thủ công |
| REQ-CUS-54 | Địa chỉ Billing/Shipping được lưu | CRM_CUS_TC_065 | ✅ 1 | 🟡 Thủ công |

### 3.3. Hồ sơ KH, sửa, Customer Admins (STORY-CUS-03)

`▰▰▰▰▰` 11/11 REQ có TC · `▰▰▰▰▰` 10/11 REQ mọi TC PASS

| REQ ID | Yêu cầu | TC IDs | Kết quả gần nhất | Trạng thái |
|---|---|---|:-:|---|
| REQ-CUS-55 | Hồ sơ KH có menu 19 tab | CRM_CUS_TC_010 | ✅ 1 | 🟡 Thủ công |
| REQ-CUS-56 | Tiêu đề hồ sơ `#<id> <Company>` kèm menu chỉ có Delete | CRM_CUS_TC_010, CRM_CUS_TC_080 | ✅ 2 | 🟡 Thủ công |
| REQ-CUS-57 | Tab Profile có 3 tab con | CRM_CUS_TC_011, CRM_CUS_TC_067 | ✅ 2 | 🟡 Thủ công |
| REQ-CUS-58 | Form sửa chỉ có nút Save | CRM_CUS_TC_011 | ✅ 1 | 🟡 Thủ công |
| REQ-CUS-59 | Sửa KH lưu đủ field | CRM_CUS_TC_012, CRM_CUS_TC_052, CRM_CUS_TC_053, CRM_CUS_TC_054, CRM_CUS_TC_055, CRM_CUS_TC_066, CRM_CUS_TC_151 | ✅ 7 | 🟡 Thủ công |
| REQ-CUS-60 | Tiêu đề tab trình duyệt của hồ sơ là tên Company | CRM_CUS_TC_010, CRM_CUS_TC_012 | ✅ 2 | 🟡 Thủ công |
| REQ-CUS-61 | Bảng Customer Admins | CRM_CUS_TC_067 | ✅ 1 | 🟡 Thủ công |
| REQ-CUS-62 | Modal Assign Admin chọn nhiều staff | CRM_CUS_TC_019, CRM_CUS_TC_068 | ✅ 2 | 🟡 Thủ công |
| REQ-CUS-63 | Gán Customer Admin | CRM_CUS_TC_069 | ✅ 1 | 🟡 Thủ công |
| REQ-CUS-64 | Gỡ Customer Admin | CRM_CUS_TC_070 | ✅ 1 | 🟡 Thủ công |
| REQ-CUS-65 | Customer Admin có tác dụng ở bộ lọc "Customers assigned to me" | CRM_CUS_TC_071 | 🚫 1 | 🟡 Thủ công · ⚪ REQ chưa kiểm chứng |

### 3.4. Active, xoá KH, Bulk Actions (STORY-CUS-04)

`▰▰▰▰▰` 19/19 REQ có TC · `▰▰▰▱▱` 10/19 REQ mọi TC PASS

| REQ ID | Yêu cầu | TC IDs | Kết quả gần nhất | Trạng thái |
|---|---|---|:-:|---|
| REQ-CUS-66 | Tắt Active ngay trên danh sách | CRM_CUS_TC_072, CRM_CUS_TC_075, CRM_CUS_TC_153 | ✅ 2 · ❌ 1 | 🟡 Thủ công |
| REQ-CUS-67 | Hồ sơ KH Inactive có banner cảnh báo | CRM_CUS_TC_073 | ✅ 1 | 🟡 Thủ công |
| REQ-CUS-68 | Mark as active kích hoạt lại KH | CRM_CUS_TC_074 | ✅ 1 | 🟡 Thủ công |
| REQ-CUS-69 | KH Inactive không có trong ô chọn khách hàng của Invoice | CRM_CUS_TC_076 | ✅ 1 | 🟡 Thủ công |
| REQ-CUS-70 | Xoá KH phải xác nhận | CRM_CUS_TC_079 | ✅ 1 | 🟡 Thủ công |
| REQ-CUS-71 | Xoá KH thành công | CRM_CUS_TC_080, CRM_CUS_TC_168 | ✅ 1 · 🚫 1 | 🟡 Thủ công |
| REQ-CUS-72 | Hồ sơ KH đã xoá trả 404 | CRM_CUS_TC_080, CRM_CUS_TC_086 | ✅ 1 · ⏩ 1 | 🟡 Thủ công |
| REQ-CUS-73 | Xoá KH đang có chứng từ | CRM_CUS_TC_081 | 🚫 1 | 🟡 Thủ công · ⚪ REQ chưa kiểm chứng |
| REQ-CUS-74 | Chọn tất cả dòng đang hiển thị | CRM_CUS_TC_082 | ✅ 1 | 🟡 Thủ công |
| REQ-CUS-75 | Modal Bulk Actions | CRM_CUS_TC_019, CRM_CUS_TC_083 | ✅ 2 | 🟡 Thủ công |
| REQ-CUS-76 | Tick Mass Delete ẩn ô chọn nhóm | CRM_CUS_TC_084 | ✅ 1 | 🟡 Thủ công |
| REQ-CUS-77 | Confirm Bulk Actions phải xác nhận | CRM_CUS_TC_085 | ⏩ 1 | 🟡 Thủ công |
| REQ-CUS-78 | Mass Delete xoá các KH đã chọn | CRM_CUS_TC_086 | ⏩ 1 | 🟡 Thủ công |
| REQ-CUS-79 | Mass Delete khi chưa chọn KH nào | CRM_CUS_TC_087 | ⏩ 1 | 🟡 Thủ công |
| REQ-CUS-80 | Bulk gán nhóm cho KH đã chọn | CRM_CUS_TC_088, CRM_CUS_TC_169 | ✅ 2 | 🟡 Thủ công |
| REQ-CUS-81 | Bulk gán nhóm ghi đè nhóm cũ | CRM_CUS_TC_089 | ✅ 1 | 🟡 Thủ công |
| REQ-CUS-82 | Bulk Confirm không chọn nhóm không được gỡ nhóm của KH âm thầm | CRM_CUS_TC_090 | ❌ 1 | 🟡 Thủ công · 🟡 ứng viên bug |
| REQ-CUS-139 | KH Inactive không có trong ô chọn khách hàng khi tạo mới các chứng từ khác | CRM_CUS_TC_077 | ✅ 1 | 🟡 Thủ công · ⚪ REQ chưa kiểm chứng |
| REQ-CUS-140 | Chứng từ đã lập trước khi KH chuyển Inactive giữ nguyên KH | CRM_CUS_TC_078 | 🚫 1 | 🟡 Thủ công · ⚪ REQ chưa kiểm chứng |

### 3.5. Import Customers (STORY-CUS-05)

`▰▰▰▰▰` 14/14 REQ có TC · `▰▰▰▰▱` 10/14 REQ mọi TC PASS

| REQ ID | Yêu cầu | TC IDs | Kết quả gần nhất | Trạng thái |
|---|---|---|:-:|---|
| REQ-CUS-83 | Trang Import có 4 hướng dẫn | CRM_CUS_TC_013, CRM_CUS_TC_155 | ✅ 2 | 🟡 Thủ công |
| REQ-CUS-84 | Bảng mẫu 27 cột, 4 cột bắt buộc | CRM_CUS_TC_013 | ✅ 1 | 🟡 Thủ công |
| REQ-CUS-85 | Form Import | CRM_CUS_TC_013, CRM_CUS_TC_097 | ✅ 2 | 🟡 Thủ công |
| REQ-CUS-86 | Chưa chọn file thì báo bắt buộc | CRM_CUS_TC_091 | ✅ 1 | 🟡 Thủ công |
| REQ-CUS-87 | File CSV rỗng báo không đủ dòng | CRM_CUS_TC_092 | ✅ 1 | 🟡 Thủ công |
| REQ-CUS-88 | Simulate hiển thị dữ liệu sẽ nhập | CRM_CUS_TC_093, CRM_CUS_TC_095 | ✅ 2 | 🟡 Thủ công |
| REQ-CUS-89 | Simulate không tạo khách hàng | CRM_CUS_TC_094 | ✅ 1 | 🟡 Thủ công |
| REQ-CUS-90 | Simulate không đánh dấu dòng lỗi | CRM_CUS_TC_095 | ✅ 1 | 🟡 Thủ công |
| REQ-CUS-91 | Simulate hiển thị Country = 0 khi để trống | CRM_CUS_TC_096 | ✅ 1 | 🟡 Thủ công |
| REQ-CUS-92 | Sau Simulate phải chọn lại file | CRM_CUS_TC_093 | ✅ 1 | 🟡 Thủ công |
| REQ-CUS-93 | Import thật tạo KH và contact chính | CRM_CUS_TC_098 | 🚫 1 | 🟡 Thủ công · ⚪ REQ chưa kiểm chứng |
| REQ-CUS-94 | Dòng trùng email không được nhập | CRM_CUS_TC_099 | 🚫 1 | 🟡 Thủ công · ⚪ REQ chưa kiểm chứng |
| REQ-CUS-95 | Download Sample tải file CSV mẫu | CRM_CUS_TC_100 | 🚫 1 | 🟡 Thủ công · ⚪ REQ chưa kiểm chứng |
| REQ-CUS-141 | Import thật bỏ qua dòng lỗi và báo số dòng bị bỏ | CRM_CUS_TC_101 | 🚫 1 | 🟡 Thủ công · ⚪ REQ chưa kiểm chứng |

### 3.6. Notes & Files (STORY-CUS-06)

`▰▰▰▰▰` 17/17 REQ có TC · `▰▰▰▱▱` 11/17 REQ mọi TC PASS

| REQ ID | Yêu cầu | TC IDs | Kết quả gần nhất | Trạng thái |
|---|---|---|:-:|---|
| REQ-CUS-96 | Tab Notes có nút New Note và bảng ghi chú | CRM_CUS_TC_014 | ✅ 1 | 🟡 Thủ công |
| REQ-CUS-97 | New Note mở ô nhập | CRM_CUS_TC_014, CRM_CUS_TC_102 | ✅ 2 | 🟡 Thủ công |
| REQ-CUS-98 | Thêm note | CRM_CUS_TC_102, CRM_CUS_TC_104, CRM_CUS_TC_152, CRM_CUS_TC_157 | ✅ 3 · ❌ 1 | 🟡 Thủ công |
| REQ-CUS-99 | Note trống bị chặn | CRM_CUS_TC_103 | ❌ 1 | 🟡 Thủ công · 🟡 ứng viên bug |
| REQ-CUS-100 | Menu tab Notes có badge đếm số note | CRM_CUS_TC_102, CRM_CUS_TC_106 | ✅ 2 | 🟡 Thủ công |
| REQ-CUS-101 | Sửa note tại chỗ | CRM_CUS_TC_105 | ✅ 1 | 🟡 Thủ công |
| REQ-CUS-102 | Xoá note | CRM_CUS_TC_106 | ✅ 1 | 🟡 Thủ công |
| REQ-CUS-103 | Tab Files có vùng upload và bảng tệp | CRM_CUS_TC_015 | ✅ 1 | 🟡 Thủ công |
| REQ-CUS-104 | Chỉ nhận 10 đuôi tệp, tối đa 128 MB | CRM_CUS_TC_108, CRM_CUS_TC_109, CRM_CUS_TC_111, CRM_CUS_TC_112 | ✅ 4 | 🟡 Thủ công |
| REQ-CUS-105 | Tệp sai đuôi bị từ chối | CRM_CUS_TC_110 | 🚫 1 | 🟡 Thủ công |
| REQ-CUS-106 | Upload tệp hợp lệ | CRM_CUS_TC_107 | ✅ 1 | 🟡 Thủ công |
| REQ-CUS-107 | Show to customers area mặc định tắt, bật được | CRM_CUS_TC_113 | ✅ 1 | 🟡 Thủ công |
| REQ-CUS-108 | Tệp bật hiển thị xem được ở cổng khách hàng | CRM_CUS_TC_114 | 🚫 1 | 🟡 Thủ công · ⚪ REQ chưa kiểm chứng |
| REQ-CUS-109 | Modal gửi tệp qua email | CRM_CUS_TC_019, CRM_CUS_TC_115 | ✅ 2 | 🟡 Thủ công |
| REQ-CUS-110 | Send File để trống báo Email Address bắt buộc | CRM_CUS_TC_116 | ❌ 1 | 🟡 Thủ công · 🟡 ứng viên bug |
| REQ-CUS-111 | Gửi tệp qua email | CRM_CUS_TC_117 | 🚫 1 | 🟡 Thủ công · ⚪ REQ chưa kiểm chứng |
| REQ-CUS-112 | Xoá tệp | CRM_CUS_TC_118 | ✅ 1 | 🟡 Thủ công |

### 3.7. Statement (sao kê) (STORY-CUS-07)

`▰▰▰▰▰` 10/10 REQ có TC · `▰▰▰▱▱` 5/10 REQ mọi TC PASS

| REQ ID | Yêu cầu | TC IDs | Kết quả gần nhất | Trạng thái |
|---|---|---|:-:|---|
| REQ-CUS-113 | Chọn khoảng thời gian có 7 lựa chọn, mặc định This Month | CRM_CUS_TC_016, CRM_CUS_TC_119 | ✅ 2 | 🟡 Thủ công |
| REQ-CUS-114 | Nội dung sao kê | CRM_CUS_TC_016 | ✅ 1 | 🟡 Thủ công |
| REQ-CUS-115 | Đổi khoảng thời gian nạp lại sao kê | CRM_CUS_TC_120 | ✅ 1 | 🟡 Thủ công |
| REQ-CUS-116 | Chọn Period hiện 2 ô ngày | CRM_CUS_TC_121, CRM_CUS_TC_124 | ✅ 1 · ❌ 1 | 🟡 Thủ công |
| REQ-CUS-117 | Khoảng ngày đảo ngược bị chặn | CRM_CUS_TC_122, CRM_CUS_TC_123 | ✅ 1 · ❌ 1 | 🟡 Thủ công · 🟡 ứng viên bug |
| REQ-CUS-118 | Nút Print và View PDF theo khoảng đang chọn | CRM_CUS_TC_016, CRM_CUS_TC_125 | ✅ 2 | 🟡 Thủ công |
| REQ-CUS-119 | PDF sao kê đúng nội dung | CRM_CUS_TC_126 | 🚫 1 | 🟡 Thủ công · ⚪ REQ chưa kiểm chứng |
| REQ-CUS-120 | Modal gửi sao kê qua email | CRM_CUS_TC_019, CRM_CUS_TC_127 | ✅ 2 | 🟡 Thủ công |
| REQ-CUS-121 | Gửi sao kê bắt buộc chọn người nhận | CRM_CUS_TC_128 | ❌ 1 | 🟡 Thủ công · 🟡 ứng viên bug |
| REQ-CUS-122 | Gửi sao kê thành công | CRM_CUS_TC_129 | 🚫 1 | 🟡 Thủ công · ⚪ REQ chưa kiểm chứng |

### 3.8. Vault & Map (STORY-CUS-08)

`▰▰▰▰▰` 16/16 REQ có TC · `▰▰▰▱▱` 10/16 REQ mọi TC PASS

| REQ ID | Yêu cầu | TC IDs | Kết quả gần nhất | Trạng thái |
|---|---|---|:-:|---|
| REQ-CUS-123 | Vault rỗng có thông báo | CRM_CUS_TC_017, CRM_CUS_TC_155 | ✅ 2 | 🟡 Thủ công |
| REQ-CUS-124 | Modal Vault Entry | CRM_CUS_TC_017, CRM_CUS_TC_019, CRM_CUS_TC_167 | ✅ 3 | 🟡 Thủ công |
| REQ-CUS-125 | 3 trường bắt buộc | CRM_CUS_TC_130, CRM_CUS_TC_131, CRM_CUS_TC_132, CRM_CUS_TC_133 | ✅ 4 | 🟡 Thủ công |
| REQ-CUS-126 | Port chỉ nhận số trong khoảng 1–65535 | CRM_CUS_TC_134, CRM_CUS_TC_135 | ✅ 1 · ❌ 1 | 🟡 Thủ công · 🟡 ứng viên bug |
| REQ-CUS-127 | Tạo vault entry | CRM_CUS_TC_136, CRM_CUS_TC_154, CRM_CUS_TC_158, CRM_CUS_TC_159 | ✅ 4 | 🟡 Thủ công |
| REQ-CUS-128 | Mật khẩu hiển thị dạng che cố định | CRM_CUS_TC_137, CRM_CUS_TC_161 | ✅ 2 | 🟡 Thủ công |
| REQ-CUS-129 | Sửa entry: điền sẵn, Password không bắt buộc | CRM_CUS_TC_138, CRM_CUS_TC_144 | ✅ 2 | 🟡 Thủ công |
| REQ-CUS-130 | Lưu khi sửa | CRM_CUS_TC_139 | ✅ 1 | 🟡 Thủ công |
| REQ-CUS-131 | Xoá vault entry | CRM_CUS_TC_140 | ✅ 1 | 🟡 Thủ công |
| REQ-CUS-132 | Xem mật khẩu phải nhập lại mật khẩu đăng nhập | CRM_CUS_TC_141 | ✅ 1 | 🟡 Thủ công |
| REQ-CUS-133 | Nhập đúng mật khẩu đăng nhập thì hiện mật khẩu vault | CRM_CUS_TC_142 | ⏩ 1 | 🟡 Thủ công · ⚪ REQ chưa kiểm chứng |
| REQ-CUS-134 | Visibility giới hạn staff được xem entry | CRM_CUS_TC_143 | 🚫 1 | 🟡 Thủ công · ⚪ REQ chưa kiểm chứng |
| REQ-CUS-135 | Tab Map có 2 ô toạ độ | CRM_CUS_TC_018 | ❌ 1 | 🟡 Thủ công |
| REQ-CUS-136 | Lưu toạ độ | CRM_CUS_TC_145 | ✅ 1 | 🟡 Thủ công |
| REQ-CUS-137 | Toạ độ phải là số trong khoảng hợp lệ | CRM_CUS_TC_145, CRM_CUS_TC_146, CRM_CUS_TC_147 | ✅ 1 · ❌ 2 | 🟡 Thủ công · 🟡 ứng viên bug |
| REQ-CUS-138 | Hiển thị bản đồ khi có Google API key | CRM_CUS_TC_148 | 🚫 1 | 🟡 Thủ công · ⚪ REQ chưa kiểm chứng |

> Checklist smoke (19 mục, TC-based — `TC ID liên quan` = 001 → 019 · 079 · 080) chỉ dùng để xét trạng thái 🟠, không in thành cột. Kết quả chạy checklist `run_1794468325` (✅ 17 · ❌ 1 — mục 15, cùng lỗi với `CRM_CUS_TC_018` · 🚫 1 — mục 19) **không** cộng vào cột `Kết quả gần nhất`.

## 4. Kết quả thực thi từng TC

Nguồn: [`run_1794391198`](../../executions/customers/web/run_1794391198/execution_report.md) — bộ TC, chạy 11-11-2026 → 12-11-2026 (8 phiên), 169/169 TC có kết quả. Chưa có `retest_*` nào cho `CUS`; lần chạy checklist `run_1794468325` không tính. Mỗi TC chỉ có một lần chạy nên kết quả dưới đây cũng là kết quả mới nhất.

| Mức ưu tiên | ✅ PASS | ❌ FAIL | 🚫 BLOCKED | ⏩ SKIPPED | Tổng |
|---|:-:|:-:|:-:|:-:|:-:|
| Critical | 7 | 0 | 0 | 1 | 8 |
| High | 41 | 5 | 7 | 2 | 55 |
| Medium | 61 | 8 | 9 | 2 | 80 |
| Low | 18 | 6 | 2 | 0 | 26 |
| **Tổng** | **127** | **19** | **18** | **5** | **169** |

*FAIL chi tiết (bước tái hiện, Expected/Actual) xem mục 3 của [execution report](../../executions/customers/web/run_1794391198/execution_report.md#3-chi-tiết-tc-fail) theo số `FAIL #n` ở cột Ghi chú. Thư mục evidence của lần chạy có 25 ảnh, chủ yếu cho TC FAIL — TC không có ảnh ghi `—`.*

<details><summary>Kết quả đầy đủ 169 TC (xếp theo TC ID)</summary>

| TC ID | REQ | Kết quả | Bước fail | Ghi chú | Evidence |
|---|---|:-:|---|---|---|
| CRM_CUS_TC_001 | 01 · 05 | ✅ PASS | — | — | — |
| CRM_CUS_TC_002 | 06 · 08 · 09 · 10 · 11 | ✅ PASS | — | — | — |
| CRM_CUS_TC_003 | 28 · 29 · 40 · 44 · 46 · 47 | ✅ PASS | — | — | — |
| CRM_CUS_TC_004 | 51 | ✅ PASS | — | — | — |
| CRM_CUS_TC_005 | 05 · 28 | ✅ PASS | — | — | — |
| CRM_CUS_TC_006 | 48 | ✅ PASS | — | — | — |
| CRM_CUS_TC_007 | 48 · 29 | ✅ PASS | — | — | — |
| CRM_CUS_TC_008 | 50 · 12 · 18 · 19 | ✅ PASS | — | — | [ảnh](../../executions/customers/web/run_1794391198/evidence/CRM_CUS_TC_008_kh_moi_tren_danh_sach.png) |
| CRM_CUS_TC_009 | 49 | ✅ PASS | — | — | — |
| CRM_CUS_TC_010 | 55 · 56 · 60 | ✅ PASS | — | — | — |
| CRM_CUS_TC_011 | 57 · 58 | ✅ PASS | — | — | — |
| CRM_CUS_TC_012 | 59 · 60 | ✅ PASS | — | — | — |
| CRM_CUS_TC_013 | 83 · 84 · 85 | ✅ PASS | — | — | — |
| CRM_CUS_TC_014 | 96 · 97 | ✅ PASS | — | — | [ảnh](../../executions/customers/web/run_1794391198/evidence/CRM_CUS_TC_014_tab_notes_rong.png) |
| CRM_CUS_TC_015 | 103 | ✅ PASS | — | — | [ảnh](../../executions/customers/web/run_1794391198/evidence/CRM_CUS_TC_015_tab_files_rong.png) |
| CRM_CUS_TC_016 | 113 · 114 · 118 | ✅ PASS | — | — | — |
| CRM_CUS_TC_017 | 123 · 124 | ✅ PASS | — | — | — |
| CRM_CUS_TC_018 | 135 | ❌ FAIL | 2 · mục `1` | `@NeedsVerify` · xem FAIL #1 | [ảnh](../../executions/customers/web/run_1794391198/evidence/CRM_CUS_TC_018_step2_thieu_open_in_google_map.png) |
| CRM_CUS_TC_019 | 41 · 62 · 75 · 109 · 120 · 124 | ✅ PASS | — | — | — |
| CRM_CUS_TC_020 | 02 | ✅ PASS | — | — | — |
| CRM_CUS_TC_021 | 03 | ⏩ SKIPPED | — | Bước 5–6 tích chọn tất cả + `Mass Delete` — thao tác phá huỷ trên môi trường dùng chung (quy tắc Auto-Skip), cần chạy tay có giám sát | — |
| CRM_CUS_TC_022 | 04 | ✅ PASS | — | — | — |
| CRM_CUS_TC_023 | 07 · 08 | ✅ PASS | — | — | — |
| CRM_CUS_TC_024 | 07 | ✅ PASS | — | — | — |
| CRM_CUS_TC_025 | 09 | ✅ PASS | — | — | — |
| CRM_CUS_TC_026 | 10 | ✅ PASS | — | — | — |
| CRM_CUS_TC_027 | 12 · 11 | ✅ PASS | — | — | — |
| CRM_CUS_TC_028 | 13 | ✅ PASS | — | — | — |
| CRM_CUS_TC_029 | 14 | ✅ PASS | — | — | — |
| CRM_CUS_TC_030 | 15 | ✅ PASS | — | — | — |
| CRM_CUS_TC_031 | 16 | ✅ PASS | — | — | — |
| CRM_CUS_TC_032 | 17 | ❌ FAIL | 1 | nghi lệch TC ↔ UI · xem FAIL #2 | [ảnh](../../executions/customers/web/run_1794391198/evidence/CRM_CUS_TC_032_step1_khong_co_tooltip.png) |
| CRM_CUS_TC_033 | 20 | ✅ PASS | — | — | — |
| CRM_CUS_TC_034 | 21 | 🚫 BLOCKED | — | `@Skip` — chưa được phép tải tệp trên môi trường dùng chung (`AMB-CUS-12` ⏭️) | — |
| CRM_CUS_TC_035 | 22 | ✅ PASS | — | — | — |
| CRM_CUS_TC_036 | 23 | ✅ PASS | — | — | — |
| CRM_CUS_TC_037 | 24 | ✅ PASS | — | — | — |
| CRM_CUS_TC_038 | 25 | ✅ PASS | — | — | — |
| CRM_CUS_TC_039 | 26 | ✅ PASS | — | — | — |
| CRM_CUS_TC_040 | 27 | 🚫 BLOCKED | — | `@Skip` — bộ lọc lưu dùng chung toàn hệ thống, chưa được phép tạo trên môi trường dùng chung (`AMB-CUS-21`) | — |
| CRM_CUS_TC_041 | 30 | ✅ PASS | — | — | — |
| CRM_CUS_TC_042 | 31 | ❌ FAIL | 3 · biến thể `a`, `b` | `@KnownBug` (`AMB-CUS-01`) — dự kiến FAIL · xem FAIL #3 | [ảnh](../../executions/customers/web/run_1794391198/evidence/CRM_CUS_TC_042_step3_tao_duoc_kh_ten_rong.png) |
| CRM_CUS_TC_043 | 32 | ✅ PASS | — | — | — |
| CRM_CUS_TC_044 | 32 | ❌ FAIL | 3 · biến thể `a`, `b` | `@KnownBug` (`AMB-CUS-02`) — dự kiến FAIL · xem FAIL #4 | [ảnh](../../executions/customers/web/run_1794391198/evidence/CRM_CUS_TC_044_step3_luu_va_cat_191.png) |
| CRM_CUS_TC_045 | 48 | ❌ FAIL | 4 · biến thể `e` | `@NeedsVerify` · xem FAIL #5 | [ảnh](../../executions/customers/web/run_1794391198/evidence/CRM_CUS_TC_045_step4_bien_the_e_con_khoang_trang.png) |
| CRM_CUS_TC_046 | 33 · 34 | ✅ PASS | — | — | — |
| CRM_CUS_TC_047 | 35 | ✅ PASS | — | — | — |
| CRM_CUS_TC_048 | 36 · 33 | ✅ PASS | — | — | — |
| CRM_CUS_TC_049 | 37 | ✅ PASS | — | — | — |
| CRM_CUS_TC_050 | 38 | ✅ PASS | — | — | — |
| CRM_CUS_TC_051 | 39 | ✅ PASS | — | — | — |
| CRM_CUS_TC_052 | 59 | ✅ PASS | — | — | — |
| CRM_CUS_TC_053 | 59 | ✅ PASS | — | — | — |
| CRM_CUS_TC_054 | 59 | ✅ PASS | — | — | — |
| CRM_CUS_TC_055 | 59 | ✅ PASS | — | — | — |
| CRM_CUS_TC_056 | 40 | ✅ PASS | — | — | — |
| CRM_CUS_TC_057 | 41 · 42 | ✅ PASS | — | — | — |
| CRM_CUS_TC_058 | 43 | 🚫 BLOCKED | — | `@Skip` — nhóm KH dùng chung, không xoá được vì khu Setup bị chặn (`RISK-CUS-03`, `AMB-SYS-01`) | — |
| CRM_CUS_TC_059 | 44 | ❌ FAIL | 3 | nghi lệch TC ↔ UI · xem FAIL #6 | [ảnh](../../executions/customers/web/run_1794391198/evidence/CRM_CUS_TC_059_step3_lua_chon_dau_trong.png) |
| CRM_CUS_TC_060 | 45 | ✅ PASS | — | — | — |
| CRM_CUS_TC_061 | 46 | ✅ PASS | — | — | — |
| CRM_CUS_TC_062 | 47 | ✅ PASS | — | — | — |
| CRM_CUS_TC_063 | 52 | ✅ PASS | — | — | — |
| CRM_CUS_TC_064 | 53 | ✅ PASS | — | — | — |
| CRM_CUS_TC_065 | 54 | ✅ PASS | — | — | — |
| CRM_CUS_TC_066 | 59 · 30 | ✅ PASS | — | — | — |
| CRM_CUS_TC_067 | 61 · 57 | ✅ PASS | — | — | — |
| CRM_CUS_TC_068 | 62 | ✅ PASS | — | — | — |
| CRM_CUS_TC_069 | 63 | ✅ PASS | — | — | — |
| CRM_CUS_TC_070 | 64 | ✅ PASS | — | — | — |
| CRM_CUS_TC_071 | 65 | 🚫 BLOCKED | 2 | Không làm được bước 2 theo đúng TC: thêm rule `Customers assigned to me` thì hiện 2 nút chọn `Yes` · `No` chưa chọn và `Apply` bị khoá (như `TC_037… | — |
| CRM_CUS_TC_072 | 66 · 50 | ✅ PASS | — | — | — |
| CRM_CUS_TC_073 | 67 | ✅ PASS | — | — | — |
| CRM_CUS_TC_074 | 68 | ✅ PASS | — | — | — |
| CRM_CUS_TC_075 | 66 | ✅ PASS | — | — | — |
| CRM_CUS_TC_076 | 69 | ✅ PASS | — | — | — |
| CRM_CUS_TC_077 | 139 | ✅ PASS | — | — | — |
| CRM_CUS_TC_078 | 140 | 🚫 BLOCKED | 1 | Không làm được bước 1 theo đúng TC: chọn KH rồi `Save as Draft` → hệ thống chặn `Enter at least one item.` — TC thiếu bước thêm dòng hàng | — |
| CRM_CUS_TC_079 | 70 | ✅ PASS | — | — | — |
| CRM_CUS_TC_080 | 71 · 56 · 72 | ✅ PASS | — | — | — |
| CRM_CUS_TC_081 | 73 | 🚫 BLOCKED | — | `@Skip` — không được xoá dữ liệu có chứng từ trên môi trường dùng chung, chỉ chạy trên môi trường riêng | — |
| CRM_CUS_TC_082 | 74 | ✅ PASS | — | — | — |
| CRM_CUS_TC_083 | 75 | ✅ PASS | — | — | — |
| CRM_CUS_TC_084 | 76 | ✅ PASS | — | — | — |
| CRM_CUS_TC_085 | 77 | ⏩ SKIPPED | — | Steps có `Mass Delete` → `Confirm` — thao tác phá huỷ trên môi trường dùng chung (quy tắc Auto-Skip, nhất quán `TC_021`), cần chạy tay có giám sát | — |
| CRM_CUS_TC_086 | 78 · 72 | ⏩ SKIPPED | — | Steps chọn tất cả + `Mass Delete` → `OK` — quy tắc Auto-Skip | — |
| CRM_CUS_TC_087 | 79 | ⏩ SKIPPED | — | Steps `Mass Delete` → `Confirm` → `OK` — quy tắc Auto-Skip (dù bảng rỗng, lỗi chọn dòng sẽ xoá dữ liệu người khác) | — |
| CRM_CUS_TC_088 | 80 · 18 | ✅ PASS | — | — | — |
| CRM_CUS_TC_089 | 81 | ✅ PASS | — | — | — |
| CRM_CUS_TC_090 | 82 | ❌ FAIL | 3 | `@KnownBug` (`AMB-CUS-10`) — dự kiến FAIL · xem FAIL #7 | [ảnh](../../executions/customers/web/run_1794391198/evidence/CRM_CUS_TC_090_step4_nhom_bi_go_am_tham.png) |
| CRM_CUS_TC_091 | 86 | ✅ PASS | — | — | — |
| CRM_CUS_TC_092 | 87 | ✅ PASS | — | — | — |
| CRM_CUS_TC_093 | 88 · 92 | ✅ PASS | — | — | — |
| CRM_CUS_TC_094 | 89 | ✅ PASS | — | — | — |
| CRM_CUS_TC_095 | 90 · 88 | ✅ PASS | — | — | — |
| CRM_CUS_TC_096 | 91 | ✅ PASS | — | — | — |
| CRM_CUS_TC_097 | 85 | ✅ PASS | — | — | — |
| CRM_CUS_TC_098 | 93 | 🚫 BLOCKED | — | `@Skip` — Import thật chưa được phép trên môi trường dùng chung, có thể gửi email tới contact (`AMB-CUS-12` ⏭️) | — |
| CRM_CUS_TC_099 | 94 | 🚫 BLOCKED | — | `@Skip` — như `TC_098` | — |
| CRM_CUS_TC_100 | 95 | 🚫 BLOCKED | — | `@Skip` — chưa được phép tải tệp trên môi trường dùng chung (`AMB-CUS-12` ⏭️) | — |
| CRM_CUS_TC_101 | 141 | 🚫 BLOCKED | — | `@Skip` — như `TC_098` | — |
| CRM_CUS_TC_102 | 98 · 100 · 97 | ✅ PASS | — | — | — |
| CRM_CUS_TC_103 | 99 | ❌ FAIL | 3 · biến thể `a`, `b` | `@KnownBug` (`AMB-CUS-15`) — dự kiến FAIL · xem FAIL #8 | [ảnh](../../executions/customers/web/run_1794391198/evidence/CRM_CUS_TC_103_step4_bien_the_a_tao_note_rong.png) |
| CRM_CUS_TC_104 | 98 | ✅ PASS | — | — | — |
| CRM_CUS_TC_105 | 101 | ✅ PASS | — | — | — |
| CRM_CUS_TC_106 | 102 · 100 | ✅ PASS | — | — | — |
| CRM_CUS_TC_107 | 106 | ✅ PASS | — | — | — |
| CRM_CUS_TC_108 | 104 | ✅ PASS | — | — | — |
| CRM_CUS_TC_109 | 104 | ✅ PASS | — | — | — |
| CRM_CUS_TC_110 | 105 | 🚫 BLOCKED | — | Chưa verify đủ 5 biến thể: `c` (`.exe`) bị bỏ qua theo quy tắc Auto-Skip (tải lên tệp thực thi trên môi trường dùng chung). 4 biến thể đã chạy đều… | — |
| CRM_CUS_TC_111 | 104 | ✅ PASS | — | — | — |
| CRM_CUS_TC_112 | 104 | ✅ PASS | — | — | — |
| CRM_CUS_TC_113 | 107 | ✅ PASS | — | — | — |
| CRM_CUS_TC_114 | 108 | 🚫 BLOCKED | — | `@Skip` — cổng khách hàng ngoài phạm vi đợt này, cần contact đăng nhập được cổng khách hàng | — |
| CRM_CUS_TC_115 | 109 | ✅ PASS | — | — | — |
| CRM_CUS_TC_116 | 110 | ❌ FAIL | 3–4 | `@KnownBug` (`AMB-CUS-16`) — dự kiến FAIL · xem FAIL #9 | [ảnh](../../executions/customers/web/run_1794391198/evidence/CRM_CUS_TC_116_step4_hop_thoai_dong_khong_bao_loi.png) |
| CRM_CUS_TC_117 | 111 | 🚫 BLOCKED | — | `@Skip` — không gửi email trên môi trường dùng chung (`AMB-CUS-25` ⏭️), cần hộp thư test | — |
| CRM_CUS_TC_118 | 112 | ✅ PASS | — | — | — |
| CRM_CUS_TC_119 | 113 | ✅ PASS | — | — | — |
| CRM_CUS_TC_120 | 115 | ✅ PASS | — | — | — |
| CRM_CUS_TC_121 | 116 | ✅ PASS | — | — | — |
| CRM_CUS_TC_122 | 117 | ❌ FAIL | 4 | `@KnownBug` (`AMB-CUS-17`) — dự kiến FAIL · xem FAIL #10 | [ảnh](../../executions/customers/web/run_1794391198/evidence/CRM_CUS_TC_122_step4_khoang_dao_nguoc_van_nap.png) |
| CRM_CUS_TC_123 | 117 | ✅ PASS | — | — | — |
| CRM_CUS_TC_124 | 116 | ❌ FAIL | 3 · biến thể `a`, `b` | `@NeedsVerify` · xem FAIL #11 | [ảnh](../../executions/customers/web/run_1794391198/evidence/CRM_CUS_TC_124_step3_bien_the_a_ngay_31_02.png) |
| CRM_CUS_TC_125 | 118 | ✅ PASS | — | — | — |
| CRM_CUS_TC_126 | 119 | 🚫 BLOCKED | — | `@Skip` — chưa được phép mở/tải PDF (`AMB-CUS-12` ⏭️) | — |
| CRM_CUS_TC_127 | 120 | ✅ PASS | — | — | — |
| CRM_CUS_TC_128 | 121 | ❌ FAIL | 3–4 | `@KnownBug` (`AMB-CUS-18`) — dự kiến FAIL · xem FAIL #12 | [ảnh](../../executions/customers/web/run_1794391198/evidence/CRM_CUS_TC_128_step4_hop_thoai_dong.png) |
| CRM_CUS_TC_129 | 122 | 🚫 BLOCKED | — | `@Skip` — không gửi email trên môi trường dùng chung (`AMB-CUS-25` ⏭️), cần hộp thư test | — |
| CRM_CUS_TC_130 | 125 | ✅ PASS | — | — | — |
| CRM_CUS_TC_131 | 125 | ✅ PASS | — | — | — |
| CRM_CUS_TC_132 | 125 | ✅ PASS | — | — | — |
| CRM_CUS_TC_133 | 125 | ✅ PASS | — | — | — |
| CRM_CUS_TC_134 | 126 | ✅ PASS | — | — | — |
| CRM_CUS_TC_135 | 126 | ❌ FAIL | 4–5 · biến thể `a`, `b`, `c`, `d` | `@KnownBug` (`AMB-CUS-19`) — dự kiến FAIL · xem FAIL #13 | [ảnh](../../executions/customers/web/run_1794391198/evidence/CRM_CUS_TC_135_step5_port_ngoai_khoang_van_luu.png) |
| CRM_CUS_TC_136 | 127 | ✅ PASS | — | — | [ảnh](../../executions/customers/web/run_1794391198/evidence/CRM_CUS_TC_136_the_vault_entry.png) |
| CRM_CUS_TC_137 | 128 | ✅ PASS | — | — | — |
| CRM_CUS_TC_138 | 129 | ✅ PASS | — | — | — |
| CRM_CUS_TC_139 | 130 | ✅ PASS | — | — | — |
| CRM_CUS_TC_140 | 131 | ✅ PASS | — | — | — |
| CRM_CUS_TC_141 | 132 | ✅ PASS | — | — | — |
| CRM_CUS_TC_142 | 133 | ⏩ SKIPPED | — | `@PersonalOnly` — QA tự chạy thủ công, tự gõ mật khẩu đăng nhập (`AMB-CUS-22`) | — |
| CRM_CUS_TC_143 | 134 | 🚫 BLOCKED | — | `@Skip` — cần tài khoản staff thứ hai không phải administrator (`AMB-CUS-23` ⏭️) | — |
| CRM_CUS_TC_144 | 129 | ✅ PASS | — | — | — |
| CRM_CUS_TC_145 | 136 · 137 | ✅ PASS | — | — | — |
| CRM_CUS_TC_146 | 137 | ❌ FAIL | 3–4 · biến thể `a`, `b`, `c` | `@KnownBug` (`AMB-CUS-20`) — dự kiến FAIL · xem FAIL #14 | [ảnh](../../executions/customers/web/run_1794391198/evidence/CRM_CUS_TC_146_step4_bien_the_a_luu_abc.png) |
| CRM_CUS_TC_147 | 137 | ❌ FAIL | 3–4 · biến thể `a`, `b`, `c`, `d` | `@KnownBug` (`AMB-CUS-20`) — dự kiến FAIL · xem FAIL #15 | [ảnh](../../executions/customers/web/run_1794391198/evidence/CRM_CUS_TC_147_step4_bien_the_d_luu_xyz.png) |
| CRM_CUS_TC_148 | 138 | 🚫 BLOCKED | — | `@Skip` — cấu hình Google API key nằm ở Setup, đang bị chặn (`AMB-SYS-01`) | — |
| CRM_CUS_TC_149 | 48 | ✅ PASS | — | — | — |
| CRM_CUS_TC_150 | 48 | ✅ PASS | — | — | — |
| CRM_CUS_TC_151 | 59 | ✅ PASS | — | — | — |
| CRM_CUS_TC_152 | 98 | ✅ PASS | — | — | — |
| CRM_CUS_TC_153 | 66 | ❌ FAIL | 3 | `@NeedsVerify` · xem FAIL #16 | [ảnh](../../executions/customers/web/run_1794391198/evidence/CRM_CUS_TC_153_step3_cong_tac_tat_sau_f5.png) |
| CRM_CUS_TC_154 | 127 | ✅ PASS | — | — | — |
| CRM_CUS_TC_155 | 01 · 28 · 83 · 123 | ✅ PASS | — | — | — |
| CRM_CUS_TC_156 | 48 | ❌ FAIL | 3–4 · biến thể `b` | `@NeedsVerify` · xem FAIL #17 | [ảnh](../../executions/customers/web/run_1794391198/evidence/CRM_CUS_TC_156_step3_bien_the_b_ten_thanh_the_img.png) |
| CRM_CUS_TC_157 | 98 | ❌ FAIL | 3 · biến thể `b` | `@NeedsVerify` · xem FAIL #18 | [ảnh](../../executions/customers/web/run_1794391198/evidence/CRM_CUS_TC_157_step3_bien_the_b_note_rong_the_img.png) |
| CRM_CUS_TC_158 | 127 | ✅ PASS | — | — | — |
| CRM_CUS_TC_159 | 127 | ✅ PASS | — | — | [ảnh](../../executions/customers/web/run_1794391198/evidence/CRM_CUS_TC_159_step4_bien_the_b_the_img_trong_short_description.png) |
| CRM_CUS_TC_160 | 12 | ✅ PASS | — | — | — |
| CRM_CUS_TC_161 | 128 | ✅ PASS | — | — | — |
| CRM_CUS_TC_162 | 48 | ✅ PASS | — | — | — |
| CRM_CUS_TC_163 | 48 | ✅ PASS | — | — | [ảnh](../../executions/customers/web/run_1794391198/evidence/CRM_CUS_TC_163_step3_mat_mang.png) |
| CRM_CUS_TC_164 | 06 | ❌ FAIL | 2 · biến thể `c` | `@NeedsVerify` · xem FAIL #19 | [ảnh](../../executions/customers/web/run_1794391198/evidence/CRM_CUS_TC_164_step2_bien_the_c_768.png) |
| CRM_CUS_TC_165 | 29 | ✅ PASS | — | — | — |
| CRM_CUS_TC_166 | 29 | ✅ PASS | — | — | — |
| CRM_CUS_TC_167 | 124 | ✅ PASS | — | — | — |
| CRM_CUS_TC_168 | 48 · 12 · 71 | 🚫 BLOCKED | — | Công cụ của agent (Playwright MCP) chỉ chạy Google Chrome — không mở được Microsoft Edge / Firefox | — |
| CRM_CUS_TC_169 | 80 | ✅ PASS | — | — | — |

</details>

## 5. 🟡 TC chưa automate

| Priority | Automation = Yes | Partial | No | Tổng |
|---|:-:|:-:|:-:|:-:|
| Critical | 8 | 0 | 0 | 8 |
| High | 53 | 2 | 0 | 55 |
| Medium | 74 | 6 | 0 | 80 |
| Low | 22 | 4 | 0 | 26 |
| **Tổng** | **157** | **12** | **0** | **169** |

*Hoãn automate theo index TC: 41 `@NeedsVerify` (chờ recon xong) · 14 `@Skip` (chờ điều kiện môi trường). `@KnownBug` vẫn automate — FAIL chính là thứ phơi bug.*

<details><summary>Danh sách đầy đủ 169 TC (xếp theo Priority)</summary>

| TC ID | Priority | Automation dự kiến | Ghi chú |
|---|---|---|---|
| CRM_CUS_TC_006 | Critical | Yes | `@Smoke` · `@CriticalPath` |
| CRM_CUS_TC_007 | Critical | Yes | `@Smoke` · `@CriticalPath` |
| CRM_CUS_TC_012 | Critical | Yes | `@Smoke` · `@CriticalPath` |
| CRM_CUS_TC_041 | Critical | Yes | `@CriticalPath` |
| CRM_CUS_TC_080 | Critical | Yes | `@CriticalPath` |
| CRM_CUS_TC_086 | Critical | Yes | `@CriticalPath` · ⏩ lần chạy gần nhất |
| CRM_CUS_TC_136 | Critical | Yes | `@CriticalPath` |
| CRM_CUS_TC_155 | Critical | Yes | `@NeedsVerify` |
| CRM_CUS_TC_008 | High | Yes | `@Smoke` · `@CriticalPath` |
| CRM_CUS_TC_009 | High | Yes | `@Smoke` |
| CRM_CUS_TC_017 | High | Yes | `@Smoke` |
| CRM_CUS_TC_027 | High | Yes | `@CriticalPath` |
| CRM_CUS_TC_038 | High | Partial | — |
| CRM_CUS_TC_042 | High | Yes | `@KnownBug` · ❌ lần chạy gần nhất |
| CRM_CUS_TC_043 | High | Yes | — |
| CRM_CUS_TC_044 | High | Yes | `@KnownBug` · ❌ lần chạy gần nhất |
| CRM_CUS_TC_060 | High | Partial | `@NeedsVerify` |
| CRM_CUS_TC_065 | High | Yes | — |
| CRM_CUS_TC_066 | High | Yes | `@NeedsVerify` |
| CRM_CUS_TC_069 | High | Yes | — |
| CRM_CUS_TC_070 | High | Yes | — |
| CRM_CUS_TC_072 | High | Yes | `@CriticalPath` |
| CRM_CUS_TC_073 | High | Yes | — |
| CRM_CUS_TC_074 | High | Yes | — |
| CRM_CUS_TC_076 | High | Yes | `@CriticalPath` |
| CRM_CUS_TC_077 | High | Yes | `@NeedsVerify` |
| CRM_CUS_TC_078 | High | Yes | `@NeedsVerify` · 🚫 lần chạy gần nhất |
| CRM_CUS_TC_079 | High | Yes | — |
| CRM_CUS_TC_081 | High | Yes | `@Skip` · 🚫 lần chạy gần nhất |
| CRM_CUS_TC_082 | High | Yes | — |
| CRM_CUS_TC_085 | High | Yes | ⏩ lần chạy gần nhất |
| CRM_CUS_TC_088 | High | Yes | — |
| CRM_CUS_TC_089 | High | Yes | — |
| CRM_CUS_TC_090 | High | Yes | `@KnownBug` · ❌ lần chạy gần nhất |
| CRM_CUS_TC_093 | High | Yes | — |
| CRM_CUS_TC_094 | High | Yes | — |
| CRM_CUS_TC_098 | High | Yes | `@Skip` · 🚫 lần chạy gần nhất |
| CRM_CUS_TC_099 | High | Yes | `@Skip` · 🚫 lần chạy gần nhất |
| CRM_CUS_TC_101 | High | Yes | `@Skip` · 🚫 lần chạy gần nhất |
| CRM_CUS_TC_102 | High | Yes | `@CriticalPath` |
| CRM_CUS_TC_107 | High | Yes | `@CriticalPath` |
| CRM_CUS_TC_110 | High | Yes | 🚫 lần chạy gần nhất |
| CRM_CUS_TC_118 | High | Yes | — |
| CRM_CUS_TC_130 | High | Yes | — |
| CRM_CUS_TC_131 | High | Yes | — |
| CRM_CUS_TC_132 | High | Yes | — |
| CRM_CUS_TC_133 | High | Yes | — |
| CRM_CUS_TC_137 | High | Yes | — |
| CRM_CUS_TC_138 | High | Yes | — |
| CRM_CUS_TC_139 | High | Yes | — |
| CRM_CUS_TC_140 | High | Yes | — |
| CRM_CUS_TC_141 | High | Yes | — |
| CRM_CUS_TC_142 | High | Yes | `@PersonalOnly` · ⏩ lần chạy gần nhất |
| CRM_CUS_TC_143 | High | Yes | `@Skip` · 🚫 lần chạy gần nhất |
| CRM_CUS_TC_149 | High | Yes | `@NeedsVerify` |
| CRM_CUS_TC_150 | High | Yes | `@NeedsVerify` |
| CRM_CUS_TC_156 | High | Yes | `@NeedsVerify` · ❌ lần chạy gần nhất |
| CRM_CUS_TC_157 | High | Yes | `@NeedsVerify` · ❌ lần chạy gần nhất |
| CRM_CUS_TC_158 | High | Yes | `@NeedsVerify` |
| CRM_CUS_TC_159 | High | Yes | `@NeedsVerify` |
| CRM_CUS_TC_161 | High | Yes | — |
| CRM_CUS_TC_162 | High | Yes | `@NeedsVerify` |
| CRM_CUS_TC_169 | High | Yes | `@NeedsVerify` |
| CRM_CUS_TC_001 | Medium | Yes | `@Smoke` |
| CRM_CUS_TC_002 | Medium | Yes | `@Smoke` |
| CRM_CUS_TC_003 | Medium | Yes | `@Smoke` |
| CRM_CUS_TC_004 | Medium | Yes | `@Smoke` · `@NeedsVerify` |
| CRM_CUS_TC_005 | Medium | Yes | `@Smoke` |
| CRM_CUS_TC_010 | Medium | Yes | `@Smoke` |
| CRM_CUS_TC_011 | Medium | Yes | `@Smoke` |
| CRM_CUS_TC_013 | Medium | Yes | `@Smoke` |
| CRM_CUS_TC_014 | Medium | Yes | `@Smoke` |
| CRM_CUS_TC_015 | Medium | Yes | `@Smoke` |
| CRM_CUS_TC_016 | Medium | Yes | `@Smoke` |
| CRM_CUS_TC_019 | Medium | Yes | `@Smoke` · `@NeedsVerify` |
| CRM_CUS_TC_020 | Medium | Partial | — |
| CRM_CUS_TC_021 | Medium | Partial | ⏩ lần chạy gần nhất |
| CRM_CUS_TC_022 | Medium | Partial | — |
| CRM_CUS_TC_023 | Medium | Yes | — |
| CRM_CUS_TC_025 | Medium | Yes | — |
| CRM_CUS_TC_026 | Medium | Yes | — |
| CRM_CUS_TC_028 | Medium | Yes | — |
| CRM_CUS_TC_029 | Medium | Yes | — |
| CRM_CUS_TC_030 | Medium | Yes | — |
| CRM_CUS_TC_032 | Medium | Yes | ❌ lần chạy gần nhất |
| CRM_CUS_TC_033 | Medium | Yes | — |
| CRM_CUS_TC_034 | Medium | Yes | `@Skip` · 🚫 lần chạy gần nhất |
| CRM_CUS_TC_035 | Medium | Yes | — |
| CRM_CUS_TC_036 | Medium | Yes | — |
| CRM_CUS_TC_037 | Medium | Yes | — |
| CRM_CUS_TC_039 | Medium | Yes | — |
| CRM_CUS_TC_040 | Medium | Yes | `@Skip` · 🚫 lần chạy gần nhất |
| CRM_CUS_TC_045 | Medium | Yes | `@NeedsVerify` · ❌ lần chạy gần nhất |
| CRM_CUS_TC_046 | Medium | Yes | — |
| CRM_CUS_TC_048 | Medium | Yes | — |
| CRM_CUS_TC_056 | Medium | Yes | `@NeedsVerify` |
| CRM_CUS_TC_057 | Medium | Yes | — |
| CRM_CUS_TC_058 | Medium | Partial | `@Skip` · 🚫 lần chạy gần nhất |
| CRM_CUS_TC_059 | Medium | Yes | ❌ lần chạy gần nhất |
| CRM_CUS_TC_063 | Medium | Yes | `@NeedsVerify` |
| CRM_CUS_TC_064 | Medium | Yes | — |
| CRM_CUS_TC_067 | Medium | Yes | — |
| CRM_CUS_TC_068 | Medium | Yes | — |
| CRM_CUS_TC_071 | Medium | Yes | `@NeedsVerify` · 🚫 lần chạy gần nhất |
| CRM_CUS_TC_075 | Medium | Yes | `@NeedsVerify` |
| CRM_CUS_TC_083 | Medium | Yes | — |
| CRM_CUS_TC_084 | Medium | Yes | `@NeedsVerify` |
| CRM_CUS_TC_087 | Medium | Yes | ⏩ lần chạy gần nhất |
| CRM_CUS_TC_091 | Medium | Yes | — |
| CRM_CUS_TC_092 | Medium | Yes | — |
| CRM_CUS_TC_095 | Medium | Yes | — |
| CRM_CUS_TC_097 | Medium | Yes | `@NeedsVerify` |
| CRM_CUS_TC_103 | Medium | Yes | `@KnownBug` · ❌ lần chạy gần nhất |
| CRM_CUS_TC_105 | Medium | Yes | — |
| CRM_CUS_TC_106 | Medium | Yes | — |
| CRM_CUS_TC_108 | Medium | Yes | — |
| CRM_CUS_TC_109 | Medium | Yes | — |
| CRM_CUS_TC_111 | Medium | Yes | `@NeedsVerify` |
| CRM_CUS_TC_113 | Medium | Yes | — |
| CRM_CUS_TC_114 | Medium | Yes | `@Skip` · 🚫 lần chạy gần nhất |
| CRM_CUS_TC_115 | Medium | Yes | — |
| CRM_CUS_TC_117 | Medium | Partial | `@Skip` · 🚫 lần chạy gần nhất |
| CRM_CUS_TC_119 | Medium | Yes | — |
| CRM_CUS_TC_120 | Medium | Yes | — |
| CRM_CUS_TC_121 | Medium | Yes | — |
| CRM_CUS_TC_122 | Medium | Yes | `@KnownBug` · ❌ lần chạy gần nhất |
| CRM_CUS_TC_123 | Medium | Yes | — |
| CRM_CUS_TC_125 | Medium | Yes | — |
| CRM_CUS_TC_126 | Medium | Yes | `@Skip` · 🚫 lần chạy gần nhất |
| CRM_CUS_TC_127 | Medium | Yes | — |
| CRM_CUS_TC_128 | Medium | Yes | `@KnownBug` · ❌ lần chạy gần nhất |
| CRM_CUS_TC_129 | Medium | Partial | `@Skip` · 🚫 lần chạy gần nhất |
| CRM_CUS_TC_134 | Medium | Yes | — |
| CRM_CUS_TC_135 | Medium | Yes | `@KnownBug` · ❌ lần chạy gần nhất |
| CRM_CUS_TC_144 | Medium | Yes | — |
| CRM_CUS_TC_151 | Medium | Yes | `@NeedsVerify` |
| CRM_CUS_TC_152 | Medium | Yes | `@NeedsVerify` |
| CRM_CUS_TC_153 | Medium | Yes | `@NeedsVerify` · ❌ lần chạy gần nhất |
| CRM_CUS_TC_154 | Medium | Yes | `@NeedsVerify` |
| CRM_CUS_TC_160 | Medium | Yes | — |
| CRM_CUS_TC_163 | Medium | Yes | `@NeedsVerify` |
| CRM_CUS_TC_166 | Medium | Yes | `@NeedsVerify` |
| CRM_CUS_TC_168 | Medium | Yes | `@NeedsVerify` · 🚫 lần chạy gần nhất |
| CRM_CUS_TC_018 | Low | Yes | `@Smoke` · `@NeedsVerify` · ❌ lần chạy gần nhất |
| CRM_CUS_TC_024 | Low | Yes | — |
| CRM_CUS_TC_031 | Low | Partial | — |
| CRM_CUS_TC_047 | Low | Yes | — |
| CRM_CUS_TC_049 | Low | Yes | — |
| CRM_CUS_TC_050 | Low | Yes | — |
| CRM_CUS_TC_051 | Low | Yes | — |
| CRM_CUS_TC_052 | Low | Yes | `@NeedsVerify` |
| CRM_CUS_TC_053 | Low | Yes | `@NeedsVerify` |
| CRM_CUS_TC_054 | Low | Yes | `@NeedsVerify` |
| CRM_CUS_TC_055 | Low | Yes | `@NeedsVerify` |
| CRM_CUS_TC_061 | Low | Yes | — |
| CRM_CUS_TC_062 | Low | Yes | — |
| CRM_CUS_TC_096 | Low | Yes | — |
| CRM_CUS_TC_100 | Low | Yes | `@Skip` · 🚫 lần chạy gần nhất |
| CRM_CUS_TC_104 | Low | Yes | `@NeedsVerify` |
| CRM_CUS_TC_112 | Low | Yes | `@NeedsVerify` |
| CRM_CUS_TC_116 | Low | Yes | `@KnownBug` · ❌ lần chạy gần nhất |
| CRM_CUS_TC_124 | Low | Yes | `@NeedsVerify` · ❌ lần chạy gần nhất |
| CRM_CUS_TC_145 | Low | Yes | — |
| CRM_CUS_TC_146 | Low | Yes | `@KnownBug` · ❌ lần chạy gần nhất |
| CRM_CUS_TC_147 | Low | Yes | `@KnownBug` · ❌ lần chạy gần nhất |
| CRM_CUS_TC_148 | Low | Partial | `@Skip` · 🚫 lần chạy gần nhất |
| CRM_CUS_TC_164 | Low | Partial | `@NeedsVerify` · ❌ lần chạy gần nhất |
| CRM_CUS_TC_165 | Low | Partial | `@NeedsVerify` |
| CRM_CUS_TC_167 | Low | Yes | `@NeedsVerify` |

</details>

## 6. Kiểm tra khác

| Hạng mục | Số lượng | Kết quả |
|---|:-:|---|
| 🔴 REQ chưa có TC | 0 | ✅ Không có |
| 🟠 REQ chỉ có checklist | 0 | ✅ Không có — 19 mục checklist (phủ 42 REQ) đều trỏ về REQ đã có TC |
| ⚪ Orphan test (không map về REQ) | 0 | ✅ Không có — chưa có script automation |
| 🧩 TC trỏ tới REQ không tồn tại | 0 | ✅ Không có — 169/169 TC trỏ về REQ đang có trong tài liệu bản 2.1 |
| 🗑️ Script trỏ vào TC Deprecated | 0 | ✅ Không có — chưa có TC `@Deprecated` |
| ⚠️ Mapping suy luận cần xác nhận | 0 | ✅ Không có — 100% mapping theo ID |

> ⚠️ **Lưu ý truy vết:** mã `REQ-CUS-nn` được **đánh lại từ 01** ngày 11-11-2026 (bản 2.0/2.1). RTM này bám bản 2.1 — tài liệu/script nào còn trỏ `REQ-CUS-nn` theo bản 1.0 (04-11-2026, chỉ còn trong git) thì **không** đọc theo ma trận này.
