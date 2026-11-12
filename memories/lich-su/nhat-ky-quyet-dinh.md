---
name: nhat-ky-quyet-dinh
description: Dòng thời gian dự án 07-10 → 12-11-2026 — việc đã làm, quyết định của user theo ngày; dùng để biết "đã đi tới đâu và vì sao"
type: project
updated: 12-11-2026
---

# Nhật ký quyết định (theo ngày)

> Chỉ ghi **quyết định và cột mốc**. Chi tiết nghiệp vụ nằm ở [`../module/`](../module/). Mỗi lần có quyết định mới của user: thêm **một dòng** ở cuối bảng.

| Ngày | Sự kiện / quyết định |
|---|---|
| 07-10-2026 | Bắt đầu: user nhờ sinh SRS cho Login từ UI `crm.anhtester.com` → file MD trong repo (commit `Create SRS_Login.md`) |
| 08-10-2026 | Giai đoạn prompt tay: viết TC Login dạng bảng (Role/Context/Constraints/Output) → `docs/testcase/TCs_<module>_<ngày>.md`, rồi thêm ma trận truy vết vào file TC |
| 09-10-2026 | Bộ framework `.claude/` (commands · skills · rules), `CLAUDE.md`, `plans/`, `prompts/` về repo — chuyển sang quy trình workflow có cấu trúc `docs/` |
| 12-10-2026 | `/discover-system` mode UI: **22 module, 22 prefix**, tiền tố TC `CRM_`, môi trường dùng chung, năng lực QA. Đổi prefix `CN→CRN`, `KB→KBASE`. User: giữ **tên module tiếng Anh**; slug file về mục 6.5. User nhắc tạo `.mcp.json` cho Playwright MCP. Recon `LOGIN` (43 REQ). User chỉ ra thiếu trang Forgot Password → mode ADD |
| 12-10-2026 | User yêu cầu rồi **huỷ** việc gộp file Login/bỏ thư mục `web/` |
| 19-10-2026 | User trả lời AMB Login: chỉ role Admin; contact bị từ chối như email không tồn tại; thông báo Forgot Password mới; còn lại theo Assumption tạm. `AMB-LOGIN-21` chốt |
| 20-10-2026 | Sinh TC Login QUICK-GỘP (65 TC). Lập review nhưng **chưa commit**, user mở review trước |
| 21-10-2026 | Chạy TC Login lần đầu (tester đăng nhập thủ công); bug `TC005` (logo) — chờ PO. Ticket `CRM-LOGIN-101` (khoá tài khoản) tới |
| 22-10-2026 | Phân tích ticket → REQ-51→68, `AMB-LOGIN-22→33`. User chốt AMB; Dev **chưa deploy** tính năng khoá; cấp tài khoản Project Manager (cùng role Admin) |
| 23-10-2026 | Áp delta TC (sửa 036), bổ sung 18 TC `066→083` (user hỏi và đồng ý gộp kiểu GỘP); lưu `EMAIL_PM`/`PASSWORD_PM` vào `.env`; chạy thử một số TC khoá |
| 03→11-11-2026 | (Ngoài repo) Làm CV từ khoá học — bộ nhớ đã tách sang `D:\HuongLan\CV\memories\` |
| 04-11-2026 | Recon `CUS` lần 1 (101 REQ, bản 1.0) |
| 11-11-2026 | **Khảo sát lại `CUS` từ đầu, đánh lại mã REQ từ 01** (138→141 REQ). User chốt AMB-CUS-01→27. Sinh TC Customers 169 TC. Review TC (TB 11.86/12). User bảo "Chạy Mode FIX" → rồi ngắt → "Khôi phục bản gốc" → chỉ áp 2 sửa (156-d, 021). Chạy toàn bộ bộ TC Customers (8 phiên, 19 FAIL) |
| 12-11-2026 | Chạy toàn bộ TC Login (83 TC). Sinh checklist Customers Smoke + Login (đổi sang Regression, một file). Chạy cả hai checklist. User chốt định dạng cột "Trạng thái", ký hiệu ✅❌⚠️⏭️. "Không sinh bug", "TC đã review rồi ngưng" |
| 12-11-2026 | User yêu cầu gom bộ nhớ vào `memories/` + `MEMORIES.md` làm map, và `CLAUDE.md` bắt buộc đọc map trước khi làm việc — chính là bộ file này |
| 12-11-2026 | Sinh RTM module Login (mục đích: báo cáo coverage trước release) → `docs/traceability/login/traceability_matrix_login.md`: REQ 68/68 có TC · automation 0/83 (repo chưa có script) · 35/68 REQ đã kiểm chứng đủ · 29 REQ chưa có bằng chứng chạy (19 là `CRM-LOGIN-101` chưa build). Chưa có RTM toàn hệ thống / `CUS` |
| 12-11-2026 | User so RTM Login với repo `claude-testing-skills-new` → lệch do **skill `skills-coverage-traceability` ở repo này là bản cũ (09-10-2026, 173 dòng)**, repo kia bản 05-11-2026 (320 dòng: tóm tắt, thanh tiến độ, nhóm Story, Việc cần làm, ẩn cột/bảng rỗng). Viết lại `traceability_matrix_login.md` theo mẫu mới, giữ số liệu. Sau đó user tự chép skill bản mới sang (`skills-coverage-traceability` + `generate-traceability-matrix` giống hệt repo kia, đã bỏ `.claude/commands/generate-traceability-matrix.md`) — RTM lần sau ra đúng mẫu mới |
| 12-11-2026 | Sinh RTM module Customers (mục đích: báo cáo coverage trước release) → `docs/traceability/customers/traceability_matrix_customers.md` + **lần đầu** tạo file chung `docs/traceability/traceability_matrix.md` (LOGIN + CUS). CUS: REQ 141/141 có TC · automation 0/169 · 99/141 REQ mọi TC PASS · 17 REQ có TC FAIL (19 TC, chưa bug nào) · 22 REQ chưa có kết quả. Pass rate RTM tính PASS/(PASS+FAIL) = 87%, khác 77.4% của execution report (tính cả BLOCKED). Không có REQ ⏭️ (index TC không có bảng "REQ cố ý không có TC") |
| 12-11-2026 | User: **bỏ cột `Checklist` khỏi mọi bảng RTM** — sinh lại RTM Customers; sửa skill `skills-coverage-traceability` (Mapping Rule 5, Quy tắc trình bày, cột ma trận, Quality Checklist) + `generate-traceability-matrix` (bước tự kiểm). Checklist vẫn là nguồn phụ để xét 🟠 và ghi ở ô `Nguồn đã quét` |
| 12-11-2026 | User: gắn kết quả execution **từng TC** vào RTM Customers → thêm mục `## 4. Kết quả thực thi từng TC` (bảng đếm theo mức ưu tiên + danh sách 169 TC: REQ · kết quả · bước fail · ghi chú · link ảnh evidence trong `executions/`). Không đổi cột ma trận REQ (giữ ô đếm để viewer đọc được). Chưa đưa quy tắc này vào skill |
| 12-11-2026 | User yêu cầu `/create-bug-report CRM_LOGIN_TC_015` → tái hiện 015-b (email `@auto` không dấu chấm: trình duyệt cho qua, máy chủ báo `The Email Address field must contain a valid email address.`; đối chứng `@auto.test` ra `Invalid email or password`) → `BUG_login_1794475498_TC015` 🟡 Minor P3, chờ PO chốt (a) chấp nhận → sửa TC / (b) lỗi → Dev đồng bộ quy tắc. Ảnh tái hiện lưu ở `run_1794453634/evidence/`. `docs/bugs/README.md` tạo lại (bản cũ có TC005 đã bị xoá trước phiên, không khôi phục) |
| 12-11-2026 | User yêu cầu `/create-bug-report CRM_LOGIN_TC_028` → tái hiện (email `auto_bug_tc028_1794476037@auto.test` → `Email not found`, 6/6 lần tính cả run cũ) → `BUG_login_1794476046_TC028` 🔴 Critical (theo bảng: lỗi bảo mật, có ghi chú có thể hạ Major) · P2. Lúc này phát hiện file bug `TC015` + `docs/bugs/README.md` vừa tạo đã bị xoá khỏi máy → tạo lại README chỉ với TC028, **không** tự khôi phục TC015 |
| 12-11-2026 | User yêu cầu `/create-bug-report CRM_CUS_TC_042, CRM_CUS_TC_044` → tái hiện trên demo (KH #16004 tên rỗng · KH #16005 Company 193 ký tự lưu còn 191), **đã xoá cả 2 KH** (404) → `BUG_customers_1794476634_TC042` 🟠 Major P2 · `BUG_customers_1794476635_TC044` 🟡 Minor P3; thêm 2 dòng vào `docs/bugs/README.md` |
