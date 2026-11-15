# Implementation Tasks — UC-007: Tạo phiên cùng chọn

**Spec:** [UC-007-create-choice.md](./UC-007-create-choice.md)  
**FR:** FR-10  
**Điều kiện:** đây là backlog đề xuất. Chốt OQ-07, OQ-11 và yêu cầu dùng chung trước phần phụ thuộc. Không triển khai G2.

Mỗi task dưới đây là một PR dự kiến khi có codebase; có thể điều chỉnh kích thước sau thiết kế giải pháp. Không phải chỉ thị bắt đầu viết code trong lần lập tài liệu này.

## Task 1 — Dữ liệu và validation

**Spec ref:** UC-007 §2, §7–8; requirement chính §6.

- [ ] Xác định model/read model cần cho mục tiêu “Mời một người cùng lựa chọn từ 2–5 địa điểm đã lưu.”; tái dùng entity chung, không tạo entity trùng giữa các UC.
- [ ] Đặc tả và hiện thực validation: saved_place_ids: tập 2–5 mã khác nhau đã lưu; kiểm tra theo chủ tại máy chủ.
- [ ] Đặc tả và hiện thực validation: expiry: hạn link cấu hình; đề xuất nguồn 7 ngày, chưa phải mặc định khách duyệt.
- [ ] Khi có database hiện hữu, lập migration và kiểm tra không phá tham chiếu/lịch sử; hiện dự án mới có tài liệu nên chưa có migration cụ thể.

## Task 2 — Quyền và xử lý nghiệp vụ

**Spec ref:** UC-007 §2–7; requirement chính §3/5/7.

- [ ] Kiểm tra từng PRE ở bước 1 và quyền/trạng thái lại ở bước 3–4.
- [ ] Bước 1: Kiểm tra tài khoản và quyền trên các địa điểm đã lưu.
- [ ] Bước 2: Hiển thị tập được chọn và cảnh báo ai có link hợp lệ có thể xem/chiếm suất người nhận.
- [ ] Bước 3: Kiểm tra số lượng, mã đã lưu, trạng thái nơi và chính sách link đã chốt.
- [ ] Bước 4: Lưu phiên/tập lựa chọn và một grant khó đoán có hạn; chưa tự thêm người nhận.
- [ ] Bước 5: Trả link và trạng thái chờ người nhận, không index; ghi invite_created. Hệ thống không tự gửi tin nhắn cho người khác.
- [ ] Enforce RULE-07, RULE-08, RULE-09, RULE-12, RULE-17, RULE-25; xử lý Ít hơn 2/nhiều hơn 5, trùng mã, nơi chưa lưu hoặc không còn đủ điều kiện: chặn tạo và chỉ trường/lựa chọn cần sửa; không sinh link cho phiên lỗi.
- [ ] Hiện thực AF-01 và EX-01–04, đặc biệt xác minh trạng thái trước retry ghi, không báo sai kết quả vì side effect lỗi.

## Task 3 — Interface và UI

**Spec ref:** UC-007 §4–6/8; requirement chính §7–8.

- [ ] Sau thiết kế giải pháp, chốt contract interface theo input/output của UC và lỗi validation/quyền/trạng thái; không tự chọn chính sách TBD bằng default framework.
- [ ] Xây giao diện thao tác của actor Thành viên tạo lời mời, có loading/empty/error/retry, nhãn/lỗi theo trường và mobile/keyboard theo NFR.
- [ ] Tích hợp đúng các bước kiểm tra server; không dùng ẩn nút làm cơ chế phân quyền.
- [ ] Kết quả cần hiển thị: Phiên mới chứa tập hợp hợp lệ và link riêng, có tối đa một suất người nhận; danh sách gốc không bị công khai.

## Task 4 — Kiểm thử và bằng chứng

**Spec ref:** UC-007 §10 và Verification Report; requirement chính §12.

- [ ] Kiểm thử AC-013: Given Chọn 3 nơi đã lưu khác nhau đủ điều kiện; When tạo lời mời; Then sinh phiên/link riêng và cảnh báo quyền của người có link.
- [ ] Kiểm thử AC-014: Given Chọn 1 hoặc 6 nơi; When tạo lời mời; Then không tạo phiên và yêu cầu chọn 2–5 nơi.

- [ ] Kiểm thử EX-01 quyền trực tiếp/thay id; EX-02 boundary nghiệp vụ; EX-03 lỗi đọc/ghi/đồng thời; EX-04 side effect lỗi theo tác động thực tế.
- [ ] Ghi kết quả theo FR/UC/AC, dữ liệu và cấu hình đã duyệt; chỉ chạy kiểm thử tải/restore ở hạng mục phù hợp, không ghi “passed” khi chưa chạy.
