# STORY-CUS-07 — Statement (sao kê)

> ← [Index module](../../REQUIREMENTS_CUSTOMERS_SUMMARY.md) · [File web](../requirements_customers_web.md) · REQ `REQ-CUS-113` → `122` (10 REQ). AMB/RISK, phân quyền, trạng thái: xem index.

**User story:** Là staff, tôi muốn xem, in và gửi sao kê công nợ của khách hàng theo một khoảng thời gian.

## 1. Yêu cầu chức năng

| REQ ID | Tên yêu cầu | Mô tả | Acceptance Criteria | Trạng thái | Cập nhật lần cuối | Nguồn |
|---|---|---|---|---|---|---|
| REQ-CUS-113 | Chọn khoảng thời gian có 7 lựa chọn, mặc định This Month | `select#range` | 7 option theo thứ tự: `Today` · `This Week` · `This Month` · `Last Month` · `This Year` · `Last Year` · `Period`; mặc định `This Month`. `value` các option là mảng `["<từ>","<đến>"]` dạng `DD-MM-YYYY` tính theo **ngày hiện tại** (động), riêng `Period` có `value=period` | 🟢 | — | UI thực tế (DOM) |
| REQ-CUS-114 | Nội dung sao kê | — | Gồm: tiêu đề **chứa** `Customer Statement For <Company>` · thông tin công ty phát hành · khối `To:` (Company, địa chỉ billing, mã quốc gia 2 ký tự, zip, `VAT Number: <vat>`) · `Account Summary` với dòng `<từ> To <đến>` và 4 mục `Beginning Balance:` · `Invoiced Amount:` · `Amount Paid:` · `Balance Due:` · câu `Showing all invoices and payments between <từ> and <đến>` · bảng `Date` · `Details` · `Amount` · `Payments` · `Balance` (dòng đầu `Beginning Balance`) · dòng cuối `Balance Due` | 🟢 | — | Kiểm chứng thực tế |
| REQ-CUS-115 | Đổi khoảng thời gian nạp lại sao kê | — | Chọn khoảng khác → `GET /admin/clients/statement?customer_id=<id>&from=<DD-MM-YYYY>&to=<DD-MM-YYYY>` (200) → `Account Summary` hiện đúng `<từ> To <đến>` mới; trang không tải lại | 🟢 | — | Kiểm chứng thực tế + API |
| REQ-CUS-116 | Chọn Period hiện 2 ô ngày | — | Chọn `Period` → hiện 2 ô `#period-from` · `#period-to` (datepicker, định dạng `DD-MM-YYYY`); điền đủ 2 ô → sao kê nạp theo khoảng đã nhập | 🟢 | — | Kiểm chứng thực tế |
| REQ-CUS-117 | Khoảng ngày đảo ngược bị chặn | From phải ≤ To | Period từ `11-11-2026` đến `01-01-2026` → sao kê **không** nạp theo khoảng đảo ngược (Account Summary không hiện `11-11-2026 To 01-01-2026`) và có thông báo lỗi — hình thức, nguyên văn: `AMB-CUS-28`. Biên: From = To (`11-11-2026` → `11-11-2026`) vẫn nạp bình thường. ⚠️ **Hiện trạng 11-11-2026 lệch:** sao kê hiện `11-11-2026 To 01-01-2026`, không báo lỗi → TC **dự kiến FAIL cho tới khi Dev sửa** — ứng viên bug (`AMB-CUS-17` ✅) | 🟡 | 11-11-2026 · `AMB-CUS-17` | Kiểm chứng thực tế (hiện trạng) + User chốt `AMB-CUS-17` theo Assumption tạm |
| REQ-CUS-118 | Nút Print và View PDF theo khoảng đang chọn | Góc phải | 3 nút icon: `Print` → `/admin/clients/statement_pdf?customer_id=<id>&from=<từ>&to=<đến>&print=true` (mở tab mới) · `View PDF` → cùng URL không có `print` · nút email (mở modal REQ-CUS-120). `from`/`to` cập nhật theo khoảng đang chọn | 🟢 | — | UI thực tế (DOM) |
| REQ-CUS-119 | PDF sao kê đúng nội dung | Tác tạo của REQ-CUS-118 | Mở `View PDF` → PDF có cùng Account Summary và bảng giao dịch như trên màn hình. **Chưa kiểm** — mở/tải PDF chưa được phép (`AMB-CUS-12` ⏭️) | ⚪ | 11-11-2026 · ✏️ biên tập | Chưa kiểm chứng |
| REQ-CUS-120 | Modal gửi sao kê qua email | — | Nút email → modal `Account Summary`: `Email to` (multi-select contact) · `CC` · `Preview Email Template` (trình soạn thảo, nội dung mẫu có biến `{contact_firstname}` · `{statement_from}` · `{statement_to}` · `{statement_balance_due}`…) · `Close` · `Send`. KH không có contact phù hợp → dòng đỏ nguyên văn `Failed to auto select customer contacts. Make sure that the customer has active contacts and associated contacts with email notifications for Invoices enabled.` và `Email to` rỗng | 🟢 | — | Kiểm chứng thực tế |
| REQ-CUS-121 | Gửi sao kê bắt buộc chọn người nhận | Validate ở client, báo đúng ô sai | `Email to` rỗng → `Send` → form **không** submit (không có request `send_statement`), modal **vẫn mở**, ô `Email to` báo `This field is required.` (thông báo bắt buộc chuẩn của hệ thống). ⚠️ **Hiện trạng 11-11-2026 lệch:** form submit `POST /admin/clients/send_statement?customer_id=<id>&from=…&to=…`, modal đóng, toast nguyên văn `Problem while sending the statement` → TC **dự kiến FAIL cho tới khi Dev sửa** — ứng viên bug (`AMB-CUS-18` ✅) | 🟡 | 11-11-2026 · `AMB-CUS-18` | Kiểm chứng thực tế (hiện trạng) + User chốt `AMB-CUS-18` theo Assumption tạm |
| REQ-CUS-122 | Gửi sao kê thành công | Tác tạo của REQ-CUS-120 | KH có contact nhận email → chọn → `Send` → contact nhận email kèm PDF sao kê. **Chưa kiểm** — không gửi email trên môi trường dùng chung, chỉ kiểm giao diện (`AMB-CUS-25` ⏭️) | ⚪ | 11-11-2026 · ✏️ biên tập | Chưa kiểm chứng |

## 2. Thông báo

| REQ ID | Trigger | Nội dung nguyên văn |
|---|---|---|
| REQ-CUS-120 | KH không có contact nhận thông báo Invoice | `Failed to auto select customer contacts. Make sure that the customer has active contacts and associated contacts with email notifications for Invoices enabled.` |
| REQ-CUS-117 | Period From > To | **Mong đợi:** có thông báo lỗi — nguyên văn chờ `AMB-CUS-28`. Hiện trạng: không báo |
| REQ-CUS-121 | Gửi không có người nhận | **Mong đợi:** `This field is required.` ở ô `Email to`. Hiện trạng: toast `Problem while sending the statement` |

## 3. Luồng chính

Hồ sơ KH → `Statement` → chọn khoảng (hoặc `Period` + 2 ngày) → xem → `Print` / `View PDF` / email → chọn contact → `Send`.
