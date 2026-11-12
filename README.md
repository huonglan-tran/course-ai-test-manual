# Kiểm thử Perfex CRM (demo) – module Login & Customers

Dự án kiểm thử thủ công (manual testing) trên bản demo Perfex CRM, khu Admin, có dùng AI Agent (Claude Code + Playwright MCP) để hỗ trợ phân tích, viết test case và chạy test. Mọi kết quả của AI đều được tester xem lại trước khi kết luận.

**Thời gian:** 10/2026 – 11/2026 · **Vai trò:** Manual Tester

## Kết quả chính

| | Login · Forgot Password | Customers | Tổng |
|---|---|---|---|
| Requirement | 68 | 141 | **209** |
| Test case | 83 | 169 | **252** |
| Requirement có test case | 68/68 | 141/141 | **100%** |
| Kết quả chạy gần nhất | 49 Pass · 4 Fail | 127 Pass · 19 Fail | **176 Pass · 23 Fail** |

Các test case còn lại bị Blocked/Skip. Lý do chính: tính năng khoá tài khoản chưa có trên bản demo, không có hộp thư test, công cụ chạy test chỉ có Chrome.

## Nên xem trước

- **Bug report:** [Danh mục bug](docs/bugs/README.md)
  - [Forgot Password để lộ email nào có tài khoản](docs/bugs/login/web/BUG_login_1794476046_TC028.md) (Critical)
  - [Tạo được khách hàng không có tên](docs/bugs/customers/web/BUG_customers_1794476634_TC042.md) (Major)
  - [Tên khách hàng bị cắt bớt khi lưu mà không báo lỗi](docs/bugs/customers/web/BUG_customers_1794476635_TC044.md) (Minor)
- **Ma trận truy vết (RTM):** [Tổng hợp](docs/traceability/traceability_matrix.md) · [Login](docs/traceability/login/traceability_matrix_login.md) · [Customers](docs/traceability/customers/traceability_matrix_customers.md)
- **Test case:** [Login](docs/testcases/login/TEST_CASES_LOGIN_SUMMARY.md) · [Customers](docs/testcases/customers/TEST_CASES_CUSTOMERS_SUMMARY.md)
- **Requirement:** [Login](docs/requirements/login/REQUIREMENTS_LOGIN_SUMMARY.md) · [Customers](docs/requirements/customers/REQUIREMENTS_CUSTOMERS_SUMMARY.md)
- **Đánh giá tác động ticket CRM-LOGIN-101 (khoá tài khoản):** [Impact Report](docs/requirements/login/impact/impact_CRM-LOGIN-101.md)
- **Báo cáo thực thi:** [Login](docs/executions/login/web/run_1794453634/execution_report.md) · [Customers](docs/executions/customers/web/run_1794391198/execution_report.md)
- **Checklist:** [Smoke – Customers](docs/checklists/customers/web/checklist_smoke_20261112.md) · [Regression – Login](docs/checklists/login/web/checklist_regression_20261112.md)

## Cách làm

1. Khảo sát hệ thống đang chạy → viết requirement, ghi lại các điểm chưa rõ và làm rõ trước khi viết test case.
2. Thiết kế test case theo Risk-Based Testing, Equivalence Partitioning, Boundary Value Analysis, chia 4 vòng: Smoke → Functional → Technical (bảo mật cơ bản SQLi/XSS, phiên đăng nhập) → Non-functional (responsive, bàn phím).
3. Chạy test bằng AI Agent trên trình duyệt thật, chụp ảnh evidence; tester xem lại từng Fail để tách lỗi sản phẩm khỏi lỗi do test case.
4. Log bug có bước tái hiện, evidence, severity/priority; theo dõi coverage bằng RTM.

## Cấu trúc thư mục

```
docs/
  requirements/   requirement theo module, điểm chưa rõ, impact report
  testcases/      test case theo module
  executions/     báo cáo từng lần chạy + ảnh evidence
  bugs/           bug report
  checklists/     checklist smoke / regression
  traceability/   ma trận truy vết RTM
prompts/, scripts/  prompt và công cụ xem tài liệu dùng trong quá trình làm
```

> Tài khoản đăng nhập để trong `.env` (không đưa lên git). Tài liệu không ghi mật khẩu thật.
