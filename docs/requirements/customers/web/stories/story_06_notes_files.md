# STORY-CUS-06 — Notes & Files trong hồ sơ KH

> ← [Index module `REQUIREMENTS_CUSTOMERS_SUMMARY.md`](../../REQUIREMENTS_CUSTOMERS_SUMMARY.md) · [File nền tảng Web](../requirements_customers_web.md)
>
> Phạm vi: `REQ-CUS-69` → `REQ-CUS-79` (11 REQ) · AMB liên quan: `AMB-CUS-11` · RISK liên quan: `RISK-CUS-04`

**Route:** Notes `?group=notes` · thêm `POST /admin/misc/add_note/<id KH>/customer` · sửa `POST /admin/misc/edit_note/<id note>` · xoá `/admin/misc/delete_note/<id note>`. Files `?group=attachments` · tải lên `POST /admin/clients/upload_attachment/<id KH>` · ẩn/hiện `GET /admin/misc/toggle_file_visibility/<id file>/<0|1>` · xoá `/admin/clients/delete_attachment/<id KH>/<id file>`.

## 1. Yêu cầu chức năng

| REQ ID | Tên yêu cầu | Mô tả | Acceptance Criteria | Trạng thái | Cập nhật lần cuối | Nguồn |
|---|---|---|---|---|---|---|
| REQ-CUS-69 | Tab Notes có nút New Note và bảng 4 cột | | Tiêu đề `Notes` · nút `New Note`; bảng cột `Description` · `Added From` · `Date Added` · `Options`; KH chưa có note → `No entries found` | 🟢 | — | Kiểm chứng thực tế |
| REQ-CUS-70 | Thêm note hiển thị dòng mới kèm người tạo và tăng bộ đếm tab | | `New Note` → form 1 ô `Note description` (textarea `description`) + nút `Save` → nhập `auto_cus_note_<ts>` → `Save` → `303` về `?group=notes`; thông báo `Note added successfully.`; dòng mới: Description = nội dung · Added From = **tên staff đang đăng nhập** (link `/admin/profile/<id>`) · Date Added dạng `DD-MM-YYYY HH:mm:ss`; số đếm cạnh tab `Notes` tăng 1 | 🟢 | — | Kiểm chứng thực tế · API `POST /admin/misc/add_note/<id>/customer` · 303 |
| REQ-CUS-71 | Note trống vẫn được lưu | Hành vi **hiện tại** — nghi lỗi | Mở `New Note`, để trống, bấm `Save` → không có thông báo lỗi; `POST` → `303`; thông báo `Note added successfully.`; bảng có dòng với Description rỗng. TC viết theo kết luận `AMB-CUS-11` 🔴 | 🟢 | — | Kiểm chứng thực tế · API `POST /admin/misc/add_note/<id>/customer` · 303 |
| REQ-CUS-72 | Sửa note tại chỗ cập nhật nội dung | | Icon sửa (`toggle_edit_note(<id>)`) → dòng hiện textarea + nút `Cancel` · `Update note` → đổi nội dung → `Update note` → `POST /admin/misc/edit_note/<id>` trả JSON `success: true` · `message: "Note updated successfully"`; thông báo nổi `Note updated successfully`; ô Description hiển thị nội dung mới, không tải lại trang | 🟢 | — | Kiểm chứng thực tế · API `POST /admin/misc/edit_note/<id>` · 200 |
| REQ-CUS-73 | Xoá note cần xác nhận | | Icon thùng rác (href `/admin/misc/delete_note/<id>`) → hộp `confirm` → đồng ý → thông báo `Note deleted`; dòng biến mất | 🟢 | — | Kiểm chứng thực tế |
| REQ-CUS-74 | Tab Files có vùng kéo-thả tải lên và ghi chú phạm vi | | Tab hiện ghi chú nguyên văn `Files from projects and tasks linked to the customer are not shown on this table.`; vùng Dropzone `Drop files here to upload` (form `#client-attachments-upload`); nút `Choose from Google Drive`; bảng cột `File` · `Show to customers area` · `Date Uploaded` · `Options` | 🟢 | — | Kiểm chứng thực tế |
| REQ-CUS-75 | Tải lên tệp hợp lệ thêm dòng mới, mặc định không hiển thị cho KH | | Tải lên 1 tệp `.png` → `POST /admin/clients/upload_attachment/<id>` → trang tải lại; bảng có 1 dòng: ô File là ảnh xem trước (link `/download/preview_image?path=uploads/clients/<id>/<tên tệp>…`) · switch `Show to customers area` **không** `checked` · Date Uploaded `DD-MM-YYYY HH:mm:ss` · Options gồm icon email (mở `#send_file`) và icon xoá | 🟢 | — | Kiểm chứng thực tế |
| REQ-CUS-76 | Tệp ngoài danh sách đuôi cho phép bị từ chối tại trình duyệt | | Danh sách đuôi cho phép lấy từ cấu hình `app.options.allowed_files` = `.png,.jpg,.pdf,.doc,.docx,.xls,.xlsx,.ppt,.pptx,.zip` (đọc 04-11-2026, cấu hình được ở Setup). Tải lên `.txt` → **không** gửi `POST`; thông báo nổi nguyên văn `You can't upload files of this type.` | 🟢 | — | Kiểm chứng thực tế (đọc DOM — ảnh chụp hụt thông báo, đã đổi ảnh) |
| REQ-CUS-77 | Bật Show to customers area lưu trạng thái hiển thị tệp | | Bấm switch của tệp → `GET /admin/misc/toggle_file_visibility/<id file>/1?csrf_token_name=<token>` trả `200`; tải lại → switch vẫn `checked`. Chưa kiểm phía cổng KH | 🟢 | — | Kiểm chứng thực tế · API `GET .../toggle_file_visibility/<id>/1` · 200 |
| REQ-CUS-78 | Xoá tệp cần xác nhận rồi gỡ khỏi bảng | | Icon xoá (href `/admin/clients/delete_attachment/<id KH>/<id file>`) → hộp `confirm` đã xuất hiện và được đồng ý; **chưa** kiểm bảng sau khi xoá (KH được xoá ngay sau đó) | ⚪ | — | Kiểm chứng một phần — kết quả chưa xác nhận |
| REQ-CUS-79 | Gửi tệp qua email từ tab Files | | Icon email mở modal `#send_file` — chưa mở, chưa gửi | ⚪ | — | UI thực tế — chưa kiểm chứng |

