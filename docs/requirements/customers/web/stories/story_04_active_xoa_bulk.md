# STORY-CUS-04 — Active, xoá KH, Bulk Actions

> ← [Index module](../../REQUIREMENTS_CUSTOMERS_SUMMARY.md) · [File web](../requirements_customers_web.md) · REQ `REQ-CUS-66` → `82` · `139` · `140` (19 REQ). AMB/RISK, phân quyền, trạng thái: xem index.

**User story:** Là staff, tôi muốn ngừng/kích hoạt lại khách hàng, xoá khách hàng và xử lý hàng loạt (xoá, gán nhóm) để giữ danh sách gọn.

## 1. Yêu cầu chức năng

| REQ ID | Tên yêu cầu | Mô tả | Acceptance Criteria | Trạng thái | Cập nhật lần cuối | Nguồn |
|---|---|---|---|---|---|---|
| REQ-CUS-66 | Tắt Active ngay trên danh sách | Switch cột Active | Bấm switch của KH đang Active → gửi `GET /admin/clients/change_client_status/<id>/0` (200), switch tắt, **không** có toast; tải lại → switch vẫn tắt | 🟢 | — | Kiểm chứng thực tế + API |
| REQ-CUS-67 | Hồ sơ KH Inactive có banner cảnh báo | — | Mở hồ sơ KH Inactive → banner `alert-warning` nguyên văn `This is inactive customer profile and some features may be disabled` + link `Mark as active` → `/admin/clients/mark_as_active/<id>` | 🟢 | — | Kiểm chứng thực tế |
| REQ-CUS-68 | Mark as active kích hoạt lại KH | — | Bấm `Mark as active` → về hồ sơ, banner biến mất, **không** có toast; danh sách: switch Active bật | 🟢 | — | Kiểm chứng thực tế |
| REQ-CUS-69 | KH Inactive không có trong ô chọn khách hàng của Invoice | Theo tooltip REQ-CUS-17 | 2 KH Active + 1 KH Inactive cùng tiền tố tên → `/admin/invoices/invoice` → ô `Customer` gõ tiền tố → kết quả (`POST /admin/misc/get_relation_data`) chỉ có 2 KH Active. Form tạo chứng từ khác: REQ-CUS-139 · chứng từ đã lập: REQ-CUS-140 (`AMB-CUS-06` ✅) | 🟢 | 11-11-2026 · ✏️ biên tập | Kiểm chứng thực tế |
| REQ-CUS-70 | Xoá KH phải xác nhận | Link `Delete` ở dòng danh sách / menu tiêu đề hồ sơ | Bấm `Delete` → hộp `confirm()` với nội dung nguyên văn `Are you sure you want to perform this action?`; bấm Cancel → ở lại trang, KH vẫn tồn tại | 🟢 | — | Kiểm chứng thực tế |
| REQ-CUS-71 | Xoá KH thành công | — | Confirm OK → chuyển `/admin/clients`, toast nguyên văn `Customer deleted` | 🟢 | — | Kiểm chứng thực tế |
| REQ-CUS-72 | Hồ sơ KH đã xoá trả 404 | — | Mở lại `/admin/clients/client/<id>` của KH vừa xoá → HTTP 404, tiêu đề `404 Page Not Found`, nội dung `The page you requested was not found.` | 🟢 | — | Kiểm chứng thực tế |
| REQ-CUS-73 | Xoá KH đang có chứng từ | — | KH có hoá đơn/dự án/hợp đồng → Delete → hệ thống **chặn** kèm thông báo, KH và chứng từ giữ nguyên (không xoá dây chuyền); nguyên văn thông báo chưa biết. **Chưa kiểm** — không xoá dữ liệu có chứng từ trên môi trường dùng chung | ⚪ | 11-11-2026 · ✏️ biên tập | Chưa kiểm chứng — hành vi chốt theo `AMB-CUS-07` ✅ |
| REQ-CUS-74 | Chọn tất cả dòng đang hiển thị | Checkbox tiêu đề `#mass_select_all` | Lọc còn N dòng → tick `#mass_select_all` → N checkbox dòng đều `checked` (đo N = 2 và 3) | 🟢 | — | Kiểm chứng thực tế |
| REQ-CUS-75 | Modal Bulk Actions | Nút `Bulk Actions` | Modal `Bulk Actions` (`#customers_bulk_action`): checkbox `Mass Delete` (`#mass_delete`, mặc định không tick) · multi-select nhóm `move_to_groups_customers_bulk[]` (cùng danh sách nhóm ở REQ-CUS-40) · nút `Close` · `Confirm` | 🟢 | — | Kiểm chứng thực tế |
| REQ-CUS-76 | Tick Mass Delete ẩn ô chọn nhóm | — | Tick `Mass Delete` → ô chọn nhóm không còn hiển thị (`offsetParent = null`), modal chỉ còn checkbox + 2 nút | 🟢 | — | Kiểm chứng thực tế |
| REQ-CUS-77 | Confirm Bulk Actions phải xác nhận | — | Bấm `Confirm` → hộp `confirm()` nguyên văn `Are you sure you want to perform this action?` | 🟢 | — | Kiểm chứng thực tế |
| REQ-CUS-78 | Mass Delete xoá các KH đã chọn | — | Lọc về 3 KH test → chọn tất cả → Bulk Actions → tick Mass Delete → Confirm → OK → `POST /admin/clients/bulk_action`, toast nguyên văn `Total customers deleted: 3`; tìm lại → `0 entries` | 🟢 | — | Kiểm chứng thực tế |
| REQ-CUS-79 | Mass Delete khi chưa chọn KH nào | — | Không tick dòng nào → Bulk Actions → Mass Delete → Confirm → OK → toast `Total customers deleted: 0`, Total Customers không đổi. Hành vi được chấp nhận, không cần chặn sớm (`AMB-CUS-08` ✅) | 🟢 | 11-11-2026 · ✏️ biên tập | Kiểm chứng thực tế |
| REQ-CUS-80 | Bulk gán nhóm cho KH đã chọn | — | Chọn 2 KH → Bulk Actions (không tick Mass Delete) → chọn nhóm G → Confirm → OK → cả 2 dòng hiện nhãn G ở cột Groups; **không** có toast | 🟢 | — | Kiểm chứng thực tế |
| REQ-CUS-81 | Bulk gán nhóm ghi đè nhóm cũ | — | KH đang thuộc nhóm A → Bulk chọn nhóm B → Confirm → cột Groups chỉ còn B (A bị gỡ). Cố ý — Bulk là "chuyển sang nhóm", không phải "thêm nhóm" (`AMB-CUS-09` ✅) | 🟢 | 11-11-2026 · ✏️ biên tập | Kiểm chứng thực tế |
| REQ-CUS-82 | Bulk Confirm không chọn nhóm không được gỡ nhóm của KH âm thầm | Chống mất phân nhóm hàng loạt do bấm nhầm | KH đang thuộc nhóm A → chọn KH → Bulk Actions, **không** tick Mass Delete, **không** chọn nhóm → Confirm → OK → hệ thống **chặn** (cột Groups vẫn là A) **hoặc** hiện cảnh báo riêng nói rõ sẽ gỡ toàn bộ nhóm trước khi thực hiện — hộp `confirm()` chung của REQ-CUS-77 **không** tính là cảnh báo. Hình thức và nguyên văn: `AMB-CUS-28`. ⚠️ **Hiện trạng 11-11-2026 lệch:** cột Groups của KH trống, không cảnh báo → TC **dự kiến FAIL cho tới khi Dev sửa** — ứng viên bug (`AMB-CUS-10` ✅) | 🟡 | 11-11-2026 · `AMB-CUS-10` | Kiểm chứng thực tế (hiện trạng) + User chốt `AMB-CUS-10` theo Assumption tạm |
| REQ-CUS-139 | KH Inactive không có trong ô chọn khách hàng khi tạo mới các chứng từ khác | Mở rộng REQ-CUS-69 sang mọi form tạo mới — theo tooltip REQ-CUS-17 | 2 KH Active + 1 KH Inactive cùng tiền tố tên → mỗi form **tạo mới** chứng từ có ô chọn khách hàng (VD Estimate · Proposal · Project — danh sách form đầy đủ xác định khi recon từng module chứng từ) → ô chọn khách hàng gõ tiền tố → chỉ có 2 KH Active. **Chưa kiểm** ngoài Invoice (REQ-CUS-69) | ⚪ | — | User chốt `AMB-CUS-06` theo Assumption tạm (11-11-2026) — chưa kiểm chứng UI |
| REQ-CUS-140 | Chứng từ đã lập trước khi KH chuyển Inactive giữ nguyên KH | Vế còn lại của `AMB-CUS-06` — ẩn chỉ áp form tạo mới | Lập Invoice cho KH Active → tắt Active của KH → mở sửa Invoice đó → ô Customer vẫn hiện KH đã gán, `Save` không đổi KH và không báo lỗi. **Chưa kiểm** | ⚪ | — | User chốt `AMB-CUS-06` theo Assumption tạm (11-11-2026) — chưa kiểm chứng UI |

