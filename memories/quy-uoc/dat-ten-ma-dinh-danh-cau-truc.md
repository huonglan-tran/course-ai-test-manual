---
name: dat-ten-ma-dinh-danh-cau-truc
description: Quy ước tên module/file, prefix REQ-TC, đánh số, cấu trúc thư mục đã chốt với user — kèm các lần user đổi ý để không lặp lại
type: feedback
updated: 12-11-2026
---

# Đặt tên · mã định danh · cấu trúc tài liệu

> Luật gốc ở `CLAUDE.md` mục 6b. File này ghi **các quyết định riêng của dự án này** và các lần user sửa lại.

## Tên module & tên file — user sửa 12-10-2026

- **Tên hiển thị** module (danh mục, bảng module, tiêu đề tài liệu) giữ **nguyên văn tiếng Anh như UI**: `Customers`, `Credit Notes`, `Knowledge Base`… Không dịch ("Khách hàng", "Ghi có"…). Nội dung giải thích vẫn tiếng Việt.
- **Tên file** `_discovery/modules/module_NN_<slug>.md` theo `.claude/commands/discover-system.md` **mục 6.5 = slug tiếng Việt không dấu** (VD `module_02_khach_hang.md`, `module_12_khach_hang_tiem_nang.md`).
- Lịch sử: user từng bảo đổi slug sang tiếng Anh (`module_12_leads.md`), vài phút sau bảo **hoàn tác về 6.5**. ⇒ **Đừng đổi slug sang tiếng Anh nữa** trừ khi user sửa chính mục 6.5.

## Thư mục & file index

- Thư mục module = tên module chữ thường không dấu (`login`, `customers`). File index **IN HOA theo tên thư mục**: `REQUIREMENTS_CUSTOMERS_SUMMARY.md`, `TEST_CASES_CUSTOMERS_SUMMARY.md` — **không** theo prefix (`CUS`).
- Luôn có tầng nền tảng `web/` (kể cả chưa có mobile/api). **User từng yêu cầu** (12-10-2026 08:28) gộp 2 file requirements Login thành `login/requirements_login.md`, bỏ thư mục `web/`, đưa evidence ra ngoài — **4 phút sau user bảo "dừng không thực hiện"** ⇒ giữ nguyên cấu trúc tầng nền tảng.
- Requirements chia theo Story: `CUS` có 8 file `stories/story_NN_*.md` vì 141 REQ vượt ngưỡng tách (80); `LOGIN` gom 1 file (68 REQ). User hỏi lý do (11-11-2026) và chấp nhận "giữ nguyên theo skill".
- TC web tách `parts/part_NN_web_<slug>.md` khi > 50 TC ở độ hạt GỘP (LOGIN 4 part, CUS 6 part).

## Mã

| Loại | Quy ước | Ghi chú dự án |
|---|---|---|
| Prefix REQ | Mỗi module một prefix **vĩnh viễn** | Đã đổi trước phát hành: `CN`→`CRN`, `KB`→`KBASE` (quy ước 3–6 ký tự). Danh sách: `docs/requirements/README.md` |
| REQ | `REQ-<PREFIX>-<nn>` | **Không** đánh lại từ 01 — *ngoại lệ có chủ ý:* `CUS` khảo sát lại 11-11-2026, user chọn đánh lại từ `REQ-CUS-01` ⇒ mã `REQ-CUS-nn` bản 04-11 **khác nghĩa** bản 11-11; TC/automation theo bản cũ (tra git) phải map lại |
| TC ID | `CRM_<PREFIX>_TC_<nnn>` | Dải dùng chung mọi nền tảng, không đánh lại; sửa TC tại chỗ, **không đổi ID**; gỡ TC thì tag `@Deprecated` |
| AMB / RISK | `AMB-<MODULE>-<nn>` / `RISK-<MODULE>-<nn>` | Đánh số riêng từng module; cấp hệ thống `AMB-SYS-nn` / `RISK-SYS-nn` |
| Checklist | `CL-<phần tên file sau checklist_>-<nn>` | VD `CL-regression_20261112-08` |
| Bug | `BUG_<module>_<timestamp>_<TC_ID>` ở `docs/bugs/<module>/<nền-tảng>/` | |
| Run | `docs/executions/<module>/web/run_<timestamp>/` | `<timestamp>` = epoch giây (VD `run_1794453634`) |

## Dữ liệu test traceable

- Mẫu: `auto_cus_tc<NNN>_<YYYYMMDD>` (TC), `auto_cus_smoke_<ngày chạy>_<số mục>` (checklist). Khi chạy **thay ngày chạy thực** vào `YYYYMMDD` (`ASM-CUS-02`).
- Email lỗi: `…@auto.test`.

## Ngày tháng

- Trong tài liệu: `DD-MM-YYYY` (VD `12-11-2026`), có giờ thì `12-11-2026 14:10`. Tên file giữ dạng cũ (`_20261112`). Năm hiện hành của dự án: **2026**.
