# ZALO-AGENT-01

[← Sổ quyết định](../decisions.md)

## Quyết định

Người dùng chốt ngày 20/11/2026 theo đề xuất của Claude, spec [`zalo-agent.md`](../../specs/zalo-agent.md):

1. **Một tiến trình nền `zalo-agent`** ở máy có Zalo thay task `WebdebitZaloSync` và `ZaloInsurancePush`. Mỗi bộ xử lý nhớ đã xử lý tới revision nào của `changes-N.json`. Con trỏ chỉ tiến sau khi đẩy lên `hao` thành công. Mọi thứ đẩy đi idempotent theo `message_id` hoặc sha256.
2. **Một kênh đẩy chung** lên `hao`: máy chủ giữ trạng thái, `umask 027`, không `chmod`, file chốt `mv` sau cùng.
3. **Kết quả đọc ảnh** lưu tại máy theo sha256 ảnh + engine + phiên bản; lên `hao` chỉ số tiền, giờ, ngân hàng và engine.
4. **Che thông tin** trước khi gửi Gemini: số tài khoản, số thẻ, tên người chuyển và người nhận. Ảnh không che được thì không gửi, đưa người xem.
5. **Số đo đúng/sai theo tuần** cho từng engine, từ bảng xác nhận của Thu chi; ngưỡng chỉ đổi bằng PR có số đo.
6. **Nhịp tim:** `status.json` đẩy lên `hao`, nhóm "Máy Zalo" ở tab Máy chạy, máy canh báo email trong 7:00–20:00 giờ VN.

Sáu câu Q1–Q6:

- **Q1:** mã agent ở repo này, `cong-no-web-handoff/tools/zalo_agent/`.
- **Q2:** Task Scheduler lúc đăng nhập, tự chạy lại khi chết, chạy cả khi dùng pin.
- **Q3:** ảnh không che được thì không gửi Gemini.
- **Q4:** che cả tên người chuyển, người nhận.
- **Q5:** cảnh báo 7:00–20:00 giờ VN.
- **Q6:** thứ tự PR-0 → PR-1 → PR-2 → Chuyển → PR-3; PR-4 sau PR-2 của Thu chi; PR-5 cuối.

## Ngày / nguồn

20/11/2026, chat. Sau các sự cố cùng ngày (lỗi quyền lặp ở hai đường đẩy, máy đứng im không ai biết, task chạy từ worktree, log 555 MB), người dùng hỏi có kiến trúc nâng cao nào để phát triển tiếp. Claude đề xuất 6 hướng. Người dùng yêu cầu "viết plan và spec cho cả 6" (FinanceBullkk/webdebit#540), rồi nói "dùng session này làm điều phối, mở các session con dùng sonnet 5.5 high để làm theo đề xuất".

## Hiệu lực và giới hạn

ĐÃ CHỐT 20/11/2026. Chưa code. Thay A1 và A4 của [`ZALO-THU-CHI-01`](zalo-thu-chi-01.md); A2 đổi thành "bản mới nhất theo `message_id` thắng; tin xóa hiện giống bản dựng đầy đủ" (chỉnh 20/11/2026 khi ghép PR-1 và PR-2, xem hợp đồng dữ liệu trong spec).

**Đối chiếu 28/11/2026:** ĐÃ CODE PR-0 đến PR-5: FinanceBullkk/zalo-audit-viewer#1, #543 (PR-1), #542 (PR-2), #548 và #549 (PR-3), #557 và #558 (PR-4a), #567 (PR-5). ĐÃ TRIỂN KHAI trên máy có Zalo (`msi`) và `hao` 20/11/2026: bước Chuyển, tắt task `WebdebitZaloSync` và `ZaloInsurancePush` ([nhật ký](../deploys/2026-11-20-zalo-agent.md)). Từ 23/11/2026 lúc 11:41 task `ZaloAgent` chạy từ bản ghim theo SHA ([nhật ký](../deploys/2026-11-23-zalo-agent-ban-ghim.md)). Đường gửi Gemini mặc định tắt (`ZALO_AGENT_EXTERNAL=off`, [spec](../../specs/zalo-agent.md)); chưa có nhật ký nào ghi đã bật.

Vẫn phải hỏi người dùng trước khi:

- bật task `zalo-agent` và tắt `WebdebitZaloSync` (bước Chuyển);
- tắt `ZaloInsurancePush` (PR-3);
- bật gửi ra Gemini (PR-4, khi đã có khóa API và địa chỉ proxy).

CÒN MỞ:

- ngưỡng "OCR chắc" T, chốt bằng dữ liệu ở PR-4;
- thời gian quét `messages.jsonl`, đo ở PR-1.
