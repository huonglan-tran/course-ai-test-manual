# Danh mục Test Cases — Perfex CRM (khu Admin)

> **Điểm vào của nhánh test case.** Đọc file này trước khi sinh / cập nhật TC của bất kỳ module nào: module nào đã có bộ TC · prefix TC ID nào đã bị chiếm · TC ID kế tiếp · độ phủ so với requirements.
>
> Danh mục requirements: [`../requirements/README.md`](../requirements/README.md)

## 1. Bảng danh mục module

| # | Module | Prefix TC ID | Nền tảng | Index | Số TC | Biến thể | Số part | Độ hạt | Dải TC ID | TC kế tiếp | Cập nhật |
|---|---|---|---|---|---|---|---|---|---|---|---|
| 1 | Login · Forgot Password | `CRM_LOGIN_TC_` | Web 83 | [`login/TEST_CASES_LOGIN_SUMMARY.md`](login/TEST_CASES_LOGIN_SUMMARY.md) | 83 | 133 | 4 | GỘP | `001` → `083` | `CRM_LOGIN_TC_084` | 23-10-2026 |
| 2 | Customers | `CRM_CUS_TC_` | Web 169 | [`customers/TEST_CASES_CUSTOMERS_SUMMARY.md`](customers/TEST_CASES_CUSTOMERS_SUMMARY.md) | 169 | 272 | 6 | GỘP | `001` → `169` | `CRM_CUS_TC_170` | 11-11-2026 |

**Prefix TC ID đã chiếm:** `CRM_LOGIN_TC_` · `CRM_CUS_TC_`

> Tiền tố hệ thống `CRM_` chốt 12-10-2026 (`docs/requirements/README.md`). TC ID dạng `CRM_<PREFIX MODULE>_TC_<3 số>` — prefix module lấy đúng prefix REQ của module. Dải TC ID dùng chung mọi nền tảng của module, **không** đánh lại từ `001`.

## 2. Độ phủ so với requirements

| Module | REQ trong requirements | REQ có TC | REQ ngoài phạm vi viết TC | 🔴 Thiếu | TC đang bị chặn chạy |
|---|---|---|---|---|---|
| `LOGIN` | 68 | 68 | 0 | 0 | 29 (`@Skip` 28 · `@PersonalOnly` 1 không kèm `@Skip`) |
| `CUS` | 141 | 141 | 0 | 0 | 15 (`@Skip` 14 · `@PersonalOnly` 1). Thêm 10 `@KnownBug` dự kiến FAIL (9 REQ 🟡) |
| 20 module còn lại | — | — | — | — | Chưa có requirements |

## 3. Cấu trúc thư mục chuẩn

```
docs/testcases/
├── README.md                                   ← DANH MỤC (file này)
└── <module>/
    ├── TEST_CASES_<TÊN_MODULE>_SUMMARY.md      ← INDEX — TÊN FILE BẤT BIẾN · không chứa dòng TC
    ├── web/test_cases_<module>_web.md          ← TC web (≤ 50 TC ở độ hạt GỘP)
    ├── web/parts/part_NN_web_<slug>.md         ← khi file web vượt ngưỡng
    ├── mobile/ · api/                          ← khi module có nền tảng đó
    ├── impact/                                 ← chế độ DELTA theo ticket
    └── review/                                 ← /review-testcases
```

## 4. Kết quả thực thi

Chưa có lần chạy nào. Kết quả `/execute-test-cases` ghi vào `docs/executions/<module>/<nền-tảng>/run_<timestamp>/execution_report.md`.

## 5. Quy trình sử dụng

| Tình huống | Workflow |
|---|---|
| Module mới có requirements, chưa có TC | `/generate-testcases-from-requirements` (QUICK) · `/generate-testcases-manual-rbt` (FULL RBT) |
| Requirements đổi theo ticket, module đã có TC | `/update-testcases-from-impact` — sửa tại chỗ, giữ TC ID |
| REQ mới chưa có TC trên module đã có bộ TC | Sinh lại bằng QUICK / FULL RBT → tự rẽ sang **chế độ BỔ SUNG**, TC ID nối tiếp |
| Chạy tay bộ TC | `/execute-test-cases` |
| Review chất lượng / chấm khả năng automation | `/review-testcases` (REVIEW · FIX · AUTOMATION) |
| Sinh automation | `/generate-automation-from-testcases` |

## 6. Nhật ký danh mục

| Ngày | Thay đổi | Nguồn |
|---|---|---|
| 20-10-2026 | Khởi tạo danh mục. `LOGIN`: bộ TC web đầu tiên — 65 TC · 115 biến thể · 3 part · phủ 50/50 REQ · chiếm prefix `CRM_LOGIN_TC_` | `/generate-testcases-from-requirements` (QUICK · GỘP) |
| 23-10-2026 | `LOGIN`: áp ticket `CRM-LOGIN-101` — sửa tại chỗ `CRM_LOGIN_TC_036` (REQ-46 🟡 khoá sau 5 lần sai). Số TC giữ 65. Requirements lên 68 REQ → phủ 50/68, REQ-51 → 68 chờ sinh bổ sung | `/update-testcases-from-impact` (APPLY) |
| 23-10-2026 | `LOGIN`: bổ sung 18 TC (`CRM_LOGIN_TC_066` → `083`) cho REQ-51 → 68 vào part mới `part_04_web_khoa_tai_khoan.md` — 83 TC · 133 biến thể · 4 part · phủ 68/68 REQ. Toàn bộ TC mới `@Skip` (tính năng chưa triển khai) | `/generate-testcases-from-requirements` (QUICK · BỔ SUNG · GỘP) |
| 11-11-2026 | `CUS`: bộ TC web đầu tiên — 169 TC · 272 biến thể · 6 part · phủ 141/141 REQ (requirements bản 2.1) · chiếm prefix `CRM_CUS_TC_`. 14 `@Skip` · 10 `@KnownBug` · 41 `@NeedsVerify` | `/generate-testcases-from-requirements` (QUICK · GỘP) |
