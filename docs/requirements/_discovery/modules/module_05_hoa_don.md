# Module 05 — Invoices (`INV`)

> ← [Bản đồ hệ thống](../system_map.md) · Trạng thái recon: [`README.md`](../../README.md) · Tầng khám phá — **không** chứa mã REQ.

| Thuộc tính | Giá trị |
|---|---|
| Tên UI | Invoices (menu Sales) |
| Route | Danh sách `/admin/invoices` · tạo mới `/admin/invoices/invoice` · chi tiết chia đôi `/admin/invoices/list_invoices/{id}` |
| Loại màn hình | Danh sách + Form + chi tiết chia đôi (danh sách trái, chứng từ phải) |
| Bộ đếm trạng thái | `Unpaid` · `Paid` · `Partially Paid` · `Overdue` · `Draft` (dạng `n / tổng` + %) |
| Nút thanh công cụ | `Create New Invoice` · `Batch Payments` · `Recurring Invoices` |
| Cột bảng danh sách | `Invoice #` · `Amount` · `Total Tax` · `Year` · `Date` · `Customer` · `Project` · `Tags` · `Due Date` · `Status` |
| Form tạo mới | 53 field nhập (đếm DOM, gồm dòng hàng) |
| CRUD | Tạo · Xem · Sửa · (Xoá chưa thấy) · hoá đơn định kỳ · thanh toán hàng loạt |
| Status flow | ✅ Có — 5 trạng thái trên bộ đếm; chuyển trạng thái do thanh toán (`PAY`) và hạn (`Overdue`). Widget **Invoice overview** trên Dashboard có thêm `Not Sent`; báo cáo Sales ghi *"Cancelled invoices are excluded from the report"* → ⚠️ có trạng thái `Cancelled` chưa thấy trên bộ đếm |
| Phụ thuộc | `CUS` · `PRJ` · `ITEM` · thuế/tiền tệ/phương thức thanh toán (Setup) → sinh `PAY`; nhận `CRN` |
| Risk | 🔴 Cao — tiền; status flow phụ thuộc thanh toán và ngày; tính thuế, giảm giá, điều chỉnh; định kỳ |
| Ước REQ | 50–60 |

## Vùng chưa xác minh

| Vùng | Lý do |
|---|---|
| Quy tắc đánh số `Invoice #`, công thức tổng/thuế/giảm giá | Chưa submit |
| Luồng Recurring Invoices · Batch Payments | Chưa mở |
| Gửi email hoá đơn · PDF | Kiểm tầng tích hợp ❔ |
| Hoá đơn sinh từ Estimate/Proposal/Expense | ⚠️ Suy diễn — xem system_map mục 4.2 |

## Evidence

> Ảnh chụp 12-10-2026 (Playwright MCP, viewport 1600×780), đã mở lại xác nhận.

| Ảnh | Màn hình | Trạng thái chụp | Phạm vi |
|---|---|---|---|
| [invoices_list_viewport.png](../evidence/invoices_list_viewport.png) | `/admin/invoices` | Danh sách 53 bản ghi · nút Create New Invoice · Batch Payments · Recurring Invoices · Filter by status | Viewport — tên KH trong ảnh là dữ liệu test tự sinh (`AT Auto …`) |
