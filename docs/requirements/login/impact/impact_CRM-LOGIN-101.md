# Impact Report — CRM-LOGIN-101 · 22-10-2026

> **Ticket:** `CRM-LOGIN-101` — Khoá tài khoản khi đăng nhập sai nhiều lần · PO chốt **21-10-2026**
> **Module:** `LOGIN` — [`REQUIREMENTS_LOGIN_SUMMARY.md`](../REQUIREMENTS_LOGIN_SUMMARY.md) (bản 1.5) → [`web/requirements_login_web.md`](../web/requirements_login_web.md) STORY-LOGIN-07
> **Nguồn TC đã rà:** [`docs/testcases/login/`](../../../testcases/login/TEST_CASES_LOGIN_SUMMARY.md) — 65 TC web (`CRM_LOGIN_TC_001` → `065`), tra theo cột `REQ ID` và theo email phép thử dùng lặp. Chưa có RTM (`docs/traceability/`), chưa có automation script cho module.
> **Workflow kế tiếp:** `/update-testcases-from-impact` — đọc file này.
>
> **Lịch sử file:** ghi lần đầu khi áp ticket (22-10-2026), rồi **ghi lại tại chỗ** cùng ngày theo các câu trả lời AMB và trạng thái triển khai. Bản này là trạng thái cuối, xem mục [Thay đổi so với bản đầu](#thay-đổi-so-với-bản-đầu). Kế hoạch `impact_plan_CRM-LOGIN-101.md` đã lập từ bản trước và **đã được cập nhật theo bản này**.

## Tóm tắt

| Nhóm | Số lượng | REQ |
|---|---|---|
| 🟢 Thêm mới (đã build) | 0 | — |
| 🟡 Sửa | 1 | REQ-LOGIN-46 — hành vi mới **chưa triển khai**, demo vẫn chạy hành vi cũ "không giới hạn" |
| 🔴 Bỏ | 0 | — |
| ⚪ Chưa build | 18 | REQ-LOGIN-51 → 56 (từ ticket) · 57 · 58 (từ `AMB-LOGIN-22` · `27`) · 59 → 68 (từ 7 AMB 🟡 chốt theo Assumption tạm). User xác nhận 22-10-2026: Dev chưa triển khai lên môi trường demo |
| ⏸️ Trùng, không tác động | 0 | — |

### Ánh xạ nguồn → REQ

| Nguồn | Nội dung (tóm tắt) | Nhóm | REQ |
|---|---|---|---|
| AC#1 | Sai mật khẩu 5 lần liên tiếp với cùng email → khoá 15 phút | 🟡 Sửa + ⚪ | REQ-46 (ngưỡng 5 lần — đảo hành vi "không giới hạn") · REQ-53 (thời hạn 15 phút) |
| AC#2 | Thông báo khoá nguyên văn `Your account is locked. Please try again in 15 minutes.` | ⚪ | REQ-51 |
| AC#3 | Đang khoá, nhập đúng mật khẩu vẫn không vào được | ⚪ | REQ-52 |
| AC#4 | Đăng nhập thành công trước khi đủ 5 lần → bộ đếm về 0 | ⚪ | REQ-54 |
| AC#5 | Chỉ khoá theo email, không theo IP | ⚪ | REQ-55 · 56 |
| AC#6 | Môi trường dùng chung: TC dùng tài khoản Project Manager, không dùng Admin | Ràng buộc kiểm thử — **không** cấp REQ | Có hiệu lực: user cấp tài khoản Project Manager (`AMB-LOGIN-30` ✅ lần 2) |
| `AMB-LOGIN-22` | Email không có trong hệ thống không bị khoá | ⚪ | REQ-57 |
| `AMB-LOGIN-23` | Lần sai thứ 5 vẫn báo `Invalid email or password`, lần thứ 6 mới báo khoá | 🟡 Sửa (AC) | REQ-46 |
| `AMB-LOGIN-27` | Bộ đếm không phân biệt hoa/thường | ⚪ | REQ-58 |
| `AMB-LOGIN-24` | Thử lại trong lúc khoá không kéo dài khoá; mốc tính từ lần sai thứ 5 | ⚪ + ✏️ AC REQ-53 | REQ-59 |
| `AMB-LOGIN-25` | Hết khoá thì bộ đếm về 0 | ⚪ | REQ-60 |
| `AMB-LOGIN-26` | Bỏ trống Password không tính vào bộ đếm | ⚪ | REQ-61 |
| `AMB-LOGIN-28` | Đang khoá vẫn dùng được Forgot Password; đặt lại mật khẩu không mở khoá sớm | ⚪ | REQ-62 · 63 |
| `AMB-LOGIN-29` | Phiên đang mở và cookie ghi nhớ không bị khoá ảnh hưởng | ⚪ | REQ-64 · 65 |
| `AMB-LOGIN-31` | Contact và staff vô hiệu hoá không bị khoá | ⚪ | REQ-66 · 67 |
| `AMB-LOGIN-32` | Bộ đếm bỏ khoảng trắng đầu/cuối email | ⚪ | REQ-68 |

**Vì sao REQ-46 là SỬA, không phải BỎ + THÊM:** REQ-46 đang mô tả chính chính sách giới hạn số lần đăng nhập sai ("không giới hạn"). Ticket giữ nguyên hành vi đó nhưng đổi tham số (không giới hạn → 5 lần) — đúng ranh giới *cùng hành vi, khác tham số* của workflow. Giữ mã để TC 036 đang neo vào REQ-46 không mồ côi.

## Tài khoản dùng cho TC của ticket

Tài khoản **Project Manager** do user cấp 22-10-2026, lưu `.env` dưới tên `EMAIL_PM` / `PASSWORD_PM` — không ghi giá trị vào tài liệu. **Cấm** dùng tài khoản Admin (ticket AC#6). Hệ quả cho người viết TC (`RISK-LOGIN-09`):

- TC làm khoá tài khoản (REQ-46 · 51 → 56 · 58 → 65 · 68) chạy **tuần tự**, cách nhau ≥ 15 phút, không chạy song song với nhau hay với TC khác dùng tài khoản Project Manager. TC dùng tài khoản Admin **không** bị ảnh hưởng.
- REQ-57 · 66 · 67 **không** khoá tài khoản test nào → chạy ở vị trí nào cũng được.
- REQ-63 đổi mật khẩu tài khoản Project Manager → cập nhật `PASSWORD_PM` ngay sau phép thử.
- REQ-55 kiểm bằng email không tồn tại — **không** dùng tài khoản Admin làm tài khoản thứ hai.

## Test case cần xử lý

| TC ID | REQ liên quan | Hành động | Lý do |
|---|---|---|---|
| CRM_LOGIN_TC_036 | REQ-LOGIN-46 | ⚠️ Review & sửa | **Expected đảo ngược:** TC đang kiểm "sai 10 lần không khoá, lần đúng sau đó vào được". Theo REQ-46 mới: sai lần **1 → 5** đều nhận `Invalid email or password`, lần thử **thứ 6** nhận thông báo khoá. Tiền điều kiện đổi sang **tài khoản Project Manager**, bỏ câu "CẤM thử trên tài khoản `.env`"; bỏ bước "đăng nhập đúng → vào Dashboard" (giờ là REQ-52, phải bị từ chối). **Giữ `@Skip`, lý do "tính năng chưa triển khai"** |
| CRM_LOGIN_TC_035 | REQ-LOGIN-13 | ✅ Không sửa | Tiền điều kiện ("cần tài khoản staff test riêng, cấm thử sai trên `.env`") vẫn đúng. Tài khoản Project Manager được cấp **cho TC khoá** — dùng cho TC 035 (gỡ `@Skip`) thì cần user đồng ý |
| CRM_LOGIN_TC_017 · 053 · 015 · 016 · 024 · 025 · 052 | REQ-LOGIN-12 | ✅ Không tác động | Email không tồn tại không bao giờ bị khoá (REQ-57) |
| CRM_LOGIN_TC_050 · 051 | REQ-LOGIN-44 · 48 | ✅ Không tác động | Mỗi TC chỉ 1 lần đăng nhập; contact / staff vô hiệu hoá không bị khoá (REQ-66 · 67) |
| — | REQ-LOGIN-51 → 68 | ➕ Viết mới (18 REQ) | Xem danh sách ở mục *Ánh xạ nguồn → REQ*. Tất cả `@Skip` lý do "tính năng chưa triển khai"; REQ-53 · 59 · 60 phải chờ 15 phút (cân nhắc `@PersonalOnly` như TC 048) · REQ-56 cần mạng thứ hai · REQ-62 · 63 cần hộp thư · REQ-66 · 67 cần tài khoản contact / staff vô hiệu hoá · REQ-68 có thể không kiểm được qua giao diện (trình duyệt tự cắt khoảng trắng ô email) |

**Mọi TC ➕ viết mới đánh `@Skip`, lý do "tính năng chưa triển khai"** — `/execute-test-cases` chấm BLOCKED, không tính FAIL. Gỡ `@Skip` khi Dev báo đã triển khai (kèm build/ngày).

**Automation:** module chưa có script — không có gì cho `/update-automation-from-impact` ở ticket này.

## Ambiguity

| Mã | Chuyển trạng thái | Ghi chú |
|---|---|---|
| AMB-LOGIN-02 | ✅ (giữ) — kết luận **bị thay thế** | "Không có giới hạn số lần sai" bị ticket thay → REQ-46 🟡. TC dựa trên kết luận cũ (TC 036) phải sửa |
| AMB-LOGIN-22 | (mới) ❓ → ✅ | Email không có trong hệ thống không bị khoá → REQ-57. Khác Assumption tạm |
| AMB-LOGIN-23 | (mới) ❓ → ✅ | Lần 5 vẫn `Invalid email or password`, lần 6 thông báo khoá → sửa AC REQ-46. Khác Assumption tạm |
| AMB-LOGIN-24 · 25 · 26 · 28 · 29 · 31 · 32 | (mới) ❓ → ✅ | Chốt **theo Assumption tạm** → REQ-59 → 68. Trùng Assumption tạm → TC hiện có không đổi |
| AMB-LOGIN-27 | (mới) ❓ → ✅ | Không phân biệt hoa/thường → REQ-58. Trùng Assumption tạm |
| AMB-LOGIN-30 | (mới) ❓ → ✅ → ✅ lần 2 | Lần 1: dùng tài khoản Admin. **Lần 2 (cuối):** dùng tài khoản Project Manager, AC#6 có hiệu lực |
| AMB-LOGIN-33 | (mới) ❓ → ✅ | Tài khoản Project Manager **cùng role Admin** — hệ thống vẫn chỉ có role Admin. Trùng Assumption tạm, không sinh REQ, Ma trận phân quyền không đổi |

## Risk

| Mã | Chuyển trạng thái | Ghi chú |
|---|---|---|
| RISK-LOGIN-01 | Cập nhật mitigation | TC khoá dùng tài khoản Project Manager, cấm Admin |
| RISK-LOGIN-04 | Cập nhật mô tả | Không vượt khoá bằng đổi hoa/thường. Thông báo khoá làm lộ email tồn tại (hệ quả `AMB-LOGIN-22`) |
| RISK-LOGIN-08 | (mới) | Khoá theo email bị lợi dụng để khoá tài khoản người khác |
| RISK-LOGIN-09 | (mới) | TC khoá trên tài khoản Project Manager: chạy tuần tự, cách 15 phút; Admin không bị ảnh hưởng |

## Cảnh báo

- 🔴 **Dev chưa triển khai ticket lên môi trường demo** (user xác nhận 22-10-2026). STORY-LOGIN-07 BLOCKED: mọi TC của ticket viết trước và đánh `@Skip`, **không** chạy trên demo. Khi Dev báo đã triển khai: ghi build/ngày vào Nhật ký requirements, gỡ `@Skip`.
- ⚠️ **`.env` chưa có `EMAIL_PM` / `PASSWORD_PM`** — user tự thêm. Agent không ghi và chưa đăng nhập thử tài khoản này.
- 🔴 **Lộ email tồn tại qua thông báo khoá** (`RISK-LOGIN-04`) và **khoá bị lợi dụng chặn người dùng** (`RISK-LOGIN-08`) — nên báo PO xác nhận là chấp nhận rủi ro có chủ đích.
- ⚠️ **Va mã ở bộ TC:** `ASM-LOGIN-03` trong [`TEST_CASES_LOGIN_SUMMARY.md`](../../../testcases/login/TEST_CASES_LOGIN_SUMMARY.md) ghi *"đề xuất mở `AMB-LOGIN-22`"* — mã đó đã cấp cho câu hỏi khác. Sửa thành *"mở AMB mới — mã kế tiếp ở metadata requirements"* (hiện `AMB-LOGIN-34`). `ASM-LOGIN-03` liên quan REQ-58 (hoa/thường) và REQ-68 (khoảng trắng).
- ⚠️ Thông báo khoá là tiếng Anh, cùng ngôn ngữ với các thông báo khác của trang Login — khác REQ-39/49 (PO chốt tiếng Việt). Theo dõi ở `RISK-LOGIN-06`.

## Thay đổi so với bản đầu

| Mục | Bản đầu | Bản này |
|---|---|---|
| REQ thêm | 51 → 56 | 51 → 68 (18 REQ) |
| AC REQ-46 | Thông báo khoá ở lần 5 hay 6 — chờ `AMB-LOGIN-23` | Lần 1 → 5 `Invalid email or password`, lần thứ 6 thông báo khoá |
| Tài khoản phép thử | Project Manager (chưa cấp) — BLOCKED | Admin (lần 1) → **Project Manager** đã cấp (lần 2, cuối) |
| TC 017 · 053 | Review có điều kiện | Không tác động |
| TC 015 · 016 · 024 · 025 · 052 | Rà dữ liệu | Không tác động |
| TC 035 | Review tiền điều kiện | Không sửa |
| AMB treo | 9 (4 🔴) | **0** — 12 AMB của ticket (`AMB-LOGIN-22` → `33`) đã chốt hết |
| Trạng thái triển khai | Chưa biết | **Chưa triển khai** — REQ-51 → 68 nhóm ⚪ Chưa build, mọi TC của ticket `@Skip` |
