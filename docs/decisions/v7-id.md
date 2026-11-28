# V7-ID

[← Sổ quyết định](../decisions.md)

## Quyết định

Người dùng chốt người thao tác là **danh tính Tailscale riêng của từng người** (19/10/2026). Bản thử: ai đã vào app cũng thao tác được, không phân quyền theo người; vẫn ghi actor từ phiên phía server để có vết, không nhận actor từ client (20/10/2026)

## Ngày / nguồn

19/10 và 20/10/2026, xác nhận trong chat

## Hiệu lực và giới hạn

ĐÃ CHỐT hướng; thay phần "tài khoản ứng dụng riêng" của `Q01`. Chưa triển khai, chưa kiểm runtime. CÒN MỞ: cách app đọc danh tính Tailscale tin cậy (không tin header client tự khai), quản lý ai được vào và thu hồi — người dùng để sau (19/10/2026); ai được xem lưu vết.

**Đối chiếu 28/11/2026:** danh tính Tailscale ĐÃ TRIỂN KHAI trên `hao`: Lịch sử ghi login người làm ([`ACCESS-01`](access-01.md) đếm sự kiện theo login từ 22/10/2026); danh sách login được vào từ 31/10/2026 ([nhật ký](../deploys/2026-10-31-khoa-truy-cap.md)); header danh tính chỉ được tin khi đến từ `tailscaled`, từ 23/11/2026 ([nhật ký](../deploys/2026-11-23-api-chiu-tai.md)); thêm/bớt người ở trang Người dùng ([`QUYEN-02`](quyen-02.md)).
