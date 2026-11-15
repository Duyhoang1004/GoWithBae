# Implementation Tasks — UC-013: Xem và quản lý kế hoạch

**Spec:** [UC-013-manage-date-plan.md](./UC-013-manage-date-plan.md)  
**FR:** FR-16, FR-17, FR-22  
**Điều kiện:** đây là backlog đề xuất. Chốt OQ-09, OQ-10, OQ-11, OQ-12 và yêu cầu dùng chung trước phần phụ thuộc. Không triển khai G2.

Mỗi task dưới đây là một PR dự kiến khi có codebase; có thể điều chỉnh kích thước sau thiết kế giải pháp. Không phải chỉ thị bắt đầu viết code trong lần lập tài liệu này.

## Task 1 — Dữ liệu và validation

**Spec ref:** UC-013 §2, §7–8; requirement chính §6.

- [ ] Xác định model/read model cần cho mục tiêu “Xem, sửa hoặc hủy kế hoạch của mình và dùng chỉ đường/thêm lịch.”; tái dùng entity chung, không tạo entity trùng giữa các UC.
- [ ] Đặc tả và hiện thực validation: plan_id: thuộc chủ sở hữu; đọc người nhận theo quyền chia sẻ riêng.
- [ ] Đặc tả và hiện thực validation: action: xem/sửa giờ, ghi chú, đổi nơi, hủy, chỉ đường hoặc thêm lịch.
- [ ] Đặc tả và hiện thực validation: new_place_id/new_time/notes: kiểm tra lại như tạo khi đổi nơi, điều kiện sửa sau giờ OQ-10.
- [ ] Đặc tả và hiện thực validation: calendar_mode: theo OQ-12, không mặc định sync hai chiều.
- [ ] Khi có database hiện hữu, lập migration và kiểm tra không phá tham chiếu/lịch sử; hiện dự án mới có tài liệu nên chưa có migration cụ thể.

## Task 2 — Quyền và xử lý nghiệp vụ

**Spec ref:** UC-013 §2–7; requirement chính §3/5/7.

- [ ] Kiểm tra từng PRE ở bước 1 và quyền/trạng thái lại ở bước 3–4.
- [ ] Bước 1: Kiểm tra phiên, chủ sở hữu hoặc quyền chỉ xem của người nhận; ẩn hành động không được phép.
- [ ] Bước 2: Hiển thị dữ liệu hiện tại và người nhận liên quan; không cho người nhận sửa.
- [ ] Bước 3: Kiểm tra lại quyền/trạng thái; đổi nơi như lúc tạo, thời gian và giờ mở cửa theo chính sách đã duyệt.
- [ ] Bước 4: Cập nhật/hủy và updated_at; tạo thông báo cho tài khoản liên quan; không tạo đặt chỗ.
- [ ] Bước 5: Trả plan hiện tại; khách nhận link xem bản mới khi mở lại; lịch/chỉ đường lỗi không xóa plan.
- [ ] Enforce RULE-01, RULE-05, RULE-13, RULE-14, RULE-15, RULE-17, RULE-24; xử lý Người nhận/người ngoài sửa, nơi mới ẩn/đóng, thay id hoặc trạng thái không cho sửa: từ chối. Thông báo/email lỗi sau lưu không đảo ngược thay đổi; người dùng vẫn thấy trạng thái đã lưu và hệ thống theo dõi lỗi.
- [ ] Hiện thực AF-01 và EX-01–04, đặc biệt xác minh trạng thái trước retry ghi, không báo sai kết quả vì side effect lỗi.

## Task 3 — Interface và UI

**Spec ref:** UC-013 §4–6/8; requirement chính §7–8.

- [ ] Sau thiết kế giải pháp, chốt contract interface theo input/output của UC và lỗi validation/quyền/trạng thái; không tự chọn chính sách TBD bằng default framework.
- [ ] Xây giao diện thao tác của actor Người tạo kế hoạch, có loading/empty/error/retry, nhãn/lỗi theo trường và mobile/keyboard theo NFR.
- [ ] Tích hợp đúng các bước kiểm tra server; không dùng ẩn nút làm cơ chế phân quyền.
- [ ] Kết quả cần hiển thị: Plan phản ánh thay đổi của chủ và thời điểm cập nhật; người nhận chỉ thấy bản được phép; lịch ngoài chỉ được cập nhật theo cơ chế đã duyệt.

## Task 4 — Kiểm thử và bằng chứng

**Spec ref:** UC-013 §10 và Verification Report; requirement chính §12.

- [ ] Kiểm thử AC-025: Given Người nhận có quyền xem plan; When gọi trực tiếp thao tác sửa/hủy; Then bị từ chối và plan giữ nguyên.
- [ ] Kiểm thử AC-026: Given Chủ tạo đổi nơi sang nơi đã ẩn; When lưu thay đổi; Then bị chặn, không thay thế địa điểm hiện tại.

- [ ] Kiểm thử EX-01 quyền trực tiếp/thay id; EX-02 boundary nghiệp vụ; EX-03 lỗi đọc/ghi/đồng thời; EX-04 side effect lỗi theo tác động thực tế.
- [ ] Ghi kết quả theo FR/UC/AC, dữ liệu và cấu hình đã duyệt; chỉ chạy kiểm thử tải/restore ở hạng mục phù hợp, không ghi “passed” khi chưa chạy.
