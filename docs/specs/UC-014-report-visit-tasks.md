# Implementation Tasks — UC-014: Xử lý lời nhắc Đi chưa và xác nhận đã đi

**Spec:** [UC-014-report-visit.md](./UC-014-report-visit.md)  
**FR:** FR-18, FR-35  
**Điều kiện:** đây là backlog đề xuất. Chốt OQ-10, OQ-25 và yêu cầu dùng chung trước phần phụ thuộc. Không triển khai G2.

Mỗi task dưới đây là một PR dự kiến khi có codebase; có thể điều chỉnh kích thước sau thiết kế giải pháp. Không phải chỉ thị bắt đầu viết code trong lần lập tài liệu này.

## Task 1 — Dữ liệu và validation

**Spec ref:** UC-014 §2, §7–8; requirement chính §6.

- [ ] Xác định model/read model cần cho mục tiêu “Đến giờ xem lời nhắc, xác nhận Đã đi hoặc chỉ Bỏ qua lời nhắc.”; tái dùng entity chung, không tạo entity trùng giữa các UC.
- [ ] Đặc tả và hiện thực validation: plan_id: kế hoạch có quyền xem và được nhắc; kiểm tra thời điểm hẹn, trạng thái hiện tại.
- [ ] Đặc tả và hiện thực validation: reminder_action: Đã đi hoặc Bỏ qua lời nhắc; không dùng Bỏ qua làm câu trả lời Không.
- [ ] Đặc tả và hiện thực validation: reminder_status: shown/answered/dismissed, thời điểm xử lý; lưu tách visit_answer.
- [ ] Đặc tả và hiện thực validation: visit_answer: Có khi xác nhận Đã đi; nếu bỏ qua giữ nguyên, chưa trả lời vẫn Chưa trả lời; sửa câu trả lời theo OQ-10/25.
- [ ] Khi có database hiện hữu, lập migration và kiểm tra không phá tham chiếu/lịch sử; hiện dự án mới có tài liệu nên chưa có migration cụ thể.

## Task 2 — Quyền và xử lý nghiệp vụ

**Spec ref:** UC-014 §2–7; requirement chính §3/5/7.

- [ ] Kiểm tra từng PRE ở bước 1 và quyền/trạng thái lại ở bước 3–4.
- [ ] Bước 1: Kiểm tra quyền, scheduled_at, kế hoạch chưa hủy và lời nhắc chưa dismiss/answered; không phụ thuộc chờ trạng thái plan thành Past.
- [ ] Bước 2: Hiển thị Đã đi và Bỏ qua lời nhắc; không tự gán đã đi hoặc đề xuất quán/lịch khác.
- [ ] Bước 3: Kiểm tra quyền xác nhận, dữ liệu kế hoạch và trạng thái hiện thời; lấy danh tính từ máy chủ.
- [ ] Bước 4: Ghi VisitSelfReport=Có một lần cho lượt hợp lệ, đóng nhắc answered; cập nhật số lần theo RULE-29; không tạo chứng nhận sử dụng G2.
- [ ] Bước 5: Đóng lời nhắc, hiển thị số lần hiện tại đúng tài khoản; ghi visit_self_reported chống trùng, không popup gợi ý khác.
- [ ] Enforce RULE-14, RULE-15, RULE-25, RULE-29; xử lý Plan chưa tới giờ/đã hủy, actor không có quyền, action sai hoặc lời nhắc đã đóng: không tạo lượt mới. Retry Đã đi không tăng count lần nữa; retry dismiss không tạo nhắc mới. Không chuyển plan hủy thành Past. Lỗi lưu dismiss không báo đã ghi thành công, có thể thử lại an toàn.
- [ ] Hiện thực AF-01 và EX-01–04, đặc biệt xác minh trạng thái trước retry ghi, không báo sai kết quả vì side effect lỗi.

## Task 3 — Interface và UI

**Spec ref:** UC-014 §4–6/8; requirement chính §7–8.

- [ ] Sau thiết kế giải pháp, chốt contract interface theo input/output của UC và lỗi validation/quyền/trạng thái; không tự chọn chính sách TBD bằng default framework.
- [ ] Xây giao diện thao tác của actor Người được nhắc có quyền theo OQ-10, có loading/empty/error/retry, nhãn/lỗi theo trường và mobile/keyboard theo NFR.
- [ ] Tích hợp đúng các bước kiểm tra server; không dùng ẩn nút làm cơ chế phân quyền.
- [ ] Kết quả cần hiển thị: Lời nhắc đóng đúng thao tác; Đã đi ghi một lượt hợp lệ, Bỏ qua chỉ lưu trạng thái đóng nhắc; không gợi ý quán/lịch mới, không suy ra Chưa đi, không tự đổi kế hoạch.

## Task 4 — Kiểm thử và bằng chứng

**Spec ref:** UC-014 §10 và Verification Report; requirement chính §12.

- [ ] Kiểm thử AC-027: Given Plan đã đến giờ nhưng người dùng chưa trả lời; When chỉ xem lời nhắc; Then visit_answer vẫn Chưa trả lời và count không tăng.
- [ ] Kiểm thử AC-028: Given Plan đã hủy trước giờ; When tác vụ thời gian chạy sau giờ hẹn; Then plan vẫn Đã hủy, không hiện nhắc và không tự xác nhận đã đến/đã qua giờ.
- [ ] Kiểm thử AC-057: Given Kế hoạch chưa hủy đã đến scheduled_at, lời nhắc còn hiệu lực và người có quyền đang mở web; When đến thời điểm nhắc; Then hiển thị Đi chưa tại giờ hẹn, không đợi quyết định mốc Past của plan.
- [ ] Kiểm thử AC-058: Given Lời nhắc Đi chưa đang hiện và câu trả lời là Chưa trả lời; When bấm Bỏ qua lời nhắc rồi tải lại trang; Then lời nhắc biến mất và không tự hiện lại; không popup khác, không đổi lịch/địa điểm/visit_answer/count.

- [ ] Kiểm thử EX-01 quyền trực tiếp/thay id; EX-02 boundary nghiệp vụ; EX-03 lỗi đọc/ghi/đồng thời; EX-04 side effect lỗi theo tác động thực tế.
- [ ] Ghi kết quả theo FR/UC/AC, dữ liệu và cấu hình đã duyệt; chỉ chạy kiểm thử tải/restore ở hạng mục phù hợp, không ghi “passed” khi chưa chạy.