## 2. Đặc tả trường dữ liệu

| Field (Label) | `name` | Loại UI | Required | Ràng buộc / mặc định | REQ liên quan |
|---|---|---|---|---|---|
| Note description | `description` | textarea | — (không validation) | Rỗng | 70 · 71 |
| Tệp (Dropzone) | `file` | file `multiple` | — | Đuôi theo `allowed_files`; chọn nhiều tệp | 75 · 76 |
| Show to customers area | switch `.onoffswitch-checkbox.customer_file` `data-switch-url=/admin/misc/toggle_file_visibility` | — | — | Tắt | 75 · 77 |

> ⚠️ Ô Options của tệp mang thuộc tính `data-path` chứa **đường dẫn tuyệt đối trên máy chủ** của tệp — `RISK-CUS-04`. Không chép giá trị vào tài liệu/TC.

## 3. Validation & thông báo

| REQ ID | Rule / Trigger | Thông báo (nguyên văn) |
|---|---|---|
| REQ-CUS-70 · 71 | Lưu note (kể cả trống) | `Note added successfully.` |
| REQ-CUS-72 | Sửa note | `Note updated successfully` (không dấu chấm cuối) |
| REQ-CUS-73 | Xoá note | `Note deleted` |
| REQ-CUS-76 | Tệp sai loại | `You can't upload files of this type.` |

## 4. Luồng xử lý

```
Notes ─→ New Note ─→ Save ─→ 303 ─→ dòng mới ─→ sửa tại chỗ (AJAX) / xoá (confirm)
Files ─→ kéo-thả / chọn tệp ─→ [đuôi hợp lệ?] ─ không ─→ "You can't upload files of this type."
                                             └ có ──→ POST upload_attachment ─→ tải lại ─→ switch hiển thị / xoá
```
