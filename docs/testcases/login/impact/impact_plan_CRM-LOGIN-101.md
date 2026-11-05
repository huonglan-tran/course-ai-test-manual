# Kế hoạch cập nhật TC — CRM-LOGIN-101 · module `login`

| Mục | Giá trị |
|---|---|
| Ticket | `CRM-LOGIN-101` — Khoá tài khoản khi đăng nhập sai nhiều lần (PO chốt 21-10-2026) |
| Ngày lập | 22-10-2026 |
| Mode | **PLAN** (22-10-2026) → **✅ Đã APPLY 23-10-2026** — kết quả: [`delta_tc_CRM-LOGIN-101.md`](delta_tc_CRM-LOGIN-101.md) |
| Impact Report nguồn | [`impact_CRM-LOGIN-101.md`](../../../requirements/login/impact/impact_CRM-LOGIN-101.md) |
| Requirements hiện hành | [`REQUIREMENTS_LOGIN_SUMMARY.md`](../../../requirements/login/REQUIREMENTS_LOGIN_SUMMARY.md) bản 1.5 · [`web/requirements_login_web.md`](../../../requirements/login/web/requirements_login_web.md) STORY-LOGIN-07 |
| Bộ TC | [`TEST_CASES_LOGIN_SUMMARY.md`](../TEST_CASES_LOGIN_SUMMARY.md) — 65 TC · 115 biến thể · Web · độ hạt **GỘP** (giữ nguyên) |
| Nền tảng bị chạm | **Web** (module chỉ có Web) |
| Nguồn map REQ → TC | Cột `REQ ID` của 3 part + Bảng Đối Soát Coverage ở index. Chưa có RTM |

## 1. Ánh xạ REQ → TC

### ✅ Chắc chắn (theo cột `REQ ID`)

| REQ | Delta | TC | File · dòng | Hành động |
|---|---|---|---|---|
| REQ-LOGIN-46 🟡 | "Không giới hạn số lần sai" → **sai 5 lần liên tiếp thì bị khoá**; lần 1 → 5 hiện `Invalid email or password`, lần thử thứ 6 hiện `Your account is locked. Please try again in 15 minutes.` Tài khoản phép thử: **Project Manager** (`EMAIL_PM` / `PASSWORD_PM`), cấm Admin. **Chưa triển khai** | `CRM_LOGIN_TC_036` | `web/parts/part_02_web_forgot_phien.md:33` | ✏️ Sửa |
| REQ-LOGIN-13 (không đổi) | Impact Report liệt kê để review tiền điều kiện | `CRM_LOGIN_TC_035` | `web/parts/part_02_web_forgot_phien.md:32` | ✅ **Không sửa** — xem mục 3 |
| REQ-LOGIN-12 (không đổi) | Email không tồn tại không bị khoá (REQ-57) → không còn nguy cơ FAIL giả | `015` · `016` · `017` · `024` · `025` · `052` · `053` | part_01 · part_03 | ✅ Không tác động |
| REQ-LOGIN-44 · 48 (không đổi) | Contact / staff vô hiệu hoá không bị khoá (REQ-66 · 67) — mỗi TC chỉ 1 lần đăng nhập | `050` · `051` | part_03 | ✅ Không tác động |

### ⚠️ Suy luận — cần xác nhận

Không có. Mọi TC bị chạm đều map qua cột `REQ ID`.

### ❓ Chưa có TC (➕ ngoài phạm vi DELTA)

