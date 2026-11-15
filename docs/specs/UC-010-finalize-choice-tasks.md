# Implementation Tasks — UC-010: Chốt địa điểm từ phiên cùng chọn

**Spec:** [UC-010-finalize-choice.md](./UC-010-finalize-choice.md)  
**FR:** FR-13  
**Điều kiện:** đây là backlog đề xuất. Chốt OQ-08, OQ-10, OQ-11 và yêu cầu dùng chung trước phần phụ thuộc. Không triển khai G2.

Mỗi task dưới đây là một PR dự kiến khi có codebase; có thể điều chỉnh kích thước sau thiết kế giải pháp. Không phải chỉ thị bắt đầu viết code trong lần lập tài liệu này.

## Task 1 — Dữ liệu và validation

**Spec ref:** UC-010 §2, §7–8; requirement chính §6.

- [ ] Xác định model/read model cần cho mục tiêu “Quyết định một nơi và tạo kế hoạch tương ứng, chuyển phiên sang chỉ xem.”; tái dùng entity chung, không tạo entity trùng giữa các UC.
- [ ] Đặc tả và hiện thực validation: choice_id/candidate_id: phiên của chủ tạo và nơi thuộc tập được mời.
- [ ] Đặc tả và hiện thực validation: scheduled_at: ngày giờ tương lai ở Việt Nam; kiểm tra giờ xác minh hiện tại.
- [ ] Đặc tả và hiện thực validation: notes: chuỗi tùy chọn, giới hạn OQ-21; không đưa vào analytics.
- [ ] Khi có database hiện hữu, lập migration và kiểm tra không phá tham chiếu/lịch sử; hiện dự án mới có tài liệu nên chưa có migration cụ thể.

## Task 2 — Quyền và xử lý nghiệp vụ

**Spec ref:** UC-010 §2–7; requirement chính §3/5/7.

- [ ] Kiểm tra từng PRE ở bước 1 và quyền/trạng thái lại ở bước 3–4.
- [ ] Bước 1: Kiểm tra tài khoản, chủ phiên và trạng thái cho phép chốt.
- [ ] Bước 2: Nạp lại thông tin nơi; không tự chọn thay người tạo theo số phiếu.
- [ ] Bước 3: Kiểm tra nơi còn công khai/thuộc tập, thời gian tương lai và giờ mở cửa; cảnh báo giờ unknown.
- [ ] Bước 4: Theo PROP OQ-08, tạo tối đa một plan và chuyển phiên chỉ xem như một kết quả nhất quán; không chốt nếu lưu plan thất bại.
- [ ] Bước 5: Trả plan, trạng thái chỉ xem và cảnh báo chưa giữ chỗ; ghi date_plan_created một lần.
- [ ] Enforce RULE-01, RULE-05, RULE-11, RULE-13, RULE-14, RULE-17, RULE-25; xử lý Người nhận chốt, phiên đã chốt, nơi không thuộc tập/bị ẩn, giờ quá khứ/đóng cửa: chặn lưu. Lưu lỗi không chuyển phiên chỉ xem; thử lại kiểm tra trạng thái để không tạo plan thứ hai.
- [ ] Hiện thực AF-01 và EX-01–04, đặc biệt xác minh trạng thái trước retry ghi, không báo sai kết quả vì side effect lỗi.

## Task 3 — Interface và UI

**Spec ref:** UC-010 §4–6/8; requirement chính §7–8.

- [ ] Sau thiết kế giải pháp, chốt contract interface theo input/output của UC và lỗi validation/quyền/trạng thái; không tự chọn chính sách TBD bằng default framework.
- [ ] Xây giao diện thao tác của actor Người tạo phiên, có loading/empty/error/retry, nhãn/lỗi theo trường và mobile/keyboard theo NFR.
- [ ] Tích hợp đúng các bước kiểm tra server; không dùng ẩn nút làm cơ chế phân quyền.
- [ ] Kết quả cần hiển thị: Một quyết định của người tạo được lưu thành kế hoạch; phiên chỉ xem sau thành công, không có plan dở dang do chốt trước.

## Task 4 — Kiểm thử và bằng chứng

**Spec ref:** UC-010 §10 và Verification Report; requirement chính §12.

- [ ] Kiểm thử AC-019: Given Chủ tạo chọn nơi hợp lệ và thời gian mở cửa tương lai; When lưu kế hoạch từ phiên; Then tạo plan và phiên chỉ xem, không tự chọn theo phiếu.
- [ ] Kiểm thử AC-020: Given Lưu kế hoạch thất bại; When thực hiện chốt; Then không báo chốt thành công và có thể kiểm tra/thử lại an toàn.

- [ ] Kiểm thử EX-01 quyền trực tiếp/thay id; EX-02 boundary nghiệp vụ; EX-03 lỗi đọc/ghi/đồng thời; EX-04 side effect lỗi theo tác động thực tế.
- [ ] Ghi kết quả theo FR/UC/AC, dữ liệu và cấu hình đã duyệt; chỉ chạy kiểm thử tải/restore ở hạng mục phù hợp, không ghi “passed” khi chưa chạy.
