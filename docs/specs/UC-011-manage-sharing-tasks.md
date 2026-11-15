# Implementation Tasks — UC-011: Thu hồi hoặc cấp lại lời mời

**Spec:** [UC-011-manage-sharing.md](./UC-011-manage-sharing.md)  
**FR:** FR-14, FR-22  
**Điều kiện:** đây là backlog đề xuất. Chốt OQ-07, OQ-09 và yêu cầu dùng chung trước phần phụ thuộc. Không triển khai G2.

Mỗi task dưới đây là một PR dự kiến khi có codebase; có thể điều chỉnh kích thước sau thiết kế giải pháp. Không phải chỉ thị bắt đầu viết code trong lần lập tài liệu này.

## Task 1 — Dữ liệu và validation

**Spec ref:** UC-011 §2, §7–8; requirement chính §6.

- [ ] Xác định model/read model cần cho mục tiêu “Kiểm soát người nhận và vô hiệu hóa quyền truy cập cũ.”; tái dùng entity chung, không tạo entity trùng giữa các UC.
- [ ] Đặc tả và hiện thực validation: choice_id/grant_id: thuộc người tạo.
- [ ] Đặc tả và hiện thực validation: action: thu hồi link, loại người nhận hoặc tạo link mới; không khôi phục token cũ.
- [ ] Đặc tả và hiện thực validation: reason: nếu quy trình vận hành yêu cầu, chưa quy định bắt buộc từ nguồn.
- [ ] Khi có database hiện hữu, lập migration và kiểm tra không phá tham chiếu/lịch sử; hiện dự án mới có tài liệu nên chưa có migration cụ thể.

## Task 2 — Quyền và xử lý nghiệp vụ

**Spec ref:** UC-011 §2–7; requirement chính §3/5/7.

- [ ] Kiểm tra từng PRE ở bước 1 và quyền/trạng thái lại ở bước 3–4.
- [ ] Bước 1: Kiểm tra chủ sở hữu và trạng thái tài khoản.
- [ ] Bước 2: Hiển thị ảnh hưởng quyền truy cập theo chính sách được duyệt.
- [ ] Bước 3: Kiểm tra phạm vi grant/phiên, không cho đổi tập cố định hoặc tự viết lại lịch sử phiếu.
- [ ] Bước 4: Thu hồi grant/phiên tương ứng ngay; tạo grant mới khi cần theo chính sách, không tái kích hoạt token cũ.
- [ ] Bước 5: Trả kết quả; mọi yêu cầu tiếp theo bằng link/phiên bị thu hồi phải bị chặn, kể cả trang cũ đang mở.
- [ ] Enforce RULE-09, RULE-10, RULE-12, RULE-14, RULE-17; xử lý Người không sở hữu cố thu hồi/cấp lại: từ chối. Link cũ, phiên khách cũ hoặc thông báo cũ không được cấp lại quyền sau thu hồi; không xóa plan chỉ vì thu hồi lời mời.
- [ ] Hiện thực AF-01 và EX-01–04, đặc biệt xác minh trạng thái trước retry ghi, không báo sai kết quả vì side effect lỗi.

## Task 3 — Interface và UI

**Spec ref:** UC-011 §4–6/8; requirement chính §7–8.

- [ ] Sau thiết kế giải pháp, chốt contract interface theo input/output của UC và lỗi validation/quyền/trạng thái; không tự chọn chính sách TBD bằng default framework.
- [ ] Xây giao diện thao tác của actor Người tạo phiên, có loading/empty/error/retry, nhãn/lỗi theo trường và mobile/keyboard theo NFR.
- [ ] Tích hợp đúng các bước kiểm tra server; không dùng ẩn nút làm cơ chế phân quyền.
- [ ] Kết quả cần hiển thị: Quyền bị thu hồi không còn truy cập được; link mới có định danh mới, phiếu và kế hoạch xử lý theo quyết định đã duyệt.

## Task 4 — Kiểm thử và bằng chứng

**Spec ref:** UC-011 §10 và Verification Report; requirement chính §12.

- [ ] Kiểm thử AC-021: Given Người nhận đang mở trang với phiên riêng; When chủ tạo thu hồi cấp quyền rồi người nhận gửi yêu cầu tiếp; Then máy chủ từ chối cả URL cũ và phiên bị thu hồi.
- [ ] Kiểm thử AC-022: Given Một người không sở hữu phiên; When gọi thu hồi lời mời; Then không thay đổi quyền chia sẻ.

- [ ] Kiểm thử EX-01 quyền trực tiếp/thay id; EX-02 boundary nghiệp vụ; EX-03 lỗi đọc/ghi/đồng thời; EX-04 side effect lỗi theo tác động thực tế.
- [ ] Ghi kết quả theo FR/UC/AC, dữ liệu và cấu hình đã duyệt; chỉ chạy kiểm thử tải/restore ở hạng mục phù hợp, không ghi “passed” khi chưa chạy.
