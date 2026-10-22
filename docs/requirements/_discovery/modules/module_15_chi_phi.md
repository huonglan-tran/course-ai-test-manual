# Module 15 — Expenses (`EXP`)

> ← [Bản đồ hệ thống](../system_map.md) · Trạng thái recon: [`README.md`](../../README.md) · Tầng khám phá — **không** chứa mã REQ.

| Thuộc tính | Giá trị |
|---|---|
| Tên UI | Expenses |
| Route | Danh sách `/admin/expenses` · tạo mới `/admin/expenses/expense` |
| Loại màn hình | Danh sách + Form |
| Nút thanh công cụ | `Record Expense` · `Import Expenses` |
| Cột bảng danh sách | (checkbox) · `#` (**có trong DOM nhưng không hiển thị** trên UI 1600px — ảnh evidence) · `Category` · `Amount` · `Name` · `Receipt` · `Date` · `Project` · `Customer` · `Invoice` · `Reference #` · `Payment Mode` |
| Form tạo mới | 21 field nhập (đếm DOM) |
| Status flow | Không có cột Status — ⚠️ trạng thái "đã lập hoá đơn" suy từ cột `Invoice` |
| Phụ thuộc | Expense Categories · Payment Modes · Taxes (Setup — bị chặn) · `CUS` · `PRJ` · ⚠️ `INV` |
| Risk | 🟡 Trung bình — tiền; upload chứng từ (`Receipt`); chi phí định kỳ; lập hoá đơn cho KH |
| Ước REQ | 25–30 |

## Vùng chưa xác minh

| Vùng | Lý do |
|---|---|
| Upload receipt (định dạng, dung lượng) · chi phí định kỳ · Import | Chưa thao tác |
| Danh mục chi phí | Setup bị chặn |

## Evidence

> Ảnh chụp 12-10-2026 (Playwright MCP, viewport 1600×780), đã mở lại xác nhận.

| Ảnh | Màn hình | Trạng thái chụp | Phạm vi |
|---|---|---|---|
| [expenses_list_viewport.png](../evidence/expenses_list_viewport.png) | `/admin/expenses` | Danh sách 42 bản ghi · nút Record Expense · Import Expenses · có file `receipt.png` ở cột Receipt | Viewport — tên KH là dữ liệu test tự sinh |
