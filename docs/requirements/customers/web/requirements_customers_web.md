# Requirements — Customers (`CUS`) · Web

> ← [Index module](../REQUIREMENTS_CUSTOMERS_SUMMARY.md) · File nền tảng Web. Module vượt ngưỡng 80 REQ nên **dòng REQ nằm ở các file Story** trong [`stories/`](stories/); file này giữ metadata khảo sát, tầng network, yêu cầu phi chức năng và Danh mục Evidence.

## 1. Metadata khảo sát

| Thuộc tính | Giá trị |
|---|---|
| **Ngày khảo sát** | 11-11-2026 |
| **Trình duyệt khảo sát** | Google Chrome 157 (Playwright MCP, headed), viewport đo được `1600×750` (cửa sổ `1617×886` — không bị cắt). Mọi AC dựa trên hộp thoại `confirm()` gốc hoặc hành vi tự điền của trình duyệt **chỉ đúng với trình duyệt này** |
| **Tài khoản** | Staff role Admin — `EMAIL_ADMIN` trong `.env`. User tự đăng nhập trên cửa sổ trình duyệt (agent không nhập mật khẩu) |
| **Môi trường** | Demo dùng chung, dữ liệu biến động (`RISK-SYS-01`). Mọi bản ghi do lượt khảo sát tạo ra mang tên `auto_cus_*_20261111_*` và **đã dọn** (xem mục 5) |
| **Tầng network** | Quan sát thụ động request do UI phát sinh (mục 2) — **không** gọi API trực tiếp |
| **Tài liệu đầu vào** | Không có — toàn bộ REQ sinh từ khảo sát UI thực tế |

### 1.1. Bản đồ phủ tài liệu

Không có tài liệu — toàn bộ REQ sinh từ khảo sát UI thực tế.

## 2. Tầng network quan sát được

| Request | Phát sinh khi | Dữ kiện rút ra | REQ |
|---|---|---|---|
| `POST /admin/clients/table` → 200 | Nạp/tìm/lọc/sắp xếp danh sách | DataTables phía server: `order[0][column]=2&dir=asc` (Company tăng dần) · `length=25` · `filters[match_type]=or` · `filters[rules][i][id\|value\|operator\|type]` (VD `id=active, value=0, type=BooleanRule`) | 08 · 09 · 25 |
| `POST /admin/clients/check_duplicate_customer_name` → 200 | Rời ô Company trên form tạo | Body phản hồi `{"exists":true,"message":"It looks that a customer with name <b>…</b> already exists, …"}` | 33 → 35 |
| `POST /admin/clients/client` (form) | Save / Save and create contact | Chuyển tới `/admin/clients/client/<id>` hoặc `…?group=contacts&new_contact=true` | 48 · 49 |
| `GET /admin/clients/change_client_status/<id>/0` → 200 | Tắt switch Active ở danh sách | Không có toast | 66 |
| `GET /admin/clients/mark_as_active/<id>` | Link `Mark as active` trên banner | — | 68 |
| `GET /admin/clients/delete/<id>` | Delete (sau confirm) | Chuyển về `/admin/clients` + toast | 71 |
| `POST /admin/clients/bulk_action` → 200 | Confirm ở Bulk Actions | Toast `Total customers deleted: <n>` khi Mass Delete | 78 · 79 |
| `POST /admin/clients/assign_admins/<id>` · `GET /admin/clients/delete_customer_admin/<id>/<staff_id>` | Gán/gỡ Customer Admin | Chuyển tới `…?tab=customer_admins` | 63 · 64 |
| `POST /admin/clients/group` | Save modal Add New Customer Group | *Không submit thật — chỉ trigger validation client* | 41 · 42 |
| `POST /admin/clients/import` (form) | Simulate Import / Import | Simulate trả lại trang có bảng Simulation Data | 86 → 92 |
| `POST /admin/misc/add_note/<id>/customer` · `POST /admin/misc/edit_note/<note_id>` · `GET /admin/misc/delete_note/<note_id>` | Thêm/sửa/xoá note | — | 97 · 101 · 102 |
| `POST /admin/clients/upload_attachment/<id>` (Dropzone, param `file`) · `GET /admin/misc/toggle_file_visibility/<file_id>/1` · `GET /admin/clients/delete_attachment/<id>/<file_id>` | Upload / bật hiển thị / xoá tệp | — | 106 · 107 · 112 |
| `GET /admin/clients/statement?customer_id=<id>&from=<DD-MM-YYYY>&to=<DD-MM-YYYY>` → 200 | Đổi khoảng thời gian sao kê | Nạp lại nội dung sao kê bằng AJAX | 115 · 117 |
| `POST /admin/clients/send_statement?customer_id=<id>&from=…&to=…` | Send ở modal Account Summary | Toast `Problem while sending the statement` khi không chọn người nhận | 121 |
| `POST /admin/clients/vault_entry_create/<id>` · `POST /admin/clients/vault_entry_update/<entry_id>` · `GET /admin/clients/vault_entry_delete/<entry_id>` | Tạo/sửa/xoá vault entry | — | 127 · 130 · 131 |
| `POST /admin/clients/vault_encrypt_password` | Confirm ở modal View Password | *Không submit thật* | 132 · 133 |
| `POST /admin/clients/save_longitude_and_latitude/<id>` → 200 | Save toạ độ Map | Tải lại trang | 136 · 137 |
| `POST /admin/misc/get_relation_data` → 200 | Gõ tìm KH trong ô Customer của form Invoice | KH Inactive không có trong kết quả | 69 |