| REQ | Nội dung | Trạng thái REQ |
|---|---|---|
| REQ-LOGIN-51 | Thông báo khoá nguyên văn từ lần thử thứ 6 | ⚪ chưa build |
| REQ-LOGIN-52 | Sau lần sai thứ 5, mật khẩu đúng vẫn bị từ chối | ⚪ chưa build |
| REQ-LOGIN-53 | Hết 15 phút đăng nhập được lại | ⚪ chưa build |
| REQ-LOGIN-54 | Đăng nhập thành công đặt lại bộ đếm | ⚪ chưa build |
| REQ-LOGIN-55 | Email khác cùng IP không bị chặn | ⚪ chưa build |
| REQ-LOGIN-56 | Email bị khoá vẫn bị khoá từ IP khác | ⚪ chưa build |
| REQ-LOGIN-57 | Email không tồn tại không bị khoá | ⚪ chưa build |
| REQ-LOGIN-58 | Bộ đếm không phân biệt hoa/thường | ⚪ chưa build |
| REQ-LOGIN-59 | Thử lại trong lúc khoá không dời mốc mở khoá | ⚪ chưa build |
| REQ-LOGIN-60 | Hết khoá thì bộ đếm về 0 | ⚪ chưa build |
| REQ-LOGIN-61 | Bỏ trống Password không tính vào bộ đếm | ⚪ chưa build |
| REQ-LOGIN-62 | Đang khoá vẫn dùng được Forgot Password | ⚪ chưa build |
| REQ-LOGIN-63 | Đặt lại mật khẩu không mở khoá sớm | ⚪ chưa build |
| REQ-LOGIN-64 | Khoá không đẩy phiên đang mở ra ngoài | ⚪ chưa build |
| REQ-LOGIN-65 | Cookie ghi nhớ vẫn tự đăng nhập khi đang khoá | ⚪ chưa build |
| REQ-LOGIN-66 | Email contact không bị khoá | ⚪ chưa build |
| REQ-LOGIN-67 | Staff vô hiệu hoá không bị khoá | ⚪ chưa build |
| REQ-LOGIN-68 | Bộ đếm bỏ khoảng trắng đầu/cuối email | ⚪ chưa build |

## 2. Nhánh 4 vòng bị chạm

```
Nhánh bị chạm:   V2 · Business Rule (TC 036) · V2 · Boundary Value Analysis (biên lần 5 / lần 6)
                 V2 · Decision Table (thêm điều kiện "số lần sai / đang khoá") · V2 · State Transition (thêm trạng thái "Tài khoản bị khoá")
Chạm, chờ TC mới: V1 · Display (khung hiển thị thông báo khoá — chưa có evidence vì chưa triển khai)
Nhánh KHÔNG đụng: V1 · UI cơ bản · Open form · Input valid · Save · Verify — toàn bộ V3, V4 giữ nguyên
```

**Vì sao `V1 · UI cơ bản` không đổi:** ticket không thêm, bớt hay đổi nhãn thành phần nào trên màn hình Login ở trạng thái mặc định. Thứ mới duy nhất nhìn thấy được là thông báo khoá, và nó chỉ hiện **sau** 5 lần gửi sai. Vì vậy nó thuộc `V2 · Business Rule`, phần hiển thị thuộc `V1 · Display` (TC mới), không thuộc bảng kiểm tĩnh của TC 001.

## 3. Kế hoạch sửa từng TC

