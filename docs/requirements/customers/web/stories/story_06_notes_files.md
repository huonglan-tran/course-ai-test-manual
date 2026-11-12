# STORY-CUS-06 — Notes & Files

> ← [Index module](../../REQUIREMENTS_CUSTOMERS_SUMMARY.md) · [File web](../requirements_customers_web.md) · REQ `REQ-CUS-96` → `112` (17 REQ). AMB/RISK, phân quyền, trạng thái: xem index.

**User story:** Là staff, tôi muốn ghi chú nội bộ và lưu tệp đính kèm cho khách hàng, chọn tệp nào khách hàng được xem.

## 1. Yêu cầu chức năng — Notes

| REQ ID | Tên yêu cầu | Mô tả | Acceptance Criteria | Trạng thái | Cập nhật lần cuối | Nguồn |
|---|---|---|---|---|---|---|
| REQ-CUS-96 | Tab Notes có nút New Note và bảng ghi chú | `?group=notes` | Tiêu đề `Notes`, nút `New Note`; bảng cột `Description` · `Added From` · `Date Added` · `Options`, có page size + `Export` + ô tìm; chưa có note → `No entries found` | 🟢 | — | Kiểm chứng thực tế |
| REQ-CUS-97 | New Note mở ô nhập | — | Bấm `New Note` → hiện textarea `#description` (nhãn `Note description`) + nút `Save`; form gửi `POST /admin/misc/add_note/<id>/customer`. Bấm `New Note` lần nữa → ẩn ô (bật/tắt) | 🟢 | — | Kiểm chứng thực tế |
| REQ-CUS-98 | Thêm note | — | Nhập nội dung → `Save` → toast nguyên văn `Note added successfully.`, dòng mới **đứng đầu** bảng (sắp `Date Added` giảm dần), `Added From` = tên staff đang đăng nhập (link `/admin/profile/<staff_id>`), `Date Added` `DD-MM-YYYY HH:mm:ss` | 🟢 | — | Kiểm chứng thực tế |
| REQ-CUS-99 | Note trống bị chặn | Không tạo note rác — badge đếm Notes giữ đúng nghĩa | Để trống `Note description` → `Save` → **không** tạo note, **không** có toast thành công, bảng và badge số note không đổi; dưới ô hiện `This field is required.` (thông báo bắt buộc chuẩn của hệ thống — như REQ-CUS-30 · 42 · 86 · 125). ⚠️ **Hiện trạng 11-11-2026 lệch:** toast `Note added successfully.`, có dòng mới với Description rỗng → TC **dự kiến FAIL cho tới khi Dev sửa** — ứng viên bug (`AMB-CUS-15` ✅) | 🟡 | 11-11-2026 · `AMB-CUS-15` | Kiểm chứng thực tế (hiện trạng) + User chốt `AMB-CUS-15` theo Assumption tạm |
| REQ-CUS-100 | Menu tab Notes có badge đếm số note | — | Sau khi thêm → menu hiện `Notes` kèm số note hiện có (đo: 2 → 3); xoá 1 → giảm 1 (3 → 2); 0 note → không có badge | 🟢 | — | Kiểm chứng thực tế |
| REQ-CUS-101 | Sửa note tại chỗ | — | Icon sửa (`toggle_edit_note(<note_id>)`) → textarea chứa nội dung cũ + nút `Update note` · `Cancel` → sửa → `Update note` → `POST /admin/misc/edit_note/<note_id>`, toast nguyên văn `Note updated successfully` (không dấu chấm), Description hiện nội dung mới | 🟢 | — | Kiểm chứng thực tế |
| REQ-CUS-102 | Xoá note | — | Icon xoá (`/admin/misc/delete_note/<note_id>`) → confirm `Are you sure you want to perform this action?` → OK → toast nguyên văn `Note deleted`, dòng biến mất, badge giảm 1 | 🟢 | — | Kiểm chứng thực tế |

## 2. Yêu cầu chức năng — Files