**Không phát hiện** response 4xx nào trong suốt lượt khảo sát — mọi validation quan sát được đều ở tầng client (jQuery validate) hoặc server trả 200 kèm toast.

## 3. Yêu cầu phi chức năng (quan sát được)

| Mã | Nội dung | Căn cứ |
|---|---|---|
| NFR-01 | Danh sách KH phân trang phía server (DataTables) — trang vẫn dùng được với ~2.900 bản ghi, page size tối đa `All` | `POST /admin/clients/table` · REQ-CUS-09 |
| NFR-02 | Mọi thao tác xoá dùng hộp xác nhận `confirm()` gốc của trình duyệt (`RISK-CUS-06`) | REQ-CUS-70 · 77 |
| NFR-03 | Mật khẩu Vault không hiển thị dạng rõ; xem phải nhập lại mật khẩu đăng nhập | REQ-CUS-128 · 132 |
| NFR-04 | Giao diện theo ngôn ngữ người dùng — 25 ngôn ngữ (`RISK-SYS-03`); khảo sát ở tiếng Anh | Bản đồ hệ thống |

## 4. Danh mục Evidence

> 22 ảnh, chụp 11-11-2026, **đã mở lại xác nhận từng ảnh**. Ảnh danh sách chỉ chụp khi bảng đã lọc về bản ghi do test tạo hoặc về rỗng — không kéo dữ liệu KH của người khác vào repo. Ảnh `_viewport` = đối tượng nằm trọn trong viewport; `_fullpage` = form dài.

