---
name: boi-canh-perfex-crm
description: Hệ thống đang kiểm thử (Perfex CRM demo), phạm vi, role, môi trường dùng chung, vùng bị chặn, năng lực QA — đọc cho MỌI tác vụ
type: project
updated: 12-11-2026
---

# Bối cảnh hệ thống — Perfex CRM (khu Admin)

> Nguồn sự thật chi tiết: [`docs/requirements/README.md`](../../docs/requirements/README.md) · [`docs/requirements/_discovery/system_map.md`](../../docs/requirements/_discovery/system_map.md). File này chỉ giữ **bối cảnh + quyết định** để mở phiên là nắm được ngay.

## Hệ thống & phạm vi

- **Hệ thống:** Perfex CRM — bản demo công khai "Anh Tester Demo" (`https://crm.anhtester.com`, URL thật nằm ở `.env` → `BASE_URL`).
- **Phạm vi:** khu **Admin** (`/admin/...`), **chỉ web**. Chưa có mặt mobile / API.
- **Ngoài phạm vi đợt này:** cổng Customer/Contact (URL khác — user sẽ cung cấp sau, chạy `/discover-system` mode ADD; nghiệp vụ trùng module đã có thì **dùng lại prefix**).
- **22 module** đã có prefix (xem [`../module/chua-khao-sat.md`](../module/chua-khao-sat.md)); mới có tài liệu: `LOGIN`, `CUS`.
- **Thứ tự khảo sát đã chốt (12-10-2026):** `LOGIN → CUS → CONT → ITEM → INV → PAY → EST → PROP → CRN → PRJ → TASK → CTR → LEAD → TKT → SUB → EXP → RPT → DASH → PROF → KBASE → ESTR → UTIL` (module được phụ thuộc đi trước).
- **Tiền tố TC ID hệ thống:** `CRM_` → `CRM_<PREFIX_MODULE>_TC_<3 số>` — user chốt 12-10-2026, **không đổi giữa chừng**.

## Role & tài khoản

- **Hệ thống chỉ có role Admin** (user xác nhận 19-10-2026, `AMB-LOGIN-18`). Ma trận phân quyền không cần cột role khác; vẫn xét loại tài khoản **không phải role**: staff bị vô hiệu hoá, contact (cổng khách hàng).
- Tài khoản **Project Manager** (user cấp 22-10-2026, dùng cho TC khoá tài khoản) **cùng role Admin** (`AMB-LOGIN-33` ✅) — hệ thống vẫn chỉ có role Admin.
- Chi tiết cách dùng tài khoản: [`tai-khoan-va-bi-mat.md`](tai-khoan-va-bi-mat.md).

## Môi trường dùng chung — ảnh hưởng mọi TC

- ✅ **Dùng chung** (demo công khai, nhiều người cùng ghi). User cho phép submit (12-10-2026) → dữ liệu test **phải sinh động, traceable, và dọn sau khi tạo**.
- Dữ liệu nền **biến động** (11-11-2026: 2.942 KH, 317 nhóm; người khác tạo/xoá liên tục) → assert **chênh lệch** do chính test gây ra, không assert số tuyệt đối (`RISK-SYS-01`).
- Quy tắc chạy chi tiết: [`../quy-uoc/chay-test-moi-truong-dung-chung.md`](../quy-uoc/chay-test-moi-truong-dung-chung.md).

## Vùng bị chặn — `AMB-SYS-01` 🔴 (còn treo duy nhất cấp hệ thống)

- Tài khoản "Admin" trong `.env` bị **Access denied toàn bộ khu Setup** (26 route: Staff, Roles, Settings, Customer Groups, Currencies, Taxes, Payment Modes, Email Templates, Activity Log…). `/admin/modules` chuyển về Dashboard không kèm thông báo (`AMB-SYS-03`).
- Hệ quả: không đọc được role, không tạo/đổi master data (nhóm KH, thuế, tiền tệ…) → TC phải dùng master data **có sẵn**; Setup chưa cấp prefix. Cần tài khoản Admin đầy đủ từ PO/người quản trị môi trường rồi `/discover-system` mode ADD.
- Mâu thuẫn đang ghi nhận: "chỉ có role Admin" nhưng tài khoản Admin lại bị chặn Setup.

## Năng lực kiểm thử của QA (chốt 12-10-2026 — đầu vào Vòng 3 của mọi bộ TC)

| Năng lực | Trạng thái |
|---|---|
| Gọi API | ❔ chưa xác nhận → mặc định đội Dev xác minh |
| Truy vấn CSDL | ❔ chưa xác nhận → đội Dev xác minh |
| Kiểm tầng tích hợp (email, cron, cổng thanh toán) | ❔ chưa xác nhận → đội Dev xác minh |
| Xem nhật ký hoạt động | ❌ đã đo — `/admin/utilities/activity_log` Access denied |
| DevTools trình duyệt | ✅ có |

## Đặc điểm hệ thống cần nhớ khi viết locator / assert

- Giao diện đổi được **25 ngôn ngữ** theo người dùng (`RISK-SYS-03`) → ưu tiên locator theo `id` / `name` / `href`, không theo text tiếng Anh.
- Danh sách nạp bằng `POST /admin/<module>/table` (DataTables phía server).
- Mọi chuyển hướng xác thực quan sát được trả `307`; không assert mã chuyển hướng, assert URL đích (`AMB-LOGIN-09`).
- 5 module **đang rỗng** trên demo (12-10-2026): `CRN` `TKT` `SUB` `KBASE` `ESTR` → recon cấp module phải tự tạo dữ liệu traceable (`auto_<module>_<timestamp>`) và dọn (`RISK-SYS-05`).
- Hồ sơ KH có 19 tab, Project 18 tab — chứng từ Sales lặp lại trong tab KH/Project → REQ của danh sách chứng từ thuộc module chứng từ; tab trong KH/Project chỉ có REQ về **lọc theo KH/Project** (`RISK-SYS-04`).
- Subscriptions có 8 trạng thái kiểu Stripe, chưa biết đã cấu hình cổng chưa (`AMB-SYS-04`).
- Trạng thái lead `jjjj` coi là dữ liệu rác (`AMB-SYS-02`).
