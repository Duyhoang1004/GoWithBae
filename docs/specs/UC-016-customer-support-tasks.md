# Implementation Tasks — UC-016: Gửi và theo dõi yêu cầu hỗ trợ

**Spec:** [UC-016-customer-support.md](./UC-016-customer-support.md)  
**FR:** FR-20, FR-22  
**Điều kiện:** đây là backlog đề xuất. Chốt OQ-13, OQ-21 và yêu cầu dùng chung trước phần phụ thuộc. Không triển khai G2.

Mỗi task dưới đây là một PR dự kiến khi có codebase; có thể điều chỉnh kích thước sau thiết kế giải pháp. Không phải chỉ thị bắt đầu viết code trong lần lập tài liệu này.

## Task 1 — Dữ liệu và validation

**Spec ref:** UC-016 §2, §7–8; requirement chính §6.

- [ ] Xác định model/read model cần cho mục tiêu “Nhận hỗ trợ và bổ sung thông tin trong yêu cầu của chính mình.”; tái dùng entity chung, không tạo entity trùng giữa các UC.
- [ ] Đặc tả và hiện thực validation: category: tài khoản/sử dụng tính năng/thông tin địa điểm; chưa có thanh toán G1.
- [ ] Đặc tả và hiện thực validation: subject/content: chuỗi bắt buộc khi tạo, không rỗng; giới hạn OQ-21.
- [ ] Đặc tả và hiện thực validation: attachments: tùy chọn, theo chính sách tệp; ticket_id: của mình khi xem/bổ sung.
- [ ] Khi có database hiện hữu, lập migration và kiểm tra không phá tham chiếu/lịch sử; hiện dự án mới có tài liệu nên chưa có migration cụ thể.

## Task 2 — Quyền và xử lý nghiệp vụ

**Spec ref:** UC-016 §2–7; requirement chính §3/5/7.

- [ ] Kiểm tra từng PRE ở bước 1 và quyền/trạng thái lại ở bước 3–4.
- [ ] Bước 1: Kiểm tra đăng nhập, trạng thái tài khoản và sở hữu ticket nếu có.
- [ ] Bước 2: Nhận nhóm/chủ đề/nội dung/tệp; không tạo phiên chat realtime.
- [ ] Bước 3: Kiểm tra trường/tệp, quyền và trạng thái ticket cho phép trả lời theo OQ-13.
- [ ] Bước 4: Tạo ticket Mới hoặc thêm message của khách và lịch sử; trạng thái phản hồi theo quy trình đã duyệt.
- [ ] Bước 5: Trả mã, trạng thái và các phản hồi public của chính yêu cầu, tuyệt đối không ghi chú nội bộ.
- [ ] Enforce RULE-17, RULE-19, RULE-27; xử lý Đổi ticket_id sang người khác, nội dung rỗng/tệp sai, tài khoản khóa cố gửi: từ chối. Ticket đóng xử lý theo OQ-13, không tự mở lại khi chưa duyệt chính sách.
- [ ] Hiện thực AF-01 và EX-01–04, đặc biệt xác minh trạng thái trước retry ghi, không báo sai kết quả vì side effect lỗi.

## Task 3 — Interface và UI

**Spec ref:** UC-016 §4–6/8; requirement chính §7–8.

- [ ] Sau thiết kế giải pháp, chốt contract interface theo input/output của UC và lỗi validation/quyền/trạng thái; không tự chọn chính sách TBD bằng default framework.
- [ ] Xây giao diện thao tác của actor Thành viên, có loading/empty/error/retry, nhãn/lỗi theo trường và mobile/keyboard theo NFR.
- [ ] Tích hợp đúng các bước kiểm tra server; không dùng ẩn nút làm cơ chế phân quyền.
- [ ] Kết quả cần hiển thị: Ticket/message đúng người được lưu với mã và lịch sử, dữ liệu nội bộ không lộ cho khách.

## Task 4 — Kiểm thử và bằng chứng

**Spec ref:** UC-016 §10 và Verification Report; requirement chính §12.

- [ ] Kiểm thử AC-031: Given Thành viên có ticket đang Chờ khách phản hồi; When bổ sung thông tin hợp lệ; Then lưu message của khách và chuyển xử lý theo quy trình được duyệt.
- [ ] Kiểm thử AC-032: Given Thành viên thay id sang ticket người khác; When xem/trả lời trực tiếp; Then không đọc được ticket, tệp hay ghi chú của người khác.

- [ ] Kiểm thử EX-01 quyền trực tiếp/thay id; EX-02 boundary nghiệp vụ; EX-03 lỗi đọc/ghi/đồng thời; EX-04 side effect lỗi theo tác động thực tế.
- [ ] Ghi kết quả theo FR/UC/AC, dữ liệu và cấu hình đã duyệt; chỉ chạy kiểm thử tải/restore ở hạng mục phù hợp, không ghi “passed” khi chưa chạy.