| Ảnh | Màn hình | Trạng thái | REQ làm bằng chứng |
|---|---|---|---|
| [customers_list_own_records_export_menu_viewport.png](evidence/customers_list_own_records_export_menu_viewport.png) | `/admin/clients` | Lọc `auto_cus_` (3 KH test) · Summary 6 bộ đếm · thanh công cụ · menu Export đang mở · switch Active (1 tắt) · nhóm `customer group b` | 01 · 05 · 06 · 11 · 17 · 18 · 19 · 20 |
| [customers_list_search_no_match_viewport.png](evidence/customers_list_search_no_match_viewport.png) | `/admin/clients` | Tìm chuỗi không khớp → `No matching records found` | 14 |
| [customer_new_form_details_default_fullpage.png](evidence/customer_new_form_details_default_fullpage.png) | `/admin/clients/client` | Form tạo mới — tab Customer Details mặc định | 28 · 29 · 40 · 44 · 46 · 47 |
| [customer_new_form_company_required_viewport.png](evidence/customer_new_form_company_required_viewport.png) | `/admin/clients/client` | Save khi trống → lỗi Company | 30 |
| [customer_profile_blank_company_saved_viewport.png](evidence/customer_profile_blank_company_saved_viewport.png) | Hồ sơ `#<id>` | Company chỉ khoảng trắng đã lưu — tiêu đề chỉ có `#<id>` | 31 · 55 · 57 |
| [customer_new_form_duplicate_name_info_viewport.png](evidence/customer_new_form_duplicate_name_info_viewport.png) | `/admin/clients/client` | Thông báo trùng tên sau khi rời ô Company | 33 |
| [customer_billing_shipping_copied_fullpage.png](evidence/customer_billing_shipping_copied_fullpage.png) | Hồ sơ — Billing & Shipping | Sau `Same as Customer Info` + `Copy Billing Address` · chỉ có nút Save | 51 · 52 · 53 · 58 |
| [customer_group_modal_required_viewport.png](evidence/customer_group_modal_required_viewport.png) | Modal Add New Customer Group | Save khi trống → lỗi Name | 41 · 42 |
| [customer_admins_assign_modal_viewport.png](evidence/customer_admins_assign_modal_viewport.png) | Hồ sơ — Customer Admins | Modal Assign Admin đã chọn 1 staff · bảng rỗng phía sau | 61 · 62 |
| [customer_profile_inactive_banner_viewport.png](evidence/customer_profile_inactive_banner_viewport.png) | Hồ sơ KH Inactive | Banner cảnh báo + link `Mark as active` | 67 |
| [invoice_customer_dropdown_inactive_hidden_viewport.png](evidence/invoice_customer_dropdown_inactive_hidden_viewport.png) | `/admin/invoices/invoice` | Gõ `auto_cus_` → chỉ 2 KH Active hiện, KH Inactive không có | 69 |
| [customers_bulk_actions_mass_delete_checked_viewport.png](evidence/customers_bulk_actions_mass_delete_checked_viewport.png) | Modal Bulk Actions | Tick Mass Delete → ô Groups ẩn (bảng phía sau đã lọc rỗng) | 75 · 76 |
| [customers_import_page_default_viewport.png](evidence/customers_import_page_default_viewport.png) | `/admin/clients/import` | Trang mặc định: 4 hướng dẫn, bảng mẫu, form | 83 · 84 · 85 |
| [customers_import_simulate_result_viewport.png](evidence/customers_import_simulate_result_viewport.png) | `/admin/clients/import` | Kết quả Simulate 5 dòng (hợp lệ, thiếu email, trùng email, email sai, thiếu company) — không dòng nào bị đánh dấu · Country = 0 · ô file đã trống | 88 · 90 · 91 · 92 |
| [customer_notes_list_empty_and_text_viewport.png](evidence/customer_notes_list_empty_and_text_viewport.png) | Hồ sơ — Notes | 3 note (2 note trống) · badge `3` · icon sửa/xoá | 96 · 98 · 99 · 100 |
| [customer_files_uploaded_png_visible_viewport.png](evidence/customer_files_uploaded_png_visible_viewport.png) | Hồ sơ — Files | Ghi chú đầu bảng · dropzone · 1 PNG đã upload, switch `Show to customers area` bật | 103 · 106 · 107 |
| [customer_statement_period_reversed_fullpage.png](evidence/customer_statement_period_reversed_fullpage.png) | Hồ sơ — Statement | Period đảo ngược `11-11-2026 To 01-01-2026` vẫn hiển thị · 3 nút Print/PDF/Email | 114 · 116 · 117 · 118 |
| [customer_statement_send_modal_no_contact_viewport.png](evidence/customer_statement_send_modal_no_contact_viewport.png) | Modal Account Summary | KH không có contact → cảnh báo đỏ · Email to rỗng | 120 |
| [customer_vault_entry_required_viewport.png](evidence/customer_vault_entry_required_viewport.png) | Modal Vault Entry | Save khi trống → 3 lỗi bắt buộc · radio/checkbox mặc định | 124 · 125 |
| [customer_vault_entry_created_masked_viewport.png](evidence/customer_vault_entry_created_masked_viewport.png) | Hồ sơ — Vault | Thẻ entry vừa tạo · Port 70000 · mật khẩu che | 126 · 127 · 128 |
| [customer_vault_view_password_modal_viewport.png](evidence/customer_vault_view_password_modal_viewport.png) | Modal View Password | Modal yêu cầu nhập mật khẩu đăng nhập (chưa nhập) | 132 |
| [customer_map_invalid_coords_saved_viewport.png](evidence/customer_map_invalid_coords_saved_viewport.png) | Hồ sơ — Map | Toạ độ `abc` / `999` còn sau khi lưu + tải lại · thông báo thiếu Google API key | 135 · 136 · 137 |

REQ 🟢 không có ảnh nào ở bảng trên đều có **số liệu đọc DOM / network ghi trong cột Acceptance Criteria** của file Story (skill 7.2.1).

## 5. Dữ liệu do lượt khảo sát tạo ra

| Bản ghi | Thao tác | Đã dọn |
|---|---|---|
| KH `#15847` (Company chỉ khoảng trắng) | Tạo → xoá đơn lẻ (REQ 70 → 72) | ✅ Trả 404 |
| KH `#15848` `auto_cus_20261111_1420` (+ 2 note, 1 tệp PNG, 1 vault entry, toạ độ Map) | Tạo (206 ký tự → cắt) · sửa · gán/gỡ admin · bulk group | ✅ Note/tệp/vault xoá riêng · KH xoá bằng Mass Delete |
| KH `#15849` `auto_cus_20261111_1420` (trùng tên) | Tạo · bulk group | ✅ Mass Delete |
| KH `#15850` `auto_cus_sc_20261111_1422` | Save and create contact · tắt/bật Active | ✅ Mass Delete (`Total customers deleted: 3`) |
| Import | Chỉ Simulate — không tạo bản ghi (REQ 89) | — |

Sau dọn: tìm `20261111_14` trả `0 entries`; Summary về `2942 Total · 9 Inactive` — đúng số trước khảo sát.
