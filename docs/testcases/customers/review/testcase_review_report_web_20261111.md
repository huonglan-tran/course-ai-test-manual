# Báo Cáo Review Test Cases — Customers · Web

> Workflow `/review-testcases` · **Mode REVIEW** — chỉ báo cáo, **không** sửa file TC nào.
> Bộ TC: [`TEST_CASES_CUSTOMERS_SUMMARY.md`](../TEST_CASES_CUSTOMERS_SUMMARY.md) → 6 part trong [`web/parts/`](../web/parts/) · Requirements: [`REQUIREMENTS_CUSTOMERS_SUMMARY.md`](../../../requirements/customers/REQUIREMENTS_CUSTOMERS_SUMMARY.md) bản 2.1 (11-11-2026)

## Tổng quan

- **Nguồn:** `docs/testcases/customers/web/parts/part_01_web_smoke.md` → `part_06_web_technical_nonfunc.md` (đọc qua index)
- **Ngày review:** 11-11-2026
- **Số TC review:** 169 (`CRM_CUS_TC_001` → `169`) · 0 TC `@Deprecated`
- **Kết quả:** 🟢 169 tốt | 🟡 0 cần sửa | 🔴 0 nên viết lại
- **Điểm trung bình:** 11,86/12 — 146 TC đạt 12/12 · 22 TC 11/12 · 1 TC 10/12
- **Tiêu chí bị trừ nhiều nhất:** Expected đo được (8 TC) · Test data cụ thể (6 TC) · Rõ ràng (5 TC) · Đúng trọng tâm (3 TC) · Độc lập (2 TC) · Truy vết (0 TC)
- **Loại trừ theo requirements:** Tab Contacts / modal `Add new contact` / `all_contacts` (→ `CONT`) · 11 tab chứng từ Invoices…Tickets (→ module chứng từ) · Reminders (→ `PROF`) · cổng Customer/Contact · quản lý Customer Groups / Currency / ngôn ngữ / Google API key / đuôi tệp ở Setup (`AMB-SYS-01`) · `AMB-CUS-05` (→ `CONT`) · `AMB-CUS-24` (không sinh REQ) · `RISK-CUS-04` (→ Dev/bảo mật)
- **Đối chiếu số liệu index:** khớp 100% — 169 TC · 272 đơn vị chạy (121 TC đơn + 151 biến thể ở 48 TC gộp) · `@Skip` 14 · `@KnownBug` 10 · `@NeedsVerify` 41 · `@TechCheck` 4 · `@Smoke` 19 · Automation `Yes` 157 / `Partial` 12. Cột `REQ ID` của 169 dòng TC khớp Bảng Đối Soát Coverage của index ở cả 141 REQ (0 lệch)
- **Trạng thái git:** thư mục `docs/testcases/customers/` **chưa được git theo dõi** (`??`) — muốn chạy Mode FIX phải commit trước để có mốc git

> 📌 **Đọc kết quả thế nào:** rubric 6 tiêu chí chấm **cách viết từng TC** — cả 169 TC đều 🟢, bộ TC viết rất chặt (bước đánh số, nhãn nguyên văn, Test Data traceable, tag đầy đủ, ngôn ngữ DOM/HTTP đã dồn đúng vào `🔧 Ghi chú kỹ thuật`). Phần cần xử lý nằm ở **mức bộ TC**: 1 TC có payload phá huỷ trên môi trường dùng chung, 1 TC thiếu chốt an toàn trước Mass Delete, và **14 kịch bản còn thiếu** ở các nhánh Security · Business Rule · BVA · Validation.

## 🚨 Cần sửa trước lần chạy đầu tiên

| # | TC | Vấn đề | Vì sao gấp |
|---|---|---|---|
| 1 | `CRM_CUS_TC_156-d` | Test Data `auto_cus_tc156d_20261111"; DROP TABLE tblclients; --` — chuỗi **có lệnh xoá bảng** | Môi trường **dùng chung** (`RISK-SYS-01`). Nếu ô Company có lỗ hổng, chạy TC là xoá bảng khách hàng của mọi người. Header part 6 tự đặt luật *"không khai thác sâu hơn việc quan sát"* — biến thể này vượt luật đó. Đề xuất thay bằng chuỗi không phá huỷ: `auto_cus_tc156d_20261111" OR "1"="1` |
| 2 | `CRM_CUS_TC_021` bước 4 | `Lọc ô tìm kiếm auto_cus_tc021, tích chọn tất cả → Bulk Actions → tích Mass Delete → Confirm → OK` — **không** có bước đếm số dòng trước khi Confirm | Mọi TC Mass Delete khác (086) đều có chốt *"Đúng N dòng — dừng nếu khác"* theo `RISK-CUS-02`. Part 2 không có header nhắc luật này như part 3. Thiếu chốt = có thể xoá nhầm KH người khác |

> ✅ **Đã sửa 11-11-2026** theo yêu cầu user (ghi ở Nhật ký index): 156-d chỉ nhắm vào KH do chính TC tạo, có bước dọn đếm đúng 1 dòng · 021 có chốt *"đúng 2 dòng, đúng tên KH test — khác thì dừng"*. Các đề xuất còn lại trong báo cáo **chưa** áp dụng.

## Chi tiết từng TC

> Cột **Trừ ở**: `1` Rõ ràng · `2` Expected đo được · `3` Độc lập · `4` Test data · `5` Truy vết · `6` Đúng trọng tâm.

