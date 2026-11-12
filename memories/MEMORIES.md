# MEMORIES.md — Bản đồ bộ nhớ của repo

> **Bộ nhớ dự án nằm TRONG repo, chia nhỏ theo phần/module.** File này là **bản đồ**: nó chỉ đường tới đúng file cần đọc, không chứa nội dung chi tiết.
> Cập nhật lần cuối: **12-11-2026**.

---

## 1. Quy trình bắt buộc khi nhận BẤT KỲ yêu cầu nào

```
1. Đọc CLAUDE.md                      ← luật chung (đã nạp tự động)
2. Đọc memories/MEMORIES.md           ← file này
3. Tra bảng mục 2 → đọc ĐÚNG các file bộ nhớ của tác vụ (không đọc hết)
4. Đọc docs/ theo chuỗi 3 tầng của CLAUDE.md mục 6b
   (README → system_map → file index của đúng module)
5. Làm việc
6. Xong: ghi lại quyết định/phát hiện mới vào bộ nhớ (mục 4)
```

**Vì sao thứ tự này:** `CLAUDE.md` = luật · `memories/` = **bối cảnh + quyết định của user + bẫy đã gặp** (thứ không suy ra được từ code/tài liệu) · `docs/` = **nguồn sự thật cho số liệu**. Bộ nhớ nói *"đã quyết gì, vì sao, đừng làm lại"*; `docs/` nói *"hiện có những gì"*. Hai nơi lệch nhau về số liệu (REQ, TC, mã kế tiếp) → **tin `docs/`**, rồi sửa bộ nhớ.

---

## 2. Tra theo tác vụ — đọc file nào

Cột **Luôn đọc** áp dụng cho mọi tác vụ. Các cột sau cộng thêm theo nhu cầu.

**Luôn đọc (3 file, ~vài trăm dòng):**
[`he-thong/boi-canh-perfex-crm.md`](he-thong/boi-canh-perfex-crm.md) · [`quy-uoc/cach-lam-viec-voi-user.md`](quy-uoc/cach-lam-viec-voi-user.md) · [`quy-uoc/dat-ten-ma-dinh-danh-cau-truc.md`](quy-uoc/dat-ten-ma-dinh-danh-cau-truc.md)

| Khi user yêu cầu… | Đọc thêm |
|---|---|
| `/discover-system` · khảo sát hệ thống · thêm module / cổng Customer / khu Setup | [`module/chua-khao-sat.md`](module/chua-khao-sat.md) · [`moi-truong-may/cong-cu-va-cau-hinh.md`](moi-truong-may/cong-cu-va-cau-hinh.md) |
| `/generate-requirements-from-*` · `/update-requirements-from-ticket` · `/analyze-requirement-document` | file module trong [`module/`](module/) · [`he-thong/tai-khoan-va-bi-mat.md`](he-thong/tai-khoan-va-bi-mat.md) (khi cần đăng nhập recon) |
| Sinh / sửa / review test case · `/update-testcases-from-impact` · `/review-testcases` | file module trong [`module/`](module/) · [`quy-uoc/chay-test-moi-truong-dung-chung.md`](quy-uoc/chay-test-moi-truong-dung-chung.md) (ý nghĩa tag, luật xoá dữ liệu) |
| `/generate-checklist-test` | [`quy-uoc/checklist-va-execution.md`](quy-uoc/checklist-va-execution.md) · file module |
| `/execute-test-cases` (bộ TC **hoặc** checklist) · `/retest-fixed-bugs` | [`quy-uoc/chay-test-moi-truong-dung-chung.md`](quy-uoc/chay-test-moi-truong-dung-chung.md) · [`quy-uoc/checklist-va-execution.md`](quy-uoc/checklist-va-execution.md) · [`he-thong/tai-khoan-va-bi-mat.md`](he-thong/tai-khoan-va-bi-mat.md) · file module |
| `/create-bug-report` · làm việc với `docs/bugs/` | file module (mục Bug / ứng viên bug) · [`quy-uoc/cach-lam-viec-voi-user.md`](quy-uoc/cach-lam-viec-voi-user.md) — **user dặn không tự sinh bug** |
| Sinh / sửa / chạy **automation** (web · mobile · API) · `/run-and-fix-tests` | [`moi-truong-may/cong-cu-va-cau-hinh.md`](moi-truong-may/cong-cu-va-cau-hinh.md) · [`quy-uoc/chay-test-moi-truong-dung-chung.md`](quy-uoc/chay-test-moi-truong-dung-chung.md) · [`he-thong/tai-khoan-va-bi-mat.md`](he-thong/tai-khoan-va-bi-mat.md) · file module |
| RTM · báo cáo tiến độ · báo cáo tổng hợp · test plan | file module của các module liên quan · [`lich-su/nhat-ky-quyet-dinh.md`](lich-su/nhat-ky-quyet-dinh.md) |
| Hỏi "đã làm tới đâu / vì sao quyết vậy" | [`lich-su/nhat-ky-quyet-dinh.md`](lich-su/nhat-ky-quyet-dinh.md) |
| Làm việc với module `<X>` đã có tài liệu | `module/<x>.md` (hiện: `login`, `customers`) |

