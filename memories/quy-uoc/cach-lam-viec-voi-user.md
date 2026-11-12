---
name: cach-lam-viec-voi-user
description: Cách user muốn Claude làm việc ở repo này — rút từ các lần user sửa/ngắt/xác nhận trong ~20 phiên (10–11/2026)
type: feedback
updated: 12-11-2026
---

# Cách làm việc với user (QA, tiếng Việt)

> Bổ sung cho `CLAUDE.md` mục 1–2. Mỗi dòng dưới đây xuất phát từ một lần user nói rõ trong phiên.

## Dừng là dừng

- Khi user nói **"dừng / ngừng / để sau / không …"** → dừng ngay, **không** làm nốt "cho xong", không nhắc lại liên tục. Các lần đã xảy ra: dừng việc gộp file Login (12-10), dừng Mode FIX của `/review-testcases` (11-11), "TC đã review rồi ngưng không review nữa" (12-11), "Để sau" (12-11, hai lần).
- Khi user ngắt rồi nói **"Khôi phục bản gốc"** → khôi phục **toàn bộ** thay đổi của lượt đó, rồi chỉ áp **đúng** các chỉnh sửa user liệt kê (11-11: chỉ TC_156-d và TC_021).

## Không mở rộng phạm vi

- **Không sinh bug report** nếu user không yêu cầu (12-11: "không sinh bug"). Bug chỉ sinh sau khi TC FAIL **và** user muốn; ngoại lệ đã có: `REQ-CUS-31` (user yêu cầu mở bug).
- **Không commit** khi user chưa nói. Ví dụ: "Chưa commit, tôi sẽ mở file review trước" (20-10). Git thay đổi trạng thái code bị cấm hẳn (`CLAUDE.md`).
- Mode REVIEW chỉ báo cáo — không tự chuyển sang FIX.
- Khi user hỏi **"tại sao …"** và nói "chưa thay đổi gì" → chỉ giải thích, **không sửa file** (11-11: hỏi vì sao requirements CUS chia theo story).

## Cách hỏi & chốt AMB

- Hỏi **một đợt gọn**, nêu Assumption tạm cho từng câu. User thường trả lời vài câu quan trọng rồi nói *"còn lại lấy giá trị gợi ý của bạn"* → ghi **"Chốt theo Assumption tạm"** (khác với "User trả lời trực tiếp") trong cột Kết luận.
- Khi user hỏi "còn những câu hỏi nào cần tôi quyết định?" hoặc "tôi cần giải đáp gì trước khi chạy test case?" → liệt kê **đúng các mục còn treo** và nêu hệ quả/mặc định nếu chưa trả lời.
- Chỉ hỏi user thứ **không suy ra được** (`CLAUDE.md` 6b). Mâu thuẫn business rule mới dừng hỏi.

## Khi chạy test cùng tester

- Tester đăng nhập thủ công khi workflow cần (xem [`../he-thong/tai-khoan-va-bi-mat.md`](../he-thong/tai-khoan-va-bi-mat.md)); user báo "Đã đăng nhập…" thì **chạy tiếp ngay từ phiên đó**, không hỏi lại.
- User nói **"Chạy tiếp + dọn"** → chạy phần còn lại và dọn dữ liệu test đã tạo.
- Gặp hết quota/giới hạn giữa chừng, user nói "tiếp tục từ chỗ dừng" → tiếp tục đúng điểm dừng, không làm lại từ đầu.

## Phong cách trả lời

- Tiếng Việt, ngắn gọn, nói rõ **cái gì đã thay đổi và cái gì chưa kiểm chứng**.
- Khi user đòi giải thích khái niệm nhầm lẫn (VD "chạy TC khác chạy checklist?") → trả lời thẳng từng ý bằng câu đúng/sai + lý do, rồi mới đề xuất bước tiếp theo.
- Bản ghi nhớ tự động trước đây của máy (nay gộp vào repo): tên module giữ tiếng Anh — xem [`dat-ten-ma-dinh-danh-cau-truc.md`](dat-ten-ma-dinh-danh-cau-truc.md).
