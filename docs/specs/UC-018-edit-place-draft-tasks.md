# Implementation Tasks — UC-018: Biên tập và gửi duyệt địa điểm

**Spec:** [UC-018-edit-place-draft.md](./UC-018-edit-place-draft.md)  
**FR:** FR-23, FR-31  
**Điều kiện:** đây là backlog đề xuất. Chốt OQ-03, OQ-04, OQ-15, OQ-18, OQ-21 và yêu cầu dùng chung trước phần phụ thuộc. Không triển khai G2.

Mỗi task dưới đây là một PR dự kiến khi có codebase; có thể điều chỉnh kích thước sau thiết kế giải pháp. Không phải chỉ thị bắt đầu viết code trong lần lập tài liệu này.

## Task 1 — Dữ liệu và validation

**Spec ref:** UC-018 §2, §7–8; requirement chính §6.

- [ ] Xác định model/read model cần cho mục tiêu “Chuẩn bị hồ sơ chi nhánh có dữ liệu/nguồn hợp lệ để người có quyền duyệt.”; tái dùng entity chung, không tạo entity trùng giữa các UC.
- [ ] Đặc tả và hiện thực validation: place fields: mã chi nhánh, tên, loại hình, địa chỉ, khu vực, tọa độ, mô tả, hoạt động chính.
- [ ] Đặc tả và hiện thực validation: price/hour_status: known/unknown bắt buộc, dữ liệu thực theo trạng thái, nguồn và ngày kiểm tra.
- [ ] Đặc tả và hiện thực validation: images: ít nhất một ảnh có quyền trước gửi xuất bản; metadata nguồn/quyền, thứ tự/ảnh đại diện.
- [ ] Đặc tả và hiện thực validation: draft_id: đúng bản được quyền sửa, không ghi đè công khai bỏ duyệt.
- [ ] Khi có database hiện hữu, lập migration và kiểm tra không phá tham chiếu/lịch sử; hiện dự án mới có tài liệu nên chưa có migration cụ thể.

## Task 2 — Quyền và xử lý nghiệp vụ

**Spec ref:** UC-018 §2–7; requirement chính §3/5/7.

- [ ] Kiểm tra từng PRE ở bước 1 và quyền/trạng thái lại ở bước 3–4.
- [ ] Bước 1: Kiểm tra phiên, vai trò biên tập và bản nháp đang cho phép sửa.
- [ ] Bước 2: Nhận hồ sơ/ảnh/nguồn, giữ unknown đúng nghĩa; xem trước không xuất bản.
- [ ] Bước 3: Kiểm tra trường xuất bản, tọa độ/giá/giờ/ảnh có quyền và dấu hiệu trùng chi nhánh.
- [ ] Bước 4: Ghi dữ liệu, chuyển Chờ duyệt khi đủ điều kiện; lưu người thực hiện và nhật ký thay đổi quan trọng.
- [ ] Bước 5: Trả mã/trạng thái và lỗi cần sửa; chỉ bản đã được duyệt mới ra công khai.
- [ ] Enforce RULE-04, RULE-20, RULE-21, RULE-22, RULE-27, RULE-28; xử lý Thiếu trường để xuất bản, ảnh không có quyền/tệp sai, mã/toạ độ/giá không hợp lệ: không chuyển sang công khai/chờ duyệt sai; cho sửa dữ liệu. Biên tập gọi thao tác xuất bản trực tiếp bị chặn.
- [ ] Hiện thực AF-01 và EX-01–04, đặc biệt xác minh trạng thái trước retry ghi, không báo sai kết quả vì side effect lỗi.

## Task 3 — Interface và UI

**Spec ref:** UC-018 §4–6/8; requirement chính §7–8.

- [ ] Sau thiết kế giải pháp, chốt contract interface theo input/output của UC và lỗi validation/quyền/trạng thái; không tự chọn chính sách TBD bằng default framework.
- [ ] Xây giao diện thao tác của actor Nhân viên biên tập, có loading/empty/error/retry, nhãn/lỗi theo trường và mobile/keyboard theo NFR.
- [ ] Tích hợp đúng các bước kiểm tra server; không dùng ẩn nút làm cơ chế phân quyền.
- [ ] Kết quả cần hiển thị: Bản nháp/Chờ duyệt có dữ liệu và truy nguyên; không thay thế bản công khai ngoài quy trình duyệt.

## Task 4 — Kiểm thử và bằng chứng

**Spec ref:** UC-018 §10 và Verification Report; requirement chính §12.

- [ ] Kiểm thử AC-035: Given Nơi unknown giá/giờ nhưng đủ trạng thái, nguồn và các trường xuất bản khác; When gửi duyệt; Then không tự gán giá/giờ và vẫn chuyển Chờ duyệt khi đủ điều kiện.
- [ ] Kiểm thử AC-036: Given Ảnh không có quyền hoặc thiếu tọa độ; When gửi duyệt; Then báo lỗi trường và không xuất bản.

- [ ] Kiểm thử EX-01 quyền trực tiếp/thay id; EX-02 boundary nghiệp vụ; EX-03 lỗi đọc/ghi/đồng thời; EX-04 side effect lỗi theo tác động thực tế.
- [ ] Ghi kết quả theo FR/UC/AC, dữ liệu và cấu hình đã duyệt; chỉ chạy kiểm thử tải/restore ở hạng mục phù hợp, không ghi “passed” khi chưa chạy.
