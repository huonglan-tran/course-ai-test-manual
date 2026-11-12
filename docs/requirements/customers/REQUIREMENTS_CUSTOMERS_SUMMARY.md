# Requirements — Customers (`CUS`)

> **INDEX module — tên file bất biến.** Phần dùng chung mọi nền tảng + Bản đồ tài liệu. REQ chi tiết nằm ở các file Story của tầng `web/` (xem [Bản đồ tài liệu](#bản-đồ-tài-liệu)).
>
> Danh mục hệ thống: [`../README.md`](../README.md) · Bản đồ khám phá: [`../_discovery/modules/module_02_khach_hang.md`](../_discovery/modules/module_02_khach_hang.md)

## 1. Metadata

| Thuộc tính | Giá trị |
|---|---|
| **Hệ thống** | Perfex CRM — khu Admin |
| **Module** | Customers (route `clients`) — danh sách, bộ lọc, tạo/sửa/xoá KH, Active, Bulk Actions, Import, hồ sơ KH và các tab thuộc `CUS`: Profile (Customer Details · Billing & Shipping · Customer Admins) · Notes · Statement · Files · Vault · Map |
| **Prefix** | `CUS` — cấp bởi `/discover-system` 12-10-2026 |
| **Tiền tố TC ID** | `CRM_CUS_TC_<3 số>` |
| **Nền tảng** | Web ✅ (khảo sát 11-11-2026) · Mobile — chưa có · API — chưa có |
| **Phiên bản tài liệu** | 2.1 — 11-11-2026: user chốt `AMB-CUS-01` → `27` (mục 6.1). 2.0 — 11-11-2026: **khảo sát lại từ đầu** theo yêu cầu user (bản 1.0 ngày 04-11-2026 đã bị gỡ khỏi thư mục làm việc, user chọn đánh lại mã từ `REQ-CUS-01`) |
| **Dải mã đã dùng** | `REQ-CUS-01` → `REQ-CUS-138` (đợt 1 UI recon 11-11-2026) · `REQ-CUS-139` → `REQ-CUS-141` (chốt AMB 11-11-2026) · `AMB-CUS-01` → `AMB-CUS-28` · `RISK-CUS-01` → `RISK-CUS-09` · `STORY-CUS-01` → `STORY-CUS-08` |
| **Mã kế tiếp** | Đợt phân tích sau bắt đầu từ `REQ-CUS-142` · `AMB-CUS-29` · `RISK-CUS-10` · `STORY-CUS-09` — **KHÔNG đánh lại từ 01** |
| **Trạng thái REQ** | 🟢 113 · 🟡 9 · 🔴 0 · ⚪ 19 — tổng 141. 🟡 = hành vi mong đợi **đã đổi** theo kết luận AMB, **hệ thống hiện chưa đạt** — ứng viên bug, TC dự kiến FAIL tới khi Dev sửa (mục 6.3). ⚪ = có trên hệ thống nhưng **chưa kiểm chứng**: không được phép / không an toàn trên môi trường dùng chung (Import thật, gửi email, tải file, xoá KH có chứng từ, lưu bộ lọc dùng chung, tạo nhóm KH), cần thao tác agent không làm (nhập mật khẩu đăng nhập, đăng nhập tài khoản thứ hai, cổng khách hàng, Google API key), hoặc chốt từ câu trả lời AMB chưa chạy trên UI (REQ-139 → 141) |

> ⚠️ **Lưu ý truy vết:** bản 1.0 (04-11-2026) cũng dùng dải `REQ-CUS-01 → 101`. Bản 2.0 **đánh lại mã** theo lựa chọn của user (11-11-2026) — cùng một mã ở hai bản **không** chỉ cùng một hành vi. Bộ test case / automation viết theo bản 1.0 (nếu có, tra git) **phải map lại** theo bản này.

## 2. Tổng quan

Module `CUS` quản lý **khách hàng (công ty hoặc cá nhân)** — thực thể gốc mà gần như mọi chứng từ Sales, dự án, hợp đồng, ticket đều gắn vào. Staff xem danh sách có 6 bộ đếm Summary, tìm kiếm và bộ lọc nâng cao (15 rule); tạo/sửa KH với thông tin công ty, nhóm, tiền tệ, ngôn ngữ, địa chỉ chính/billing/shipping; gán Customer Admin; bật/tắt Active; xoá đơn lẻ hoặc hàng loạt; gán nhóm hàng loạt; import CSV (có chế độ Simulate); và quản lý hồ sơ KH qua 19 tab.

| Trong phạm vi | Ngoài phạm vi |
|---|---|
| Danh sách `/admin/clients` · Summary · tìm kiếm · bộ lọc · Export | Tab **Contacts**, modal "Add new contact" và `/admin/clients/all_contacts` → module `CONT` |
| Form tạo/sửa KH (2 tab + Customer Admins) · modal thêm nhóm KH | Danh sách chứng từ trong 11 tab Invoices…Tickets → module chứng từ; `CUS` chỉ giữ REQ về KH Inactive bị ẩn khỏi ô chọn KH (`RISK-SYS-04`) |
| Active · Delete · Bulk Actions (Mass Delete, Groups) | Tab **Reminders** → module `PROF` |
| Import Customers (giao diện + Simulate) | Cổng Customer/Contact (URL khác) |
| Tab Notes · Statement · Files · Vault · Map | Quản lý Customer Groups (sửa/xoá), Currency, ngôn ngữ, Google API key, đuôi tệp cho phép — nằm ở Setup (bị chặn, `AMB-SYS-01`) |

### 2.1. Bản đồ phủ tài liệu

Không có tài liệu — toàn bộ REQ sinh từ khảo sát UI thực tế (user xác nhận 11-11-2026).

## Bản đồ tài liệu

| Nền tảng | File | Story | REQ bao phủ |
|---|---|---|---|
| Chung ≥ 2 nền tảng | chính file này — mục 3 | — | *(chưa có — module mới có 1 nền tảng)* |
| Web — metadata khảo sát · NFR · Danh mục Evidence | [web/requirements_customers_web.md](web/requirements_customers_web.md) | — | *(không chứa dòng REQ)* |
| Web | [web/stories/story_01_danh_sach_bo_loc.md](web/stories/story_01_danh_sach_bo_loc.md) | STORY-CUS-01 | `REQ-CUS-01` → `27` |
| Web | [web/stories/story_02_tao_khach_hang.md](web/stories/story_02_tao_khach_hang.md) | STORY-CUS-02 | `REQ-CUS-28` → `54` |
| Web | [web/stories/story_03_ho_so_sua_customer_admins.md](web/stories/story_03_ho_so_sua_customer_admins.md) | STORY-CUS-03 | `REQ-CUS-55` → `65` |
| Web | [web/stories/story_04_active_xoa_bulk.md](web/stories/story_04_active_xoa_bulk.md) | STORY-CUS-04 | `REQ-CUS-66` → `82` · `139` · `140` |
| Web | [web/stories/story_05_import.md](web/stories/story_05_import.md) | STORY-CUS-05 | `REQ-CUS-83` → `95` · `141` |
| Web | [web/stories/story_06_notes_files.md](web/stories/story_06_notes_files.md) | STORY-CUS-06 | `REQ-CUS-96` → `112` |
| Web | [web/stories/story_07_statement.md](web/stories/story_07_statement.md) | STORY-CUS-07 | `REQ-CUS-113` → `122` |
| Web | [web/stories/story_08_vault_map.md](web/stories/story_08_vault_map.md) | STORY-CUS-08 | `REQ-CUS-123` → `138` |

## 3. Yêu cầu dùng chung nhiều nền tảng

*Chưa có.* Module hiện chỉ khảo sát Web — REQ chưa kiểm trên nền tảng thứ hai thì nằm ở tầng `web/` (skill mục 2.2).

## 4. Ma trận phân quyền

> Hệ thống **chỉ có role Admin** (user xác nhận 19-10-2026 — danh mục). Không có cột role khác. Cột Contact để ghi nhận đã cân nhắc loại tài khoản không phải staff.

| Hành động | Staff role Admin — tài khoản `.env` | Contact (khách hàng) |
|---|---|---|
| Xem danh sách · Summary · tìm kiếm · lọc (REQ 01 → 26) | ✅ | — |
| Tạo KH (REQ 48 · 49) | ✅ | — |
| Sửa KH (REQ 59) | ✅ | — |
| Bật/tắt Active (REQ 66 · 68) | ✅ | — |
| Xoá KH · Mass Delete (REQ 71 · 78) | ✅ | — |
| Bulk gán nhóm (REQ 80) | ✅ | — |
| Import — Simulate (REQ 88) | ✅ | — |
| Gán/gỡ Customer Admin (REQ 63 · 64) | ✅ | — |
| Notes · Files · Statement · Map (REQ 98 · 106 · 115 · 136) | ✅ | — |
| Tạo/sửa/xoá Vault entry (REQ 127 · 130 · 131) | ✅ | — |
| Xem mật khẩu Vault (REQ 133) | ❔ | — |
| Xem vault entry `Visible only to me` do staff khác tạo (REQ 134) | ❔ | — |

```
Tổng 24 ô = Đã kiểm chứng 10 · Suy diễn 0 · Chưa rõ 2 · Không áp dụng 12
12 ô "không áp dụng": cột [Contact] — contact không đăng nhập được khu Admin (REQ-LOGIN-44, theo xác nhận user AMB-LOGIN-20).
2 ô "chưa rõ": [Xem mật khẩu Vault] cần nhập mật khẩu đăng nhập — agent không nhập, user tự chạy TC thủ công (AMB-CUS-22 ✅) ·
  [Vault visibility] cần đăng nhập đồng thời 2 staff — chấp nhận chưa kiểm ở giai đoạn này (AMB-CUS-23 ⏭️).
```

## 5. Ma trận trạng thái KH

> Entity KH chỉ có cờ **Active** (bật/tắt), không có status flow nhiều bước.

| Trạng thái hiện tại | Hành động | Trạng thái kế tiếp | Ai thực hiện | REQ |
|---|---|---|---|---|
| *(chưa tồn tại)* | Save / Save and create contact | Active | Staff role Admin | 48 · 49 · 50 |
| Active | Tắt switch Active ở danh sách | Inactive (banner cảnh báo trên hồ sơ, ẩn khỏi ô chọn KH của Invoice) | Staff role Admin | 66 · 67 · 69 |
| Inactive | `Mark as active` trên banner hồ sơ | Active | Staff role Admin | 68 |
| Inactive | Bật switch Active ở danh sách | Active | Staff role Admin | 66 — chiều bật **chưa** thử trực tiếp (endpoint cùng mẫu `.../<id>/<0\|1>`) |
| Active / Inactive | Delete (menu hồ sơ / link dòng) · Mass Delete | Đã xoá (hồ sơ trả 404) | Staff role Admin | 70 · 71 · 72 · 78 |
| Active / Inactive (có chứng từ) | Delete | **Bị chặn** — giữ nguyên trạng thái, không xoá dây chuyền (chưa kiểm) | Staff role Admin | 73 ⚪ · `AMB-CUS-07` ✅ |

## 6. Điểm mơ hồ & Rủi ro

### 6.1. Ambiguities

| Mã | Câu hỏi | Nguy cơ | Mức độ | Assumption tạm | Trạng thái | Kết luận |
|---|---|---|---|---|---|---|
| AMB-CUS-01 | Company chỉ gồm khoảng trắng vẫn tạo được KH tên rỗng (REQ-CUS-31) — lỗi hay chấp nhận? | KH không tên xuất hiện ở danh sách và mọi dropdown chứng từ, không tìm được | 🔴 | **Lỗi** — phải chặn như bỏ trống, báo `This field is required.` | ✅ Đã trả lời 11-11-2026 | User: **là lỗi**. `REQ-CUS-31` 🟡 đổi sang kỳ vọng đúng: chỉ khoảng trắng bị chặn như bỏ trống, báo `This field is required.`. Hệ thống hiện **chưa đạt** → **mở bug** (mục 6.3) |
| AMB-CUS-02 | Company > 191 ký tự bị cắt âm thầm còn 191 (REQ-CUS-32). Giới hạn đúng là bao nhiêu? UI có nên đặt `maxlength` / báo lỗi? | Mất dữ liệu không ai biết | 🔴 | Giới hạn 191; UI nên chặn và báo | ✅ Đã trả lời 11-11-2026 | User: **giới hạn 191 ký tự; UI phải chặn**, câu thông báo do QA đề xuất → `REQ-CUS-32` 🟡: 191 lưu đủ, 192 bị chặn với `Please enter no more than 191 characters.`. Hệ thống hiện chưa đạt → ứng viên bug |
| AMB-CUS-03 | Cảnh báo trùng tên cho phép lưu trùng (REQ-CUS-36) — cố ý? | Tạo trùng KH → công nợ tách đôi | 🟡 | Cố ý — đúng câu chữ cảnh báo | ✅ Đã trả lời 11-11-2026 | Chốt theo Assumption tạm: cố ý — `REQ-CUS-36` không đổi |
| AMB-CUS-04 | VAT Number · Phone · Website không có ràng buộc định dạng (REQ-CUS-37 · 38 · 39) — cố ý? | Dữ liệu liên lạc sai định dạng in lên chứng từ | 🟡 | Cố ý — chấp nhận mọi chuỗi | ✅ Đã trả lời 11-11-2026 | Chốt theo Assumption tạm: cố ý, chấp nhận mọi chuỗi — `REQ-CUS-37` · `38` · `39` không đổi |
| AMB-CUS-05 | Checkbox `Show primary contact full name on Invoices, Estimates, Payments, Credit Notes` có trong DOM nhưng ẩn ở KH chưa có contact — hiện khi nào? | Bỏ sót TC tuỳ chọn hiển thị trên chứng từ | 🟡 | Chỉ hiện khi KH có primary contact — kiểm lại khi recon `CONT` | ✅ Đã trả lời 11-11-2026 | Chốt theo Assumption tạm: checkbox chỉ hiện khi KH có primary contact — không sinh REQ ở `CUS`, kiểm khi recon `CONT` |
| AMB-CUS-06 | KH Inactive bị ẩn khỏi **mọi** ô chọn KH (Estimate, Proposal, Project…) như tooltip nói, hay chỉ Invoice (đã kiểm — REQ-CUS-69)? Áp cho chứng từ đang sửa không? | Lập chứng từ cho KH ngừng hoạt động | 🟡 | Ẩn ở mọi form tạo mới; chứng từ cũ giữ nguyên | ✅ Đã trả lời 11-11-2026 | Chốt theo Assumption tạm: ẩn ở **mọi** form tạo mới, chứng từ đã lập giữ nguyên KH → sinh `REQ-CUS-139` · `140` (⚪) |
| AMB-CUS-07 | Xoá KH đang có hoá đơn / dự án / hợp đồng: chặn, hay xoá dây chuyền chứng từ (REQ-CUS-73)? | Mất dữ liệu kế toán | 🔴 | Hệ thống chặn kèm thông báo | ✅ Đã trả lời 11-11-2026 | Chốt theo Assumption tạm: **chặn** kèm thông báo, không xoá dây chuyền — ghi vào AC `REQ-CUS-73` (vẫn ⚪, nguyên văn thông báo chưa biết) |
| AMB-CUS-08 | Bulk Actions khi **chưa chọn KH nào** vẫn hỏi xác nhận và báo `Total customers deleted: 0` (REQ-CUS-79) — có nên chặn trước bằng thông báo "chưa chọn"? | Người dùng tưởng đã xoá | 🟡 | Chấp nhận hành vi hiện tại | ✅ Đã trả lời 11-11-2026 | Chốt theo Assumption tạm: chấp nhận hành vi hiện tại — `REQ-CUS-79` không đổi |
| AMB-CUS-09 | Bulk Groups **ghi đè** nhóm cũ thay vì thêm (REQ-CUS-81) — cố ý? | Mất phân nhóm cũ hàng loạt | 🟡 | Cố ý (tên field `move_to_groups...`) | ✅ Đã trả lời 11-11-2026 | Chốt theo Assumption tạm: cố ý ghi đè — `REQ-CUS-81` không đổi |
| AMB-CUS-10 | Bulk Confirm khi **không chọn nhóm nào** và không tick Mass Delete **xoá sạch** nhóm của mọi KH đã chọn, không cảnh báo (REQ-CUS-82) — lỗi? | Mất phân nhóm hàng loạt chỉ vì bấm nhầm Confirm | 🔴 | **Lỗi** — phải chặn hoặc cảnh báo | ✅ Đã trả lời 11-11-2026 | Chốt theo Assumption tạm: **là lỗi** → `REQ-CUS-82` 🟡: không được gỡ nhóm âm thầm (chặn hoặc cảnh báo riêng). Hệ thống hiện chưa đạt → ứng viên bug. Hình thức + nguyên văn chưa có → mở `AMB-CUS-28` |
| AMB-CUS-11 | Simulate Import không đánh dấu dòng thiếu cột bắt buộc, email sai định dạng, email trùng (REQ-CUS-90). Import thật xử lý các dòng này thế nào? | Import "thành công" nhưng thiếu dữ liệu / tạo KH rác | 🔴 | Import bỏ qua dòng lỗi và báo số dòng bị bỏ | ✅ Đã trả lời 11-11-2026 | Chốt theo Assumption tạm: Import thật bỏ qua dòng lỗi và báo số dòng bị bỏ → sinh `REQ-CUS-141` (⚪). `REQ-CUS-90` giữ là hành vi hiện tại của Simulate |
| AMB-CUS-12 | Có được chạy Import thật, tải `Download Sample`, Export file trên môi trường demo không (REQ-CUS-21 · 93 · 95)? Import có gửi email chào mừng tới contact không? | Không kiểm được tác tạo; gửi email tới địa chỉ thật | 🟡 | Không chạy cho tới khi PO/user cho phép | ⏭️ Bỏ qua 11-11-2026 | Chốt theo Assumption tạm: **không** chạy Import thật, Download Sample, Export, PDF cho tới khi PO/user cho phép. `REQ-CUS-21` · `93` · `94` · `95` · `119` · `141` giữ ⚪, TC viết trước và đánh `skip`. Câu hỏi email chào mừng khi Import để mở — chấp nhận rủi ro |
| AMB-CUS-13 | Toast file CSV rỗng ghi `Not enought rows for importing` (sai chính tả "enought") — sửa thành "enough"? (REQ-CUS-87) | TC assert nguyên văn sẽ đỏ khi Dev sửa | 🟢 | Lỗi chính tả; TC assert **chứa** `rows for importing` | ✅ Đã trả lời 11-11-2026 | Chốt theo Assumption tạm: lỗi chính tả → ứng viên bug nhẹ; `REQ-CUS-87` giữ assert **chứa** `rows for importing` |
| AMB-CUS-14 | Simulate hiển thị `0` ở cột Country khi CSV để trống (REQ-CUS-91) — Import thật có gán country `0`? | Dữ liệu quốc gia rác | 🟢 | Chỉ là cách hiển thị | ✅ Đã trả lời 11-11-2026 | Chốt theo Assumption tạm: chỉ là cách hiển thị — `REQ-CUS-91` không đổi |
| AMB-CUS-15 | Note trống vẫn lưu và báo thành công (REQ-CUS-99) — lỗi? | Rác dữ liệu, badge đếm Notes sai nghĩa | 🔴 | **Lỗi** — phải chặn note trống | ✅ Đã trả lời 11-11-2026 | Chốt theo Assumption tạm: **là lỗi** → `REQ-CUS-99` 🟡: note trống bị chặn, báo `This field is required.`. Hệ thống hiện chưa đạt → ứng viên bug |
| AMB-CUS-16 | Send File để trống: modal đóng, không gửi request, không thông báo gì (REQ-CUS-110) — đúng thiết kế? | Người dùng tưởng đã gửi | 🟡 | Lỗi nhẹ — nên báo `This field is required.` cho Email Address | ✅ Đã trả lời 11-11-2026 | Chốt theo Assumption tạm: lỗi nhẹ → `REQ-CUS-110` 🟡: modal giữ nguyên, Email Address báo `This field is required.`. Hệ thống hiện chưa đạt → ứng viên bug |
| AMB-CUS-17 | Statement nhận khoảng ngày đảo ngược (From > To) mà không báo lỗi (REQ-CUS-117) — có validate? | Sao kê sai kỳ gửi tới khách | 🟡 | Nên chặn From > To | ✅ Đã trả lời 11-11-2026 | Chốt theo Assumption tạm: chặn From > To → `REQ-CUS-117` 🟡. Hệ thống hiện chưa đạt → ứng viên bug. Nguyên văn thông báo: `AMB-CUS-28` |
| AMB-CUS-18 | Gửi sao kê khi không chọn người nhận: form vẫn submit, server báo `Problem while sending the statement` (REQ-CUS-121) — nên chặn ở client? | Thông báo lỗi chung chung, người dùng không biết sai ở đâu | 🟡 | Nên validate `Email to` bắt buộc | ✅ Đã trả lời 11-11-2026 | Chốt theo Assumption tạm: `Email to` bắt buộc ở client → `REQ-CUS-121` 🟡 (`This field is required.`). Hệ thống hiện chưa đạt → ứng viên bug |
| AMB-CUS-19 | Port của Vault nhận `70000` (> 65535) (REQ-CUS-126) — có giới hạn 1–65535? | Lưu thông tin máy chủ sai | 🟡 | Nên giới hạn 1–65535 | ✅ Đã trả lời 11-11-2026 | Chốt theo Assumption tạm: giới hạn 1–65535 → `REQ-CUS-126` 🟡. Hệ thống hiện chưa đạt → ứng viên bug. Nguyên văn thông báo: `AMB-CUS-28` |
| AMB-CUS-20 | Map lưu được toạ độ `abc` / `999` (REQ-CUS-137) — có validate vĩ độ [-90, 90], kinh độ [-180, 180]? | Bản đồ KH sai / lỗi khi bật Google Maps | 🔴 | **Lỗi** — phải validate số và khoảng | ✅ Đã trả lời 11-11-2026 | Chốt theo Assumption tạm: **là lỗi** → `REQ-CUS-137` 🟡: validate số, vĩ độ [-90, 90], kinh độ [-180, 180]. Hệ thống hiện chưa đạt → ứng viên bug. Nguyên văn thông báo: `AMB-CUS-28` |
| AMB-CUS-21 | Bộ lọc đã lưu do người khác tạo hiện trong menu lọc của tôi — bộ lọc lưu dùng chung hay riêng? Nhiều rule kết hợp `match_type=or` (OR) hay có tuỳ chọn AND? (REQ-CUS-22 · 25 · 27) | TC lọc phụ thuộc dữ liệu người khác; kết quả lọc nhiều rule sai kỳ vọng | 🟡 | Dùng chung toàn hệ thống; nhiều rule là OR | ✅ Đã trả lời 11-11-2026 | Chốt theo Assumption tạm: bộ lọc lưu **dùng chung toàn hệ thống**; nhiều rule kết hợp **OR** — `REQ-CUS-22` · `25` · `27` không đổi hành vi |
| AMB-CUS-22 | Xem mật khẩu Vault đòi nhập mật khẩu đăng nhập — agent không nhập mật khẩu thật. User tự kiểm hay chấp nhận ⚪ (REQ-CUS-133)? | Chức năng nhạy cảm không có TC chạy được | 🔴 | User tự chạy TC này thủ công | ✅ Đã trả lời 11-11-2026 | Chốt theo Assumption tạm: **user tự chạy** TC của `REQ-CUS-133` thủ công; REQ giữ ⚪ ở phía agent |
| AMB-CUS-23 | Kiểm 3 mức visibility của Vault cần 2 staff đăng nhập song song (tài khoản PM có trong `.env` nhưng agent không tự đăng nhập). User có hỗ trợ đăng nhập tài khoản thứ hai không (REQ-CUS-134)? | Lộ thông tin bí mật cho staff không được phép | 🔴 | Chưa kiểm; giữ ⚪ | ⏭️ Bỏ qua 11-11-2026 | Chốt theo Assumption tạm: chưa kiểm, `REQ-CUS-134` giữ ⚪, TC viết trước và đánh `skip`. Chấp nhận rủi ro visibility Vault chưa được kiểm |
| AMB-CUS-24 | Ô `Username` của Vault bị Chrome tự điền email đăng nhập dù form có trường mồi chống autofill (trường mồi chỉ chặn được mật khẩu) — cần `autocomplete="off"` / `new-password`? (REQ-CUS-124) | Lưu nhầm email staff làm username của KH | 🟡 | Lỗi nhẹ; TC luôn xoá trắng ô trước khi nhập | ✅ Đã trả lời 11-11-2026 | Chốt theo Assumption tạm: lỗi nhẹ → ứng viên bug; TC luôn xoá trắng ô Username trước khi nhập (`REQ-CUS-124`). Không sinh REQ — hành vi tự điền phụ thuộc trình duyệt |
| AMB-CUS-25 | Được phép gửi email (sao kê, gửi tệp) trên demo không? Cần KH test có contact nhận email thử (REQ-CUS-111 · 122) | Không kiểm được tác tạo email | 🟡 | Chỉ kiểm giao diện, không gửi | ⏭️ Bỏ qua 11-11-2026 | Chốt theo Assumption tạm: **không** gửi email trên demo, chỉ kiểm giao diện. `REQ-CUS-111` · `122` giữ ⚪, TC đánh `skip` |
| AMB-CUS-26 | Tooltip Currency nói không đổi được tiền tệ sau khi KH có giao dịch (REQ-CUS-45) — chặn ở UI (disable) hay server báo lỗi? | TC không biết kỳ vọng gì | 🟢 | Ô Currency bị khoá khi KH có giao dịch | ✅ Đã trả lời 11-11-2026 | Chốt theo Assumption tạm: ô Currency **bị khoá** khi KH có giao dịch — ghi vào AC `REQ-CUS-45` (vẫn ⚪) |
| AMB-CUS-27 | Customer Admin được gán có tác dụng gì: KH hiện ở rule lọc `Customers assigned to me` / `Responsible admin`? Ảnh hưởng quyền xem của staff khác? (REQ-CUS-65) | Tính năng không có TC chứng minh giá trị | 🟡 | KH hiện trong rule `Customers assigned to me` của staff được gán | ✅ Đã trả lời 11-11-2026 | Chốt theo Assumption tạm: KH hiện trong rule `Customers assigned to me` của staff được gán — `REQ-CUS-65` là hành vi mong đợi, vẫn ⚪ |
| AMB-CUS-28 | Hình thức chặn và nguyên văn thông báo cho 4 REQ đã đổi kỳ vọng theo AMB chốt giả định tạm nhưng chưa có câu chữ: `REQ-CUS-82` (Bulk Confirm không chọn nhóm — **chặn** hay **cảnh báo riêng**?) · `117` (Period From > To) · `126` (Port ngoài 1–65535) · `137` (toạ độ không phải số / ngoài khoảng) | TC không assert được nguyên văn; Dev tự đặt câu chữ, không nhất quán giữa các form | 🟡 | REQ-82 chọn **chặn** (nhóm giữ nguyên). Cả 4 REQ: TC assert hành vi chặn (không gỡ / không nạp / không lưu) + **có** thông báo lỗi hiển thị, **không** assert nguyên văn cho tới khi chốt (nhãn `assumption-based`) | ❓ Chờ trả lời | — |

### 6.2. Risks

| Mã | Rủi ro | Mô tả | Mitigation |
|---|---|---|---|
| RISK-CUS-01 | 🔴 Dữ liệu nền biến động | Môi trường dùng chung (`RISK-SYS-01`): 11-11-2026 có 2.942 KH, 317 nhóm; người khác tạo/xoá liên tục → bộ đếm Summary và số option dropdown đổi giữa hai lần chạy | Assert **chênh lệch** bộ đếm trước/sau thao tác của chính test, không assert số tuyệt đối; chọn option theo tên duy nhất do test tạo |
| RISK-CUS-02 | 🔴 Thao tác hàng loạt trên môi trường dùng chung | Checkbox chọn tất cả + Mass Delete / Bulk Groups tác động mọi dòng đang hiển thị — chạy khi danh sách chưa lọc là xoá/ghi đè dữ liệu người khác | **Luôn** lọc danh sách về đúng bản ghi do test tạo (tên traceable) và đếm số dòng đã chọn trước khi Confirm |
| RISK-CUS-03 | 🟡 Master data nhóm KH bị ô nhiễm | Danh sách nhóm có nhiều tên trùng (VD 12 lần `ctp group`) — chọn theo nhãn dễ chọn nhầm; nhóm test tạo ra không xoá được vì Setup bị chặn (`AMB-SYS-01`) | Chọn nhóm theo `value`; hạn chế tạo nhóm mới trong test |
| RISK-CUS-04 | 🟡 Đường dẫn tuyệt đối của máy chủ lộ ra ở client | Tab Files: thuộc tính `data-path` và input ẩn `file_path` của form Send File chứa đường dẫn tuyệt đối trên server; form gửi đường dẫn này ngược lên server | Báo Dev/bảo mật đánh giá (lộ cấu trúc thư mục, nguy cơ sửa `file_path` để gửi tệp tuỳ ý). QA **không** tự thử khai thác |
| RISK-CUS-05 | 🟡 Toast tự tắt nhanh | Toast (`Customer deleted`, `Not enought rows for importing`…) biến mất sau vài giây — chụp/assert chậm là mất | Automation assert toast ngay sau hành động bằng smart wait; manual chụp ngay |
| RISK-CUS-06 | 🟡 Hộp thoại xác nhận gốc của trình duyệt | Delete KH, xoá note/tệp/vault/admin, Bulk Confirm dùng `confirm()` gốc — automation không xử lý dialog sẽ treo | Đăng ký xử lý dialog trước khi bấm; assert nội dung message do ứng dụng truyền vào |
| RISK-CUS-07 | 🟡 Tên KH được phép trùng | REQ-CUS-36: tìm theo tên có thể trả nhiều dòng | Tên KH test luôn chứa timestamp; định vị bản ghi theo id |
| RISK-CUS-08 | 🟡 Ảnh hưởng chéo module | Tắt Active làm KH biến mất khỏi ô chọn KH của Invoice (REQ-CUS-69) và mọi form tạo chứng từ khác (REQ-CUS-139, chốt `AMB-CUS-06`) | TC của module chứng từ dùng KH Active; bật lại Active trong teardown |
| RISK-CUS-09 | 🟡 Locator theo text | Giao diện đổi được 25 ngôn ngữ theo người dùng (`RISK-SYS-03`) | Ưu tiên locator theo `id` / `name` / `href` (`#company`, `#mass_delete`, `a[href*="group=notes"]`) |

### 6.3. Ứng viên bug từ kết luận AMB (11-11-2026)

> Hệ thống **hiện chưa đạt** hành vi mong đợi đã chốt. Bug report chính thức sinh bằng `/create-bug-report` **sau khi** TC tương ứng chạy FAIL (bug phải trỏ về TC ID + evidence trong `executions/`) — module chưa có bộ TC nên chưa mở được file `BUG_*`.

| # | REQ | AMB | Hiện trạng (lệch) | Mong đợi | Ghi chú |
|---|---|---|---|---|---|
| 1 | `REQ-CUS-31` 🟡 | `AMB-CUS-01` | Company chỉ khoảng trắng tạo được KH tên rỗng | Chặn như bỏ trống, `This field is required.` | **User yêu cầu mở bug** |
| 2 | `REQ-CUS-32` 🟡 | `AMB-CUS-02` | Company > 191 ký tự bị cắt âm thầm | Chặn, `Please enter no more than 191 characters.` | User chốt giới hạn + yêu cầu UI chặn |
| 3 | `REQ-CUS-82` 🟡 | `AMB-CUS-10` | Bulk Confirm không chọn nhóm gỡ sạch nhóm, không cảnh báo | Chặn hoặc cảnh báo riêng | Nguyên văn: `AMB-CUS-28` |
| 4 | `REQ-CUS-99` 🟡 | `AMB-CUS-15` | Note trống lưu được, báo thành công | Chặn, `This field is required.` | |
| 5 | `REQ-CUS-110` 🟡 | `AMB-CUS-16` | Send File để trống đóng modal, không báo gì | Modal giữ nguyên, Email Address `This field is required.` | Lỗi nhẹ |
| 6 | `REQ-CUS-117` 🟡 | `AMB-CUS-17` | Sao kê nhận khoảng From > To | Chặn kèm thông báo | Nguyên văn: `AMB-CUS-28` |
| 7 | `REQ-CUS-121` 🟡 | `AMB-CUS-18` | Gửi sao kê không người nhận → lỗi chung từ server | Chặn ở client, `This field is required.` | |
| 8 | `REQ-CUS-126` 🟡 | `AMB-CUS-19` | Port nhận `70000` | Chỉ nhận 1–65535 | Nguyên văn: `AMB-CUS-28` |
| 9 | `REQ-CUS-137` 🟡 | `AMB-CUS-20` | Map lưu toạ độ `abc` / `999` | Số, vĩ độ [-90, 90], kinh độ [-180, 180] | Nguyên văn: `AMB-CUS-28` |
| 10 | `REQ-CUS-87` 🟢 | `AMB-CUS-13` | Toast `Not enought rows for importing` sai chính tả | `enough` | Lỗi nhẹ; TC assert phần chứa nên không FAIL |
| 11 | `REQ-CUS-124` 🟢 | `AMB-CUS-24` | Chrome tự điền email đăng nhập vào ô Username của Vault | Ô Username không bị tự điền | Lỗi nhẹ; không có TC FAIL — mở bug thủ công nếu cần |

## 7. Phân rã Epic / Story

| Story ID | Tên Story | REQ bao phủ | Số REQ | AMB / RISK liên quan | Ghi chú phạm vi |
|---|---|---|---|---|---|
| STORY-CUS-01 | Danh sách, Summary, tìm kiếm, bộ lọc, Export | `REQ-CUS-01` → `27` | 27 | AMB-CUS-21 | Trang `/admin/clients` |
| STORY-CUS-02 | Tạo khách hàng (form 2 tab + modal nhóm) | `REQ-CUS-28` → `54` | 27 | AMB-CUS-01 · 02 · 03 · 04 · 05 · 26 · RISK-CUS-03 · 07 | `/admin/clients/client` |
| STORY-CUS-03 | Hồ sơ KH, sửa, Customer Admins | `REQ-CUS-55` → `65` | 11 | AMB-CUS-27 | Tab Profile |
| STORY-CUS-04 | Active, xoá KH, Bulk Actions | `REQ-CUS-66` → `82` · `139` · `140` | 19 | AMB-CUS-06 · 07 · 08 · 09 · 10 · RISK-CUS-02 · 06 · 08 | Danh sách + hồ sơ; REQ-139 · 140 kiểm ở form chứng từ |
| STORY-CUS-05 | Import Customers | `REQ-CUS-83` → `95` · `141` | 14 | AMB-CUS-11 · 13 · 14 · RISK-CUS-05 | `/admin/clients/import` |
| STORY-CUS-06 | Notes & Files | `REQ-CUS-96` → `112` | 17 | AMB-CUS-15 · 16 · RISK-CUS-04 | Tab Notes · Files |
| STORY-CUS-07 | Statement (sao kê) | `REQ-CUS-113` → `122` | 10 | AMB-CUS-17 · 18 | Tab Statement |
| STORY-CUS-08 | Vault & Map | `REQ-CUS-123` → `138` | 16 | AMB-CUS-19 · 20 · 22 · 23 · 24 | Tab Vault · Map |

**Tổng: 8 Story / 141 REQ** — `27 + 27 + 11 + 19 + 14 + 17 + 10 + 16 = 141 ✔` — mọi REQ thuộc đúng một Story, không mồ côi, không trùng.

**Đối chiếu AMB/RISK:**

| Nhóm | Mã | Nằm ở đâu |
|---|---|---|
| AMB thuộc Story | AMB-CUS-01 → 11 · 13 → 24 · 26 · 27 | phân bổ ở bảng Story |
| AMB cấp Epic | AMB-CUS-12 · AMB-CUS-25 · AMB-CUS-28 | 12 · 25: quyền tạo tác tạo (file Export/Import/Sample, email) trên môi trường dùng chung — cắt ngang STORY-01 · 05 · 06 · 07 · 28: nguyên văn thông báo chặn — cắt ngang STORY-04 · 07 · 08 |
| RISK thuộc Story | RISK-CUS-02 → 08 | phân bổ ở bảng Story |
| RISK cấp Epic | RISK-CUS-01 · RISK-CUS-09 | rủi ro mức module (dữ liệu nền biến động, locator theo text) |

Kiểm đếm: AMB 25 thuộc Story + 3 cấp Epic = 28 ✔ · RISK 7 thuộc Story + 2 cấp Epic = 9 ✔.

**Hạng mục cấp Epic (không gán Story):** Ma trận phân quyền (mục 4) · Ma trận trạng thái (mục 5) · NFR (file web) · AMB-CUS-12 · AMB-CUS-25 · AMB-CUS-28 · RISK-CUS-01 · RISK-CUS-09 — đều cắt ngang nhiều Story.

**Thứ tự triển khai đề xuất:**
1. STORY-CUS-02 (tạo KH) → STORY-CUS-03 (sửa) — mọi Story khác cần KH test do chính test tạo
2. STORY-CUS-01 (danh sách) → STORY-CUS-04 (Active, xoá, bulk — dùng để dọn dữ liệu test)
3. STORY-CUS-06 · 07 · 08 (các tab hồ sơ, độc lập nhau)
4. STORY-CUS-05 (Import) — phần Simulate làm được; Import thật (REQ-93 · 94 · 95 · 141) **BLOCKED** — chưa được phép chạy (`AMB-CUS-12` ⏭️), TC viết trước đánh `skip`
5. STORY-CUS-08: REQ-133 (xem mật khẩu) do **user chạy thủ công** (`AMB-CUS-22` ✅) · REQ-134 (visibility) đánh `skip` (`AMB-CUS-23` ⏭️)
6. 9 REQ 🟡 (mục 6.3) viết TC theo **kỳ vọng mới** — dự kiến FAIL tới khi Dev sửa; FAIL → `/create-bug-report`. REQ-82 · 117 · 126 · 137 assert theo Assumption tạm của `AMB-CUS-28`

## 8. Nhật ký thay đổi

| Ngày | Nguồn | REQ ảnh hưởng | Loại | Tóm tắt thay đổi | TC cần xử lý |
|---|---|---|---|---|---|
| 11-11-2026 | User chốt AMB đợt 1 (các AMB còn lại theo Assumption tạm) | REQ-CUS-21 · 22 · 27 · 36 → 39 · 45 · 65 · 69 · 73 · 79 · 81 · 87 · 90 · 91 · 93 · 95 · 111 · 119 · 122 · 124 · 133 · 134 | ✏️ Biên tập | `AMB-CUS-03` → `09` · `11` · `13` · `14` · `21` · `22` · `24` · `26` · `27` ✅ · `AMB-CUS-12` · `23` · `25` ⏭️. Không đổi hành vi: thay "chờ AMB" bằng kết luận; REQ-45 ghi cách chặn (ô Currency bị khoá) · REQ-73 bỏ chữ "giả định" (chặn, không xoá dây chuyền) · REQ-133 do user chạy thủ công. Mở `AMB-CUS-28` 🟡 (nguyên văn thông báo của REQ-82 · 117 · 126 · 137). Thêm mục 6.3 Ứng viên bug. Ma trận trạng thái: dòng xoá KH có chứng từ → *Bị chặn* | — (chưa có bộ TC `CUS`). Khi viết TC: REQ-21 · 93 · 95 · 119 · 111 · 122 · 134 đánh `skip`; REQ-133 giao user chạy thủ công |
| 11-11-2026 | User chốt AMB đợt 1 (`AMB-CUS-06` · `11` theo Assumption tạm) | REQ-CUS-139 → 141 | 🟢 Thêm | 139 KH Inactive không có trong ô chọn KH khi tạo mới các chứng từ khác · 140 chứng từ đã lập giữ nguyên KH khi KH chuyển Inactive · 141 Import thật bỏ qua dòng lỗi và báo số dòng bị bỏ. Cả ba ⚪ — chưa kiểm chứng UI (141 còn chờ được phép chạy Import thật). STORY-04 thêm 139 · 140, STORY-05 thêm 141 | Viết mới — 141 đánh `skip` |
| 11-11-2026 | User chốt AMB đợt 1 (`AMB-CUS-10` · `15` → `20` theo Assumption tạm) | REQ-CUS-82 · 99 · 110 · 117 · 121 · 126 · 137 | 🟡 Sửa | Đổi sang hành vi mong đợi, hiện trạng ghi lại làm "lệch": 82 Bulk không chọn nhóm không được gỡ nhóm âm thầm · 99 note trống bị chặn · 110 Send File trống báo Email Address bắt buộc · 117 Period From > To bị chặn · 121 sao kê bắt buộc chọn người nhận · 126 Port 1–65535 · 137 toạ độ phải là số trong khoảng hợp lệ. Hệ thống **chưa đạt** → ứng viên bug (mục 6.3) | — (chưa có bộ TC `CUS`). Viết TC theo kỳ vọng mới, **dự kiến FAIL** tới khi Dev sửa; 82 · 117 · 126 · 137 gắn nhãn `assumption-based` (`AMB-CUS-28`) |
| 11-11-2026 | User trả lời `AMB-CUS-01` · `02` | REQ-CUS-31 · 32 | 🟡 Sửa | 31: Company chỉ khoảng trắng phải bị chặn như bỏ trống (`This field is required.`) — user xác nhận **là lỗi, cần mở bug**. 32: giới hạn 191 ký tự, UI phải chặn — câu thông báo `Please enter no more than 191 characters.` do QA đề xuất theo uỷ quyền của user. Cả hai: hệ thống **chưa đạt** | — (chưa có bộ TC `CUS`). Viết TC theo kỳ vọng mới, **dự kiến FAIL** tới khi Dev sửa → `/create-bug-report` |
| 11-11-2026 | UI recon (khảo sát lại) | `REQ-CUS-01` → `138` | 🟢 Thêm | Khởi tạo lại tài liệu từ khảo sát UI thực tế — user chọn làm lại module Customers và đánh mã lại từ `REQ-CUS-01` (bản 1.0 ngày 04-11-2026 đã bị gỡ khỏi thư mục làm việc, còn trong lịch sử git). 138 REQ (122 🟢 · 16 ⚪), 27 AMB, 9 RISK, 8 Story | ⚠️ TC/automation theo bản 1.0 (nếu có) phải map lại — cùng mã khác nghĩa |
