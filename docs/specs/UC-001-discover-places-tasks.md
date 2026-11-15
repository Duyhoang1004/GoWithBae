# Implementation Tasks — UC-001: Tìm địa điểm phù hợp

**Spec:** [UC-001-discover-places.md](./UC-001-discover-places.md)  
**FR:** FR-01, FR-02, FR-03, FR-04  
**Điều kiện:** đây là backlog đề xuất. Chốt OQ-03, OQ-04, OQ-05, OQ-20 và yêu cầu dùng chung trước phần phụ thuộc. Không triển khai G2.

Mỗi task dưới đây là một PR dự kiến khi có codebase; có thể điều chỉnh kích thước sau thiết kế giải pháp. Không phải chỉ thị bắt đầu viết code trong lần lập tài liệu này.

## Task 1 — Dữ liệu và validation

**Spec ref:** UC-001 §2, §7–8; requirement chính §6.

- [ ] Xác định model/read model cần cho mục tiêu “Nhận danh sách địa điểm công khai đáp ứng từ khóa và điều kiện đã chọn.”; tái dùng entity chung, không tạo entity trùng giữa các UC.
- [ ] Đặc tả và hiện thực validation: query: chuỗi tùy chọn — tên/khu vực/hoạt động; tìm không dấu và không phân biệt hoa/thường.
- [ ] Đặc tả và hiện thực validation: filters: tập mã danh mục hợp lệ; ngân sách VND cho hai người; ngày giờ Việt Nam; tọa độ/bán kính theo OQ-05.
- [ ] Đặc tả và hiện thực validation: sort: mặc định, gần nhất hoặc giá tăng; gần nhất phải có điểm xuất phát phù hợp.
- [ ] Đặc tả và hiện thực validation: collection_id: mã tùy chọn của bộ sưu tập trong lịch hiển thị; không cho vượt điều kiện xuất bản.
- [ ] Khi có database hiện hữu, lập migration và kiểm tra không phá tham chiếu/lịch sử; hiện dự án mới có tài liệu nên chưa có migration cụ thể.

## Task 2 — Quyền và xử lý nghiệp vụ

**Spec ref:** UC-001 §2–7; requirement chính §3/5/7.

- [ ] Kiểm tra từng PRE ở bước 1 và quyền/trạng thái lại ở bước 3–4.
- [ ] Bước 1: Kiểm tra điều kiện công khai và chọn nguồn địa điểm được phép hiển thị.
- [ ] Bước 2: Chuẩn hóa từ khóa và giá trị, giữ điều kiện người dùng chọn; chưa tự gửi mỗi ký tự thành lượt tìm.
- [ ] Bước 3: Kiểm tra nhãn, giá, lịch mở cửa, vị trí; áp dụng AND/OR, chỉ dữ liệu đạt điều kiện và thứ tự ổn định.
- [ ] Bước 4: Truy vấn kết quả/thẻ/số lượng và khoảng cách ước tính khi đủ gốc; không cần đăng nhập.
- [ ] Bước 5: Hiển thị kết quả/empty và cách mở rộng do người dùng chọn; ghi search_submitted/results_shown chống trùng.
- [ ] Enforce RULE-01, RULE-02, RULE-03, RULE-04, RULE-05, RULE-25; xử lý Mã lọc sai, bán kính không hợp lệ, giá/giờ chưa rõ mà bộ lọc yêu cầu có thông tin, hoặc địa điểm không công khai: không đưa nơi đó vào kết quả; báo lỗi trường nếu đầu vào sai. 0 kết quả là kết quả hợp lệ, không phải lỗi hệ thống.
- [ ] Hiện thực AF-01 và EX-01–04, đặc biệt xác minh trạng thái trước retry ghi, không báo sai kết quả vì side effect lỗi.

## Task 3 — Interface và UI

**Spec ref:** UC-001 §4–6/8; requirement chính §7–8.

- [ ] Sau thiết kế giải pháp, chốt contract interface theo input/output của UC và lỗi validation/quyền/trạng thái; không tự chọn chính sách TBD bằng default framework.
- [ ] Xây giao diện thao tác của actor Khách truy cập web, có loading/empty/error/retry, nhãn/lỗi theo trường và mobile/keyboard theo NFR.
- [ ] Tích hợp đúng các bước kiểm tra server; không dùng ẩn nút làm cơ chế phân quyền.
- [ ] Kết quả cần hiển thị: Kết quả, điều kiện lọc, sắp xếp và thông tin chưa rõ nhất quán; sự kiện tìm/kết quả được ghi không trùng.

## Task 4 — Kiểm thử và bằng chứng

**Spec ref:** UC-001 §10 và Verification Report; requirement chính §12.

- [ ] Kiểm thử AC-001: Given Có nơi A giá 300.000 cho hai người và đủ hai tiện ích, nơi B thiếu một tiện ích; When tìm không dấu và lọc 200.000–<400.000 cùng hai tiện ích; Then chỉ nơi A đạt lọc, ghi đúng một lượt tìm.
- [ ] Kiểm thử AC-002: Given Bản đồ lỗi và người dùng không cấp vị trí; When mở kết quả theo khu vực nhập thủ công; Then danh sách vẫn dùng được và bộ lọc không tự thay đổi.

- [ ] Kiểm thử EX-01 quyền trực tiếp/thay id; EX-02 boundary nghiệp vụ; EX-03 lỗi đọc/ghi/đồng thời; EX-04 side effect lỗi theo tác động thực tế.
- [ ] Ghi kết quả theo FR/UC/AC, dữ liệu và cấu hình đã duyệt; chỉ chạy kiểm thử tải/restore ở hạng mục phù hợp, không ghi “passed” khi chưa chạy.
