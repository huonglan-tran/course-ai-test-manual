# STORY-CUS-05 — Import Customers

> ← [Index module](../../REQUIREMENTS_CUSTOMERS_SUMMARY.md) · [File web](../requirements_customers_web.md) · REQ `REQ-CUS-83` → `95` · `141` (14 REQ). AMB/RISK, phân quyền, trạng thái: xem index.

**User story:** Là staff, tôi muốn nhập hàng loạt khách hàng (kèm contact chính) từ file CSV, được chạy thử (Simulate) trước khi nhập thật.

## 1. Yêu cầu chức năng

| REQ ID | Tên yêu cầu | Mô tả | Acceptance Criteria | Trạng thái | Cập nhật lần cuối | Nguồn |
|---|---|---|---|---|---|---|
| REQ-CUS-83 | Trang Import có 4 hướng dẫn | `/admin/clients/import`, tiêu đề tab `Import` | Khung `alert-info` có 4 mục, mục 4 tô đỏ. Nội dung **chứa**: (1) `The first line of your CSV file should be the column headers as in the table example. Also make sure that your file is UTF-8` · (2) `formatted in format Y-m-d (<ngày hôm nay YYYY-MM-DD>)` — phần ngày là **động** · (3) `Setup->Settings->Customers` · (4) `Duplicate email rows won't be imported.` | 🟢 | — | Kiểm chứng thực tế |
| REQ-CUS-84 | Bảng mẫu 27 cột, 4 cột bắt buộc | — | Bảng mẫu có 27 cột theo thứ tự: `Firstname` · `Lastname` · `Email` · `Contact phonenumber` · `Position` (5 cột mang nhãn phụ `Contact field`) · `Company` · `Vat` · `Phonenumber` · `Country` · `City` · `Zip` · `State` · `Address` · `Website` · `Billing street/city/state/zip/country` · `Shipping street/city/state/zip/country` · `Longitude` · `Latitude` · `Stripe id`. Có dấu `*`: Firstname · Lastname · Email · Company. Dòng mẫu `Sample Data`; email mẫu dạng `<hex>@example.com` **đổi mỗi lần tải trang** | 🟢 | — | Kiểm chứng thực tế (DOM) |
| REQ-CUS-85 | Form Import | — | `* Choose CSV File` (`input[type=file]#file_csv`, không giới hạn `accept`) · `Groups` (multi-select + nút `+`) · `Default password for all contacts` (text) · nút `Import` · `Simulate Import`; góc phải nút `Download Sample` | 🟢 | — | Kiểm chứng thực tế (DOM) |
| REQ-CUS-86 | Chưa chọn file thì báo bắt buộc | — | Không chọn file → `Simulate Import` → `This field is required.` (`#file_csv-error`), không gửi request | 🟢 | — | Kiểm chứng thực tế |
| REQ-CUS-87 | File CSV rỗng báo không đủ dòng | — | Chọn file 0 byte → `Simulate Import` → toast **chứa** `rows for importing` (nguyên văn hiện tại `Not enought rows for importing` — lỗi chính tả "enought", ứng viên bug nhẹ; TC **chỉ** assert phần chứa để không đỏ khi Dev sửa — `AMB-CUS-13` ✅); không hiện bảng Simulation Data | 🟢 | 11-11-2026 · ✏️ biên tập | Kiểm chứng thực tế (DOM — toast tự tắt, không kịp chụp) |
| REQ-CUS-88 | Simulate hiển thị dữ liệu sẽ nhập | — | File CSV đúng tiêu đề, 5 dòng → `Simulate Import` → mục `Simulation Data` + chú thích `Max 100 rows are shown` + dòng `If you are satisfied with the results upload the file again and click import.` + bảng 27 cột với đủ 5 dòng | 🟢 | — | Kiểm chứng thực tế |
| REQ-CUS-89 | Simulate không tạo khách hàng | Khẳng định phủ định — phép thử trạng thái sạch | (1) Company trong CSV mang tiền tố `auto_imp_<YYYYMMDD>` chưa từng dùng → (2) Simulate → (3) danh sách KH tìm `auto_imp_` → `0 entries` | 🟢 | — | Kiểm chứng thực tế |
| REQ-CUS-90 | Simulate không đánh dấu dòng lỗi | Hành vi hiện tại — Simulate chỉ hiển thị, việc lọc dòng lỗi do Import thật làm (REQ-CUS-141) | 5 dòng: hợp lệ · thiếu Email · trùng Email với dòng 1 · Email `not-an-email` · thiếu Company → bảng Simulation Data hiện cả 5 dòng như nhau, không màu/nhãn/thông báo lỗi (`AMB-CUS-11` ✅) | 🟢 | 11-11-2026 · ✏️ biên tập | Kiểm chứng thực tế |
| REQ-CUS-91 | Simulate hiển thị Country = 0 khi để trống | — | Cột Country của CSV trống → cột Country trong Simulation Data hiện `0` — chỉ là cách hiển thị, không phải giá trị sẽ gán (`AMB-CUS-14` ✅) | 🟢 | 11-11-2026 · ✏️ biên tập | Kiểm chứng thực tế |
| REQ-CUS-92 | Sau Simulate phải chọn lại file | — | Sau khi trang Simulate tải lại, ô `Choose CSV File` trống (`files.length = 0`, `No file chosen`) — đúng với hướng dẫn "upload the file again" | 🟢 | — | Kiểm chứng thực tế |
| REQ-CUS-93 | Import thật tạo KH và contact chính | Tác tạo chính của Story | CSV hợp lệ → `Import` → mỗi dòng tạo 1 KH + 1 contact chính, gán vào Groups đã chọn, contact nhận mật khẩu mặc định nếu có nhập. **Chưa kiểm** — Import thật trên môi trường dùng chung chưa được phép, có thể gửi email (`AMB-CUS-12` ⏭️) | ⚪ | 11-11-2026 · ✏️ biên tập | Chưa kiểm chứng |
| REQ-CUS-94 | Dòng trùng email không được nhập | Theo hướng dẫn mục 4 (REQ-CUS-83) | CSV có 2 dòng cùng Email → Import → chỉ 1 dòng được nhập. **Chưa kiểm** — phụ thuộc REQ-CUS-93 | ⚪ | — | Chưa kiểm chứng |
| REQ-CUS-95 | Download Sample tải file CSV mẫu | — | `Download Sample` → tải 1 file CSV có dòng tiêu đề đúng 27 cột của REQ-CUS-84. **Chưa kiểm** — tải tệp chưa được phép (`AMB-CUS-12` ⏭️) | ⚪ | 11-11-2026 · ✏️ biên tập | Chưa kiểm chứng |
| REQ-CUS-141 | Import thật bỏ qua dòng lỗi và báo số dòng bị bỏ | Bổ sung REQ-CUS-94 (trùng email) cho các loại dòng lỗi khác | CSV 5 dòng như REQ-CUS-90 (hợp lệ · thiếu Email · trùng Email với dòng 1 · Email sai định dạng · thiếu Company) → `Import` → chỉ dòng hợp lệ tạo KH; 4 dòng lỗi **không** tạo KH nào; sau Import có thông báo cho biết **số dòng bị bỏ qua** (nguyên văn chưa biết). **Chưa kiểm** — Import thật chưa được phép (`AMB-CUS-12` ⏭️) | ⚪ | — | User chốt `AMB-CUS-11` theo Assumption tạm (11-11-2026) — chưa kiểm chứng UI |

