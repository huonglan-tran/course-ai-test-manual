# STORY-CUS-04 — Trạng thái Active, Bulk Actions & xoá KH

> ← [Index module `REQUIREMENTS_CUSTOMERS_SUMMARY.md`](../../REQUIREMENTS_CUSTOMERS_SUMMARY.md) · [File nền tảng Web](../requirements_customers_web.md)
>
> Phạm vi: `REQ-CUS-53` → `REQ-CUS-61` (9 REQ) · AMB liên quan: `AMB-CUS-05` · `06` · `07` · `08` · RISK liên quan: `RISK-CUS-03` · `RISK-CUS-06`

## 1. Yêu cầu chức năng

| REQ ID | Tên yêu cầu | Mô tả | Acceptance Criteria | Trạng thái | Cập nhật lần cuối | Nguồn |
|---|---|---|---|---|---|---|
| REQ-CUS-53 | Tắt switch Active trên danh sách lưu trạng thái không hoạt động | | Tìm KH tự tạo (Active đang bật) → bấm switch → `GET /admin/clients/change_client_status/<id>/0?csrf_token_name=<token>` trả `200`; switch bỏ `checked`; tải lại trang và tìm lại → switch vẫn **không** `checked` | 🟢 | — | Kiểm chứng thực tế · API `GET .../change_client_status/<id>/0` · 200 |
| REQ-CUS-54 | Switch Active có tooltip giải thích hệ quả | | Bọc ngoài switch (`.onoffswitch`) có `data-title` nguyên văn `Won't be shown in dropdowns when creating new records` | 🟢 | — | UI thực tế |
| REQ-CUS-55 | KH không hoạt động không xuất hiện trong dropdown chọn KH khi tạo chứng từ | Hệ quả của REQ-CUS-54 | Chưa kiểm chứng được — dropdown KH ở form tạo Invoice nạp kiểu ajax, phép thử tự động không bắt được kết quả (`AMB-CUS-05`) | ⚪ | — | UI thực tế (tooltip) — chưa kiểm chứng |
| REQ-CUS-56 | Modal Bulk Actions có Mass Delete và chuyển nhóm | | Bấm `Bulk Actions` → modal `#customers_bulk_action` tiêu đề `Bulk Actions`: checkbox `Mass Delete` (`#mass_delete`, không tick) · multi-select `Groups` (`move_to_groups_customers_bulk[]`); nút `Close` · `Confirm` | 🟢 | — | Kiểm chứng thực tế |
| REQ-CUS-57 | Tick Mass Delete ẩn ô Groups | Tránh vừa xoá vừa chuyển nhóm | Tick `Mass Delete` → khối chứa ô Groups mang class `hide` (`display:none`); bỏ tick → ô Groups hiện lại | 🟢 | — | Kiểm chứng thực tế |
| REQ-CUS-58 | Mass Delete xoá đúng các KH đã chọn kèm thông báo số lượng | | Lọc danh sách chỉ còn N KH tự tạo → tick checkbox chọn dòng (`value=<id>`) của từng KH → `Bulk Actions` → tick `Mass Delete` → `Confirm` → hộp `confirm` → đồng ý → thông báo nguyên văn `Total customers deleted: <N>` (đo: `3`); tìm lại → `No matching records found` | 🟢 | — | Kiểm chứng thực tế |
| REQ-CUS-59 | Bulk Actions chuyển các KH đã chọn vào nhóm | | Chưa thử — chưa biết thêm nhóm hay ghi đè nhóm cũ (`AMB-CUS-08`) | ⚪ | — | UI thực tế — chưa kiểm chứng |
| REQ-CUS-60 | Xoá một KH cần xác nhận rồi quay về danh sách | | Hồ sơ KH → mũi tên cạnh heading → `Delete` (href `/admin/clients/delete/<id>`, class `_delete`) → hộp `confirm` → đồng ý → về `/admin/clients`, thông báo nguyên văn `Customer deleted`; mở lại hồ sơ → 404 (REQ-CUS-47) | 🟢 | — | Kiểm chứng thực tế |
| REQ-CUS-61 | Xoá KH đang có chứng từ được xử lý theo quy tắc ràng buộc | | Không thử trên môi trường dùng chung (cần KH có hoá đơn/dự án). Chưa biết chặn hay xoá dây chuyền (`AMB-CUS-07` 🔴) | ⚪ | — | Chưa kiểm chứng |

## 2. Đặc tả trường dữ liệu

| Field (Label) | Loại UI | Required | Ràng buộc / mặc định | REQ liên quan |
|---|---|---|---|---|
| Mass Delete | checkbox `#mass_delete` | — | Không tick | 56 · 57 · 58 |
| Groups | `select#move_to_groups_customers_bulk[]` multiple | — | Không chọn; ẩn khi tick Mass Delete | 56 · 57 · 59 |

## 3. Validation & thông báo

| REQ ID | Rule / Trigger | Thông báo (nguyên văn) |
|---|---|---|
| REQ-CUS-58 | Mass Delete thành công | `Total customers deleted: <N>` |
| REQ-CUS-60 | Xoá 1 KH thành công | `Customer deleted` |
| REQ-CUS-58 · 60 | Trước khi xoá | `Are you sure you want to perform this action?` (hộp `confirm` gốc của trình duyệt) |
| — | Bấm `Confirm` ở Bulk Actions khi **chưa chọn dòng nào** | Vẫn hiện hộp `confirm` như trên — không có cảnh báo "chưa chọn KH" (`AMB-CUS-06`); agent đã chọn **huỷ** |

## 4. Luồng xử lý

```
Danh sách ─→ switch Active ─→ GET change_client_status/<id>/<0|1>
Danh sách ─→ tick dòng ─→ Bulk Actions ─→ [Mass Delete | Groups] ─→ Confirm ─→ confirm()
Hồ sơ ─→ ▾ ─→ Delete ─→ confirm() ─→ /admin/clients "Customer deleted"
```