| TC ID | Điểm | Xếp loại | Trừ ở | Vấn đề chính | Đề xuất sửa |
|---|---|---|---|---|---|
| CRM_CUS_TC_001 | 12/12 | 🟢 | — | Đạt đủ 6 tiêu chí | — |
| CRM_CUS_TC_002 | 12/12 | 🟢 | — | Đạt đủ 6 tiêu chí | — |
| CRM_CUS_TC_003 | 12/12 | 🟢 | — | Đạt đủ 6 tiêu chí | — |
| CRM_CUS_TC_004 | 12/12 | 🟢 | — | Đạt đủ 6 tiêu chí | — |
| CRM_CUS_TC_005 | 12/12 | 🟢 | — | Đạt đủ 6 tiêu chí | — |
| CRM_CUS_TC_006 | 12/12 | 🟢 | — | Đạt đủ 6 tiêu chí | — |
| CRM_CUS_TC_007 | 12/12 | 🟢 | — | Đạt đủ 6 tiêu chí | — |
| CRM_CUS_TC_008 | 12/12 | 🟢 | — | Đạt đủ 6 tiêu chí | — |
| CRM_CUS_TC_009 | 12/12 | 🟢 | — | Đạt đủ 6 tiêu chí | — |
| CRM_CUS_TC_010 | 12/12 | 🟢 | — | Đạt đủ 6 tiêu chí | — |
| CRM_CUS_TC_011 | 12/12 | 🟢 | — | Đạt đủ 6 tiêu chí | — |
| CRM_CUS_TC_012 | 12/12 | 🟢 | — | Đạt đủ 6 tiêu chí | — |
| CRM_CUS_TC_013 | 12/12 | 🟢 | — | Đạt đủ 6 tiêu chí | — |
| CRM_CUS_TC_014 | 12/12 | 🟢 | — | Đạt đủ 6 tiêu chí | — |
| CRM_CUS_TC_015 | 12/12 | 🟢 | — | Đạt đủ 6 tiêu chí | — |
| CRM_CUS_TC_016 | 12/12 | 🟢 | — | Đạt đủ 6 tiêu chí | — |
| CRM_CUS_TC_017 | 11/12 | 🟢 | 1 | Bảng kiểm `4` chứa một **thao tác**: *"Gõ thử `abc123` vào ô Password → chỉ hiện dấu chấm che"* — Bảng kiểm Kiểu B là quan sát tĩnh | Thêm bước 5 `Gõ abc123 vào ô Password` · Expected 5 `Ô Password chỉ hiện 6 dấu chấm, không thấy abc123`; xoá mục `4` khỏi Bảng kiểm |
| CRM_CUS_TC_018 | 12/12 | 🟢 | — | Đạt đủ 6 tiêu chí | — |
| CRM_CUS_TC_019 | 11/12 | 🟢 | 6 | Gộp 6 hộp thoại thuộc 6 màn hình / 4 nhóm chức năng (form tạo KH · Customer Admins · danh sách · Vault · Files · Statement) — vi phạm *CẤM gộp khác nhóm chức năng / khác màn hình* | 019 chỉ giữ biến thể `a`; thêm bước `×` và `Esc` vào TC sẵn có của từng màn hình: 068 (Assign Admin) · 083 (Bulk Actions) · 017 (Vault Entry) · 115 (Send File) · 127 (Account Summary) |
| CRM_CUS_TC_020 | 12/12 | 🟢 | — | Đạt đủ 6 tiêu chí | — |
| CRM_CUS_TC_021 | 11/12 | 🟢 | 1 | Bước 4 dồn 6 thao tác *"Lọc …, tích chọn tất cả → Bulk Actions → tích Mass Delete → Confirm → OK"* và **không** đếm số dòng trước Confirm (`RISK-CUS-02`) | Tách: 4. Gõ `auto_cus_tc021` vào ô tìm kiếm → *Bảng còn đúng 2 dòng — **dừng** nếu khác 2* · 5. Tích ô chọn tất cả · 6. `Bulk Actions` → tích `Mass Delete` → `Confirm` → `OK` → `Total customers deleted: 2` |
| CRM_CUS_TC_022 | 12/12 | 🟢 | — | Đạt đủ 6 tiêu chí | — |
| CRM_CUS_TC_023 | 12/12 | 🟢 | — | Đạt đủ 6 tiêu chí | — |
| CRM_CUS_TC_024 | 11/12 | 🟢 | 2 | Expected 4 *"Nhóm giá trị đứng đầu ở bước 2 chuyển xuống cuối"* — bước 4 chỉ quan sát 10 dòng đầu trong ~2.900 KH, không nhìn thấy "cuối" | Expected 4: *10 dòng đầu mang nhóm giá trị **khác** nhóm ở bước 2 (VD `a`: bước 2 toàn công tắc tắt → bước 4 toàn công tắc bật)* |
| CRM_CUS_TC_025 | 12/12 | 🟢 | — | Đạt đủ 6 tiêu chí | — |
| CRM_CUS_TC_026 | 11/12 | 🟢 | 2 | Expected 4 *"Về trang liền trước trang cuối"* không nói dấu hiệu đo được | Expected 4: *Số trang liền trước trang cuối tô xanh · `Showing <a−25> to <a−1> of N entries` · `Next` bấm được* |
| CRM_CUS_TC_027 | 12/12 | 🟢 | — | Đạt đủ 6 tiêu chí | — |
| CRM_CUS_TC_028 | 12/12 | 🟢 | — | Đạt đủ 6 tiêu chí | — |
| CRM_CUS_TC_029 | 12/12 | 🟢 | — | Đạt đủ 6 tiêu chí | — |
| CRM_CUS_TC_030 | 12/12 | 🟢 | — | Đạt đủ 6 tiêu chí | — |
| CRM_CUS_TC_031 | 11/12 | 🟢 | 3 | Dựa vào dữ liệu có sẵn *"ít nhất 1 KH không có Company nhưng có contact"*, phải dò trong ~2.900 dòng | Ghi đích danh `#<id>` của một KH cá nhân vào ASM sau lượt recon (hoặc seed theo điều kiện #2 ở index); bước 1 mở thẳng danh sách đã tìm theo tên contact |
| CRM_CUS_TC_032 | 12/12 | 🟢 | — | Đạt đủ 6 tiêu chí | — |
| CRM_CUS_TC_033 | 12/12 | 🟢 | — | Đạt đủ 6 tiêu chí | — |
| CRM_CUS_TC_034 | 12/12 | 🟢 | — | Đạt đủ 6 tiêu chí | — |
| CRM_CUS_TC_035 | 12/12 | 🟢 | — | Đạt đủ 6 tiêu chí | — |
| CRM_CUS_TC_036 | 12/12 | 🟢 | — | Đạt đủ 6 tiêu chí | — |
| CRM_CUS_TC_037 | 12/12 | 🟢 | — | Đạt đủ 6 tiêu chí | — |
| CRM_CUS_TC_038 | 12/12 | 🟢 | — | Đạt đủ 6 tiêu chí | — |
| CRM_CUS_TC_039 | 11/12 | 🟢 | 1 | Bước 2 *"Áp bộ lọc `Active` = `No` như `CRM_CUS_TC_038` bước 2"* — người chạy phải mở TC khác | Viết thẳng: *Bấm nút phễu → `New Filter` → `Add Rule` → `Active` → chọn `No` → `Apply` (không bật `Save Filter`)* |
| CRM_CUS_TC_040 | 12/12 | 🟢 | — | Đạt đủ 6 tiêu chí | — |
| CRM_CUS_TC_041 | 12/12 | 🟢 | — | Đạt đủ 6 tiêu chí | — |
| CRM_CUS_TC_042 | 12/12 | 🟢 | — | Đạt đủ 6 tiêu chí | — |
| CRM_CUS_TC_043 | 12/12 | 🟢 | — | Đạt đủ 6 tiêu chí | — |
| CRM_CUS_TC_044 | 12/12 | 🟢 | — | Đạt đủ 6 tiêu chí | — |
| CRM_CUS_TC_045 | 11/12 | 🟢 | 6 | Header *"Mọi biến thể cho cùng kết quả"* nhưng `e` có kết quả khác (*"riêng `e` đã bỏ khoảng trắng đầu/cuối"*); việc tự cắt khoảng trắng không có REQ và không có evidence | Tách `e` thành **TC mới nối tiếp dải** (`CRM_CUS_TC_170`) gắn `@NeedsVerify`; 045 giữ `a`–`d` |
| CRM_CUS_TC_046 | 12/12 | 🟢 | — | Đạt đủ 6 tiêu chí | — |
| CRM_CUS_TC_047 | 12/12 | 🟢 | — | Đạt đủ 6 tiêu chí | — |
| CRM_CUS_TC_048 | 12/12 | 🟢 | — | Đạt đủ 6 tiêu chí | — |
| CRM_CUS_TC_049 | 12/12 | 🟢 | — | Đạt đủ 6 tiêu chí | — |
| CRM_CUS_TC_050 | 12/12 | 🟢 | — | Đạt đủ 6 tiêu chí | — |
| CRM_CUS_TC_051 | 12/12 | 🟢 | — | Đạt đủ 6 tiêu chí | — |
| CRM_CUS_TC_052 | 12/12 | 🟢 | — | Đạt đủ 6 tiêu chí | — |
| CRM_CUS_TC_053 | 12/12 | 🟢 | — | Đạt đủ 6 tiêu chí | — |
| CRM_CUS_TC_054 | 12/12 | 🟢 | — | Đạt đủ 6 tiêu chí | — |
| CRM_CUS_TC_055 | 12/12 | 🟢 | — | Đạt đủ 6 tiêu chí | — |
| CRM_CUS_TC_056 | 11/12 | 🟢 | 4 | Bước 3 *"Chọn thêm nhóm thứ hai, ghi tên nhóm vào báo cáo"* — dữ liệu không cố định giữa các lần chạy | Chốt 1 nhóm thứ hai cụ thể (tên + value, `RISK-CUS-03`) vào `ASM-CUS-07`, Test Data ghi đích danh |
| CRM_CUS_TC_057 | 12/12 | 🟢 | — | Đạt đủ 6 tiêu chí | — |
| CRM_CUS_TC_058 | 12/12 | 🟢 | — | Đạt đủ 6 tiêu chí | — |
| CRM_CUS_TC_059 | 12/12 | 🟢 | — | Đạt đủ 6 tiêu chí | — |
| CRM_CUS_TC_060 | 11/12 | 🟢 | 3 | Pre-Condition dựa vào KH có sẵn có hoá đơn của người khác (`ASM-CUS-11`) | Nếu hoá đơn nháp được phép (như 078 bước 1): tạo KH test → tạo hoá đơn nháp → kiểm Currency → dọn. Nếu không: ghi đích danh `#<id>` KH vào ASM-CUS-11 |
| CRM_CUS_TC_061 | 12/12 | 🟢 | — | Đạt đủ 6 tiêu chí | — |
| CRM_CUS_TC_062 | 12/12 | 🟢 | — | Đạt đủ 6 tiêu chí | — |
| CRM_CUS_TC_063 | 12/12 | 🟢 | — | Đạt đủ 6 tiêu chí | — |
| CRM_CUS_TC_064 | 12/12 | 🟢 | — | Đạt đủ 6 tiêu chí | — |
| CRM_CUS_TC_065 | 12/12 | 🟢 | — | Đạt đủ 6 tiêu chí | — |
| CRM_CUS_TC_066 | 12/12 | 🟢 | — | Đạt đủ 6 tiêu chí | — |
| CRM_CUS_TC_067 | 12/12 | 🟢 | — | Đạt đủ 6 tiêu chí | — |
| CRM_CUS_TC_068 | 11/12 | 🟢 | 4 | Staff 2: *"staff bất kỳ khác trong danh sách (ghi tên vào báo cáo)"* | Chốt tên staff thứ hai cố định vào `ASM-CUS-12`, Test Data ghi đích danh |
| CRM_CUS_TC_069 | 12/12 | 🟢 | — | Đạt đủ 6 tiêu chí | — |
| CRM_CUS_TC_070 | 12/12 | 🟢 | — | Đạt đủ 6 tiêu chí | — |
| CRM_CUS_TC_071 | 12/12 | 🟢 | — | Đạt đủ 6 tiêu chí | — |
| CRM_CUS_TC_072 | 12/12 | 🟢 | — | Đạt đủ 6 tiêu chí | — |
| CRM_CUS_TC_073 | 12/12 | 🟢 | — | Đạt đủ 6 tiêu chí | — |
| CRM_CUS_TC_074 | 12/12 | 🟢 | — | Đạt đủ 6 tiêu chí | — |
| CRM_CUS_TC_075 | 12/12 | 🟢 | — | Đạt đủ 6 tiêu chí | — |
| CRM_CUS_TC_076 | 12/12 | 🟢 | — | Đạt đủ 6 tiêu chí | — |
| CRM_CUS_TC_077 | 12/12 | 🟢 | — | Đạt đủ 6 tiêu chí | — |
| CRM_CUS_TC_078 | 12/12 | 🟢 | — | Đạt đủ 6 tiêu chí | — |
| CRM_CUS_TC_079 | 12/12 | 🟢 | — | Đạt đủ 6 tiêu chí | — |
| CRM_CUS_TC_080 | 12/12 | 🟢 | — | Đạt đủ 6 tiêu chí | — |
| CRM_CUS_TC_081 | 12/12 | 🟢 | — | Đạt đủ 6 tiêu chí | — |
| CRM_CUS_TC_082 | 12/12 | 🟢 | — | Đạt đủ 6 tiêu chí | — |
| CRM_CUS_TC_083 | 12/12 | 🟢 | — | Đạt đủ 6 tiêu chí | — |
| CRM_CUS_TC_084 | 12/12 | 🟢 | — | Đạt đủ 6 tiêu chí | — |
| CRM_CUS_TC_085 | 12/12 | 🟢 | — | Đạt đủ 6 tiêu chí | — |
| CRM_CUS_TC_086 | 12/12 | 🟢 | — | Đạt đủ 6 tiêu chí | — |
| CRM_CUS_TC_087 | 12/12 | 🟢 | — | Đạt đủ 6 tiêu chí | — |
| CRM_CUS_TC_088 | 12/12 | 🟢 | — | Đạt đủ 6 tiêu chí | — |
| CRM_CUS_TC_089 | 11/12 | 🟢 | 4 | *"Nhóm mới: nhóm thứ hai"* — không cố định | Như 056 — dùng nhóm thứ hai đã chốt trong `ASM-CUS-07` |
| CRM_CUS_TC_090 | 11/12 | 🟢 | 2 | Expected 3 cho phép 2 nhánh *"chặn … — hoặc hiện cảnh báo riêng"*; nếu là cảnh báo rồi bấm `OK` (bước 3) thì nhóm bị gỡ, mâu thuẫn Expected 4 *"Cột Groups vẫn là customer group b"* | Theo giả định `AMB-CUS-28` (chặn): Expected 3 *có thông báo lỗi nhìn thấy được, không thực hiện*; bỏ nhánh "cảnh báo riêng". Khi AMB-28 chốt khác → sửa qua `/update-testcases-from-impact` |
| CRM_CUS_TC_091 | 12/12 | 🟢 | — | Đạt đủ 6 tiêu chí | — |
| CRM_CUS_TC_092 | 12/12 | 🟢 | — | Đạt đủ 6 tiêu chí | — |
| CRM_CUS_TC_093 | 12/12 | 🟢 | — | Đạt đủ 6 tiêu chí | — |
| CRM_CUS_TC_094 | 12/12 | 🟢 | — | Đạt đủ 6 tiêu chí | — |
| CRM_CUS_TC_095 | 11/12 | 🟢 | 4 | *"Dòng 2 thiếu Email · Dòng 3 Email trùng dòng 1 · Dòng 4 … · Dòng 5 thiếu Company"* — không ghi Firstname/Lastname/Company của dòng 2–5 | Ghi đủ từng dòng, VD dòng 2: Firstname `Auto` · Lastname `Row2` · Email trống · Company `auto_imp_tc095_r2_20261111` |
| CRM_CUS_TC_096 | 12/12 | 🟢 | — | Đạt đủ 6 tiêu chí | — |
| CRM_CUS_TC_097 | 11/12 | 🟢 | 2 | Expected 3 *"Có thông báo lỗi hoặc không có bảng `Simulation Data` chứa ký tự rác"* — "ký tự rác" không đo được | Tạm viết phần đo được: *không có KH nào được tạo, không có bảng `Simulation Data` nào*; thông báo cụ thể để ở `⚠️` tới khi recon |
| CRM_CUS_TC_098 | 11/12 | 🟢 | 2 | Tiêu đề hứa kiểm *"… gán nhóm và mật khẩu mặc định"* nhưng Expected 4 không kiểm mật khẩu | Bỏ "và mật khẩu mặc định" khỏi tiêu đề (kiểm cần cổng KH — ngoài phạm vi), hoặc ghi rõ phần này không kiểm |
| CRM_CUS_TC_099 | 11/12 | 🟢 | 2 | Expected 3 *"Chỉ 1 KH được tạo"* — không nói KH nào (`r1` hay `r2`) | *Chỉ có `auto_imp_tc099_r1_20261111`; không có `…_r2_…`* — gắn `⚠️` cho tới khi chạy được Import thật |
| CRM_CUS_TC_100 | 12/12 | 🟢 | — | Đạt đủ 6 tiêu chí | — |
| CRM_CUS_TC_101 | 12/12 | 🟢 | — | Đạt đủ 6 tiêu chí | — |
| CRM_CUS_TC_102 | 12/12 | 🟢 | — | Đạt đủ 6 tiêu chí | — |
| CRM_CUS_TC_103 | 12/12 | 🟢 | — | Đạt đủ 6 tiêu chí | — |
| CRM_CUS_TC_104 | 12/12 | 🟢 | — | Đạt đủ 6 tiêu chí | — |
| CRM_CUS_TC_105 | 12/12 | 🟢 | — | Đạt đủ 6 tiêu chí | — |
| CRM_CUS_TC_106 | 12/12 | 🟢 | — | Đạt đủ 6 tiêu chí | — |
| CRM_CUS_TC_107 | 12/12 | 🟢 | — | Đạt đủ 6 tiêu chí | — |
| CRM_CUS_TC_108 | 12/12 | 🟢 | — | Đạt đủ 6 tiêu chí | — |
| CRM_CUS_TC_109 | 12/12 | 🟢 | — | Đạt đủ 6 tiêu chí | — |
| CRM_CUS_TC_110 | 12/12 | 🟢 | — | Đạt đủ 6 tiêu chí | — |
| CRM_CUS_TC_111 | 12/12 | 🟢 | — | Đạt đủ 6 tiêu chí | — |
| CRM_CUS_TC_112 | 12/12 | 🟢 | — | Đạt đủ 6 tiêu chí | — |
| CRM_CUS_TC_113 | 12/12 | 🟢 | — | Đạt đủ 6 tiêu chí | — |
| CRM_CUS_TC_114 | 12/12 | 🟢 | — | Đạt đủ 6 tiêu chí | — |
| CRM_CUS_TC_115 | 12/12 | 🟢 | — | Đạt đủ 6 tiêu chí | — |
| CRM_CUS_TC_116 | 12/12 | 🟢 | — | Đạt đủ 6 tiêu chí | — |
| CRM_CUS_TC_117 | 12/12 | 🟢 | — | Đạt đủ 6 tiêu chí | — |
| CRM_CUS_TC_118 | 12/12 | 🟢 | — | Đạt đủ 6 tiêu chí | — |
| CRM_CUS_TC_119 | 12/12 | 🟢 | — | Đạt đủ 6 tiêu chí | — |
| CRM_CUS_TC_120 | 11/12 | 🟢 | 2 | Biến thể `b` *"từ ngày đầu đến ngày cuối của tuần chứa 11-11-2026"* — không có ngày cụ thể (11-11-2026 là Thứ Tư) | Ghi cụ thể sau khi xác minh ngày đầu tuần: `09-11-2026 To 15-11-2026` (Thứ Hai) hoặc `08-11-2026 To 14-11-2026` (Chủ Nhật) |
| CRM_CUS_TC_121 | 12/12 | 🟢 | — | Đạt đủ 6 tiêu chí | — |
| CRM_CUS_TC_122 | 12/12 | 🟢 | — | Đạt đủ 6 tiêu chí | — |
| CRM_CUS_TC_123 | 12/12 | 🟢 | — | Đạt đủ 6 tiêu chí | — |
| CRM_CUS_TC_124 | 12/12 | 🟢 | — | Đạt đủ 6 tiêu chí | — |
| CRM_CUS_TC_125 | 12/12 | 🟢 | — | Đạt đủ 6 tiêu chí | — |
| CRM_CUS_TC_126 | 12/12 | 🟢 | — | Đạt đủ 6 tiêu chí | — |
| CRM_CUS_TC_127 | 12/12 | 🟢 | — | Đạt đủ 6 tiêu chí | — |
| CRM_CUS_TC_128 | 12/12 | 🟢 | — | Đạt đủ 6 tiêu chí | — |
| CRM_CUS_TC_129 | 12/12 | 🟢 | — | Đạt đủ 6 tiêu chí | — |
| CRM_CUS_TC_130 | 12/12 | 🟢 | — | Đạt đủ 6 tiêu chí | — |
| CRM_CUS_TC_131 | 12/12 | 🟢 | — | Đạt đủ 6 tiêu chí | — |
| CRM_CUS_TC_132 | 12/12 | 🟢 | — | Đạt đủ 6 tiêu chí | — |
| CRM_CUS_TC_133 | 12/12 | 🟢 | — | Đạt đủ 6 tiêu chí | — |
| CRM_CUS_TC_134 | 12/12 | 🟢 | — | Đạt đủ 6 tiêu chí | — |
| CRM_CUS_TC_135 | 12/12 | 🟢 | — | Đạt đủ 6 tiêu chí | — |
| CRM_CUS_TC_136 | 12/12 | 🟢 | — | Đạt đủ 6 tiêu chí | — |
| CRM_CUS_TC_137 | 12/12 | 🟢 | — | Đạt đủ 6 tiêu chí | — |
| CRM_CUS_TC_138 | 12/12 | 🟢 | — | Đạt đủ 6 tiêu chí | — |
| CRM_CUS_TC_139 | 12/12 | 🟢 | — | Đạt đủ 6 tiêu chí | — |
| CRM_CUS_TC_140 | 12/12 | 🟢 | — | Đạt đủ 6 tiêu chí | — |
| CRM_CUS_TC_141 | 12/12 | 🟢 | — | Đạt đủ 6 tiêu chí | — |
| CRM_CUS_TC_142 | 12/12 | 🟢 | — | Đạt đủ 6 tiêu chí | — |
| CRM_CUS_TC_143 | 12/12 | 🟢 | — | Đạt đủ 6 tiêu chí | — |
| CRM_CUS_TC_144 | 11/12 | 🟢 | 6 | Biến thể `c` là ô tích `Share this vault entry…` — **khác trường** với `a` · `b` (nhóm radio visibility) | Tách `c` thành **TC mới nối tiếp dải** (`CRM_CUS_TC_171`) |
| CRM_CUS_TC_145 | 12/12 | 🟢 | — | Đạt đủ 6 tiêu chí | — |
| CRM_CUS_TC_146 | 12/12 | 🟢 | — | Đạt đủ 6 tiêu chí | — |
| CRM_CUS_TC_147 | 12/12 | 🟢 | — | Đạt đủ 6 tiêu chí | — |
| CRM_CUS_TC_148 | 11/12 | 🟢 | 1 | Bước 1 *"Cấu hình Google API key ở Setup"* — không nói vào đâu, lấy key ở đâu | Ghi đường dẫn tới ô cấu hình (xác định khi được cấp quyền Setup — `AMB-SYS-01`) và nguồn key là biến trong `.env`, không ghi giá trị |
| CRM_CUS_TC_149 | 12/12 | 🟢 | — | Đạt đủ 6 tiêu chí | — |
| CRM_CUS_TC_150 | 12/12 | 🟢 | — | Đạt đủ 6 tiêu chí | — |
| CRM_CUS_TC_151 | 12/12 | 🟢 | — | Đạt đủ 6 tiêu chí | — |
| CRM_CUS_TC_152 | 12/12 | 🟢 | — | Đạt đủ 6 tiêu chí | — |
| CRM_CUS_TC_153 | 12/12 | 🟢 | — | Đạt đủ 6 tiêu chí | — |
| CRM_CUS_TC_154 | 12/12 | 🟢 | — | Đạt đủ 6 tiêu chí | — |
| CRM_CUS_TC_155 | 12/12 | 🟢 | — | Đạt đủ 6 tiêu chí | — |
| CRM_CUS_TC_156 | 10/12 | 🟢 | 2 · 4 | Biến thể `d` chứa `DROP TABLE tblclients` — payload **phá huỷ** trên môi trường dùng chung; Expected 3 *"Total không đổi bất thường"* mơ hồ | Thay `d` bằng `auto_cus_tc156d_20261111" OR "1"="1`; Expected 3: *bỏ tìm kiếm, F5 → `Total Customers` ≥ số ghi trước bước 1 (không mất bản ghi)* |
| CRM_CUS_TC_157 | 12/12 | 🟢 | — | Đạt đủ 6 tiêu chí | — |
| CRM_CUS_TC_158 | 12/12 | 🟢 | — | Đạt đủ 6 tiêu chí | — |
| CRM_CUS_TC_159 | 12/12 | 🟢 | — | Đạt đủ 6 tiêu chí | — |
| CRM_CUS_TC_160 | 12/12 | 🟢 | — | Đạt đủ 6 tiêu chí | — |
| CRM_CUS_TC_161 | 12/12 | 🟢 | — | Đạt đủ 6 tiêu chí | — |
| CRM_CUS_TC_162 | 12/12 | 🟢 | — | Đạt đủ 6 tiêu chí | — |
| CRM_CUS_TC_163 | 11/12 | 🟢 | 1 | Bước 4 dồn 4 thao tác có điều kiện *"Bật lại mạng, bấm Back về form nếu đang ở trang lỗi, nhập lại Company nếu ô đã trống, bấm Save"* | Sau lượt recon chốt màn hình thật khi mất mạng rồi tách: 4. Bật lại mạng · 5. Bấm Back · 6. Nhập lại Company · 7. Bấm `Save` |
| CRM_CUS_TC_164 | 12/12 | 🟢 | — | Đạt đủ 6 tiêu chí | — |
| CRM_CUS_TC_165 | 12/12 | 🟢 | — | Đạt đủ 6 tiêu chí | — |
| CRM_CUS_TC_166 | 12/12 | 🟢 | — | Đạt đủ 6 tiêu chí | — |
| CRM_CUS_TC_167 | 12/12 | 🟢 | — | Đạt đủ 6 tiêu chí | — |
| CRM_CUS_TC_168 | 12/12 | 🟢 | — | Đạt đủ 6 tiêu chí | — |
| CRM_CUS_TC_169 | 11/12 | 🟢 | 4 | Biến thể `a` *"Chọn nhóm thứ hai (`ASM-CUS-07`)"* — không cố định | Như 056 — dùng nhóm thứ hai đã chốt |

## Đối soát loại kiểm thử (4 vòng)

| Vòng | Nhánh | Trạng thái | Ghi chú |
|---|---|---|---|
| 1 | UI cơ bản | ✅ | 001 → 004 · 010 · 011 · 013 → 018 · 067 — nhãn nguyên văn, thứ tự field, trạng thái mặc định, con trỏ ở ô Company (003-`3`) đều có |
| 1 | Open form | 🟡 Nông | Mọi lối vào: nút + URL (005) · tên KH + `View` (030). Đóng hộp thoại: `×` · `Close` · `Esc` (019). **Thiếu:** đóng bằng **bấm ra vùng nền tối** bên ngoài hộp thoại |
| 1 | Display | 🟡 Nông | Định dạng số (001) · ngày giờ (008 · 069 · 102 · 107) · tiền tệ (016) · trạng thái rỗng (014 · 015 · 017 · 029 · 067). **Thiếu:** trạng thái **đang tải** của bảng danh sách khi đổi trang / sắp xếp |
| 1 | Input valid data · Save · Verify data | ✅ | Tối thiểu 006 · đầy đủ 007 · đối chiếu danh sách 008 · đối chiếu chi tiết 007 · sửa 012 |
| 2 | UI Behavior | ✅ | 014 · 037 · 047 · 084 · 121 |
| 2 | Required | ✅ | Bỏ trống **từng** trường + bỏ trống tất cả ở Vault (130 → 133) · Company 041 · 066 · Name nhóm 057 · tệp CSV 091 · Note 103 · Send File 116 · Email to 128 · View Password 141 |
| 2 | Validation | 🟡 Nông | Company đủ 8/8 mục. **Thiếu:** File Upload tab Files 4/7 mục · trường địa chỉ không có HTML/XSS · 10 trường Billing/Shipping **không có dòng** trong bảng Field-Level của index · Vault Server Address/Username thiếu khoảng trắng · Port thiếu số 0 đứng đầu — xem [Đối soát 15 loại field](#đối-soát-bảng-15-loại-field). ⚠️ Index chấm ✅ trong khi chính bảng Field-Level của index ghi *"Tệp 0 KB … kéo-thả: **chưa phủ**"* — lỗi ghi nhãn |
| 2 | Equivalence Partitioning | ✅ | Chuỗi tự do VAT/Phone/Website 049 → 051 · đuôi tệp hợp lệ/không hợp lệ 107 → 110 · KH Active/Inactive 076 |
| 2 | Boundary Value Analysis | 🟡 Nông | Company 190/191/192 · Port 0/1/65535/65536 · toạ độ ±90/±91/±180/±181 · Period From = To. **Thiếu:** dung lượng tệp **đúng 128 MB** (chỉ có 129 MB — 111) · Simulate Import **101 dòng** so với nhãn `Max 100 rows are shown` (093) |
| 2 | Business Rule | 🟡 Nông | Trùng tên chỉ cảnh báo 046 → 048 · ghi đè nhóm 089 · KH Inactive ẩn 076 → 078 · khoá Currency 059/060 · chặn xoá KH có chứng từ 081 · Simulate không tạo KH 094. **Thiếu:** bộ lọc **nhiều rule kết hợp OR** — kết luận của `AMB-CUS-21` ✅ không có TC nào kiểm |
| 2 | Decision Table | ✅ | Bulk Actions đủ 6 rule R1 → R6: 086 · 087 · 088/089 · 090 · 169-a · 169-b |
| 2 | State Transition | ✅ | Đủ chuyển hợp lệ + chuyển bị chặn (xoá KH có chứng từ 081 · mở KH đã xoá 080/086) |
| 2 | Dependency | ✅ | Same as Customer Info / Copy Billing 063 · 064 · Active → ô chọn KH chứng từ 076 → 078 · Mass Delete ẩn ô nhóm 084 · Customer Admin → bộ lọc 071 |
| 2 | Use Case / Scenario | ✅ | Tạo → đếm → xoá 021 · vòng đời note 102 → 106 · tạo → tìm → xoá 168 |
| 2 | Save / Edit / Delete | 🟡 Nông | CRUD đủ cho KH · admin · note · tệp · vault. Huỷ giữa chừng có ở note (105 Cancel) và các hộp xác nhận xoá. **Thiếu:** rời form **tạo KH** đã nhập mà không Save · bấm `Close` sau khi **sửa** Vault Entry · xoá toạ độ đã lưu ở tab Map |
| 2 | Error Guessing | ✅ | Bấm 2 lần 149 · 152 · 153 · 154 · F5/Back 150 · 2 tab cùng sửa 151. Bổ sung đề xuất (Low): tab A xoá KH, tab B đang sửa bấm Save |
| 3 | Permission | ➖ / ⏭️ | Role: ➖ hợp lệ — hệ thống chỉ có role Admin, tài khoản Project Manager cũng role Admin (`AMB-LOGIN-33`). Quyền theo đối tượng (visibility Vault 143): ⏭️ `AMB-CUS-23`. ⚠️ Index chỉ ghi ➖ cho cả nhánh — nên tách nhãn `➖ role · ⏭️ AMB-CUS-23 visibility` |
| 3 | Security | 🟡 Nông | Chưa đăng nhập 155 · XSS/SQLi ở Company · Note · Vault · ô tìm kiếm 156 → 160 · che mật khẩu 137 · 161 · mất phiên 162 · chặn đuôi tệp 110. **Thiếu:** HTML/XSS ở **trường địa chỉ** (Address · City · Billing Street…) — dữ liệu này hiển thị lại ở khối `To:` của Statement (016) mà không TC nào thử |
| 3 | API | ➖ | QA chưa xác nhận quyền gọi API — đội Dev xác minh (`docs/requirements/README.md`, chốt 12-10-2026) |
| 3 | Database | ➖ | Không có quyền truy vấn CSDL — đội Dev xác minh |
| 3 | Integration | ⏭️ | Email · Google Maps · cổng KH — hợp lệ: có lý do, người quyết (user 11-11-2026, `AMB-CUS-25`, `AMB-SYS-01`) và điều kiện rà lại |
| 3 | Logging / Audit | ✅ | `Added From` 102 · `created by` 136 · `Last updated by` 139 · `Date Assigned` 069 |
| 4 | Compatibility | ✅ | Edge · Firefox 168 |
| 4 | Responsive | ✅ | 1366 · 1024 · 768 — 164 · 165 |
| 4 | Accessibility | ✅ | Thứ tự Tab 166 · hộp thoại bằng bàn phím 167 · Esc 019 |
| 4 | Performance | ➖ | Không có ngưỡng cam kết, không có công cụ — đội Hạ tầng |
| 4 | Regression | ➖ | Module chưa có bug nào được đóng |
| 4 | E2E | ✅ | Customers → Invoice / Estimate / Proposal / Project 076 → 078 |

## Đối soát bảng 15 loại field

> Chỉ liệt kê field **còn thiếu mục** hoặc đáng ghi chú. Các field còn lại (Company · Groups · Currency · Default Language · Country · Note description · ô tìm kiếm · Latitude/Longitude · Period · Choose CSV File · Password Vault) đã đủ mục hoặc có lý do `không áp dụng` hợp lệ ở index.

| Field (màn hình) | Loại | Mục đã có | Mục thiếu (đích danh) |
|---|---|---|---|
| Address · City · State · Zip Code (Customer Details) | Textarea · Text | Xuống dòng · tiếng Việt · ký tự đặc biệt · chữ số (052 → 055) | **HTML tags / XSS** — index ghi *"HTML đại diện ở Note 157"*, nhưng Note không hiển thị ở nơi khác còn địa chỉ KH hiển thị lại ở Statement và chứng từ → đại diện không thay được. Max length: chờ recon (`ASM-CUS-05`) — chấp nhận tạm |
| Street · City · State · Zip · Country của **Billing** và **Shipping** (10 trường) | Textarea · Text · Dropdown | Giá trị hợp lệ 063 · 064 · 065 | **Không có dòng nào** trong bảng Field-Level của index. Tối thiểu: Street **giữ xuống dòng** sau khi lưu · Street chứa **HTML/XSS** |
| Tệp ở tab Files | File Upload | Đuôi hợp lệ 107 → 109 · sai đuôi 110 · quá 128 MB 111 · nhiều tệp 112 | **Tệp 0 KB** · **tên tệp có ký tự đặc biệt / tiếng Việt** · **kéo-thả** · biên max **đúng 128 MB** |
| Choose CSV File (Import) | File Upload | Bắt buộc 091 · tệp 0 byte 092 · sai loại 097 | **Tệp chỉ có dòng tiêu đề** (0 dòng dữ liệu) — khác với tệp 0 byte · **101 dòng** (biên hiển thị 100 dòng) |
| Server Address · Username (Vault) | Text | Bắt buộc 130 → 132 · XSS Server Address 158 | **Chỉ gồm khoảng trắng** (`   `) — chưa có REQ quy định → cần hỏi như `AMB-CUS-01` đã làm cho Company · XSS ở Username |
| Port (Vault) | Number | Min/Max · 0 · âm · tràn · để trống 134 · 135 | **Số 0 đứng đầu** (`0022`). Số thập phân: chờ `AMB-CUS-28` — index đã ghi, chấp nhận |
| Short Description (Vault) | Textarea | HTML 159 | **Xuống dòng** · nội dung dài |
| Latitude · Longitude (Map) | Number | Thập phân · biên · 0 · âm · chữ 145 → 147 | **Để trống cả 2 ô** (xoá toạ độ đã lưu) |
| Show to customers area (Files) | Checkbox | Mặc định tắt · bật giữ sau F5 (113) | **Tắt lại** (bật → tắt, giữ sau F5) |

## Coverage Gaps (TC còn thiếu)

| # | Kịch bản thiếu | Vòng / Nhánh | Priority đề xuất |
|---|---|---|---|
| 1 | Sửa KH: Address = `<img src=x onerror=alert('addr')>12 Auto Street`, City = `<script>alert('city')</script>AutoCity` → Save → mở tab `Statement` → **không** có hộp thoại bật lên, khối `To:` hiện địa chỉ dạng chữ. Lặp lại với Billing Street | V3 · Security | High |
| 2 | Bộ lọc 2 rule: `Active` = `No` **và** `Phone` = `0918<số riêng của TC>` (KH Active có Phone đó) → `Apply` → bảng có **cả** KH Inactive lẫn KH có Phone đó (kết hợp OR theo `AMB-CUS-21`); gõ tiền tố TC vào ô tìm kiếm → thấy đủ 2 KH test | V2 · Business Rule | Medium |
| 3 | Simulate Import tệp **101 dòng** hợp lệ → bảng `Simulation Data` hiện đúng **100** dòng (dòng 101 không hiện) | V2 · BVA | Medium |
| 4 | Upload tệp `.pdf` **0 KB** · tệp tên `Hóa đơn #1 (bản sao) auto_cus_tcNNN.pdf` · **kéo-thả** tệp `.png` vào vùng `Drop files here to upload` — mỗi kịch bản 1 TC (khác loại phản hồi chưa biết) | V2 · Validation | Medium |
| 5 | Sửa KH tab Billing & Shipping: Street 2 dòng `98 Billing Road` ⏎ `Floor 3` → Save → F5 → Street vẫn 2 dòng | V2 · Validation | Low |
| 6 | Tab A mở hồ sơ KH, tab B xoá chính KH đó, tab A sửa Phone bấm `Save` → không tạo lại KH, có thông báo lỗi / trang `404 Page Not Found` | V2 · Error Guessing | Medium |
| 7 | Upload tệp `.zip` **đúng 128 MB** → được nhận, bảng thêm 1 dòng | V2 · BVA | Low |
| 8 | Vault: Server Address = `   ` (3 dấu cách), Username/Password hợp lệ → bị chặn như bỏ trống — **cần AMB** vì REQ-125 chưa nói | V2 · Validation | Low |
| 9 | Đóng 6 hộp thoại của module bằng **bấm ra vùng nền tối** bên ngoài hộp thoại | V1 · Open form | Low |
| 10 | Đổi trang / sắp xếp bảng danh sách (~2.900 KH) → thấy dấu hiệu **đang tải** trước khi bảng nạp xong | V1 · Display | Low |
| 11 | Mở `/admin/clients/client`, nhập Company `auto_cus_tcNNN_20261111`, bấm menu `Customers` (rời form, không Save) → tìm tên → `No matching records found` | V2 · Save/Edit/Delete | Low |
| 12 | Mở sửa Vault Entry, đổi Short Description, bấm `Close` → thẻ vẫn mang nội dung cũ | V2 · Save/Edit/Delete | Low |
| 13 | Tab Map của KH đã lưu toạ độ: xoá trắng 2 ô → `Save` → F5 → 2 ô trống (hoặc giữ cũ — cần xác minh) | V2 · Save/Edit/Delete | Low |
| 14 | Port = `0022` → lưu, thẻ hiện `Port: 22` hoặc `0022` (cần xác minh) · Short Description 3 dòng → giữ xuống dòng · công tắc `Show to customers area` bật → tắt → F5 vẫn tắt | V2 · Validation | Low |

## TC trùng lặp — đề xuất merge

**Không có cặp TC trùng lặp cần merge.** Ba chỗ chồng lấn nhỏ, chấp nhận vì khác trọng tâm:

- `014` bước 3–4 và `102` bước 1 cùng kiểm `New Note` mở ô nhập — 014 kiểm bật/tắt (UI Behavior), 102 kiểm thêm note
- `019-a` và `057` bước 4 cùng đóng hộp thoại nhóm bằng `Close` — 057 trọng tâm là lỗi bắt buộc
- `136` và `137` cùng nhìn dòng `Password:` — 136 kiểm cả thẻ, 137 kiểm đúng 10 dấu chấm với 2 độ dài

**Số biến thể:** 151 biến thể ở 48 TC gộp (trung bình 3,1/TC, không TC nào quá 6) + 121 TC đơn = 272 đơn vị chạy — không có dấu hiệu rụng case.

## Priority

| TC | Hiện tại | Đề xuất | Lý do |
|---|---|---|---|
| `CRM_CUS_TC_018` | Low | Medium | Mang tag `@Smoke` (chạy đầu tiên mỗi build) nhưng priority thấp nhất bộ — mâu thuẫn |
| `CRM_CUS_TC_103` | Medium | High | `AMB-CUS-15` mức 🔴 — note rỗng làm sai nghĩa badge đếm. Các TC `@KnownBug` có AMB 🔴 khác (042 · 090) đều High |
| `CRM_CUS_TC_146` · `147` | Low | Medium | `AMB-CUS-20` mức 🔴 — *"Bản đồ KH sai / lỗi khi bật Google Maps"* |

Các TC còn lại: priority khớp rủi ro — Critical dành cho tạo/sửa/xoá KH, Mass Delete, chặn Company trống, chặn truy cập khi chưa đăng nhập.

## Đối chiếu kết quả chạy

`docs/executions/customers/` **chưa tồn tại** — bộ TC chưa chạy lần nào. 41 TC `@NeedsVerify` và các ghi chú `⚠️ chưa có evidence` **vẫn còn hiệu lực**, không có ghi chú nào đã cũ cần dọn. Lần chạy đầu (`/execute-test-cases`) là lượt xác nhận cho 41 TC này.

## Ghi chú khác (không trừ điểm)

- **Mâu thuẫn `ASM-CUS-11` ↔ `CRM_CUS_TC_078`:** ASM-11 viết *"không tạo giao dịch trên môi trường dùng chung"* nên TC 060 dùng KH có sẵn; nhưng TC 078 bước 1 tạo hoá đơn nháp cho KH test. Chốt một quy tắc: nếu hoá đơn nháp được phép (như 078), TC 060 tự dựng được tiền đề — cần xác minh hoá đơn **nháp** có khoá Currency không
- **Lệnh dọn dữ liệu nằm trong Expected** ở 042 · 044 · 103 · 135 (*"Dọn: xoá … nếu bị tạo"*) — nên chuyển thành bước cuối cùng của Test Steps để người chạy không bỏ sót
- `CRM_CUS_TC_114` (cổng khách hàng) và `148` bước 1 (cấu hình ở Setup) chạm khu vực ngoài phạm vi của requirements. Giữ `@Skip` là đúng vì `REQ-CUS-108` · `138` vẫn có hiệu lực — nếu PO chốt REQ-108 ngoài phạm vi viết TC thì chuyển 114 sang `@Deprecated`
- `CRM_CUS_TC_077` gộp 3 form tạo chứng từ (Estimate · Proposal · Project) — chấp nhận: cùng một REQ-139, cùng phép kiểm, mã biến thể vẫn chỉ ra form hỏng. Khác với 019 (gộp xuyên 6 nhóm chức năng của chính module)

## Kết luận & Khuyến nghị

1. **Sửa 2 TC trước lần chạy đầu:** thay payload `DROP TABLE` ở `156-d`; thêm chốt *"đúng 2 dòng — dừng nếu khác"* vào `021` bước 4
2. **Bổ sung 3 TC High/Medium có giá trị nhất:** XSS ở trường địa chỉ hiển thị lại ở Statement (gap #1) · bộ lọc nhiều rule OR (gap #2) · Simulate 101 dòng (gap #3); sau đó nhóm File Upload (gap #4)
3. **Sửa nhãn index:** `V2 · Validation` và `V2 · BVA` từ ✅ xuống 🟡 cho khớp bảng Field-Level của chính index; thêm dòng 10 trường Billing/Shipping vào bảng Field-Level; tách nhãn Permission `➖ role · ⏭️ AMB-CUS-23`
4. **Chốt dữ liệu cố định** cho "nhóm thứ hai" (056 · 089 · 169), staff thứ hai (068), KH cá nhân (031), KH có hoá đơn (060) — ghi tên + value vào ASM sau lượt recon kế tiếp; giải mâu thuẫn ASM-CUS-11 ↔ 078
5. **Chỉnh priority** 018 · 103 · 146 · 147. Muốn agent sửa luôn các điểm trên → **commit** thư mục `docs/testcases/customers/` trước (đang chưa được git theo dõi), rồi chạy `/review-testcases` **Mode FIX**
