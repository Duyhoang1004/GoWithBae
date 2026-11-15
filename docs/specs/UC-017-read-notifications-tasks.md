# Implementation Tasks — UC-017: Xem thông báo liên quan

**Spec:** [UC-017-read-notifications.md](./UC-017-read-notifications.md)  
**FR:** FR-21, FR-22  
**Điều kiện:** đây là backlog đề xuất. Chốt OQ-09, OQ-14 và yêu cầu dùng chung trước phần phụ thuộc. Không triển khai G2.

Mỗi task dưới đây là một PR dự kiến khi có codebase; có thể điều chỉnh kích thước sau thiết kế giải pháp. Không phải chỉ thị bắt đầu viết code trong lần lập tài liệu này.

## Task 1 — Dữ liệu và validation

**Spec ref:** UC-017 §2, §7–8; requirement chính §6.

- [ ] Xác định model/read model cần cho mục tiêu “Biết cập nhật buổi hẹn/hỗ trợ và mở đối tượng còn được phép truy cập.”; tái dùng entity chung, không tạo entity trùng giữa các UC.
- [ ] Đặc tả và hiện thực validation: notification_id: thuộc người nhận; loại cập nhật plan/hỗ trợ.
- [ ] Đặc tả và hiện thực validation: target_id: máy chủ kiểm tra quyền đối tượng khi mở.
- [ ] Đặc tả và hiện thực validation: read_action: chỉ nếu thiết kế đọc/chưa đọc đã được duyệt OQ-14.
- [ ] Khi có database hiện hữu, lập migration và kiểm tra không phá tham chiếu/lịch sử; hiện dự án mới có tài liệu nên chưa có migration cụ thể.

## Task 2 — Quyền và xử lý nghiệp vụ

**Spec ref:** UC-017 §2–7; requirement chính §3/5/7.

- [ ] Kiểm tra từng PRE ở bước 1 và quyền/trạng thái lại ở bước 3–4.
- [ ] Bước 1: Kiểm tra phiên và người nhận, không nạp thông báo tài khoản khác.
- [ ] Bước 2: Lấy nội dung tối thiểu và đối tượng liên quan.
- [ ] Bước 3: Kiểm tra quyền hiện thời trên plan/ticket/grant; thông báo cũ không tái cấp quyền đã thu hồi.
- [ ] Bước 4: Trả đối tượng hiện tại, ghi trạng thái đọc nếu chức năng này đã duyệt.
- [ ] Bước 5: Hiển thị kết quả; email xử lý/tiếp thị theo cấu hình riêng; lời nhắc trong web theo UC-014, chưa bổ sung email/push nhắc hẹn.
- [ ] Enforce RULE-12, RULE-14, RULE-19, RULE-24; xử lý Thông báo không thuộc mình hoặc quyền target đã mất: không trả nội dung nhạy cảm; hiển thị không còn truy cập được. Lỗi gửi email không tạo quyền khác hoặc báo sai trạng thái nghiệp vụ.
- [ ] Hiện thực AF-01 và EX-01–04, đặc biệt xác minh trạng thái trước retry ghi, không báo sai kết quả vì side effect lỗi.

## Task 3 — Interface và UI

**Spec ref:** UC-017 §4–6/8; requirement chính §7–8.

- [ ] Sau thiết kế giải pháp, chốt contract interface theo input/output của UC và lỗi validation/quyền/trạng thái; không tự chọn chính sách TBD bằng default framework.
- [ ] Xây giao diện thao tác của actor Thành viên, có loading/empty/error/retry, nhãn/lỗi theo trường và mobile/keyboard theo NFR.
- [ ] Tích hợp đúng các bước kiểm tra server; không dùng ẩn nút làm cơ chế phân quyền.
- [ ] Kết quả cần hiển thị: Chỉ thông báo của mình được đọc; mở đối tượng không vượt quyền hiện tại.

## Task 4 — Kiểm thử và bằng chứng

**Spec ref:** UC-017 §10 và Verification Report; requirement chính §12.

- [ ] Kiểm thử AC-033: Given Có thông báo về plan đã bị thu hồi chia sẻ; When mở liên kết trong thông báo; Then không xem được plan bằng thông báo cũ.
- [ ] Kiểm thử AC-034: Given Người dùng chưa chọn nhận tiếp thị; When có sự kiện hỗ trợ cần thiết; Then không tự biến đồng ý nhận hỗ trợ thành đồng ý tiếp thị.

- [ ] Kiểm thử EX-01 quyền trực tiếp/thay id; EX-02 boundary nghiệp vụ; EX-03 lỗi đọc/ghi/đồng thời; EX-04 side effect lỗi theo tác động thực tế.
- [ ] Ghi kết quả theo FR/UC/AC, dữ liệu và cấu hình đã duyệt; chỉ chạy kiểm thử tải/restore ở hạng mục phù hợp, không ghi “passed” khi chưa chạy.
