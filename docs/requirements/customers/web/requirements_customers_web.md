# Requirements — Customers (`CUS`) · Web

> ← [Index module `REQUIREMENTS_CUSTOMERS_SUMMARY.md`](../REQUIREMENTS_CUSTOMERS_SUMMARY.md) — phân quyền, trạng thái, AMB/RISK, Story, Nhật ký nằm ở index.
>
> File này chứa **metadata khảo sát Web**, bản đồ file Story và Danh mục Evidence. REQ chi tiết nằm trong 8 file `stories/` (101 REQ vượt ngưỡng 80 — skill mục 5.1): `REQ-CUS-01` → `REQ-CUS-101`.

## Bảng metadata khảo sát

| Thuộc tính | Giá trị |
|---|---|
| **Nền tảng** | Web — khu Admin |
| **Route** | Danh sách `/admin/clients` · tạo `/admin/clients/client` · hồ sơ `/admin/clients/client/<id>?group=<tab>` · import `/admin/clients/import` |
| **Ngày khảo sát** | 04-11-2026 |
| **Trình duyệt khảo sát** | Google Chrome (Playwright MCP, headed), viewport `1600×780` (đo bằng `innerWidth × innerHeight`, cửa sổ `1618×915` — không bị cắt). Mọi AC dựa trên hộp thoại / chuỗi mặc định của trình duyệt (`confirm`, `beforeunload`, autofill) **chỉ đúng với trình duyệt này** |
| **Tài khoản** | Staff role Admin (`.env` — `EMAIL_ADMIN`). **Người dùng tự đăng nhập**, agent không nhập mật khẩu |
| **Dữ liệu test** | Môi trường dùng chung — agent chỉ ghi/xoá bản ghi **do phiên này tạo**: 4 KH (`auto_cus_<ts>` · `auto_cus_sac_<ts>` · `auto_cus_long_<ts>_…` · 1 KH Company chỉ khoảng trắng), 2 note, 1 tệp ảnh, 1 vault entry (giá trị giả `auto_vault_<ts>`), 1 lần gán/gỡ admin. **Đã dọn hết**: 4/4 KH đã xoá (Summary về đúng số ban đầu), note/tệp/vault xoá theo KH. Simulate Import không ghi dữ liệu |
| **Tầng network** | Quan sát thụ động request do UI phát sinh (method · status · `Location` · tên field payload · body JSON nhỏ). **Không** gọi API trực tiếp. Không lưu body danh sách KH vào repo (chứa dữ liệu cá nhân) — tệp tạm đã xoá |
| **Bản đồ phủ tài liệu** | Không có tài liệu — toàn bộ REQ sinh từ khảo sát UI thực tế (user xác nhận 04-11-2026) |
| **Ngoài phạm vi lượt này** | Tab Contacts (`CONT`) · tab chứng từ (module chứng từ — `CUS` chỉ giữ REQ lọc theo KH) · tab Reminders (`PROF`) · cổng KH |

## Bản đồ file Story

| Story | File | REQ | Số REQ |
|---|---|---|---|
| STORY-CUS-01 | [stories/story_01_danh_sach_bo_loc.md](stories/story_01_danh_sach_bo_loc.md) — Danh sách, tìm kiếm & bộ lọc | 01 → 16 | 16 |
| STORY-CUS-02 | [stories/story_02_tao_khach_hang.md](stories/story_02_tao_khach_hang.md) — Tạo khách hàng | 17 → 37 | 21 |
| STORY-CUS-03 | [stories/story_03_ho_so_sua_customer_admins.md](stories/story_03_ho_so_sua_customer_admins.md) — Hồ sơ, sửa & Customer Admins | 38 → 52 | 15 |
| STORY-CUS-04 | [stories/story_04_active_bulk_xoa.md](stories/story_04_active_bulk_xoa.md) — Active, Bulk Actions & xoá | 53 → 61 | 9 |
| STORY-CUS-05 | [stories/story_05_import.md](stories/story_05_import.md) — Import Customers | 62 → 68 | 7 |
| STORY-CUS-06 | [stories/story_06_notes_files.md](stories/story_06_notes_files.md) — Notes & Files | 69 → 79 | 11 |
| STORY-CUS-07 | [stories/story_07_statement.md](stories/story_07_statement.md) — Statement | 80 → 87 | 8 |
| STORY-CUS-08 | [stories/story_08_vault_map.md](stories/story_08_vault_map.md) — Vault & Map | 88 → 101 | 14 |