---

## 3. Cây thư mục

```
memories/
├── MEMORIES.md                          ← BẢN ĐỒ (file này)
├── he-thong/                            ← nền tảng cấp hệ thống
│   ├── boi-canh-perfex-crm.md           · hệ thống · phạm vi · role · môi trường dùng chung · vùng bị chặn · năng lực QA
│   └── tai-khoan-va-bi-mat.md           · khoá .env · ai gõ mật khẩu · bẫy autofill/cookie
├── quy-uoc/                             ← quy ước & phản hồi của user
│   ├── dat-ten-ma-dinh-danh-cau-truc.md · tên module/file · prefix · REQ/TC/AMB · cấu trúc docs
│   ├── chay-test-moi-truong-dung-chung.md · tag @Skip/@KnownBug… · luật xoá dữ liệu · việc cấm chạy
│   ├── checklist-va-execution.md        · chạy checklist ≠ chạy TC · định dạng checklist/report
│   └── cach-lam-viec-voi-user.md        · "dừng là dừng" · không tự sinh bug · cách chốt AMB
├── module/                              ← mỗi module một file (không dồn chung)
│   ├── login.md                         · LOGIN — trạng thái · quyết định AMB · ticket CRM-LOGIN-101
│   ├── customers.md                     · CUS — trạng thái · quyết định AMB · ứng viên bug
│   └── chua-khao-sat.md                 · 20 module còn lại + Setup + cổng Customer
├── moi-truong-may/
│   └── cong-cu-va-cau-hinh.md           · Windows · không có Python · Playwright MCP · .gitignore · phần chỉ có ở local
└── lich-su/
    └── nhat-ky-quyet-dinh.md            · dòng thời gian 07-10 → 12-11-2026
```

> CV cá nhân **không** thuộc repo này — bộ nhớ riêng nằm ở `D:\HuongLan\CV\memories\`. Khi user nhờ làm/sửa CV thì làm việc ở thư mục đó, không dùng bộ nhớ của repo.

---

## 4. Quy tắc duy trì (để không nhiễu, không phình)

| Quy tắc | Chi tiết |
|---|---|
| **Một file = một chủ đề** | Chủ đề mới → file mới, **ghi vào bản đồ này ngay** (mục 2 và mục 3). File mồ côi (không có trong bản đồ) coi như không tồn tại |
| **Module mới → file mới** | Module vừa có tài liệu đầu tiên thì tạo `module/<module>.md` theo mẫu `login.md`, gạch khỏi `module/chua-khao-sat.md` |
| **Giữ file ≤ ~120 dòng** | Phình quá thì tách theo chủ đề con (VD `module/customers.md` → `module/customers/`), để lại link ở file cũ |
| **Chỉ ghi thứ không suy ra được** | Quyết định của user, lý do, bẫy đã gặp, trạng thái đang dở. Không chép lại mã/số liệu đã có ở `docs/` — chỉ trỏ link |
| **Sai thì sửa/xoá, không chồng** | Phát hiện bộ nhớ lệch với `docs/`/code → sửa tại chỗ, ghi `updated:` ở frontmatter. Quyết định bị user đảo → ghi cả "trước đây X, nay Y" (như `AMB-LOGIN-02`) |
| **Ghi sau mỗi lượt có quyết định mới** | Thêm 1 dòng vào `lich-su/nhat-ky-quyet-dinh.md` + cập nhật file chủ đề + file module |
| 🔒 **Không ghi bí mật / dữ liệu cá nhân** | Không mật khẩu, token, cookie, session id; không thông tin cá nhân của user. `memories/` **được commit** (không nằm trong `.gitignore`) |
| **Ngày** | `DD-MM-YYYY`, như `docs/` |
| **Khung mỗi file** | Frontmatter `name` · `description` · `type` (`project`/`feedback`/`reference`) · `updated`; thân ngắn, có **lý do** đi kèm quy tắc |

---

## 5. Nguồn đã gom vào bộ nhớ này (12-11-2026)

- Bộ nhớ tự động trong máy (`~/.claude/projects/<project>/memory/`) — chỉ có 1 mục "tên module giữ tiếng Anh" → đã chuyển vào `quy-uoc/dat-ten-ma-dinh-danh-cau-truc.md`. Bản gốc ở máy giữ nguyên (không xoá).
- Toàn bộ lời user nói trong ~20 phiên làm việc (07-10 → 12-11-2026): quyết định, sửa lỗi, lệnh dừng.
- Danh mục & index trong `docs/` (requirements · testcases · bugs · executions · checklists) đối chiếu đến 12-11-2026.
- `CLAUDE.md`, `.mcp.json`, `.gitignore`, `.claude/settings.json`, lịch sử git.

> Những gì **không** gom: nội dung chi tiết REQ/TC (đã ở `docs/`), mật khẩu/`.env`. Phần làm CV cá nhân đã tách sang `D:\HuongLan\CV\memories\` (12-11-2026).
