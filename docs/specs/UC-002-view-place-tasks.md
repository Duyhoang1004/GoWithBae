# Implementation Tasks — UC-002: Xem hồ sơ địa điểm

**Spec:** [UC-002-view-place.md](./UC-002-view-place.md)  
**FR:** FR-05  
**Điều kiện:** đây là backlog đề xuất. Chốt OQ-11 và yêu cầu dùng chung trước phần phụ thuộc. Không triển khai G2.

Mỗi task dưới đây là một PR dự kiến khi có codebase; có thể điều chỉnh kích thước sau thiết kế giải pháp. Không phải chỉ thị bắt đầu viết code trong lần lập tài liệu này.

## Task 1 — Dữ liệu và validation

**Spec ref:** UC-002 §2, §7–8; requirement chính §6.

- [ ] Xác định model/read model cần cho mục tiêu “Đọc thông tin đã duyệt của một chi nhánh để cân nhắc buổi hẹn.”; tái dùng entity chung, không tạo entity trùng giữa các UC.
- [ ] Đặc tả và hiện thực validation: place_id: mã chi nhánh bắt buộc, tồn tại và được phép xem.
- [ ] Đặc tả và hiện thực validation: contact_action: gọi/chỉ đường tùy chọn, chỉ dùng liên hệ chính thức đã có.
- [ ] Khi có database hiện hữu, lập migration và kiểm tra không phá tham chiếu/lịch sử; hiện dự án mới có tài liệu nên chưa có migration cụ thể.

## Task 2 — Quyền và xử lý nghiệp vụ

**Spec ref:** UC-002 §2–7; requirement chính §3/5/7.

- [ ] Kiểm tra từng PRE ở bước 1 và quyền/trạng thái lại ở bước 3–4.
- [ ] Bước 1: Kiểm tra mã và trạng thái công khai hoặc quyền xem lịch sử tương ứng.
- [ ] Bước 2: Nạp ảnh hợp lệ, giá hai người, trải nghiệm, tiện ích, giờ, địa chỉ, lưu ý và ngày kiểm tra.
- [ ] Bước 3: Phân biệt known/unknown; không thay dữ liệu thiếu bằng không có, 0 đồng hoặc sao giả.
- [ ] Bước 4: Trả liên hệ/địa chỉ có dữ liệu; thao tác lưu/so sánh/tạo kế hoạch chuyển UC tương ứng.
- [ ] Bước 5: Hiển thị đúng thứ tự và hành động; ghi place_viewed theo quy tắc phân tích.
- [ ] Enforce RULE-01, RULE-04, RULE-28, RULE-25; xử lý Mã không tồn tại/không được xem không trả nội dung riêng. Ảnh thiếu quyền sử dụng không được công bố; liên hệ thiếu không tạo số điện thoại hay đường dẫn giả.
- [ ] Hiện thực AF-01 và EX-01–04, đặc biệt xác minh trạng thái trước retry ghi, không báo sai kết quả vì side effect lỗi.

## Task 3 — Interface và UI

**Spec ref:** UC-002 §4–6/8; requirement chính §7–8.

- [ ] Sau thiết kế giải pháp, chốt contract interface theo input/output của UC và lỗi validation/quyền/trạng thái; không tự chọn chính sách TBD bằng default framework.
- [ ] Xây giao diện thao tác của actor Khách truy cập web, có loading/empty/error/retry, nhãn/lỗi theo trường và mobile/keyboard theo NFR.
- [ ] Tích hợp đúng các bước kiểm tra server; không dùng ẩn nút làm cơ chế phân quyền.
- [ ] Kết quả cần hiển thị: Người xem nhận hồ sơ được phép, có nguồn/ngày cập nhật và không bị dẫn hiểu sai bởi dữ liệu thiếu.

## Task 4 — Kiểm thử và bằng chứng

**Spec ref:** UC-002 §10 và Verification Report; requirement chính §12.

- [ ] Kiểm thử AC-003: Given Nơi công khai có trạng thái giá và giờ chưa rõ; When mở chi tiết; Then hiện rõ chưa có thông tin, không hiển thị miễn phí/đang mở hoặc số sao giả.
- [ ] Kiểm thử AC-004: Given Nơi bị ẩn và người xem không có quyền lịch sử; When mở URL trực tiếp; Then không nhận nội dung địa điểm bị hạn chế.

- [ ] Kiểm thử EX-01 quyền trực tiếp/thay id; EX-02 boundary nghiệp vụ; EX-03 lỗi đọc/ghi/đồng thời; EX-04 side effect lỗi theo tác động thực tế.
- [ ] Ghi kết quả theo FR/UC/AC, dữ liệu và cấu hình đã duyệt; chỉ chạy kiểm thử tải/restore ở hạng mục phù hợp, không ghi “passed” khi chưa chạy.
