# Module 03 — Contacts (`CONT`)

> ← [Bản đồ hệ thống](../system_map.md) · Trạng thái recon: [`README.md`](../../README.md) · Tầng khám phá — **không** chứa mã REQ.

| Thuộc tính | Giá trị |
|---|---|
| Tên UI | Contacts |
| Route | Danh sách toàn hệ thống `/admin/clients/all_contacts` (nút `Contacts` trên trang Customers) · trong hồ sơ KH `/admin/clients/client/{id}?group=contacts` |
| Loại màn hình | Danh sách + modal (dự kiến thêm/sửa trong hồ sơ KH) |
| Cột bảng danh sách | `First Name` · `Last Name` · `Email` · `Company` · `Phone` · `Position` · `Last Login` · `Active` |
| CRUD | Xem danh sách ✅ · thêm/sửa/xoá: chưa mở (dự kiến trong tab Contacts của KH) |
| Status flow | Bật/tắt `Active` |
| Lý do tách khỏi `CUS` | Có vòng đời + CRUD riêng; là **danh tính đăng nhập cổng Customer** (cột `Last Login`) — user chốt tách 12-10-2026 |
| Risk | 🔴 Cao — tài khoản đăng nhập cổng KH (mật khẩu, quyền truy cập của contact); email là dữ liệu cá nhân |
| Ước REQ | 20–30 |

## Vùng chưa xác minh

| Vùng | Lý do |
|---|---|
| Form thêm contact (mật khẩu, primary contact, quyền theo từng loại chứng từ, gửi email đặt mật khẩu) | Chưa mở modal |
| Hành vi đăng nhập của contact | Thuộc cổng Customer — ngoài phạm vi đợt này |

## Evidence

> Ảnh chụp 12-10-2026 (Playwright MCP, viewport 1600×780), đã mở lại xác nhận.

| Ảnh | Màn hình | Trạng thái chụp | Phạm vi |
|---|---|---|---|
| [contacts_list_header_clip.png](../evidence/contacts_list_header_clip.png) | `/admin/clients/all_contacts` | Thanh Export · hàng tiêu đề 8 cột | Cắt tới hết hàng tiêu đề — **cố ý bỏ dòng dữ liệu** (email, điện thoại contact) |
