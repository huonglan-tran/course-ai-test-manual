# Requirements — Customers (`CUS`)

> **INDEX module — tên file bất biến.** Phần dùng chung mọi nền tảng + Bản đồ tài liệu. REQ chi tiết nằm ở file nền tảng và các file Story (xem [Bản đồ tài liệu](#bản-đồ-tài-liệu)).
>
> Danh mục hệ thống: [`../README.md`](../README.md) · Bản đồ khám phá: [`../_discovery/modules/module_02_khach_hang.md`](../_discovery/modules/module_02_khach_hang.md)

## 1. Metadata

| Thuộc tính | Giá trị |
|---|---|
| **Hệ thống** | Perfex CRM — khu Admin |
| **Module** | Customers (route `clients`) — danh sách, tạo/sửa/xoá KH, Active, Bulk Actions, Import, hồ sơ KH và các tab thuộc `CUS`: Profile · Notes · Statement · Files · Vault · Map |
| **Prefix** | `CUS` — cấp bởi `/discover-system` 12-10-2026 |
| **Tiền tố TC ID** | `CRM_CUS_TC_<3 số>` |
| **Nền tảng** | Web ✅ (khảo sát 04-11-2026) · Mobile — chưa có · API — chưa có |
| **Phiên bản tài liệu** | 1.0 — 04-11-2026: khởi tạo từ khảo sát UI thực tế |
| **Dải mã đã dùng** | `REQ-CUS-01` → `REQ-CUS-101` (đợt 1 UI recon 04-11-2026) · `AMB-CUS-01` → `AMB-CUS-19` · `RISK-CUS-01` → `RISK-CUS-09` · `STORY-CUS-01` → `STORY-CUS-08` |
| **Mã kế tiếp** | Đợt phân tích sau bắt đầu từ `REQ-CUS-102` · `AMB-CUS-20` · `RISK-CUS-10` · `STORY-CUS-09` — **KHÔNG đánh lại từ 01** |
| **Trạng thái REQ** | 🟢 84 · 🟡 0 · 🔴 0 · ⚪ 17 (chưa kiểm chứng được — không được phép / không thể thao tác trên môi trường dùng chung, hoặc cần tài khoản/mật khẩu agent không nhập) — tổng 101 |

## 2. Tổng quan

Module `CUS` quản lý **khách hàng (công ty)** — thực thể gốc mà gần như mọi chứng từ Sales, dự án, hợp đồng, ticket đều gắn vào. Staff xem danh sách có bộ đếm tổng hợp, tìm kiếm và bộ lọc nâng cao; tạo/sửa KH với thông tin công ty, nhóm, tiền tệ, ngôn ngữ, địa chỉ chính/billing/shipping; bật/tắt trạng thái Active; xoá đơn lẻ hoặc hàng loạt; import từ CSV; và quản lý hồ sơ KH qua 19 tab.

| Trong phạm vi | Ngoài phạm vi |
|---|---|
| Danh sách `/admin/clients` · Summary · tìm kiếm · bộ lọc · Export | Tab **Contacts** và danh sách `/admin/clients/all_contacts` → module `CONT` |
| Form tạo/sửa KH (2 tab + Customer Admins) | Danh sách chứng từ trong 11 tab Invoices…Tickets → module chứng từ; `CUS` chỉ giữ REQ **lọc theo KH** (`RISK-SYS-04`) |
| Active · Bulk Actions · xoá KH | Tab **Reminders** → module `PROF` |
| Import Customers (giao diện + Simulate) | Cổng Customer/Contact (URL khác) |
| Tab Profile · Notes · Statement · Files · Vault · Map | Quản lý Customer Groups, Currency, ngôn ngữ, Google API key, đuôi tệp cho phép — nằm ở Setup (bị chặn, `AMB-SYS-01`) |

### 2.1. Bản đồ phủ tài liệu

Không có tài liệu — toàn bộ REQ sinh từ khảo sát UI thực tế (user xác nhận 04-11-2026).

## Bản đồ tài liệu

| Nền tảng | File | Story | REQ bao phủ |
|---|---|---|---|
| Chung ≥ 2 nền tảng | chính file này — mục 3 | — | *(chưa có — module mới có 1 nền tảng)* |
| Web — metadata khảo sát · NFR · Evidence | [web/requirements_customers_web.md](web/requirements_customers_web.md) | — | *(không chứa dòng REQ)* |
| Web | [web/stories/story_01_danh_sach_bo_loc.md](web/stories/story_01_danh_sach_bo_loc.md) | STORY-CUS-01 | `REQ-CUS-01` → `16` |
| Web | [web/stories/story_02_tao_khach_hang.md](web/stories/story_02_tao_khach_hang.md) | STORY-CUS-02 | `REQ-CUS-17` → `37` |
| Web | [web/stories/story_03_ho_so_sua_customer_admins.md](web/stories/story_03_ho_so_sua_customer_admins.md) | STORY-CUS-03 | `REQ-CUS-38` → `52` |
| Web | [web/stories/story_04_active_bulk_xoa.md](web/stories/story_04_active_bulk_xoa.md) | STORY-CUS-04 | `REQ-CUS-53` → `61` |
| Web | [web/stories/story_05_import.md](web/stories/story_05_import.md) | STORY-CUS-05 | `REQ-CUS-62` → `68` |
| Web | [web/stories/story_06_notes_files.md](web/stories/story_06_notes_files.md) | STORY-CUS-06 | `REQ-CUS-69` → `79` |
| Web | [web/stories/story_07_statement.md](web/stories/story_07_statement.md) | STORY-CUS-07 | `REQ-CUS-80` → `87` |
| Web | [web/stories/story_08_vault_map.md](web/stories/story_08_vault_map.md) | STORY-CUS-08 | `REQ-CUS-88` → `101` |

## 3. Yêu cầu dùng chung nhiều nền tảng

*Chưa có.* Module hiện chỉ khảo sát Web — REQ chưa kiểm trên nền tảng thứ hai thì nằm ở file `web/` (skill mục 2.2).

## 4. Ma trận phân quyền

> Hệ thống **chỉ có role Admin** (user xác nhận 19-10-2026 — danh mục). Không có cột role khác. Cột Contact để ghi nhận đã cân nhắc loại tài khoản không phải staff.

| Hành động | Staff role Admin — tài khoản `.env` | Contact (khách hàng) |
|---|---|---|
| Xem danh sách KH · Summary · bộ lọc (REQ 01 → 15) | ✅ | — |
| Tạo KH (REQ 34 · 36) | ✅ | — |
| Sửa KH (REQ 42) | ✅ | — |
| Bật/tắt Active (REQ 53) | ✅ | — |
| Xoá KH · Mass Delete (REQ 58 · 60) | ✅ | — |
| Import — Simulate (REQ 66) | ✅ | — |
| Gán/gỡ Customer Admin (REQ 49 · 50) | ✅ | — |
| Xem mật khẩu Vault (REQ 94) | ❔ | — |
| Xem vault entry `Visible only to me` do staff khác tạo (REQ 95) | ❔ | — |

```
Tổng 18 ô = Đã kiểm chứng 7 · Suy diễn 0 · Chưa rõ 2 · Không áp dụng 9
9 ô "không áp dụng": cột [Contact] — contact không đăng nhập được khu Admin (REQ-LOGIN-44, ⚠️ theo xác nhận user).
2 ô "chưa rõ": [Xem mật khẩu Vault] cần nhập mật khẩu đăng nhập — agent không nhập (AMB-CUS-15) ·
  [Vault visibility] cần đăng nhập đồng thời 2 staff (AMB-CUS-18).
```

## 5. Ma trận trạng thái KH

> Entity KH chỉ có cờ **Active** (bật/tắt), không có status flow nhiều bước.

| Trạng thái hiện tại | Hành động | Trạng thái kế tiếp | Ai thực hiện | REQ |
|---|---|---|---|---|
| *(chưa tồn tại)* | Save / Save and create contact | Active | Staff role Admin | 34 · 35 · 36 |
| Active | Tắt switch Active ở danh sách | Inactive (banner cảnh báo trên hồ sơ) | Staff role Admin | 53 · 45 |
| Inactive | Bật switch Active ở danh sách | Active | Staff role Admin | 53 — chiều bật **chưa** thử (chỉ quan sát URL `.../<id>/0`) |
| Inactive | `Mark as active` trên banner hồ sơ | Active | Staff role Admin | 46 ⚪ |
| Active / Inactive | Delete · Mass Delete | Đã xoá (hồ sơ trả 404) | Staff role Admin | 58 · 60 · 47 |
| Active / Inactive (có chứng từ) | Delete | ❔ chặn hay xoá dây chuyền | Staff role Admin | 61 ⚪ · `AMB-CUS-07` |

## 6. Điểm mơ hồ & Rủi ro

### 6.1. Ambiguities

| Mã | Câu hỏi | Nguy cơ | Mức độ | Assumption tạm | Trạng thái | Kết luận |
|---|---|---|---|---|---|---|
| AMB-CUS-01 | Company chỉ gồm khoảng trắng vẫn tạo được KH với tên rỗng (REQ-CUS-20) — lỗi hay chấp nhận? | KH không tên xuất hiện ở danh sách và mọi dropdown chứng từ, không tìm được | 🔴 | **Lỗi** — phải chặn như bỏ trống, báo `This field is required.` | ❓ Chờ trả lời | — |
| AMB-CUS-02 | Company > 191 ký tự bị cắt âm thầm (REQ-CUS-21). Giới hạn đúng là bao nhiêu? UI có nên đặt `maxlength` / báo lỗi? | Mất dữ liệu không ai biết | 🔴 | Giới hạn 191; UI nên chặn và báo | ❓ Chờ trả lời | — |
| AMB-CUS-03 | Cảnh báo trùng tên có thật sự cho phép lưu trùng không (REQ-CUS-26)? | Tạo trùng KH → công nợ tách đôi | 🟡 | Không chặn — đúng câu chữ cảnh báo | ❓ Chờ trả lời | — |
| AMB-CUS-04 | Checkbox `Show primary contact full name on Invoices, Estimates, Payments, Credit Notes` luôn ẩn ở KH chưa có contact — hiện khi nào? | Bỏ sót TC cho tuỳ chọn hiển thị trên chứng từ | 🟡 | Chỉ hiện khi KH có primary contact — kiểm lại khi recon `CONT` | ❓ Chờ trả lời | — |
| AMB-CUS-05 | KH Inactive có bị ẩn khỏi **mọi** dropdown chọn KH (Invoice, Estimate, Project…) như tooltip nói không (REQ-CUS-55)? Áp cho chứng từ đang sửa không? | Lập chứng từ cho KH ngừng hoạt động | 🟡 | Ẩn khi tạo mới; vẫn hiện ở chứng từ đã có | ❓ Chờ trả lời | — |
| AMB-CUS-06 | Bấm `Confirm` ở Bulk Actions khi **chưa chọn KH nào** vẫn hỏi xác nhận — sau đó hệ thống làm gì? Có nên báo "chưa chọn"? | Người dùng tưởng đã xoá/chuyển nhóm | 🟡 | Không làm gì; nên có cảnh báo | ❓ Chờ trả lời | — |
| AMB-CUS-07 | Xoá KH đang có hoá đơn / dự án / hợp đồng: chặn, hay xoá dây chuyền chứng từ (REQ-CUS-61)? | Mất dữ liệu kế toán | 🔴 | Hệ thống chặn kèm thông báo | ❓ Chờ trả lời | — |
| AMB-CUS-08 | Bulk Actions → Groups: **thêm** vào nhóm hay **ghi đè** nhóm cũ (REQ-CUS-59)? | Mất phân nhóm cũ hàng loạt | 🟡 | Ghi đè | ❓ Chờ trả lời | — |
| AMB-CUS-09 | Simulate Import không đánh dấu dòng thiếu cột bắt buộc (Email) — Import thật bỏ qua dòng đó, báo lỗi, hay tạo KH không contact (REQ-CUS-67)? | Import "thành công" nhưng thiếu dữ liệu | 🔴 | Import bỏ qua dòng thiếu cột bắt buộc và báo số dòng bị bỏ | ❓ Chờ trả lời | — |
| AMB-CUS-10 | Có được chạy Import thật trên môi trường demo không? Import có gửi email chào mừng tới contact không (REQ-CUS-68)? | Gửi email tới địa chỉ thật | 🟡 | Không chạy cho tới khi PO cho phép | ❓ Chờ trả lời | — |
| AMB-CUS-11 | Note trống vẫn được lưu và báo thành công (REQ-CUS-71) — lỗi? | Rác dữ liệu, bộ đếm Notes sai nghĩa | 🔴 | **Lỗi** — phải chặn note trống | ❓ Chờ trả lời | — |
| AMB-CUS-12 | Map lưu được toạ độ `abc` / `999` (REQ-CUS-99) — có phải validate vĩ độ [-90, 90], kinh độ [-180, 180]? | Bản đồ KH sai / lỗi khi bật Google Maps | 🔴 | **Lỗi** — phải validate số và khoảng | ❓ Chờ trả lời | — |
| AMB-CUS-13 | Bộ lọc đã lưu do người khác tạo hiển thị trong menu lọc của tôi — bộ lọc lưu dùng chung hay riêng? `Save Filter` làm gì (REQ-CUS-12 · 16)? | TC lọc phụ thuộc dữ liệu người khác | 🟡 | Dùng chung toàn hệ thống | ❓ Chờ trả lời | — |
| AMB-CUS-14 | Được phép mở PDF / gửi email sao kê trên demo không? Cần KH test có contact nhận email thử (REQ-CUS-85 · 87) | Không kiểm được tác tạo PDF/email | 🟡 | Chỉ kiểm liên kết, không gửi email | ❓ Chờ trả lời | — |
| AMB-CUS-15 | Xem mật khẩu Vault đòi nhập mật khẩu đăng nhập — agent không nhập mật khẩu thật. User tự kiểm hay chấp nhận ⚪ (REQ-CUS-94)? | Chức năng nhạy cảm không có TC chạy được | 🔴 | User tự chạy TC này thủ công | ❓ Chờ trả lời | — |
| AMB-CUS-16 | Ô `Username` của Vault bị Chrome tự điền email đăng nhập dù form có trường mồi chống autofill — lỗi? | Lưu nhầm email staff làm username của KH | 🟡 | Lỗi nhẹ; TC luôn xoá trắng ô trước khi nhập | ❓ Chờ trả lời | — |
| AMB-CUS-17 | VAT Number · Phone · Website không có ràng buộc định dạng nào — cố ý? | Dữ liệu liên lạc sai định dạng | 🟡 | Cố ý — chấp nhận mọi chuỗi | ❓ Chờ trả lời | — |
| AMB-CUS-18 | Kiểm 3 mức visibility của Vault cần 2 staff đăng nhập song song (tài khoản PM có trong `.env` nhưng agent không tự đăng nhập). User có hỗ trợ đăng nhập tài khoản thứ hai không (REQ-CUS-95)? | Lộ thông tin bí mật cho staff không được phép | 🔴 | Chưa kiểm; giữ ⚪ | ❓ Chờ trả lời | — |
| AMB-CUS-19 | Simulate Import hiển thị `0` ở cột Country khi CSV để trống Country — Import thật có gán country `0`? | Dữ liệu quốc gia rác | 🟢 | Chỉ là cách hiển thị | ❓ Chờ trả lời | — |

### 6.2. Risks

| Mã | Rủi ro | Mô tả | Mitigation |
|---|---|---|---|
| RISK-CUS-01 | Bộ đếm và danh sách biến động | Môi trường dùng chung (`RISK-SYS-01`): số KH thay đổi liên tục bởi người khác | Assert **chênh lệch** trước/sau trong cùng phiên, hoặc theo bản ghi tự tạo; không assert số tuyệt đối |
| RISK-CUS-02 | Dữ liệu cá nhân KH | Danh sách chứa tên, email, điện thoại KH thật | Evidence/TC chỉ thao tác và chụp bản ghi tự tạo `auto_cus_<ts>`; lọc trước khi chụp |
| RISK-CUS-03 | Đổi trạng thái qua `GET` mang CSRF token trên URL | `change_client_status` · `toggle_file_visibility` dùng `GET` + `csrf_token_name` ở query string — token lộ vào log/lịch sử, dễ bị CSRF qua link | Đề nghị Dev/Security review; TC không chép giá trị token |
| RISK-CUS-04 | Lộ đường dẫn máy chủ | `data-path` của tệp trong tab Files chứa đường dẫn tuyệt đối trên máy chủ | Báo Dev; không chép giá trị vào tài liệu |
| RISK-CUS-05 | Mật khẩu mặc định hiển thị rõ | Ô `Default password for all contacts` ở Import là `type=text` | Báo Dev; TC dùng mật khẩu test sinh động |
| RISK-CUS-06 | Xoá hàng loạt trên môi trường chung | Mass Delete xoá được mọi KH đang tick | TC bắt buộc lọc chỉ còn bản ghi tự tạo và kiểm `value` checkbox trước khi xoá; không dùng `Chọn tất cả` |
| RISK-CUS-07 | Phụ thuộc chéo qua 19 tab | Hồ sơ KH chứa dữ liệu 13 module khác — đổi ở `CUS` ảnh hưởng nhiều module | Regression `CUS` kèm smoke các tab chứng từ |
| RISK-CUS-08 | Master data Groups trùng tên | 317 nhóm, nhiều nhóm trùng tên (do người khác tạo) | TC chọn nhóm theo `value` của nhóm tự tạo, không chọn theo text |
| RISK-CUS-09 | Vault chứa thông tin đăng nhập của KH | Tab nhạy cảm; sai quyền visibility là lộ bí mật | Ưu tiên kiểm REQ-CUS-92 · 93 · 95; chỉ dùng giá trị giả |

## 7. Phân rã Epic / Story

| Story ID | Tên Story | REQ bao phủ | Số REQ | AMB / RISK liên quan | Ghi chú phạm vi |
|---|---|---|---|---|---|
| STORY-CUS-01 | Danh sách, tìm kiếm & bộ lọc | REQ-CUS-01 → 16 | 16 | AMB-CUS-13 · RISK-CUS-01 | Danh sách, Summary, Export, New Filter |
| STORY-CUS-02 | Tạo khách hàng | REQ-CUS-17 → 37 | 21 | AMB-CUS-01 · 02 · 03 · 04 · 17 · RISK-CUS-08 | Form 2 tab, kiểm trùng tên, copy địa chỉ |
| STORY-CUS-03 | Hồ sơ, sửa & Customer Admins | REQ-CUS-38 → 52 | 15 | RISK-CUS-07 | 19 tab, sửa Profile, gán admin, lọc chứng từ theo KH |
| STORY-CUS-04 | Active, Bulk Actions & xoá | REQ-CUS-53 → 61 | 9 | AMB-CUS-05 · 06 · 07 · 08 · RISK-CUS-03 · 06 | |
| STORY-CUS-05 | Import Customers | REQ-CUS-62 → 68 | 7 | AMB-CUS-09 · 10 · 19 · RISK-CUS-05 | Import thật chưa chạy |
| STORY-CUS-06 | Notes & Files | REQ-CUS-69 → 79 | 11 | AMB-CUS-11 · RISK-CUS-04 | |
| STORY-CUS-07 | Statement | REQ-CUS-80 → 87 | 8 | AMB-CUS-14 | PDF/email chưa kiểm |
| STORY-CUS-08 | Vault & Map | REQ-CUS-88 → 101 | 14 | AMB-CUS-12 · 15 · 16 · 18 · RISK-CUS-09 | |

**Tổng: 8 Story / 101 REQ — mọi REQ thuộc đúng một Story, không mồ côi, không trùng: 16 + 21 + 15 + 9 + 7 + 11 + 8 + 14 = 101 ✔**

**Đối chiếu AMB/RISK:**

| Nhóm | Mã | Nằm ở đâu |
|---|---|---|
| AMB thuộc Story | AMB-CUS-01 → 19 (19 mã) | Phân bổ ở bảng Story |
| AMB cấp Epic | — | `AMB-SYS-01` (cấp hệ thống) ảnh hưởng master data Groups/Currency/ngôn ngữ/Google API key/đuôi tệp |
| RISK thuộc Story | RISK-CUS-01 · 03 · 04 · 05 · 06 · 07 · 08 · 09 | Phân bổ ở bảng Story |
| RISK cấp Epic | RISK-CUS-02 | Dữ liệu cá nhân — cắt ngang mọi Story |

**Hạng mục cấp Epic** (cố ý không gán Story): Ma trận phân quyền (mục 4) và Ma trận trạng thái (mục 5) — cắt ngang Story 01 · 02 · 03 · 04; NFR (file web) — áp toàn module; `RISK-CUS-02` — mọi Story đụng dữ liệu KH.

**Thứ tự triển khai đề xuất:** STORY-CUS-02 (tạo KH — dữ liệu nền cho mọi TC khác) → 03 → 01 → 04 → 06 → 08 → 07 → 05. Chưa Story nào `BLOCKED` toàn bộ; các REQ ⚪ trong Story 02 · 04 · 05 · 07 · 08 chờ `AMB-CUS-03` · `05` · `07` · `08` · `10` · `14` · `15` · `18`.

## 8. Nhật ký thay đổi

| Ngày | Nguồn | REQ ảnh hưởng | Loại | Tóm tắt thay đổi | TC cần xử lý |
|---|---|---|---|---|---|
| 04-11-2026 | UI recon | REQ-CUS-01 → 101 | 🟢 Thêm | Khởi tạo tài liệu từ khảo sát UI thực tế (`/generate-requirements-from-website CUSTOMER`) — 8 Story, 19 AMB (8 🔴), 9 RISK | — (viết TC mới) |
