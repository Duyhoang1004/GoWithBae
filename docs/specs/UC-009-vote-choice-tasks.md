# Implementation Tasks — UC-009: Bỏ phiếu và xem lựa chọn trùng

**Spec:** [UC-009-vote-choice.md](./UC-009-vote-choice.md)  
**FR:** FR-12, FR-22  
**Điều kiện:** đây là backlog đề xuất. Chốt OQ-07, OQ-08, OQ-09 và yêu cầu dùng chung trước phần phụ thuộc. Không triển khai G2.

Mỗi task dưới đây là một PR dự kiến khi có codebase; có thể điều chỉnh kích thước sau thiết kế giải pháp. Không phải chỉ thị bắt đầu viết code trong lần lập tài liệu này.

## Task 1 — Dữ liệu và validation

**Spec ref:** UC-009 §2, §7–8; requirement chính §6.

- [ ] Xác định model/read model cần cho mục tiêu “Lưu sở thích của chính mình và biết các nơi cả hai cùng thích.”; tái dùng entity chung, không tạo entity trùng giữa các UC.
- [ ] Đặc tả và hiện thực validation: choice_id/candidate_id: thuộc đúng phiên và tập cố định.
- [ ] Đặc tả và hiện thực validation: preference: Thích hoặc Bỏ qua khi gửi; chưa gửi hiển thị Chưa trả lời.
- [ ] Đặc tả và hiện thực validation: participant: lấy từ phiên máy chủ, không tin id người bỏ phiếu gửi lên.
- [ ] Khi có database hiện hữu, lập migration và kiểm tra không phá tham chiếu/lịch sử; hiện dự án mới có tài liệu nên chưa có migration cụ thể.

## Task 2 — Quyền và xử lý nghiệp vụ

**Spec ref:** UC-009 §2–7; requirement chính §3/5/7.

- [ ] Kiểm tra từng PRE ở bước 1 và quyền/trạng thái lại ở bước 3–4.
- [ ] Bước 1: Kiểm tra tư cách người tham gia, grant, trạng thái và tập địa điểm.
- [ ] Bước 2: Nhận lựa chọn cho từng nơi, gắn người tham gia từ danh tính truy cập.
- [ ] Bước 3: Kiểm tra nơi thuộc tập, quyền của đúng người và phiên chưa chốt.
- [ ] Bước 4: Tạo/cập nhật phiếu của người đó; tính giao nơi cả hai cùng thích, chưa trả lời không phải bỏ qua.
- [ ] Bước 5: Hiển thị nhóm trùng riêng, nơi còn lại vẫn xem được; ghi vote_submitted; không tự chốt.
- [ ] Enforce RULE-09, RULE-10, RULE-11, RULE-12, RULE-25, RULE-30; xử lý Giả id người kia, candidate ngoài tập, phiên chốt hoặc quyền bị thu hồi: không ghi phiếu; tải lại trạng thái và xin mời lại nếu cần. Tập sau join không được sửa trực tiếp.
- [ ] Hiện thực AF-01 và EX-01–04, đặc biệt xác minh trạng thái trước retry ghi, không báo sai kết quả vì side effect lỗi.

## Task 3 — Interface và UI

**Spec ref:** UC-009 §4–6/8; requirement chính §7–8.

- [ ] Sau thiết kế giải pháp, chốt contract interface theo input/output của UC và lỗi validation/quyền/trạng thái; không tự chọn chính sách TBD bằng default framework.
- [ ] Xây giao diện thao tác của actor Người tham gia phiên cùng chọn, có loading/empty/error/retry, nhãn/lỗi theo trường và mobile/keyboard theo NFR.
- [ ] Tích hợp đúng các bước kiểm tra server; không dùng ẩn nút làm cơ chế phân quyền.
- [ ] Kết quả cần hiển thị: Phiếu đúng chủ/người/nơi được lưu, kết quả giao đúng và không tự quyết định địa điểm cuối.

## Task 4 — Kiểm thử và bằng chứng

**Spec ref:** UC-009 §10 và Verification Report; requirement chính §12.

- [ ] Kiểm thử AC-017: Given Cả hai Thích A, một người chưa trả lời B; When xem kết quả; Then A nằm trong nhóm trùng, B không được xem là cả hai thích.
- [ ] Kiểm thử AC-018: Given Phiên đã chốt hoặc quyền bị thu hồi; When gửi sửa phiếu; Then phiếu không thay đổi và nhận hướng dẫn trạng thái/quyền.

- [ ] Kiểm thử EX-01 quyền trực tiếp/thay id; EX-02 boundary nghiệp vụ; EX-03 lỗi đọc/ghi/đồng thời; EX-04 side effect lỗi theo tác động thực tế.
- [ ] Ghi kết quả theo FR/UC/AC, dữ liệu và cấu hình đã duyệt; chỉ chạy kiểm thử tải/restore ở hạng mục phù hợp, không ghi “passed” khi chưa chạy.
