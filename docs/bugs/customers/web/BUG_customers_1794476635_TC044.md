# [BUG] Customers — Company dài hơn 191 ký tự bị cắt bớt khi lưu mà không báo lỗi

## Thông tin chung
- **ID:** BUG_customers_1794476635_TC044
- **Severity:** 🟡 Minor
- **Priority:** P3
- **Nền tảng:** `web`
- **Môi trường:** Demo dùng chung `https://crm.anhtester.com/admin/clients/client` | Chrome (Playwright MCP), headed | Windows 11 | viewport 1600×750
- **Build / Version:** Không có thông tin build trên demo — ghi nhận theo ngày chạy 11-11-2026 (`run_1794391198`), tái hiện 12-11-2026
- **Test case liên quan:** `CRM_CUS_TC_044-a` (192 ký tự) · `CRM_CUS_TC_044-b` (206 ký tự)
- **REQ ID liên quan:** `REQ-CUS-32` · `AMB-CUS-02` — user chốt giới hạn 191 ký tự, giao diện phải chặn (11-11-2026)
- **Test data đã dùng:** `auto_cus_tc044a_20261111_` + 166 `x` + `Z` (192) và `auto_cus_tc044b_20261111_` + 180 `x` + `Z` (206) ở `run_1794391198` → KH id 15874, 15875 · `auto_cus_bug044_1794476700_` + `x` + `Z` (193 ký tự) ở lần tái hiện → KH id 16005. **Đã xoá cả 3** (mở lại hồ sơ trả 404)
- **Ngày phát hiện:** 11-11-2026

## Steps to Reproduce
1. Đăng nhập Admin, mở `/admin/clients/client` (New Customer)
2. Dán vào Company một chuỗi **192** ký tự kết thúc bằng chữ `Z` (VD `auto_cus_tc044a_<ngày>_` + 166 chữ `x` + `Z`)
3. Bấm `Save`
4. Xem lại ô Company trên hồ sơ vừa mở

## Expected Result
Theo `REQ-CUS-32`:
- **Không** tạo khách hàng, trang vẫn ở `/admin/clients/client`
- Dưới ô Company hiện nguyên văn `Please enter no more than 191 characters.` (câu QA đề xuất theo uỷ quyền của user — PO có thể đổi câu chữ, nhưng **phải có** thông báo và **không** được lưu)

## Actual Result
- Ô Company nhận đủ chuỗi dài (không có `maxlength`), trình duyệt không chặn.
- Bấm `Save` → `Customer added successfully.`, chuyển sang hồ sơ khách hàng mới.
- Company được lưu **đúng 191 ký tự đầu** — phần đuôi (có chữ `Z`) **bị mất**, không có thông báo nào.

→ Người dùng tin rằng tên đã lưu đủ, trong khi dữ liệu đã bị cắt âm thầm.

## Evidence
- Screenshot lần chạy gốc: [CRM_CUS_TC_044_step3_luu_va_cat_191.png](../../../executions/customers/web/run_1794391198/evidence/CRM_CUS_TC_044_step3_luu_va_cat_191.png)
- Screenshot lần tái hiện (12-11-2026, KH #16005): [CRM_CUS_TC_044_tai_hien_bug.png](../../../executions/customers/web/run_1794391198/evidence/CRM_CUS_TC_044_tai_hien_bug.png)
- Kết quả kiểm DOM lúc tái hiện:

```text
Trước Save : #company.value.length = 193 · không có maxlength
Sau Save   : URL = /admin/clients/client/16005
             alert = "Customer added successfully."
             #company.value.length = 191 · kết thúc "xxxxxx" (mất "xZ")
```

## Ghi chú thêm
- **Tần suất tái hiện:** Luôn luôn — 3/3 (2 biến thể ở `run_1794391198` + 1 lần tái hiện 12-11-2026)
- **Workaround:** Người dùng tự giữ tên ≤ 191 ký tự
- **Nghi ngờ root cause:** Cột Company trong CSDL giới hạn 191 ký tự; ô nhập không khai `maxlength`, máy chủ không có quy tắc độ dài → CSDL tự cắt khi lưu. Hướng sửa: thêm `maxlength="191"` cho ô nhập **và** quy tắc độ dài ở máy chủ, trả thông báo lỗi khi vượt.
- **Chưa kiểm vế (1) của REQ-CUS-32** (đúng 191 ký tự lưu đủ) trong lần tái hiện này — kết quả lưu đúng 191 ký tự ở trên cho thấy giới hạn nằm ở 191.
- **Về Severity:** 🟡 Minor — mất dữ liệu nhưng chỉ ở tên rất dài (hiếm gặp với tên công ty thật), không chặn luồng chính.

## Lịch sử retest
<!-- Do /retest-fixed-bugs ghi. Dòng mới thêm LÊN ĐẦU bảng. Bảng rỗng = chưa retest lần nào -->

| Ngày | Build | Kết quả | Retest report | Ghi chú |
|---|---|---|---|---|
