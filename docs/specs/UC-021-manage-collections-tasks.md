# Implementation Tasks — UC-021: Biên tập bộ sưu tập khám phá

**Spec:** [UC-021-manage-collections.md](./UC-021-manage-collections.md)  
**FR:** FR-26  
**Điều kiện:** đây là backlog đề xuất. Chốt OQ-01, OQ-18, OQ-21 và yêu cầu dùng chung trước phần phụ thuộc. Không triển khai G2.

Mỗi task dưới đây là một PR dự kiến khi có codebase; có thể điều chỉnh kích thước sau thiết kế giải pháp. Không phải chỉ thị bắt đầu viết code trong lần lập tài liệu này.

## Task 1 — Dữ liệu và validation

**Spec ref:** UC-021 §2, §7–8; requirement chính §6.

- [ ] Xác định model/read model cần cho mục tiêu “Lên nội dung khám phá theo tập địa điểm và lịch hiển thị.”; tái dùng entity chung, không tạo entity trùng giữa các UC.
- [ ] Đặc tả và hiện thực validation: title/content: tiêu đề và nội dung theo form đã duyệt.
- [ ] Đặc tả và hiện thực validation: place_ids/order: các mã và thứ tự, chỉ nơi công khai được hiển thị cho khách.
- [ ] Đặc tả và hiện thực validation: image/start_at/end_at: ảnh có quyền, thời gian kết thúc sau bắt đầu nếu có cả hai.
- [ ] Khi có database hiện hữu, lập migration và kiểm tra không phá tham chiếu/lịch sử; hiện dự án mới có tài liệu nên chưa có migration cụ thể.

## Task 2 — Quyền và xử lý nghiệp vụ

**Spec ref:** UC-021 §2–7; requirement chính §3/5/7.

- [ ] Kiểm tra từng PRE ở bước 1 và quyền/trạng thái lại ở bước 3–4.
- [ ] Bước 1: Kiểm tra quyền theo ma trận đã duyệt.
- [ ] Bước 2: Nhận tập mã nơi và dữ liệu nội dung, không tự lấy ảnh bên ngoài.
- [ ] Bước 3: Kiểm tra quyền ảnh, đầu vào, lịch và điều kiện công khai của các nơi.
- [ ] Bước 4: Ghi nội dung/thứ tự/lịch và trạng thái theo quy trình duyệt đã thống nhất.
- [ ] Bước 5: Bộ sưu tập chỉ hiện đúng lịch, chỉ nơi còn công khai; không kéo nơi đã ẩn trở lại kết quả.
- [ ] Enforce RULE-01, RULE-21, RULE-27, RULE-28; xử lý Không có quyền, ảnh sai quyền, lịch không hợp lệ hoặc mã nơi không hợp lệ: không lưu/xuất bản sai; báo trường để sửa. Chưa có quyền duyệt cụ thể thì không tự cấp.
- [ ] Hiện thực AF-01 và EX-01–04, đặc biệt xác minh trạng thái trước retry ghi, không báo sai kết quả vì side effect lỗi.

## Task 3 — Interface và UI

**Spec ref:** UC-021 §4–6/8; requirement chính §7–8.

- [ ] Sau thiết kế giải pháp, chốt contract interface theo input/output của UC và lỗi validation/quyền/trạng thái; không tự chọn chính sách TBD bằng default framework.
- [ ] Xây giao diện thao tác của actor Nhân viên có quyền bộ sưu tập, có loading/empty/error/retry, nhãn/lỗi theo trường và mobile/keyboard theo NFR.
- [ ] Tích hợp đúng các bước kiểm tra server; không dùng ẩn nút làm cơ chế phân quyền.
- [ ] Kết quả cần hiển thị: Bộ sưu tập được lưu có lịch/thứ tự và chỉ nội dung đủ điều kiện đến người dùng.

## Task 4 — Kiểm thử và bằng chứng

**Spec ref:** UC-021 §10 và Verification Report; requirement chính §12.

- [ ] Kiểm thử AC-041: Given Bộ sưu tập hợp lệ đang trong lịch hiển thị; When khách mở Khám phá; Then xem các nơi còn công khai theo thứ tự đã cấu hình.
- [ ] Kiểm thử AC-042: Given Một nơi trong bộ sưu tập vừa bị ẩn; When khách mở bộ sưu tập; Then không công khai lại nơi đó qua bộ sưu tập.

- [ ] Kiểm thử EX-01 quyền trực tiếp/thay id; EX-02 boundary nghiệp vụ; EX-03 lỗi đọc/ghi/đồng thời; EX-04 side effect lỗi theo tác động thực tế.
- [ ] Ghi kết quả theo FR/UC/AC, dữ liệu và cấu hình đã duyệt; chỉ chạy kiểm thử tải/restore ở hạng mục phù hợp, không ghi “passed” khi chưa chạy.
