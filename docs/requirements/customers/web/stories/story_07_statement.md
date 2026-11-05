# STORY-CUS-07 — Statement (sao kê công nợ)

> ← [Index module `REQUIREMENTS_CUSTOMERS_SUMMARY.md`](../../REQUIREMENTS_CUSTOMERS_SUMMARY.md) · [File nền tảng Web](../requirements_customers_web.md)
>
> Phạm vi: `REQ-CUS-80` → `REQ-CUS-87` (8 REQ) · AMB liên quan: `AMB-CUS-14` · RISK liên quan: —

**Route:** `?group=statement` · PDF: `/admin/clients/statement_pdf?customer_id=<id>&from=<DD-MM-YYYY>&to=<DD-MM-YYYY>` (in: thêm `&print=true`).

## 1. Yêu cầu chức năng

| REQ ID | Tên yêu cầu | Mô tả | Acceptance Criteria | Trạng thái | Cập nhật lần cuối | Nguồn |
|---|---|---|---|---|---|---|
| REQ-CUS-80 | Ô chọn kỳ sao kê có 7 lựa chọn, mặc định This Month | | `select#range` có 7 option theo thứ tự: `Today` · `This Week` · `This Month` · `Last Month` · `This Year` · `Last Year` · `Period`; `This Month` đang `selected`. 6 option đầu có value là mảng JSON `["<từ ngày>","<đến ngày>"]` dạng `DD-MM-YYYY` tính theo ngày hiện tại (VD This Month = ngày 01 → ngày cuối tháng); `Period` có value `period`. **Không** assert ngày cụ thể | 🟢 | — | UI thực tế |
| REQ-CUS-81 | Đổi kỳ cập nhật khoảng ngày của sao kê và liên kết PDF | | Chọn `Last Month` → href `#statement_pdf` đổi sang `from=<ngày 01 tháng trước>&to=<ngày cuối tháng trước>`; dòng dưới `Account Summary` hiển thị `<từ> To <đến>` cùng khoảng đó | 🟢 | — | Kiểm chứng thực tế |
| REQ-CUS-82 | Chọn Period hiện 2 ô chọn ngày | | Chọn `Period` → hiện 2 ô `input.datepicker` `#period-from` · `#period-to`, rỗng, mỗi ô có nút lịch | 🟢 | — | Kiểm chứng thực tế |
| REQ-CUS-83 | Sao kê hiển thị thông tin KH, tóm tắt tài khoản và bảng giao dịch | | Nội dung có tiêu đề **chứa** `Customer Statement For <tên KH>`; khối `To:` gồm tên, địa chỉ, `VAT Number: <vat>`; `Account Summary` gồm 4 dòng `Beginning Balance:` · `Invoiced Amount:` · `Amount Paid:` · `Balance Due:`; câu `Showing all invoices and payments between <từ> and <đến>`; bảng cột `Date` · `Details` · `Amount` · `Payments` · `Balance` với dòng đầu `Beginning Balance`; dòng cuối `Balance Due`. KH mới không có giao dịch → mọi số tiền `$0.00` / `0.00` | 🟢 | — | Kiểm chứng thực tế |
| REQ-CUS-84 | Nút Print và View PDF mang đúng mã KH và khoảng ngày | | Icon in `#statement_print` (tooltip `Print`) href **chứa** `statement_pdf?customer_id=<id>&from=<từ>&to=<đến>&print=true`; icon `#statement_pdf` (tooltip `View PDF`) cùng tham số, không có `print=true` | 🟢 | — | UI thực tế |
| REQ-CUS-85 | Tệp PDF sao kê mở được và đúng nội dung | Tác tạo phải dùng được (skill 4.3.8) | Chưa mở PDF / chưa in (`AMB-CUS-14`) | ⚪ | — | Chưa kiểm chứng |
| REQ-CUS-86 | Gửi sao kê cho KH không có contact báo không chọn được người nhận | | Icon email (`data-target=#statement_send_to_client`) → modal tiêu đề `Account Summary`, hiện thông báo đỏ nguyên văn `Failed to auto select customer contacts. Make sure that the customer has active contacts and associated contacts with email notifications for Invoices enabled.`; ô `Email to` (`send_to[]`) không có option; ô `CC`; khung soạn thư có mẫu email; nút `Close` · `Send` | 🟢 | — | Kiểm chứng thực tế |
| REQ-CUS-87 | Gửi sao kê qua email tới contact của KH | | Không gửi email thật trên môi trường dùng chung; KH test không có contact (`AMB-CUS-14`) | ⚪ | — | Chưa kiểm chứng |

## 2. Đặc tả trường dữ liệu

| Field (Label) | `name` | Loại UI | Required | Ràng buộc / mặc định | REQ liên quan |
|---|---|---|---|---|---|
| Kỳ | `range` | select | — | 7 option, mặc định `This Month` | 80 · 81 |
| Từ ngày / Đến ngày | `period-from` · `period-to` | text datepicker | — | Chỉ hiện khi chọn `Period` | 82 |
| Email to | `send_to[]` | multi-select | — | Option = contact của KH | 86 · 87 |
| CC | `cc` | text | — | Rỗng | 86 |

## 3. Validation & thông báo

| REQ ID | Rule / Trigger | Thông báo (nguyên văn) |
|---|---|---|
| REQ-CUS-86 | Mở modal gửi khi KH không có contact phù hợp | `Failed to auto select customer contacts. Make sure that the customer has active contacts and associated contacts with email notifications for Invoices enabled.` |
