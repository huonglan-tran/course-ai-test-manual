---
name: cong-cu-va-cau-hinh
description: Máy Windows của user — công cụ có/không có, Playwright MCP, .gitignore, phần nào của repo chỉ nằm local, git đang dở
type: reference
updated: 12-11-2026
---

# Công cụ & cấu hình máy

- **Hệ điều hành:** Windows 11 Pro. Shell: PowerShell + Git Bash.
- **Python: KHÔNG cài** (lệnh `python` chỉ mở Microsoft Store) → script phụ trợ viết bằng **Node.js** (`node` v24.21 có sẵn). Dùng `npx`/`npm` cho Playwright/Allure.
- **Trình duyệt:** Google Chrome. Playwright MCP chạy headed; chỉ có Chromium/Chrome (không Edge/Firefox) nên TC đa trình duyệt chỉ kiểm được Chrome.

## Playwright MCP (`.mcp.json` ở gốc repo)

- User nhắc tới file này ngày 12-10-2026 (file hiện có mtime 09-10, nên có thể đã có từ bộ khung). Server `playwright` chạy `npx -y @playwright/mcp --viewport-size=1600,780 --config .claude/playwright-mcp-config.json`; đổi viewport phải **khởi động lại Claude Code**.
- ⚠️ **Lệch cần biết:** `CLAUDE.md` ghi viewport headed chuẩn **1600×770**, còn `.mcp.json` đang đặt **1600,780**; các report ghi 1600×750 – 1600×780. Chưa user nào yêu cầu thống nhất — không tự sửa, hỏi nếu cần đồng bộ.
- Evidence MCP rơi vào `.playwright-mcp/` của repo (đã `.gitignore`).

## `.gitignore` hiện tại (12-11-2026)

`reqs/` · `.env` · `reports/` · `.allure/` · `.playwright-mcp/` · **`.claude/`** · **`plans/`** · **`prompts/`**

- ⚠️ `.claude/` (commands, skills, rules, settings), `plans/`, `prompts/` đã **bị gỡ khỏi git** (commit `be370dd` "Stop tracking .claude", `c816e8a`/`a35b41a` update gitignore) → chúng **chỉ tồn tại trên máy này**, không có bản sao trên remote. Nhắc user sao lưu nếu họ định đổi máy.
- `memories/` **không** bị ignore → sẽ được commit cùng repo (vì vậy không chứa bí mật hay dữ liệu cá nhân).
- Muốn đẩy `docs/` lên repo công khai: ảnh trong `docs/executions/*/evidence/` chụp hệ thống thật (có thể có dữ liệu khách) — rà trước.

## Git

- Quy tắc `CLAUDE.md`: **không** `pull/checkout/merge/rebase/reset`; chỉ `status`/`diff`/`log`. Lịch sử git là nguồn tra bản cũ (VD `customers/` bản 04-11-2026). Không commit nếu user chưa bảo.
- Lịch sử commit gần đây: SRS Login → TC Login → update module Customer → các lần update `.gitignore`.
- Tại 12-11-2026 working tree có nhiều thay đổi chưa commit (xoá run cũ của Login, ảnh evidence Customers đổi/xoá, `docs/checklists/`, `docs/executions/customers/` mới) — chạy `git status` trước khi giả định trạng thái.

## Công cụ có sẵn trong repo

- Web viewer offline (double-click, không cài gì): `scripts/testcases-viewer/bundle.html` · `scripts/execution-viewer/bundle.html` (cả RTM) · `scripts/bugs-viewer/bundle.html`; còn `scripts/traceability-viewer/`, `scripts/user-guide/`.
- `plans/` (manual · automation · cross-module · master-test-plan) và `prompts/` (prompt_00 → prompt_32 `.txt`) — bộ prompt sẵn dùng; có `prompts/README.md`.
- Slash command & skill đều ở `.claude/commands/`, `.claude/skills/` (danh sách ở `CLAUDE.md` mục 5, 10).
- `.claude/settings.json` (repo): cho phép đọc project/`.env`, chạy npm/npx/playwright/maven; **cấm** xoá source không xác nhận, commit/push remote, cài global package.