| # | `file:dòng` | TC ID | Vòng · Nhánh | Sửa ô nào | Nội dung sửa | Evidence |
|---|---|---|---|---|---|---|
| 1 | `part_02_web_forgot_phien.md:33` | `CRM_LOGIN_TC_036` | V2 · Business Rule + BVA | `Test Scenario` · `Pre-Condition` · `Test Steps` · `Test Data` · `Expected Result` · `Tags` | **Test Scenario:** `Đăng nhập sai mật khẩu 5 lần liên tiếp thì tài khoản bị khoá — thông báo khoá hiện từ lần thử thứ 6`<br>**Pre-Condition:** cửa sổ ẩn danh Chrome mới, chưa đăng nhập · tài khoản **Project Manager** (`EMAIL_PM` / `PASSWORD_PM` trong `.env` — tester tự nhập) vừa đăng nhập thành công rồi đăng xuất (bộ đếm sạch) · **cấm** dùng tài khoản Admin · ⛔ tính năng **chưa triển khai** lên demo · ⚠️ TC làm khoá tài khoản Project Manager 15 phút → chạy tuần tự, cách TC khoá khác ≥ 15 phút. Bỏ câu "CẤM thử trên tài khoản `.env`" (đã thay bằng chỉ định tài khoản)<br>**Steps:** 1. Mở `/admin/authentication` · 2. Nhập email tài khoản Project Manager + Password sai thứ nhất, bấm `Login` · 3. Lặp lại bước 2 cho đủ **5** lần, mỗi lần một chuỗi Password sai khác nhau · 4. Quan sát trang sau lần thứ 5 · 5. Lần thứ 6: nhập email tài khoản Project Manager + Password sai, bấm `Login` · 6. Quan sát trang<br>**Test Data:** Password sai `Wrong_20261022_01` → `Wrong_20261022_06` (mỗi lần một chuỗi)<br>**Expected:** 2–3. Mỗi lần ở lại trang Login, khung đỏ `Invalid email or password` · 4. Lần thứ 5 **vẫn** là `Invalid email or password` — **chưa** có thông báo khoá · 6. Hiện thông báo nguyên văn `Your account is locked. Please try again in 15 minutes.`, không vào Dashboard. ⚠️ Kiểu khung (màu, vị trí) chưa có evidence → không đối chiếu kiểu, chỉ đối chiếu chữ<br>**Tags:** `@Regression @Security @Skip` → thêm `@NeedsVerify` | ⛔ Không có — tính năng chưa triển khai. Câu chữ lấy nguyên văn từ ticket AC#2 (PO chốt) và `AMB-LOGIN-23` ✅ → **sửa được phần chữ**, gắn `@NeedsVerify` cho phần kiểu hiển thị |
| 2 | `part_02_web_forgot_phien.md:32` | `CRM_LOGIN_TC_035` | V2 · Business Rule | — | **Không sửa.** Tiền điều kiện hiện tại ("cần tài khoản staff test riêng · CẤM thử sai trên `.env`") **vẫn đúng**: tài khoản Project Manager được cấp cho TC khoá của STORY-07. Expected `Invalid email or password` cũng vẫn đúng: 1 lần sai < 5. 💡 Nếu user đồng ý dùng tài khoản Project Manager cho TC 035 thì TC này gỡ `@Skip` và chạy được ngay hôm nay (chỉ 1 lần sai, không khoá) — **cần quyết định riêng** | — |

> Cột `Automation` của TC 036: giữ **`Partial`**. Trục chặn đổi từ "tiền đề · môi trường dùng chung — Điều kiện #1" sang **"4 · khoá tài khoản dùng chung"** (tiêu chí 4 của `automation_criteria.md`). Thêm **⏸️ Hoãn — tính năng chưa lên môi trường test** (mục 6 của tiêu chí). `Auto Type` giữ `UI`.

## 4. Tác động lan toả — phần ngoài dòng TC