## 2. Đặc tả trường dữ liệu

| Field | Loại UI · `name` | Required | Ràng buộc | REQ |
|---|---|---|---|---|
| Choose CSV File | file · `file_csv` | ✅ (client) | Không đặt `accept`; file rỗng bị từ chối ở server | 85 → 87 |
| Groups | multi-select · `groups_in[]` | — | Cùng danh sách nhóm KH | 85 · 93 |
| Default password for all contacts | text · `default_pass_all` | — | Ô `type=text` (mật khẩu hiện rõ khi gõ) | 85 · 93 |
| *(ẩn)* | `download_sample` · `clients_import` | — | Cờ phân biệt hành động | — |

## 3. Validation messages

| REQ ID | Rule / Trigger | Thông báo nguyên văn |
|---|---|---|
| REQ-CUS-86 | Chưa chọn file | `This field is required.` |
| REQ-CUS-87 | File CSV rỗng | `Not enought rows for importing` (hiện trạng — sai chính tả, `AMB-CUS-13`; TC assert chứa `rows for importing`) |

## 4. Luồng chính

`Import Customers` → chọn CSV → `Simulate Import` → xem bảng → chọn lại file → `Import`.

> 📎 File CSV dùng để khảo sát (5 dòng — hợp lệ, thiếu email, trùng email, email sai, thiếu company) lưu ngoài repo, chỉ chứa dữ liệu giả `auto_imp_*_20261111`.
