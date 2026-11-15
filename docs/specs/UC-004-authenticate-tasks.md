# Implementation Tasks — UC-004: Đăng nhập tài khoản

**Spec:** [UC-004-authenticate.md](./UC-004-authenticate.md)  
**FR:** FR-07  
**Điều kiện:** đây là backlog đề xuất. Chốt OQ-06, OQ-17 và yêu cầu dùng chung trước phần phụ thuộc. Không triển khai G2.

Mỗi task dưới đây là một PR dự kiến khi có codebase; có thể điều chỉnh kích thước sau thiết kế giải pháp. Không phải chỉ thị bắt đầu viết code trong lần lập tài liệu này.

## Task 1 — Dữ liệu và validation

**Spec ref:** UC-004 §2, §7–8; requirement chính §6.

- [ ] Xác định model/read model cần cho mục tiêu “Xác minh email và nhận phiên truy cập tài khoản của mình.”; tái dùng entity chung, không tạo entity trùng giữa các UC.
- [ ] Đặc tả và hiện thực validation: email: chuỗi bắt buộc, đúng định dạng, email được xác minh bằng mã hợp lệ.
- [ ] Đặc tả và hiện thực validation: otp: mã bắt buộc ở bước xác minh, còn hạn, đúng challenge, chưa dùng, trong hạn mức sai.
- [ ] Đặc tả và hiện thực validation: resend: yêu cầu gửi lại phải trong hạn mức; không ghi OTP/token vào log.
- [ ] Khi có database hiện hữu, lập migration và kiểm tra không phá tham chiếu/lịch sử; hiện dự án mới có tài liệu nên chưa có migration cụ thể.

## Task 2 — Quyền và xử lý nghiệp vụ

**Spec ref:** UC-004 §2–7; requirement chính §3/5/7.

- [ ] Kiểm tra từng PRE ở bước 1 và quyền/trạng thái lại ở bước 3–4.
- [ ] Bước 1: Kiểm tra định dạng, tình trạng tài khoản và giới hạn yêu cầu.
- [ ] Bước 2: Tạo/gửi challenge có hạn, không công bố mã trong phản hồi hoặc log.
- [ ] Bước 3: Kiểm tra đúng mã/email, còn hạn, chưa dùng, số lần sai và trạng thái tài khoản tại thời điểm xác minh.
- [ ] Bước 4: Đánh dấu mã đã dùng, xác thực email và tạo phiên cho đúng tài khoản đủ điều kiện.
- [ ] Bước 5: Trả trạng thái đăng nhập; không yêu cầu thông tin người yêu hay tình trạng quan hệ.
- [ ] Enforce RULE-16, RULE-17, RULE-27; xử lý Mã sai/hết hạn/dùng lại/quá số lần, gửi lại quá mức hoặc tài khoản không đủ điều kiện: không cấp phiên; hướng dẫn chờ hoặc xin mã mới theo cấu hình. Email gửi lỗi có mã tra cứu.
- [ ] Hiện thực AF-01 và EX-01–04, đặc biệt xác minh trạng thái trước retry ghi, không báo sai kết quả vì side effect lỗi.

## Task 3 — Interface và UI

**Spec ref:** UC-004 §4–6/8; requirement chính §7–8.

- [ ] Sau thiết kế giải pháp, chốt contract interface theo input/output của UC và lỗi validation/quyền/trạng thái; không tự chọn chính sách TBD bằng default framework.
- [ ] Xây giao diện thao tác của actor Người dùng muốn đăng nhập, có loading/empty/error/retry, nhãn/lỗi theo trường và mobile/keyboard theo NFR.
- [ ] Tích hợp đúng các bước kiểm tra server; không dùng ẩn nút làm cơ chế phân quyền.
- [ ] Kết quả cần hiển thị: Email được xác minh bởi mã hợp lệ và phiên đúng tài khoản được cấp; mã cũ không tái sử dụng.

## Task 4 — Kiểm thử và bằng chứng

**Spec ref:** UC-004 §10 và Verification Report; requirement chính §12.

- [ ] Kiểm thử AC-007: Given Mã đúng, còn hạn và tài khoản hoạt động; When xác minh mã; Then tạo phiên đúng tài khoản và mã không dùng lại được.
- [ ] Kiểm thử AC-008: Given Mã đã dùng hoặc hết hạn; When xác minh lại; Then không cấp phiên mới, có hướng dẫn lấy mã hợp lệ.

- [ ] Kiểm thử EX-01 quyền trực tiếp/thay id; EX-02 boundary nghiệp vụ; EX-03 lỗi đọc/ghi/đồng thời; EX-04 side effect lỗi theo tác động thực tế.
- [ ] Ghi kết quả theo FR/UC/AC, dữ liệu và cấu hình đã duyệt; chỉ chạy kiểm thử tải/restore ở hạng mục phù hợp, không ghi “passed” khi chưa chạy.