| # | Vị trí | Thay đổi đề xuất | Lý do |
|---|---|---|---|
| L1 | `part_02_web_forgot_phien.md` — header, dòng **Tài khoản thật** | Thêm dòng: *"TC khoá tài khoản (`CRM_LOGIN_TC_036` và TC của STORY-LOGIN-07) dùng **tài khoản Project Manager** (`EMAIL_PM` / `PASSWORD_PM`) — cấm tài khoản Admin (ticket `CRM-LOGIN-101` AC#6) · chạy tuần tự, cách nhau ≥ 15 phút"* | Header hiện chỉ nói tài khoản Admin và cấm thử sai trên `.env` — thiếu chỉ dẫn tài khoản cho TC 036 mới |
| L2 | `part_02_web_forgot_phien.md` — header, dòng **Tag `@Skip`** | Thêm lý do "tính năng chưa triển khai (`CRM-LOGIN-101`)" | Lý do hiện tại chỉ có "thiếu tài khoản / hộp thư / bộ đếm giờ" |
| L3 | Index · **Bảng Đối Soát Coverage** | Dòng REQ-46 → `Khoá sau 5 lần sai liên tiếp` · 🟡 · `036` · ghi chú `@Skip` — chưa triển khai. Thêm 18 dòng REQ-51 → 68: 0 TC · `➕ chưa có TC — ngoài phạm vi DELTA`. Dòng tổng → `50/68 REQ có TC · 18 REQ ⚪ chưa build chờ sinh TC` | Gate 4: REQ 🟡 vẫn có TC active (036) |
| L4 | Index · **Decision Table — kết quả bấm Login** | Thêm cột điều kiện **"Trạng thái khoá"**. R6 ghi rõ `lần sai 1 → 5`. R7 thêm `không bị khoá`. Thêm R10 *Tồn tại · Sai · lần thử thứ 6 → thông báo khoá* (TC 036) · R11 *Tồn tại · Đúng · đang khoá → thông báo khoá* (`—` chờ TC REQ-52) · R12 *Không tồn tại · Có · lần ≥ 6 → `Invalid email or password`* (`—` chờ TC REQ-57) · R13 *Contact · Sai · lần ≥ 6 → không khoá* (`—` chờ TC REQ-66) · R14 *Staff vô hiệu hoá · Sai · lần ≥ 6 → không thông báo khoá* (`—` chờ TC REQ-67) · R15 *Tồn tại · Trống · — → `The Password field is required.`, không tính lần sai* (`—` chờ TC REQ-61) | Thêm điều kiện thứ 4 vào bảng ≥ 3 điều kiện → bảng cũ không còn đủ rule |
| L5 | Index · **State Transition** | Thêm hàng **"Tài khoản bị khoá"** và cột **"Hết 15 phút"**. Chuyển `Chưa đăng nhập → Bị khoá` = 036. Các ô còn lại (đăng nhập đúng khi khoá · thử lại khi khoá · hết 15 phút · Forgot khi khoá · phiên đang mở · cookie ghi nhớ khi khoá · email khác) = `—` chờ TC REQ-52 · 59 · 53/60 · 62/63 · 64 · 65 · 55 | Đối chiếu ma trận trạng thái phiên đã cập nhật ở requirements |
| L6 | Index · **Đối soát 4 vòng** | `V2 · Business Rule`: `036 (không giới hạn sai)` → `036 (khoá sau 5 lần sai)` · `V2 · Boundary Value Analysis`: thêm `036 (lần 5 / lần 6)` · `V2 · Decision Table`: `9 rule` → `12 rule (2 rule chờ TC mới)` · `V2 · State Transition`: `3 trạng thái` → `4 trạng thái`. `V1 · Display`: ghi chú thông báo khoá chờ TC mới | Chỉ chấm lại nhánh bị chạm |
| L7 | Index · **Đối soát cột Automation** | Điều kiện #1: bỏ `036` khỏi danh sách phụ thuộc · thêm **Điều kiện #7** *"Tài khoản Project Manager trong `.env` (`EMAIL_PM` / `PASSWORD_PM`) — user cấp 22-10-2026, ⏳ chờ thêm vào `.env`"* → `036` + TC khoá mới · thêm **Điều kiện #8** *"Dev triển khai `CRM-LOGIN-101` lên demo — ⏳ chưa có"* → `036` + TC mới · dòng `CRM_LOGIN_TC_036`: `Partial · ⏸️ Hoãn` · trục `4 · khoá tài khoản dùng chung` · bảng đếm `⏸️ Hoãn 12 → 13` | Theo mục 3 |
| L8 | Index · **Assumptions** `ASM-LOGIN-03` | *"đề xuất mở `AMB-LOGIN-22`"* → *"mở AMB mới — mã kế tiếp ở metadata requirements (hiện `AMB-LOGIN-34`)"*; ghi thêm liên quan REQ-58 (bộ đếm không phân biệt hoa/thường) và REQ-68 (bộ đếm bỏ khoảng trắng) | `AMB-LOGIN-22` đã cấp cho câu hỏi khác — để nguyên là trỏ sai |
| L9 | Index · **Nhãn đặc biệt** dòng `@Skip` · `@NeedsVerify` | `@Skip`: thêm lý do "tính năng chưa triển khai (036)" · `@NeedsVerify`: 12 → **13** TC, thêm `036` | Đếm tag phải khớp |
| L10 | Index · **Vùng chưa có evidence** | Thêm dòng *"Thông báo khoá tài khoản (kiểu khung, vị trí) → 036 → chụp sau khi Dev triển khai"* | `@NeedsVerify` mới phải có dòng ở đây |
| L11 | Index · metadata | `Nguồn requirement` → bản 1.5, `REQ-LOGIN-01` → `68` · `Phạm vi REQ` → `50/68 REQ có TC — REQ-51 → 68 chưa có TC (ngoài phạm vi DELTA)` | Bản nguồn đã đổi |
| L12 | Index · **ISO 25010** — Security | Ghi rõ 036 kiểm cơ chế khoá chống dò mật khẩu | Đặc tính bị ticket chạm. Các đặc tính khác giữ nguyên |
| L13 | `docs/testcases/README.md` | Cập nhật: REQ bao phủ 50/68, ngày cập nhật. Số TC giữ nguyên (65) | Gate 7 |

