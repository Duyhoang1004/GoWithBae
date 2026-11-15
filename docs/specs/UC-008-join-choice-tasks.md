# Implementation Tasks — UC-008: Tham gia phiên cùng chọn

**Spec:** [UC-008-join-choice.md](./UC-008-join-choice.md)  
**FR:** FR-11, FR-22  
**Điều kiện:** đây là backlog đề xuất. Chốt OQ-07, OQ-09, OQ-21 và yêu cầu dùng chung trước phần phụ thuộc. Không triển khai G2.

Mỗi task dưới đây là một PR dự kiến khi có codebase; có thể điều chỉnh kích thước sau thiết kế giải pháp. Không phải chỉ thị bắt đầu viết code trong lần lập tài liệu này.

## Task 1 — Dữ liệu và validation

**Spec ref:** UC-008 §2, §7–8; requirement chính §6.

- [ ] Xác định model/read model cần cho mục tiêu “Xem nội dung từ link hợp lệ không cần đăng nhập và tham gia bằng phiên khách khi còn suất.”; tái dùng entity chung, không tạo entity trùng giữa các UC.
- [ ] Đặc tả và hiện thực validation: invite_token: bí mật link hợp lệ; không log công khai.
- [ ] Đặc tả và hiện thực validation: display_name: tên hiển thị bắt buộc, không chỉ khoảng trắng; độ dài OQ-21.
- [ ] Khi có database hiện hữu, lập migration và kiểm tra không phá tham chiếu/lịch sử; hiện dự án mới có tài liệu nên chưa có migration cụ thể.

## Task 2 — Quyền và xử lý nghiệp vụ

**Spec ref:** UC-008 §2–7; requirement chính §3/5/7.

- [ ] Kiểm tra từng PRE ở bước 1 và quyền/trạng thái lại ở bước 3–4.
- [ ] Bước 1: Kiểm tra grant/hạn/thu hồi và hiển thị nội dung được chia sẻ; không redirect sang màn đăng nhập, không đòi tài khoản.
- [ ] Bước 2: Nhận tên hiển thị mà không buộc đăng ký/đăng nhập hoặc khai quan hệ.
- [ ] Bước 3: Kiểm tra lại link và suất người nhận ở thời điểm ghi; nếu đồng thời chỉ một yêu cầu được chiếm suất.
- [ ] Bước 4: Hệ thống tự giữ phiên khách theo link để nhớ người nhận/quyền, không tạo phiên đăng nhập tài khoản; cố định tập địa điểm, không cấp quyền sửa plan.
- [ ] Bước 5: Chỉ trả dữ liệu được mời, không toàn bộ danh sách/email/lịch sử người tạo.
- [ ] Enforce RULE-08, RULE-09, RULE-12, RULE-30; xử lý Link hết hạn/thu hồi, tên rỗng hoặc suất đã có người: không tham gia; link lỗi không lộ nội dung, suất đầy giải thích liên hệ chủ tạo. Hai yêu cầu đồng thời không tạo hai người nhận.
- [ ] Hiện thực AF-01 và EX-01–04, đặc biệt xác minh trạng thái trước retry ghi, không báo sai kết quả vì side effect lỗi.

## Task 3 — Interface và UI

**Spec ref:** UC-008 §4–6/8; requirement chính §7–8.

- [ ] Sau thiết kế giải pháp, chốt contract interface theo input/output của UC và lỗi validation/quyền/trạng thái; không tự chọn chính sách TBD bằng default framework.
- [ ] Xây giao diện thao tác của actor Người nhận lời mời, có loading/empty/error/retry, nhãn/lỗi theo trường và mobile/keyboard theo NFR.
- [ ] Tích hợp đúng các bước kiểm tra server; không dùng ẩn nút làm cơ chế phân quyền.
- [ ] Kết quả cần hiển thị: Khách xem được nội dung theo link không cần tài khoản; tham gia thì tối đa một người nhận có phiên khách theo link, tập lựa chọn cố định; không có quyền cá nhân của chủ tạo.

## Task 4 — Kiểm thử và bằng chứng

**Spec ref:** UC-008 §10 và Verification Report; requirement chính §12.

- [ ] Kiểm thử AC-015: Given Hai người khác nhau dùng link để tham gia cùng lúc; When hai yêu cầu chiếm suất được xử lý; Then chỉ một người nhận thành công và tập lựa chọn không bị thay.
- [ ] Kiểm thử AC-016: Given Link đã thu hồi; When mở hoặc gửi tham gia; Then bị chặn và không nhận nội dung phiên.
- [ ] Kiểm thử AC-055: Given Trình duyệt chưa đăng nhập và có link hợp lệ; When mở link và tham gia bằng tên hiển thị; Then xem và tham gia bằng phiên khách, không bị đưa sang đăng nhập.
- [ ] Kiểm thử AC-056: Given Khách đang xem một nơi qua link hợp lệ; When bấm Lưu yêu thích; Then yêu cầu đăng nhập cho thao tác cá nhân, quyền xem link không bị biến thành bắt buộc đăng nhập.

- [ ] Kiểm thử EX-01 quyền trực tiếp/thay id; EX-02 boundary nghiệp vụ; EX-03 lỗi đọc/ghi/đồng thời; EX-04 side effect lỗi theo tác động thực tế.
- [ ] Ghi kết quả theo FR/UC/AC, dữ liệu và cấu hình đã duyệt; chỉ chạy kiểm thử tải/restore ở hạng mục phù hợp, không ghi “passed” khi chưa chạy.
