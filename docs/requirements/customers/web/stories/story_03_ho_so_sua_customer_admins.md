# STORY-CUS-03 — Hồ sơ KH, sửa, Customer Admins

> ← [Index module](../../REQUIREMENTS_CUSTOMERS_SUMMARY.md) · [File web](../requirements_customers_web.md) · REQ `REQ-CUS-55` → `65` (11 REQ). AMB/RISK, phân quyền, trạng thái: xem index.

**User story:** Là staff, tôi muốn xem và sửa hồ sơ khách hàng, và chỉ định staff phụ trách (Customer Admin) cho khách hàng đó.

## 1. Yêu cầu chức năng

| REQ ID | Tên yêu cầu | Mô tả | Acceptance Criteria | Trạng thái | Cập nhật lần cuối | Nguồn |
|---|---|---|---|---|---|---|
| REQ-CUS-55 | Hồ sơ KH có menu 19 tab | `/admin/clients/client/<id>` | Menu trái theo thứ tự, `href` dạng `…/<id>?group=<khoá>`: Profile (`profile`) · Contacts (`contacts`) · Notes (`notes`) · Statement (`statement`) · Invoices · Payments · Proposals · Credit Notes (`credit_notes`) · Estimates · Subscriptions · Expenses · Contracts · Projects · Tasks · Tickets · Files (**`attachments`**) · Vault (`vault`) · Reminders · Map (`map`) | 🟢 | — | Kiểm chứng thực tế (DOM `href`) |
| REQ-CUS-56 | Tiêu đề hồ sơ `#<id> <Company>` kèm menu chỉ có Delete | — | Tiêu đề trái `#<id> <Company>` (tên dài bị cắt `…`; Company rỗng → chỉ `#<id>`); mũi tên cạnh tiêu đề mở menu có **duy nhất** 1 mục `Delete` → `/admin/clients/delete/<id>` | 🟢 | — | Kiểm chứng thực tế |
| REQ-CUS-57 | Tab Profile có 3 tab con | — | `Customer Details` (`#contact_info`) · `Billing & Shipping` (`#billing_and_shipping`) · `Customer Admins` (`#customer_admins`) — tab thứ 3 **không** có trên form tạo mới | 🟢 | — | Kiểm chứng thực tế |
| REQ-CUS-58 | Form sửa chỉ có nút Save | — | Chân form sửa chỉ có `Save` — **không** có `Save and create contact` | 🟢 | — | Kiểm chứng thực tế |
| REQ-CUS-59 | Sửa KH lưu đủ field | — | Sửa Company, VAT, Phone, Website, Address, City, State, Zip, Country, Currency, Default Language → `Save` → toast nguyên văn `Customer updated successfully.`, URL giữ `/admin/clients/client/<id>`, 11 field mang giá trị mới | 🟢 | — | Kiểm chứng thực tế |
| REQ-CUS-60 | Tiêu đề tab trình duyệt của hồ sơ là tên Company | — | `document.title` = Company đã lưu (đổi theo lần sửa); Company rỗng → `Perfex CRM \| Anh Tester Demo` | 🟢 | — | Kiểm chứng thực tế |
| REQ-CUS-61 | Bảng Customer Admins | Tab con Customer Admins | Nút `Assign Admin`; bảng cột `Staff Member` · `Date Assigned` · `Options`, có page size + `Export` + ô tìm; chưa gán ai → `No entries found` | 🟢 | — | Kiểm chứng thực tế |
| REQ-CUS-62 | Modal Assign Admin chọn nhiều staff | — | `Assign Admin` → modal `Assign Admin`, 1 multi-select `customer_admins[]` liệt kê staff (11-11-2026: 3 staff — biến động), nút `Close` · `Save`; form gửi `/admin/clients/assign_admins/<id>` | 🟢 | — | Kiểm chứng thực tế |
| REQ-CUS-63 | Gán Customer Admin | — | Chọn 1 staff → `Save` → toast `Customer updated successfully.`, URL `…/<id>?tab=customer_admins`, bảng có dòng: tên staff (link `/admin/profile/<staff_id>`) · Date Assigned `DD-MM-YYYY HH:mm:ss` · icon xoá | 🟢 | — | Kiểm chứng thực tế |
| REQ-CUS-64 | Gỡ Customer Admin | — | Icon xoá (`/admin/clients/delete_customer_admin/<id>/<staff_id>`) → confirm `Are you sure you want to perform this action?` → OK → bảng về `No entries found` (không có toast) | 🟢 | — | Kiểm chứng thực tế |
| REQ-CUS-65 | Customer Admin có tác dụng ở bộ lọc "Customers assigned to me" | Tác tạo của REQ-CUS-63 (skill 4.3.8) | Gán chính staff đang đăng nhập làm admin của KH test → lọc rule `Customers assigned to me` → KH test có trong kết quả. **Chưa kiểm** — tác dụng chốt theo `AMB-CUS-27` ✅ | ⚪ | 11-11-2026 · ✏️ biên tập | Chưa kiểm chứng |

## 2. Đặc tả trường dữ liệu

Form sửa dùng **cùng field** với form tạo (Story 02 mục 2) — đã kiểm chứng trên form sửa ở REQ-CUS-59. Thêm:

| Field | Loại UI · `name` | Ràng buộc | REQ |
|---|---|---|---|
| Assign Admin — staff | multi-select · `customer_admins[]` | Mặc định `Nothing selected` | 62 · 63 |

## 3. Thông báo

| REQ ID | Trigger | Nội dung nguyên văn |
|---|---|---|
| REQ-CUS-59 · 63 | Lưu hồ sơ / gán admin | `Customer updated successfully.` |
| REQ-CUS-64 | Gỡ admin | Confirm: `Are you sure you want to perform this action?` |

## 4. Luồng chính

Hồ sơ KH → Profile → sửa → `Save` · Customer Admins → `Assign Admin` → chọn staff → `Save` → icon xoá để gỡ.