**Tổng: 16 + 21 + 15 + 9 + 7 + 11 + 8 + 14 = 101 ✔**

## Yêu cầu phi chức năng quan sát được (Web)

| Hạng mục | Quan sát | Ghi chú |
|---|---|---|
| Phân trang & tìm kiếm phía máy chủ | Danh sách KH, Notes, Files, Customer Admins dùng DataTables; danh sách KH tải qua `POST /admin/clients/table` | Danh sách ~2.900 KH vẫn tải từng trang — không đo thời gian phản hồi |
| Định dạng ngày | Mọi ngày giờ hiển thị `DD-MM-YYYY` / `DD-MM-YYYY HH:mm:ss`; tham số ngày của Statement cùng định dạng | Theo cấu hình hệ thống (Setup bị chặn — `AMB-SYS-01`) |
| Bảo vệ CSRF | Mọi form có `csrf_token_name` (`<32 ký tự hex>`); thao tác bật/tắt qua `GET` mang token trên query string | `RISK-CUS-03` |
| Thông báo | Thành công/thất bại hiển thị bằng thông báo nổi góc phải (`alert_float`), tự biến mất sau vài giây — chụp ảnh dễ hụt | TC nên assert bằng locator thông báo ngay sau thao tác |
| Bảo vệ dữ liệu nhạy cảm | Mật khẩu Vault không có trong HTML; xem lại phải nhập mật khẩu đăng nhập | REQ-CUS-92 · 93 |
| Responsive | Chỉ khảo sát viewport `1600×780` — chưa kiểm viewport nhỏ | |

## Danh mục Evidence

> Ảnh chụp 04-11-2026 bằng Playwright MCP, **đã mở lại xác nhận từng ảnh**. Chỉ chụp màn hình chứa **dữ liệu do agent tạo** (`auto_cus_…`); ảnh danh sách chụp sau khi lọc chỉ còn KH tự tạo — không có tên/email/điện thoại của KH thật. 3 ảnh chụp hụt trạng thái đã **loại** (Company báo lỗi bị cuộn khỏi khung · modal contact đang mờ dần và lộ email đăng nhập do autofill · menu Export chưa kịp hiện) — các REQ tương ứng dựa trên số liệu đọc DOM ghi trong AC. Thông báo nổi của Simulate/Export/Files không có ảnh vì tự tắt.

