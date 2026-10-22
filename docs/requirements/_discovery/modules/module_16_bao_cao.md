# Module 16 — Reports (`RPT`)

> ← [Bản đồ hệ thống](../system_map.md) · Trạng thái recon: [`README.md`](../../README.md) · Tầng khám phá — **không** chứa mã REQ.

| Thuộc tính | Giá trị |
|---|---|
| Tên UI | Reports (menu cha, 6 mục con) |
| Loại màn hình | Báo cáo — bảng tổng hợp, biểu đồ, bộ lọc |
| Status flow | Không |
| Phụ thuộc | Đọc dữ liệu của `INV` · `PAY` · `CRN` · `EST` · `PROP` · `ITEM` · `EXP` · `LEAD` · `TASK` · `KBASE` |
| Risk | 🟡 Trung bình — số liệu tổng hợp phải khớp chứng từ gốc; dữ liệu môi trường dùng chung biến động (`RISK-SYS-01`) khiến khó assert số tuyệt đối |
| Ước REQ | 20–30 |

## 6 báo cáo

| Báo cáo | Route | Quan sát (DOM 12-10-2026) |
|---|---|---|
| Sales | `/admin/reports/sales` | Trên UI: **Sales Report** 7 mục (Invoices · Items · Payments Received · Credit Notes · Proposals · Estimates · Customers Report) + **Charts Based Report** 3 mục (Total Income · Payment Modes (Transactions) · Total Value By Customer Groups) · cảnh báo *"Cancelled invoices are excluded from the report"*. Trong DOM — nhiều bảng con: theo khách hàng (`Customer` · `Total Invoices` · `Amount` · `Amount with Tax`), hoá đơn (`Discount` · `Adjustment` · `Applied Credits` · `Amount open`…), ghi có (`Remaining Amount` · `Refunded Amount`), hàng bán (`Item` · `Quantity Sold` · `Total Amount` · `Average Price`), báo giá |
| Expenses | `/admin/reports/expenses` | Bảng `Category` × 12 tháng + `Year (2026)` · nút `Detailed Report` |
| Expenses vs Income | `/admin/reports/expenses_vs_income` | Biểu đồ (không có bảng) |
| Leads | `/admin/reports/leads` | Nút `Switch to staff report` · ⚠️ tiêu đề tab chỉ là `Perfex CRM \| Anh Tester Demo` (thiếu tên báo cáo, khác các trang khác) |
| Timesheets overview | `/admin/staff/timesheets?view=all` | Cột: `Staff Member` · `Task` · `Timesheet Tags` · `Start Time` · `End Time` · `Note` · `Related` · `Time (h)` · `Time (decimal)` · nút `My Timesheets` · `Apply` · ⚠️ tiêu đề tab **rỗng** |
| KB Articles | `/admin/reports/knowledge_base_articles` | Báo cáo bài viết KB |

## Vùng chưa xác minh

| Vùng | Lý do |
|---|---|
| Bộ lọc kỳ báo cáo, tiền tệ, xuất file | Chưa thao tác |
| Đối chiếu số liệu báo cáo ↔ chứng từ | Việc của recon module |

## Evidence

> Ảnh chụp 12-10-2026 (Playwright MCP, viewport 1600×780), đã mở lại xác nhận.

| Ảnh | Màn hình | Trạng thái chụp | Phạm vi |
|---|---|---|---|
| [reports_sales_viewport.png](../evidence/reports_sales_viewport.png) | `/admin/reports/sales` | Danh sách loại báo cáo (dạng thu gọn) + cảnh báo đỏ | Viewport |
