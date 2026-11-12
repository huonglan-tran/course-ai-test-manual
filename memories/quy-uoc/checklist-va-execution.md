---
name: checklist-va-execution
description: Chạy checklist khác chạy bộ TC; quy ước đặt tên/định dạng file checklist và execution report mà user đã chốt ngày 12-11-2026
type: feedback
updated: 12-11-2026
---

# Checklist & Execution report

## Chạy checklist ≠ chạy bộ TC (user hỏi và đã hiểu 12-11-2026)

- Hai thứ khác nhau. Run theo **Bộ TC** ghi `Loại nguồn = Bộ TC`; run theo **checklist** ghi `Loại nguồn = Checklist <loại>`.
- **Không** lấy kết quả chạy TC dán vào checklist rồi coi là "đã chạy checklist": mã mục `CL-…-NN` khi đó chỉ là ánh xạ từ TC ID, không do `/execute-test-cases` sinh. Muốn kết quả đúng chuẩn checklist → chạy `/execute-test-cases` với **loại checklist**.
- Hai nguồn **không cộng chung** pass rate. Báo cáo ghi rõ "Đây là kết quả chạy checklist, không phải bộ TC".
- Ví dụ thực tế: `run_1794453634` (Login, bộ TC) được user gán tạm vào checklist → sau đó chạy lại đúng chuẩn thành `run_1794460519` (checklist regression).

## File checklist (`docs/checklists/<module>/web/`)

- Tên `checklist_<loại>_<YYYYMMDD>.md`; mã `CL-<loại>_<YYYYMMDD>`; mục = `CL-<loại>_<YYYYMMDD>-<2 chữ số>`.
- **Một file cho một loại/ngày.** Login: user đổi loại Smoke → **Regression**, chọn "giữ một file", đổi tên thành `checklist_regression_20261112.md`, và yêu cầu định dạng như một checklist Regression bình thường — **không** chia "Phần A / Phần B bổ sung".
- Customers: giữ loại **Smoke** (`checklist_smoke_20261112.md`, 19 mục).
- Checklist là ảnh chụp tại một thời điểm — không sửa tại chỗ khi TC đổi (lập file mới).
- **Định dạng cột (user chốt 12-11-2026):**
  - Cột thứ 2 tên **"Trạng thái"** (không để tiêu đề là ký hiệu ✅).
  - Ký hiệu kết quả dùng **✅ PASS · ❌ FAIL · ⚠️ BLOCKED · ⏭️ SKIPPED** — **không** dùng ☑/☐. Ô BLOCKED dùng ký hiệu có màu dễ phân biệt thay cho ☐ (checklist Customers dùng ⚠️).
  - Tick kết quả cho **cả** các mục (user: "tick vào các ô của 28 mục").

## Execution report (`docs/executions/<module>/web/run_<timestamp>/`)

- `execution_report.md` + `evidence/<TC_ID>_<mô_tả>.png`. Ô `Loại nguồn` phân biệt Bộ TC / Checklist.
- Retest bug → `retest_<timestamp>/`; phân tích → `analysis_<timestamp>.md`.
- Báo cáo ghi pass rate hai cách: không tính SKIPPED, và trên số TC thực chạy xong (PASS+FAIL).
- Có thể chia **đợt** ghi chung một report (LOGIN 12-11: đợt 1 `001→040`, đợt 2 `041→083`).
- Xem kết quả: `scripts/execution-viewer/bundle.html` (đã gộp phân biệt nhãn `CHECKLIST`).
