# STORY-CUS-05 — Import Customers

> ← [Index module `REQUIREMENTS_CUSTOMERS_SUMMARY.md`](../../REQUIREMENTS_CUSTOMERS_SUMMARY.md) · [File nền tảng Web](../requirements_customers_web.md)
>
> Phạm vi: `REQ-CUS-62` → `REQ-CUS-68` (7 REQ) · AMB liên quan: `AMB-CUS-09` · `10` · `19` · RISK liên quan: `RISK-CUS-05`

**Route:** `/admin/clients/import` (tiêu đề tab `Import`) · Form `#import_form` → `POST /admin/clients/import`.

## 1. Yêu cầu chức năng

| REQ ID | Tên yêu cầu | Mô tả | Acceptance Criteria | Trạng thái | Cập nhật lần cuối | Nguồn |
|---|---|---|---|---|---|---|
| REQ-CUS-62 | Trang Import hiển thị 4 hướng dẫn định dạng tệp | | Khối hướng dẫn đầu trang **chứa** đủ 4 câu nguyên văn: `1. Your CSV data should be in the format below. The first line of your CSV file should be the column headers as in the table example. Also make sure that your file is UTF-8 to avoid unnecessary encoding problems.` · `2. If the column you are trying to import is date make sure that is formatted in format Y-m-d (<YYYY-MM-DD hôm nay>).` · `3. Make sure you configure the default contact permission in Setup->Settings->Customers to get the best results like auto assigning contact permissions and email notification settings based on the permission.` · `4. Duplicate email rows won't be imported.` — phần ngày trong câu 2 là **ngày hiện tại**, không assert khớp tuyệt đối | 🟢 | — | UI thực tế |
| REQ-CUS-63 | Bảng mẫu có 27 cột, 4 cột bắt buộc | Định nghĩa cột CSV | Bảng mẫu có 27 tiêu đề theo thứ tự: `Firstname` · `Lastname` · `Email` · `Contact phonenumber` · `Position` (5 cột kèm chú thích `Contact field`) · `Company` · `Vat` · `Phonenumber` · `Country` · `City` · `Zip` · `State` · `Address` · `Website` · `Billing street` · `Billing city` · `Billing state` · `Billing zip` · `Billing country` · `Shipping street` · `Shipping city` · `Shipping state` · `Shipping zip` · `Shipping country` · `Longitude` · `Latitude` · `Stripe id`. Có dấu `*`: `Firstname` · `Lastname` · `Email` · `Company` | 🟢 | — | UI thực tế |
| REQ-CUS-64 | Nút Download Sample tải tệp CSV mẫu | | Nút `Download Sample` (`type=submit`) có trên trang. Chưa bấm — chưa kiểm tệp tải về | ⚪ | — | UI thực tế — chưa kiểm chứng |
| REQ-CUS-65 | Không chọn tệp thì không chạy Import/Simulate | | Không chọn tệp, bấm `Simulate Import` → không gửi `POST`; ô `* Choose CSV File` báo `p#file_csv-error` nguyên văn `This field is required.` | 🟢 | — | Kiểm chứng thực tế |
| REQ-CUS-66 | Simulate Import hiển thị dữ liệu đọc được mà không tạo KH | Xem trước trước khi import | Chọn CSV 2 dòng (Company `auto_cus_imp_<ts>`…) → `Simulate Import` → trang tải lại, có tiêu đề `Simulation Data` + chú thích `Max 100 rows are shown` + câu `If you are satisfied with the results upload the file again and click import.`; bảng hiển thị đúng 2 dòng; ô chọn tệp trống lại. Tìm Company `auto_cus_imp_<ts>` ở danh sách KH → **không** có bản ghi | 🟢 | — | Kiểm chứng thực tế |
| REQ-CUS-67 | Simulate Import không đánh dấu dòng thiếu cột bắt buộc | Hành vi **hiện tại** — nghi thiếu | Dòng CSV để trống `Email` (cột có `*`) vẫn hiển thị như dòng hợp lệ: không class/màu/biểu tượng lỗi trên `<tr>` hay `<td>`. TC viết theo kết luận `AMB-CUS-09` 🔴 | 🟢 | — | Kiểm chứng thực tế |
| REQ-CUS-68 | Import tạo KH và contact từ tệp CSV | | Không chạy Import thật — tạo contact và có thể gửi email trên môi trường dùng chung (`AMB-CUS-10`) | ⚪ | — | Chưa kiểm chứng |

## 2. Đặc tả trường dữ liệu

| Field (Label) | `name` | Loại UI | Required | Ràng buộc / mặc định | REQ liên quan | Ghi chú |
|---|---|---|---|---|---|---|
| Choose CSV File | `file_csv` | file | ✅ (`*`, JS) | `accept` **rỗng** — trình chọn tệp không lọc đuôi `.csv` | 65 · 66 | Chưa thử tệp không phải CSV |
| Groups | `groups_in[]` | multi-select | — | Không chọn | — | Gán nhóm cho KH import |
| Default password for all contacts | `default_pass_all` | **text** | — | Rỗng | — | Mật khẩu hiển thị rõ khi gõ — `RISK-CUS-05` |
| Nút Import / Simulate Import | — | `button[type=button]` | — | Class `btn-import-submit` (Simulate thêm class `simulate`) | 65 · 66 | Submit bằng JS |

## 3. Validation & thông báo

| REQ ID | Rule / Trigger | Thông báo (nguyên văn) |
|---|---|---|
| REQ-CUS-65 | Chưa chọn tệp | `This field is required.` |
| REQ-CUS-66 | Simulate xong | `Simulation Data` · `Max 100 rows are shown` · `If you are satisfied with the results upload the file again and click import.` |
| REQ-CUS-67 | Dòng thiếu Email | *(không có thông báo)* |

> Quan sát phụ: cột `Country` của bảng Simulation hiển thị `0` khi ô Country trong CSV để trống (`AMB-CUS-19`).

## 4. Luồng xử lý

```
Import Customers ─→ chọn CSV ─→ Simulate Import ─→ bảng Simulation Data (không ghi DB)
                              └→ Import ─→ (chưa kiểm — AMB-CUS-10)
```
