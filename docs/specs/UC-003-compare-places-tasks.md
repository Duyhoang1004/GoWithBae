# Implementation Tasks — UC-003: So sánh địa điểm

**Spec:** [UC-003-compare-places.md](./UC-003-compare-places.md)  
**FR:** FR-06  
**Điều kiện:** đây là backlog đề xuất. Chốt OQ-11 và yêu cầu dùng chung trước phần phụ thuộc. Không triển khai G2.

Mỗi task dưới đây là một PR dự kiến khi có codebase; có thể điều chỉnh kích thước sau thiết kế giải pháp. Không phải chỉ thị bắt đầu viết code trong lần lập tài liệu này.

## Task 1 — Dữ liệu và validation

**Spec ref:** UC-003 §2, §7–8; requirement chính §6.

- [ ] Xác định model/read model cần cho mục tiêu “Đối chiếu 2–3 địa điểm trên cùng tiêu chí và chọn nơi muốn lên kế hoạch.”; tái dùng entity chung, không tạo entity trùng giữa các UC.
- [ ] Đặc tả và hiện thực validation: place_ids: tập 2–3 mã khác nhau; thêm/bỏ theo thao tác; không thêm trùng.
- [ ] Đặc tả và hiện thực validation: origin: điểm xuất phát tùy chọn để có khoảng cách ước tính.
- [ ] Khi có database hiện hữu, lập migration và kiểm tra không phá tham chiếu/lịch sử; hiện dự án mới có tài liệu nên chưa có migration cụ thể.

## Task 2 — Quyền và xử lý nghiệp vụ

**Spec ref:** UC-003 §2–7; requirement chính §3/5/7.

- [ ] Kiểm tra từng PRE ở bước 1 và quyền/trạng thái lại ở bước 3–4.
- [ ] Bước 1: Kiểm tra các địa điểm đã chọn được phép xem.
- [ ] Bước 2: Duy trì tập mã khác nhau; cập nhật số lượng hiện tại.
- [ ] Bước 3: Kiểm tra đủ 2, tối đa 3 nơi, trường thiếu và trạng thái hiện tại.
- [ ] Bước 4: Nạp giá hai người, khu vực/khoảng cách, giờ, hoạt động, không khí, tiện ích và đặt trước.
- [ ] Bước 5: Hiển thị bảng; điều hướng UC-012 khi chọn nơi, chưa tạo kế hoạch; ghi comparison_opened.
- [ ] Enforce RULE-01, RULE-04, RULE-06, RULE-25; xử lý Nơi thứ tư bị chặn với hướng dẫn bỏ một nơi; thiếu 2 nơi gợi ý thêm; chọn trùng không tạo cột mới. Trạng thái nơi thay đổi yêu cầu nạp lại và kiểm tra trước chốt.
- [ ] Hiện thực AF-01 và EX-01–04, đặc biệt xác minh trạng thái trước retry ghi, không báo sai kết quả vì side effect lỗi.

## Task 3 — Interface và UI

**Spec ref:** UC-003 §4–6/8; requirement chính §7–8.

- [ ] Sau thiết kế giải pháp, chốt contract interface theo input/output của UC và lỗi validation/quyền/trạng thái; không tự chọn chính sách TBD bằng default framework.
- [ ] Xây giao diện thao tác của actor Khách truy cập web, có loading/empty/error/retry, nhãn/lỗi theo trường và mobile/keyboard theo NFR.
- [ ] Tích hợp đúng các bước kiểm tra server; không dùng ẩn nút làm cơ chế phân quyền.
- [ ] Kết quả cần hiển thị: Bảng không quá 3 nơi, cùng tiêu chí, dữ liệu thiếu ghi đúng; không tự tạo kế hoạch.

## Task 4 — Kiểm thử và bằng chứng

**Spec ref:** UC-003 §10 và Verification Report; requirement chính §12.

- [ ] Kiểm thử AC-005: Given Đã chọn 3 nơi khác nhau; When thêm nơi thứ tư; Then không thêm và hướng dẫn bỏ một nơi trước.
- [ ] Kiểm thử AC-006: Given Hai nơi có một tiện ích chưa biết; When xem bảng so sánh; Then trường thiếu ghi chưa có thông tin và không bị biến thành Không có.

- [ ] Kiểm thử EX-01 quyền trực tiếp/thay id; EX-02 boundary nghiệp vụ; EX-03 lỗi đọc/ghi/đồng thời; EX-04 side effect lỗi theo tác động thực tế.
- [ ] Ghi kết quả theo FR/UC/AC, dữ liệu và cấu hình đã duyệt; chỉ chạy kiểm thử tải/restore ở hạng mục phù hợp, không ghi “passed” khi chưa chạy.
