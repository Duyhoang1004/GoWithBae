# Implementation Tasks — UC-027: Xem số lần đã đi tại địa điểm

**Spec:** [UC-027-view-visit-count.md](./UC-027-view-visit-count.md)  
**FR:** FR-35, FR-22  
**Điều kiện:** đây là backlog đề xuất. Chốt OQ-10, OQ-11, OQ-25 và yêu cầu dùng chung trước phần phụ thuộc. Không triển khai G2.

Mỗi task dưới đây là một PR dự kiến khi có codebase; có thể điều chỉnh kích thước sau thiết kế giải pháp. Không phải chỉ thị bắt đầu viết code trong lần lập tài liệu này.

## Task 1 — Dữ liệu và validation

**Spec ref:** UC-027 §2, §7–8; requirement chính §6.

- [ ] Xác định model/read model cần cho mục tiêu “Biết đã tự xác nhận đi một chi nhánh bao nhiêu lần, kể cả các lần quay lại.”; tái dùng entity chung, không tạo entity trùng giữa các UC.
- [ ] Đặc tả và hiện thực validation: place_id: mã chi nhánh đang xem hoặc nơi trong kế hoạch; lịch sử nơi ẩn theo OQ-11.
- [ ] Đặc tả và hiện thực validation: user_id: lấy từ tài khoản phía máy chủ, không nhận id người khác để xem count.
- [ ] Đặc tả và hiện thực validation: visit_count: số nguyên không âm, tính từ lượt Đã đi hợp lệ không trùng; một plan/người chỉ một lượt; không lấy tổng click/analytics event.
- [ ] Khi có database hiện hữu, lập migration và kiểm tra không phá tham chiếu/lịch sử; hiện dự án mới có tài liệu nên chưa có migration cụ thể.

## Task 2 — Quyền và xử lý nghiệp vụ

**Spec ref:** UC-027 §2–7; requirement chính §3/5/7.

- [ ] Kiểm tra từng PRE ở bước 1 và quyền/trạng thái lại ở bước 3–4.
- [ ] Bước 1: Kiểm tra tài khoản, quyền dữ liệu cá nhân và mã chi nhánh.
- [ ] Bước 2: Xác định các lượt tự xác nhận thuộc đúng tài khoản và đúng chi nhánh.
- [ ] Bước 3: Chỉ chọn xác nhận Đã đi hợp lệ; loại plan chưa đi/đã hủy không có xác nhận hợp lệ/dismiss và bản ghi hoặc retry trùng.
- [ ] Bước 4: Tính X từ lượt duy nhất; lần đầu là 1, plan khác đã đi cùng nơi là 2, 3…; không reset theo ngày/tuần.
- [ ] Bước 5: Hiển thị số cá nhân ở chi tiết/buổi hẹn; không đưa số này vào nội dung chia sẻ cho khách hoặc coi là chứng nhận G2.
- [ ] Enforce RULE-14, RULE-15, RULE-17, RULE-29, RULE-30; xử lý Đổi user_id, không có tài khoản, đếm nhiều lần do cùng plan/retry, dùng visit_answer chưa trả lời/Không hoặc dismissal làm lượt: bị chặn hoặc loại khỏi tổng. Nếu đọc count lỗi, báo chưa tải được thay vì giả tổng bằng 0.
- [ ] Hiện thực AF-01 và EX-01–04, đặc biệt xác minh trạng thái trước retry ghi, không báo sai kết quả vì side effect lỗi.

## Task 3 — Interface và UI

**Spec ref:** UC-027 §4–6/8; requirement chính §7–8.

- [ ] Sau thiết kế giải pháp, chốt contract interface theo input/output của UC và lỗi validation/quyền/trạng thái; không tự chọn chính sách TBD bằng default framework.
- [ ] Xây giao diện thao tác của actor Thành viên, có loading/empty/error/retry, nhãn/lỗi theo trường và mobile/keyboard theo NFR.
- [ ] Tích hợp đúng các bước kiểm tra server; không dùng ẩn nút làm cơ chế phân quyền.
- [ ] Kết quả cần hiển thị: Đúng tài khoản thấy X lượt hợp lệ của đúng chi nhánh; số tăng theo lượt quay lại thực sự được tự xác nhận, không theo click hay ngày liên tiếp.

## Task 4 — Kiểm thử và bằng chứng

**Spec ref:** UC-027 §10 và Verification Report; requirement chính §12.

- [ ] Kiểm thử AC-053: Given Tài khoản có hai plan khác nhau tại cùng nơi đều xác nhận Đã đi; When mở chi tiết nơi đó; Then hiển thị Đã đi 2 lần; nơi khác và tài khoản khác không bị cộng.
- [ ] Kiểm thử AC-054: Given Một plan đã được ghi Đã đi và count là 1; When gửi lại xác nhận hoặc tải lại nhiều lần; Then count vẫn 1, không tăng theo số lần click.

- [ ] Kiểm thử EX-01 quyền trực tiếp/thay id; EX-02 boundary nghiệp vụ; EX-03 lỗi đọc/ghi/đồng thời; EX-04 side effect lỗi theo tác động thực tế.
- [ ] Ghi kết quả theo FR/UC/AC, dữ liệu và cấu hình đã duyệt; chỉ chạy kiểm thử tải/restore ở hạng mục phù hợp, không ghi “passed” khi chưa chạy.
