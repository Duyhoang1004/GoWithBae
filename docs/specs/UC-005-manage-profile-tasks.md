# Implementation Tasks — UC-005: Quản lý tài khoản cá nhân

**Spec:** [UC-005-manage-profile.md](./UC-005-manage-profile.md)  
**FR:** FR-08, FR-22  
**Điều kiện:** đây là backlog đề xuất. Chốt OQ-06, OQ-17, OQ-21 và yêu cầu dùng chung trước phần phụ thuộc. Không triển khai G2.

Mỗi task dưới đây là một PR dự kiến khi có codebase; có thể điều chỉnh kích thước sau thiết kế giải pháp. Không phải chỉ thị bắt đầu viết code trong lần lập tài liệu này.

## Task 1 — Dữ liệu và validation

**Spec ref:** UC-005 §2, §7–8; requirement chính §6.

- [ ] Xác định model/read model cần cho mục tiêu “Cập nhật hồ sơ và kiểm soát truy cập/xóa tài khoản của chính mình.”; tái dùng entity chung, không tạo entity trùng giữa các UC.
- [ ] Đặc tả và hiện thực validation: name: tên hiển thị khi cập nhật; không chỉ khoảng trắng, độ dài OQ-21.
- [ ] Đặc tả và hiện thực validation: avatar: tệp tùy chọn theo loại/kích thước/kiểm tra nội dung đã chốt.
- [ ] Đặc tả và hiện thực validation: new_email: tùy chọn, đúng định dạng và phải xác minh mới thay.
- [ ] Đặc tả và hiện thực validation: action: sửa hồ sơ/đổi email/đăng xuất/yêu cầu xóa; chỉ áp dụng tài khoản hiện tại.
- [ ] Khi có database hiện hữu, lập migration và kiểm tra không phá tham chiếu/lịch sử; hiện dự án mới có tài liệu nên chưa có migration cụ thể.

## Task 2 — Quyền và xử lý nghiệp vụ

**Spec ref:** UC-005 §2–7; requirement chính §3/5/7.

- [ ] Kiểm tra từng PRE ở bước 1 và quyền/trạng thái lại ở bước 3–4.
- [ ] Bước 1: Kiểm tra phiên, chủ tài khoản và quyền thao tác theo trạng thái.
- [ ] Bước 2: Nhận tên/ảnh/email hoặc yêu cầu quản lý truy cập; không yêu cầu dữ liệu quan hệ.
- [ ] Bước 3: Kiểm tra dữ liệu, tệp, trạng thái khóa; đổi email chỉ tiếp tục khi email mới đã xác minh.
- [ ] Bước 4: Cập nhật hồ sơ; đăng xuất thu hồi phiên tương ứng; xóa tài khoản theo chính sách đã duyệt đồng thời thu hồi phiên/link.
- [ ] Bước 5: Trả trạng thái đúng: đã lưu, đã đăng xuất hoặc đã nhận/đã xử lý yêu cầu xóa; không báo đã xóa toàn bộ nếu chưa làm.
- [ ] Enforce RULE-16, RULE-17, RULE-27, RULE-12; xử lý Đổi id sang người khác, tài khoản bị khóa cố sửa, email mới chưa xác minh hoặc tệp sai: không cập nhật. Nếu chính sách xóa chưa chốt, không giả lập đã xóa; ghi blocker OQ-17.
- [ ] Hiện thực AF-01 và EX-01–04, đặc biệt xác minh trạng thái trước retry ghi, không báo sai kết quả vì side effect lỗi.

## Task 3 — Interface và UI

**Spec ref:** UC-005 §4–6/8; requirement chính §7–8.

- [ ] Sau thiết kế giải pháp, chốt contract interface theo input/output của UC và lỗi validation/quyền/trạng thái; không tự chọn chính sách TBD bằng default framework.
- [ ] Xây giao diện thao tác của actor Thành viên, có loading/empty/error/retry, nhãn/lỗi theo trường và mobile/keyboard theo NFR.
- [ ] Tích hợp đúng các bước kiểm tra server; không dùng ẩn nút làm cơ chế phân quyền.
- [ ] Kết quả cần hiển thị: Chỉ tài khoản đúng được thay đổi; quyền đăng nhập/link bị thu hồi theo thao tác; dữ liệu xóa tuân chính sách được duyệt.

## Task 4 — Kiểm thử và bằng chứng

**Spec ref:** UC-005 §10 và Verification Report; requirement chính §12.

- [ ] Kiểm thử AC-009: Given Email mới chưa xác minh; When yêu cầu thay email; Then email cũ chưa bị thay và người dùng được hướng dẫn xác minh.
- [ ] Kiểm thử AC-010: Given Yêu cầu xóa đã được xử lý theo chính sách; When dùng lại phiên hoặc link riêng cũ; Then không truy cập tiếp được bằng phiên/link đã thu hồi.

- [ ] Kiểm thử EX-01 quyền trực tiếp/thay id; EX-02 boundary nghiệp vụ; EX-03 lỗi đọc/ghi/đồng thời; EX-04 side effect lỗi theo tác động thực tế.
- [ ] Ghi kết quả theo FR/UC/AC, dữ liệu và cấu hình đã duyệt; chỉ chạy kiểm thử tải/restore ở hạng mục phù hợp, không ghi “passed” khi chưa chạy.
