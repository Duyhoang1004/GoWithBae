# Implementation Tasks — UC-025: Xử lý yêu cầu hỗ trợ

**Spec:** [UC-025-handle-support.md](./UC-025-handle-support.md)  
**FR:** FR-29  
**Điều kiện:** đây là backlog đề xuất. Chốt OQ-13, OQ-14, OQ-18, OQ-21 và yêu cầu dùng chung trước phần phụ thuộc. Không triển khai G2.

Mỗi task dưới đây là một PR dự kiến khi có codebase; có thể điều chỉnh kích thước sau thiết kế giải pháp. Không phải chỉ thị bắt đầu viết code trong lần lập tài liệu này.

## Task 1 — Dữ liệu và validation

**Spec ref:** UC-025 §2, §7–8; requirement chính §6.

- [ ] Xác định model/read model cần cho mục tiêu “Nhận việc, trao đổi và đóng yêu cầu hỗ trợ trong phạm vi được cấp.”; tái dùng entity chung, không tạo entity trùng giữa các UC.
- [ ] Đặc tả và hiện thực validation: ticket_id/assignee: yêu cầu trong phạm vi và người phụ trách được phân công.
- [ ] Đặc tả và hiện thực validation: message: nội dung/tệp, phân loại phản hồi cho khách hoặc ghi chú nội bộ.
- [ ] Đặc tả và hiện thực validation: new_status: trạng thái chuyển hợp lệ, không đóng tự động do timeout chưa được duyệt.
- [ ] Khi có database hiện hữu, lập migration và kiểm tra không phá tham chiếu/lịch sử; hiện dự án mới có tài liệu nên chưa có migration cụ thể.

## Task 2 — Quyền và xử lý nghiệp vụ

**Spec ref:** UC-025 §2–7; requirement chính §3/5/7.

- [ ] Kiểm tra từng PRE ở bước 1 và quyền/trạng thái lại ở bước 3–4.
- [ ] Bước 1: Kiểm tra quyền/phạm vi, tải yêu cầu và lịch sử đúng quyền.
- [ ] Bước 2: Nhận loại message public/internal, không mặc định ghi chú nội bộ gửi cho khách.
- [ ] Bước 3: Kiểm tra nội dung/tệp, trạng thái và quyền; chờ khách/đóng theo quy trình.
- [ ] Bước 4: Ghi người phụ trách, phản hồi/ghi chú, lịch sử và trạng thái; tạo thông báo đúng khách với message public.
- [ ] Bước 5: Nhân sự thấy nội dung theo quyền, khách chỉ thấy phản hồi public; email lỗi theo dõi riêng không đảo ngược phản hồi đã lưu.
- [ ] Enforce RULE-19, RULE-21, RULE-24, RULE-27; xử lý Sai phạm vi, cố lộ internal, tệp sai hoặc chuyển trạng thái chưa được duyệt: từ chối. Thông báo/email lỗi sau lưu không báo phản hồi chưa lưu; có retry theo dõi riêng.
- [ ] Hiện thực AF-01 và EX-01–04, đặc biệt xác minh trạng thái trước retry ghi, không báo sai kết quả vì side effect lỗi.

## Task 3 — Interface và UI

**Spec ref:** UC-025 §4–6/8; requirement chính §7–8.

- [ ] Sau thiết kế giải pháp, chốt contract interface theo input/output của UC và lỗi validation/quyền/trạng thái; không tự chọn chính sách TBD bằng default framework.
- [ ] Xây giao diện thao tác của actor Nhân viên hỗ trợ, có loading/empty/error/retry, nhãn/lỗi theo trường và mobile/keyboard theo NFR.
- [ ] Tích hợp đúng các bước kiểm tra server; không dùng ẩn nút làm cơ chế phân quyền.
- [ ] Kết quả cần hiển thị: Yêu cầu có người xử lý, lịch sử/trạng thái, và nội dung public/internal được tách đúng.

## Task 4 — Kiểm thử và bằng chứng

**Spec ref:** UC-025 §10 và Verification Report; requirement chính §12.

- [ ] Kiểm thử AC-049: Given Nhân sự thêm ghi chú internal; When khách xem lịch sử ticket; Then không thấy ghi chú hay tệp internal.
- [ ] Kiểm thử AC-050: Given Yêu cầu đang xử lý và có phản hồi cần khách bổ sung; When nhân sự lưu phản hồi public, chuyển chờ khách; Then khách nhận thông báo phù hợp, lịch sử giữ phản hồi và trạng thái.

- [ ] Kiểm thử EX-01 quyền trực tiếp/thay id; EX-02 boundary nghiệp vụ; EX-03 lỗi đọc/ghi/đồng thời; EX-04 side effect lỗi theo tác động thực tế.
- [ ] Ghi kết quả theo FR/UC/AC, dữ liệu và cấu hình đã duyệt; chỉ chạy kiểm thử tải/restore ở hạng mục phù hợp, không ghi “passed” khi chưa chạy.
