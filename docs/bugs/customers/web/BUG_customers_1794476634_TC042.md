# [BUG] Customers — Company chỉ gồm khoảng trắng vẫn tạo được khách hàng không có tên

## Thông tin chung
- **ID:** BUG_customers_1794476634_TC042
- **Severity:** 🟠 Major
- **Priority:** P2
- **Nền tảng:** `web`
- **Môi trường:** Demo dùng chung `https://crm.anhtester.com/admin/clients/client` | Chrome (Playwright MCP), headed | Windows 11 | viewport 1600×750
- **Build / Version:** Không có thông tin build trên demo — ghi nhận theo ngày chạy 11-11-2026 (`run_1794391198`), tái hiện 12-11-2026
- **Test case liên quan:** `CRM_CUS_TC_042-a` (3 dấu cách) · `CRM_CUS_TC_042-b` (1 dấu cách)
- **REQ ID liên quan:** `REQ-CUS-31` (cùng quy tắc với `REQ-CUS-30` — Company bỏ trống bị chặn) · `AMB-CUS-01` — user xác nhận **là lỗi, yêu cầu mở bug** (11-11-2026)
- **Test data đã dùng:** Company = 3 dấu cách / 1 dấu cách. KH bị tạo ra: id 15870, 15871 (`run_1794391198`), id 16004 (lần tái hiện) — **đã xoá cả 3** (mở lại hồ sơ trả 404)
- **Ngày phát hiện:** 11-11-2026

## Steps to Reproduce
1. Đăng nhập Admin, mở `/admin/clients/client` (New Customer)
2. Nhập Company = `   ` (3 dấu cách), để trống mọi ô khác
3. Bấm `Save`

## Expected Result
Theo `REQ-CUS-31`: khoảng trắng không phải tên — xử lý như bỏ trống Company:
- **Không** tạo khách hàng, trang vẫn ở `/admin/clients/client`, không có `Customer added successfully.`
- Dưới ô Company hiện `This field is required.`

## Actual Result
- Hiện thông báo `Customer added successfully.`, trang chuyển sang `/admin/clients/client/<id>` — **khách hàng mới được tạo**.
- Ô Company của khách hàng là chuỗi **rỗng** (khoảng trắng bị cắt khi lưu); tiêu đề hồ sơ chỉ còn `#<id>`, tiêu đề tab trình duyệt `Perfex CRM | Anh Tester Demo` (không có tên).
- Không có thông báo lỗi nào.

→ Danh sách khách hàng xuất hiện bản ghi **không có tên**, không biết là ai, không tìm được theo tên, và lọt vào các ô chọn khách hàng khi tạo hoá đơn / báo giá / dự án dưới dạng dòng trống.

## Evidence
- Screenshot lần chạy gốc: [CRM_CUS_TC_042_step3_tao_duoc_kh_ten_rong.png](../../../executions/customers/web/run_1794391198/evidence/CRM_CUS_TC_042_step3_tao_duoc_kh_ten_rong.png)
- Screenshot lần tái hiện (12-11-2026, KH #16004): [CRM_CUS_TC_042_tai_hien_bug.png](../../../executions/customers/web/run_1794391198/evidence/CRM_CUS_TC_042_tai_hien_bug.png)
- Kết quả kiểm DOM lúc tái hiện:

```text
Sau khi bấm Save : URL     = /admin/clients/client/16004
                   alert   = "Customer added successfully."
                   tiêu đề = "#16004"
                   #company.value = ""
```

## Ghi chú thêm
- **Tần suất tái hiện:** Luôn luôn — 3/3 (2 biến thể ở `run_1794391198` + 1 lần tái hiện 12-11-2026)
- **Workaround:** Người dùng tự kiểm tra trước khi bấm Save; KH rỗng lỡ tạo thì xoá thủ công
- **Nghi ngờ root cause:** Quy tắc bắt buộc của Company kiểm **trước** khi cắt khoảng trắng (chuỗi `"   "` không rỗng nên qua `required`), sau đó mới cắt rồi lưu thành rỗng. Kiểm phía trình duyệt cũng chỉ kiểm rỗng. Hướng sửa: cắt khoảng trắng **trước** khi kiểm bắt buộc, ở cả trình duyệt và máy chủ.
- **Liên quan:** khoảng trắng **đầu/cuối** của tên hợp lệ lại **không** bị cắt khi lưu (`CRM_CUS_TC_045-e`, đang chờ đối chiếu REQ-CUS-48) — Dev nên xử lý chung một chỗ để hai hành vi nhất quán.
- **Về Severity:** 🟠 Major — chức năng chính (tạo khách hàng) để lọt dữ liệu sai, sinh bản ghi rác lan sang các module dùng danh sách khách hàng; có workaround (xoá thủ công).

## Lịch sử retest
<!-- Do /retest-fixed-bugs ghi. Dòng mới thêm LÊN ĐẦU bảng. Bảng rỗng = chưa retest lần nào -->

| Ngày | Build | Kết quả | Retest report | Ghi chú |
|---|---|---|---|---|