**Không có:**
- TC bị Deprecated, và không TC nào lấy TC bị đổi làm tiền đề.
- TC mới trong phạm vi REQ 🟡. Biên lần 5 / lần 6 nằm trong chính TC 036. Phép thử "4 lần sai → đúng" là REQ-54, nằm ngoài phạm vi.
- Vượt ngưỡng tách part: số TC giữ 65, part_02 vẫn 23 TC.

## 5. Điều kiện trước khi APPLY

| # | Điều kiện | Trạng thái |
|---|---|---|
| 1 | **Mốc git:** `docs/testcases/` **chưa từng được commit** (`git status` → `?? docs/testcases/`). Không có mốc thì sau khi sửa tại chỗ không ai đối chiếu được bản trước | ✅ User đã commit — mốc `f8025be` |
| 2 | User duyệt kế hoạch — đặc biệt 3 điểm: (a) TC 035 **không sửa** · (b) TC 036 sửa chữ + gắn `@NeedsVerify` dù chưa có evidence · (c) ghi ràng buộc "chạy tuần tự, cách nhau 15 phút" vào **Pre-Condition** thay vì tạo tag mới (`/execute-test-cases` và viewer không hiểu tag lạ) | ✅ User chạy APPLY 23-10-2026 |
| 3 | `.env` có `EMAIL_PM` / `PASSWORD_PM` — **không** chặn APPLY (TC 036 vẫn `@Skip`), nhưng là điều kiện để chạy TC sau này | ⏳ User tự thêm |

## 6. Ngoài phạm vi

| Việc | Command |
|---|---|
| Sinh TC cho REQ-LOGIN-51 → 68 (18 REQ ⚪ chưa build). Bổ sung nối tiếp từ `CRM_LOGIN_TC_066`, độ hạt GỘP, `@Skip` lý do "chưa triển khai", tài khoản Project Manager, Pre-Condition ghi ràng buộc chạy tuần tự. REQ-57 · 66 · 67 không khoá tài khoản test → không cần ràng buộc đó | `/generate-testcases-from-requirements` (chế độ BỔ SUNG) |
| Automation | Module chưa có script — không có gì cho `/update-automation-from-impact` |
| Ma trận truy vết | `/generate-traceability-matrix` sau khi APPLY + bổ sung TC |
| Gỡ `@Skip` của 036 và TC mới | Khi Dev báo đã triển khai — ghi build/ngày vào Nhật ký requirements |