| Tệp | Màn hình | Trạng thái | REQ |
|---|---|---|---|
| [evidence/customers_list_search_own_record_inactive_viewport.png](evidence/customers_list_search_own_record_inactive_viewport.png) | Danh sách KH | Thanh công cụ · Summary 6 bộ đếm · 9 cột · tìm theo tên KH tự tạo → 1 dòng · switch Active đã tắt · row options `View` / `Contacts` / `Delete` | 01 · 02 · 03 · 05 · 06 · 07 · 08 · 53 |
| [evidence/customer_new_form_details_default_fullpage.png](evidence/customer_new_form_details_default_fullpage.png) | Form tạo — Customer Details | Mặc định, Company đang focus | 17 · 18 · 27 · 29 · 30 · 31 |
| [evidence/customer_new_form_billing_shipping_copied_fullpage.png](evidence/customer_new_form_billing_shipping_copied_fullpage.png) | Form tạo — Billing & Shipping | Sau `Same as Customer Info` và `Copy Billing Address` | 32 · 33 |
| [evidence/customer_new_form_duplicate_name_info_viewport.png](evidence/customer_new_form_duplicate_name_info_viewport.png) | Form tạo | Cảnh báo trùng tên (tên KH tự tạo) | 22 |
| [evidence/customer_group_modal_required_viewport.png](evidence/customer_group_modal_required_viewport.png) | Modal Add New Customer Group | Sau Save khi trống | 28 |
| [evidence/customer_profile_after_create_fullpage.png](evidence/customer_profile_after_create_fullpage.png) | Hồ sơ KH — Profile | Ngay sau khi tạo: heading `#<id> <tên>` · 19 tab · 3 tab con · nút globe Website | 34 · 38 · 39 · 40 · 43 · 44 |
| [evidence/customer_admins_assign_modal_viewport.png](evidence/customer_admins_assign_modal_viewport.png) | Customer Admins | Modal Assign Admin đang mở (chụp lúc modal còn hiệu ứng mờ, đọc được) · bảng `No entries found` | 48 |
| [evidence/customers_bulk_actions_modal_mass_delete_checked_viewport.png](evidence/customers_bulk_actions_modal_mass_delete_checked_viewport.png) | Bulk Actions | Đã tick Mass Delete — ô Groups ẩn | 56 · 57 |
| [evidence/customers_import_empty_file_error_fullpage.png](evidence/customers_import_empty_file_error_fullpage.png) | Import Customers | 4 hướng dẫn · bảng mẫu · lỗi chưa chọn tệp | 62 · 63 · 65 |
| [evidence/customers_import_simulate_result_viewport.png](evidence/customers_import_simulate_result_viewport.png) | Import Customers | Kết quả Simulate: 2 dòng, dòng thiếu Email không bị đánh dấu | 66 · 67 |
| [evidence/customer_notes_empty_submit_viewport.png](evidence/customer_notes_empty_submit_viewport.png) | Notes | Sau khi lưu note trống: `Note added successfully.` + dòng Description rỗng · banner KH inactive | 45 · 71 |
| [evidence/customer_notes_list_after_add_edit_viewport.png](evidence/customer_notes_list_after_add_edit_viewport.png) | Notes | 2 note (1 đã sửa `_edited`, 1 trống) · bộ đếm tab `2` | 69 · 70 · 72 |
| [evidence/customer_files_inactive_banner_uploaded_png_viewport.png](evidence/customer_files_inactive_banner_uploaded_png_viewport.png) | Files | Ghi chú phạm vi · Dropzone · 1 tệp ảnh vừa tải, switch hiển thị tắt · banner KH inactive | 45 · 74 · 75 |
| [evidence/customer_statement_period_selected_fullpage.png](evidence/customer_statement_period_selected_fullpage.png) | Statement | Đã chọn `Period` (2 ô ngày) · nội dung sao kê kỳ trước · 3 icon Print/PDF/Email | 82 · 83 · 84 |
| [evidence/customer_statement_send_modal_viewport.png](evidence/customer_statement_send_modal_viewport.png) | Statement — gửi email | KH không có contact: thông báo đỏ · Email to rỗng | 86 |
| [evidence/customer_vault_entry_required_viewport.png](evidence/customer_vault_entry_required_viewport.png) | Vault Entry | 3 lỗi bắt buộc · 3 radio visibility (mặc định mức 1) · checkbox share | 89 · 90 |
| [evidence/customer_vault_entry_created_masked_viewport.png](evidence/customer_vault_entry_created_masked_viewport.png) | Vault | Entry vừa tạo (giá trị giả), mật khẩu che `••••••••••` | 91 |
| [evidence/customer_vault_view_password_confirm_modal_viewport.png](evidence/customer_vault_view_password_confirm_modal_viewport.png) | Vault — View Password | Modal yêu cầu nhập lại mật khẩu (chưa nhập) | 93 |
| [evidence/customer_map_invalid_coords_saved_viewport.png](evidence/customer_map_invalid_coords_saved_viewport.png) | Map | Sau tải lại: `abc` / `999` vẫn được lưu · câu thiếu Google API key | 97 · 99 |

**REQ 🟢 không có ảnh — căn cứ là số liệu DOM / network ghi trong AC:** 04 · 09 · 10 · 11 · 12 · 13 · 14 · 15 · 19 · 20 · 21 · 23 · 24 · 25 · 35 · 36 · 37 · 41 · 42 · 47 · 49 · 50 · 51 · 54 · 58 · 60 · 73 · 76 · 77 · 80 · 81 · 88 · 92 · 96 · 98.