| REQ ID | Tên yêu cầu | Mô tả | Acceptance Criteria | Trạng thái | Cập nhật lần cuối | Nguồn |
|---|---|---|---|---|---|---|
| REQ-CUS-103 | Tab Files có vùng upload và bảng tệp | `?group=attachments` | Dòng chú thích nguyên văn `Files from projects and tasks linked to the customer are not shown on this table.` · vùng `Drop files here to upload` · nút `Choose from Google Drive` · bảng cột `File` · `Show to customers area` · `Date Uploaded` · `Options`; rỗng → `No entries found` | 🟢 | — | Kiểm chứng thực tế |
| REQ-CUS-104 | Chỉ nhận 10 đuôi tệp, tối đa 128 MB | Cấu hình Dropzone | `acceptedFiles` = `.png,.jpg,.pdf,.doc,.docx,.xls,.xlsx,.ppt,.pptx,.zip`; `maxFilesize` = `128` (MB); upload song song 2 tệp; gửi tới `/admin/clients/upload_attachment/<id>` (param `file`). Giới hạn 128 MB **chưa** thử bằng tệp thật | 🟢 | — | UI thực tế (DOM — cấu hình Dropzone) |
| REQ-CUS-105 | Tệp sai đuôi bị từ chối | — | Chọn tệp `.txt` → thông báo nguyên văn `You can't upload files of this type.`, bảng không thêm dòng | 🟢 | — | Kiểm chứng thực tế |
| REQ-CUS-106 | Upload tệp hợp lệ | — | Chọn tệp `.png` → bảng thêm 1 dòng: ảnh xem trước (link `/download/preview_image?path=uploads/clients/<id>/<tên tệp>&type=image/png`) · `Date Uploaded` `DD-MM-YYYY HH:mm:ss` · icon email + icon xoá | 🟢 | — | Kiểm chứng thực tế |
| REQ-CUS-107 | Show to customers area mặc định tắt, bật được | — | Dòng tệp mới: switch tắt → bấm → `GET /admin/misc/toggle_file_visibility/<file_id>/1` (200), switch bật; tải lại → vẫn bật | 🟢 | — | Kiểm chứng thực tế + API |
| REQ-CUS-108 | Tệp bật hiển thị xem được ở cổng khách hàng | Tác tạo của REQ-CUS-107 | Contact của KH đăng nhập cổng khách hàng → thấy tệp; tắt → không thấy. **Chưa kiểm** — cổng khách hàng ngoài phạm vi | ⚪ | — | Chưa kiểm chứng |
| REQ-CUS-109 | Modal gửi tệp qua email | Icon email ở dòng tệp | Modal `Send File`: `Email Subject` · `Email Address` · `Message` (textarea) · nút `Close` · `Send File`; form gửi `/admin/misc/send_file/` | 🟢 | — | Kiểm chứng thực tế |
| REQ-CUS-110 | Send File để trống báo Email Address bắt buộc | Người dùng phải biết vì sao chưa gửi | Để trống 3 ô → `Send File` → modal **vẫn mở**, **không** có request `send_file`, dưới ô `Email Address` hiện `This field is required.`. ⚠️ **Hiện trạng 11-11-2026 lệch:** modal đóng, không request, không thông báo gì → TC **dự kiến FAIL cho tới khi Dev sửa** — ứng viên bug nhẹ (`AMB-CUS-16` ✅) | 🟡 | 11-11-2026 · `AMB-CUS-16` | Kiểm chứng thực tế (hiện trạng) + User chốt `AMB-CUS-16` theo Assumption tạm |
| REQ-CUS-111 | Gửi tệp qua email | Tác tạo của REQ-CUS-109 | Nhập email nhận + subject → `Send File` → người nhận nhận email có tệp đính kèm. **Chưa kiểm** — không gửi email trên môi trường dùng chung, chỉ kiểm giao diện (`AMB-CUS-25` ⏭️) | ⚪ | 11-11-2026 · ✏️ biên tập | Chưa kiểm chứng |
| REQ-CUS-112 | Xoá tệp | — | Icon xoá (`/admin/clients/delete_attachment/<id>/<file_id>`) → confirm `Are you sure you want to perform this action?` → OK → dòng biến mất (bảng về `No entries found`), **không** có toast | 🟢 | — | Kiểm chứng thực tế |

## 3. Đặc tả trường dữ liệu

| Field | Loại UI · `name` | Required | Ràng buộc | REQ |
|---|---|---|---|---|
| Note description | textarea · `description` | **Mong đợi** ✅ · hiện trạng ❌ (không chặn trống) | — | 97 → 99 |
| File | Dropzone · `file` (multiple) | — | 10 đuôi, ≤ 128 MB | 104 → 106 |
| Show to customers area | switch `input.onoffswitch-checkbox.customer_file#<file_id>` | — | Mặc định tắt | 107 |
| Send File — Email Subject / Email Address / Message | `send_file_subject` · `send_file_email` · `send_file_message` | **Mong đợi:** Email Address ✅ · hiện trạng không đánh dấu | Input ẩn `file_path` chứa **đường dẫn tuyệt đối trên server** (`RISK-CUS-04`) | 109 · 110 |

## 4. Thông báo

| REQ ID | Trigger | Nội dung nguyên văn |
|---|---|---|
| REQ-CUS-98 | Thêm note | `Note added successfully.` |
| REQ-CUS-99 | Note trống | `This field is required.` — **mong đợi**; hiện trạng báo nhầm `Note added successfully.` |
| REQ-CUS-110 | Send File — Email Address trống | `This field is required.` — **mong đợi**; hiện trạng không có thông báo |
| REQ-CUS-101 | Sửa note | `Note updated successfully` |
| REQ-CUS-102 | Xoá note | `Note deleted` |
| REQ-CUS-105 | Tệp sai đuôi | `You can't upload files of this type.` |