## 2. Thông báo

| REQ ID | Trigger | Nội dung nguyên văn |
|---|---|---|
| REQ-CUS-67 | Mở hồ sơ KH Inactive | `This is inactive customer profile and some features may be disabled` · link `Mark as active` |
| REQ-CUS-70 · 77 | Delete / Confirm Bulk | `Are you sure you want to perform this action?` (chuỗi do ứng dụng truyền vào `confirm()` gốc — khung hộp thoại thuộc trình duyệt) |
| REQ-CUS-71 | Xoá thành công | `Customer deleted` |
| REQ-CUS-78 · 79 | Mass Delete | `Total customers deleted: <n>` |
| REQ-CUS-72 | Hồ sơ đã xoá | `404 Page Not Found` · `The page you requested was not found.` |
| REQ-CUS-82 | Bulk Confirm không chọn nhóm, không tick Mass Delete | **Mong đợi:** chặn hoặc cảnh báo riêng — nguyên văn chờ `AMB-CUS-28`. Hiện trạng: không có thông báo nào ngoài `confirm()` chung |

## 3. Luồng chính

1. Danh sách → switch Active → KH ngừng hoạt động → hồ sơ có banner → `Mark as active`
2. `Delete` (dòng / menu hồ sơ) → confirm → KH bị xoá
3. **Lọc về đúng bản ghi cần xử lý** (`RISK-CUS-02`) → chọn tất cả → `Bulk Actions` → Mass Delete **hoặc** chọn nhóm → `Confirm` → confirm
