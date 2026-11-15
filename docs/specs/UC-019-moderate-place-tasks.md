# Implementation Tasks — UC-019: Duyệt và điều chỉnh hiển thị địa điểm

**Spec:** [UC-019-moderate-place.md](./UC-019-moderate-place.md)  
**FR:** FR-24, FR-31  
**Điều kiện:** đây là backlog đề xuất. Chốt OQ-11, OQ-15, OQ-18 và yêu cầu dùng chung trước phần phụ thuộc. Không triển khai G2.

Mỗi task dưới đây là một PR dự kiến khi có codebase; có thể điều chỉnh kích thước sau thiết kế giải pháp. Không phải chỉ thị bắt đầu viết code trong lần lập tài liệu này.

## Task 1 — Dữ liệu và validation

**Spec ref:** UC-019 §2, §7–8; requirement chính §6.

- [ ] Xác định model/read model cần cho mục tiêu “Chỉ công bố hồ sơ đạt chất lượng, ẩn/ngừng hoạt động có kiểm soát.”; tái dùng entity chung, không tạo entity trùng giữa các UC.
- [ ] Đặc tả và hiện thực validation: place_version_id: bản cần duyệt/ẩn đúng trạng thái.
- [ ] Đặc tả và hiện thực validation: decision: duyệt/trả về/tạm ẩn/ngừng hoạt động; chỉ quyền được cấp.
- [ ] Đặc tả và hiện thực validation: reason: bắt buộc khi trả về và các thao tác yêu cầu lý do/audit; source/check_date: kiểm tra trước xuất bản.
- [ ] Khi có database hiện hữu, lập migration và kiểm tra không phá tham chiếu/lịch sử; hiện dự án mới có tài liệu nên chưa có migration cụ thể.

## Task 2 — Quyền và xử lý nghiệp vụ

**Spec ref:** UC-019 §2–7; requirement chính §3/5/7.

- [ ] Kiểm tra từng PRE ở bước 1 và quyền/trạng thái lại ở bước 3–4.
- [ ] Bước 1: Kiểm tra quyền và trạng thái/phiên bản hiện hành.
- [ ] Bước 2: Hiển thị đầy đủ hồ sơ/xem trước và dấu hiệu trùng, không tự duyệt thay người.
- [ ] Bước 3: Kiểm tra điều kiện xuất bản, quyền, trạng thái, lý do và giới hạn tự duyệt đã thống nhất.
- [ ] Bước 4: Duyệt sang Đang hiển thị, trả về Nháp, ẩn/ngừng hoạt động theo trạng thái; ghi audit trước/sau/lý do, giữ tham chiếu lịch sử.
- [ ] Bước 5: Tìm kiếm chỉ nhận bản đang hiển thị; danh sách/kế hoạch cũ xử lý theo quyền và cảnh báo, không bị xóa dây chuyền.
- [ ] Enforce RULE-01, RULE-20, RULE-21, RULE-22, RULE-28; xử lý Thiếu điều kiện, không có quyền, lý do bắt buộc rỗng, trạng thái thay đổi hoặc cố xóa phá lịch sử: từ chối; nạp lại hồ sơ và sửa/kiểm tra trước thử lại.
- [ ] Hiện thực AF-01 và EX-01–04, đặc biệt xác minh trạng thái trước retry ghi, không báo sai kết quả vì side effect lỗi.

## Task 3 — Interface và UI

**Spec ref:** UC-019 §4–6/8; requirement chính §7–8.

- [ ] Sau thiết kế giải pháp, chốt contract interface theo input/output của UC và lỗi validation/quyền/trạng thái; không tự chọn chính sách TBD bằng default framework.
- [ ] Xây giao diện thao tác của actor Nhân viên duyệt nội dung, có loading/empty/error/retry, nhãn/lỗi theo trường và mobile/keyboard theo NFR.
- [ ] Tích hợp đúng các bước kiểm tra server; không dùng ẩn nút làm cơ chế phân quyền.
- [ ] Kết quả cần hiển thị: Công khai chỉ dữ liệu đủ điều kiện, trạng thái và nhật ký nhất quán, lịch sử không mất.

## Task 4 — Kiểm thử và bằng chứng

**Spec ref:** UC-019 §10 và Verification Report; requirement chính §12.

- [ ] Kiểm thử AC-037: Given Biên tập không có quyền duyệt; When gọi trực tiếp xuất bản; Then bị từ chối và nơi không ra công khai.
- [ ] Kiểm thử AC-038: Given Nơi đã nằm trong kế hoạch; When nhân sự tạm ẩn nơi; Then nơi không xuất hiện trong tìm mới, tham chiếu kế hoạch cũ còn tồn tại.

- [ ] Kiểm thử EX-01 quyền trực tiếp/thay id; EX-02 boundary nghiệp vụ; EX-03 lỗi đọc/ghi/đồng thời; EX-04 side effect lỗi theo tác động thực tế.
- [ ] Ghi kết quả theo FR/UC/AC, dữ liệu và cấu hình đã duyệt; chỉ chạy kiểm thử tải/restore ở hạng mục phù hợp, không ghi “passed” khi chưa chạy.
