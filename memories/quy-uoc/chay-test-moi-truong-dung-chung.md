---
name: chay-test-moi-truong-dung-chung
description: Luật khi chạy TC/checklist trên demo dùng chung — ý nghĩa tag, việc không được làm, quy tắc xoá dữ liệu user đã chốt, cấu hình trình duyệt khi chạy
type: feedback
updated: 12-11-2026
---

# Chạy test trên môi trường dùng chung

## Ý nghĩa tag khi `/execute-test-cases` chạy

| Tag | Cách chấm / xử lý |
|---|---|
| `@Skip` | Chấm **BLOCKED** (thiếu điều kiện: tài khoản, hộp thư, tính năng chưa build, không an toàn trên môi trường chung) |
| `@PersonalOnly` | Chấm **SKIPPED**, workflow bỏ qua — QA tự chạy tay (chờ thời gian thật / phải gõ mật khẩu thật) |
| `@KnownBug` | Viết theo hành vi **mong đợi** đã chốt ở AMB → **dự kiến FAIL** tới khi Dev sửa. FAIL → `/create-bug-report` *(chỉ khi user muốn — xem [`cach-lam-viec-voi-user.md`](cach-lam-viec-voi-user.md))* |
| `@NeedsVerify` | Chạy lần đầu = lượt xác nhận Expected. Lệch → **sửa TC**, **không** mở bug ngay |
| `@TechCheck` | Cần DevTools (QA có quyền); che dòng `🔧 Ghi chú kỹ thuật` thì TC vẫn chấm được |
| `@Smoke` | Chạy đầu tiên mỗi build. **Vòng 1 FAIL → dừng**, mọi TC Vòng 2 coi như BLOCKED |

## Luật xoá / ghi dữ liệu — user chốt 11-11-2026

- **TC_156-d** (chuỗi chèn SQL ở Company): **không** xoá toàn bộ khách hàng, chỉ thao tác lên khách hàng **do chính TC tạo**; có bước dọn đếm đúng 1 dòng.
- **TC_021** (Mass Delete): thêm chốt *"đúng 2 dòng, đúng tên KH test — khác thì dừng"* và **chỉ xoá khách hàng do chính TC tạo**, không xoá KH khác.
- Tổng quát (`RISK-CUS-02`): Mass Delete / Bulk Groups luôn **lọc về đúng bản ghi test tạo** (tên traceable), **đếm số dòng đã chọn trước khi Confirm**; khác kỳ vọng thì dừng. Auto-skip TC phá huỷ **luôn BẬT** trên môi trường dùng chung.
- Dọn dữ liệu bằng xoá **từng** KH sau khi kiểm đúng tên (đã làm ở checklist smoke Customers 12-11-2026). Tắt Active của KH test thì **bật lại** trong teardown (`RISK-CUS-08`).

## Không chạy trên demo (đã chốt, 11-11-2026)

- Import thật, `Simulate Import` ghi thật, `Download Sample`, Export/PDF/tải tệp (`AMB-CUS-12` ⏭️).
- Gửi email: sao kê, gửi tệp (`AMB-CUS-25` ⏭️) — chỉ kiểm giao diện.
- Xoá KH có chứng từ; tạo bộ lọc/nhóm KH dùng chung (`AMB-CUS-21`, `RISK-CUS-03`).
- Xem mật khẩu Vault → user tự chạy tay (`AMB-CUS-22`); 3 mức visibility Vault cần staff thứ hai → bỏ qua (`AMB-CUS-23` ⏭️).

## Trình duyệt & cách assert

- Playwright MCP, **headed**, Chrome, **context cách ly mới** (xem [`../he-thong/tai-khoan-va-bi-mat.md`](../he-thong/tai-khoan-va-bi-mat.md)); viewport thực tế các lần chạy 1600×750 – 1600×780. Không `browser_resize` vượt cửa sổ (`CLAUDE.md`).
- Assert **chênh lệch** bộ đếm trước/sau thao tác của chính test; chọn option theo `value`/tên duy nhất (nhóm KH có tên trùng, VD 12 lần `ctp group`).
- Toast tự tắt sau vài giây → assert/chụp ngay sau hành động.
- Khoảng ngày Statement (`This Month`/`Last Month`…) tính theo **ngày chạy** (`ASM-CUS-13`).
- TC có chờ thời gian thật phải chờ thật (TC 056 LOGIN chờ 5 phút); TC khoá 15 phút không chạy liền nhau.
- Evidence chụp **đúng phạm vi** (viewport, không full-page thừa) và **mở lại ảnh xác nhận** trước khi ghi vào danh mục (`CLAUDE.md` 6b).
- Ảnh trong `executions/` chụp hệ thống thật, có thể chứa dữ liệu khách → cẩn thận khi commit.
