# Module 14 — Subscriptions (`SUB`)

> ← [Bản đồ hệ thống](../system_map.md) · Trạng thái recon: [`README.md`](../../README.md) · Tầng khám phá — **không** chứa mã REQ.

| Thuộc tính | Giá trị |
|---|---|
| Tên UI | Subscriptions |
| Route | Danh sách `/admin/subscriptions` · tạo mới `/admin/subscriptions/create` |
| Loại màn hình | Danh sách + Form |
| Bộ đếm trạng thái | `Not Subscribed` · `Active` · `Future` · `Past Due` · `Unpaid` · `Incomplete` · `Canceled` · `Incomplete Expired` (12-10-2026: tất cả = 0) |
| Nút thanh công cụ | `New Subscription` |
| Cột bảng danh sách | `#` · `Subscription Name` · `Customer` · `Project` · `Status` · `Next Billing Cycle` · `Date Subscribed` · `Last Sent` |
| Form tạo mới | 12 field nhập (đếm DOM) |
| Status flow | ✅ 8 trạng thái — trùng tên trạng thái của Stripe |
| Phụ thuộc | `CUS` · `PRJ` · ⚠️ cổng thanh toán Stripe (Settings — bị chặn, `AMB-SYS-04`) |
| Risk | 🟡 Trung bình — tiền định kỳ; phụ thuộc hệ thống ngoài; nhiều khả năng BLOCKED nếu chưa cấu hình Stripe |
| Ước REQ | 15–25 |

## Vùng chưa xác minh

| Vùng | Lý do |
|---|---|
| Form có yêu cầu chọn Stripe plan không | Chưa mở chi tiết field |
| Đăng ký của khách, chuyển trạng thái | Cổng Customer + Stripe — ngoài phạm vi |

## Evidence

> Ảnh chụp 12-10-2026 (Playwright MCP, viewport 1600×780), đã mở lại xác nhận.

| Ảnh | Màn hình | Trạng thái chụp | Phạm vi |
|---|---|---|---|
| [subscriptions_list_viewport.png](../evidence/subscriptions_list_viewport.png) | `/admin/subscriptions` | Subscriptions Summary 8 trạng thái = 0, **logo `stripe`** cạnh tiêu đề · danh sách **rỗng** | Viewport |
