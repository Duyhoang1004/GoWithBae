# Implementation Tasks — UC-015: Báo thông tin địa điểm sai

**Spec:** [UC-015-submit-place-report.md](./UC-015-submit-place-report.md)  
**FR:** FR-19  
**Điều kiện:** đây là backlog đề xuất. Chốt OQ-21 và yêu cầu dùng chung trước phần phụ thuộc. Không triển khai G2.

Mỗi task dưới đây là một PR dự kiến khi có codebase; có thể điều chỉnh kích thước sau thiết kế giải pháp. Không phải chỉ thị bắt đầu viết code trong lần lập tài liệu này.

## Task 1 — Dữ liệu và validation

**Spec ref:** UC-015 §2, §7–8; requirement chính §6.

- [ ] Xác định model/read model cần cho mục tiêu “Gửi thông tin nghi sai để nhân sự kiểm tra.”; tái dùng entity chung, không tạo entity trùng giữa các UC.
- [ ] Đặc tả và hiện thực validation: place_id: mã địa điểm hợp lệ.
- [ ] Đặc tả và hiện thực validation: reason: sai giá/giờ/địa chỉ/đã đóng cửa/ảnh không đúng/khác.
- [ ] Đặc tả và hiện thực validation: description: mô tả theo cấu hình OQ-21; evidence: ảnh tùy chọn qua kiểm tra tệp.
- [ ] Khi có database hiện hữu, lập migration và kiểm tra không phá tham chiếu/lịch sử; hiện dự án mới có tài liệu nên chưa có migration cụ thể.

## Task 2 — Quyền và xử lý nghiệp vụ

**Spec ref:** UC-015 §2–7; requirement chính §3/5/7.

- [ ] Kiểm tra từng PRE ở bước 1 và quyền/trạng thái lại ở bước 3–4.
- [ ] Bước 1: Kiểm tra mã nơi và giới hạn gửi, lấy người gửi nếu đã đăng nhập.
- [ ] Bước 2: Nhận nội dung; không bắt buộc đăng ký.
- [ ] Bước 3: Kiểm tra reason, trường nội dung và ảnh theo hạn mức/validation đã chốt.
- [ ] Bước 4: Tạo mã, gắn địa điểm/người gửi nếu có/thời gian, trạng thái Mới; không sửa hồ sơ công khai.
- [ ] Bước 5: Trả mã báo sai và hướng dẫn phù hợp; không công khai nội dung/người gửi cho khách khác.
- [ ] Enforce RULE-18, RULE-27; xử lý Reason sai, ảnh không hợp lệ hoặc gửi vượt giới hạn: chặn phần không hợp lệ, hướng dẫn sửa/chờ. Đóng báo sai sau này cần ghi chú qua UC-024.
- [ ] Hiện thực AF-01 và EX-01–04, đặc biệt xác minh trạng thái trước retry ghi, không báo sai kết quả vì side effect lỗi.

## Task 3 — Interface và UI

**Spec ref:** UC-015 §4–6/8; requirement chính §7–8.

- [ ] Sau thiết kế giải pháp, chốt contract interface theo input/output của UC và lỗi validation/quyền/trạng thái; không tự chọn chính sách TBD bằng default framework.
- [ ] Xây giao diện thao tác của actor Khách hoặc thành viên, có loading/empty/error/retry, nhãn/lỗi theo trường và mobile/keyboard theo NFR.
- [ ] Tích hợp đúng các bước kiểm tra server; không dùng ẩn nút làm cơ chế phân quyền.
- [ ] Kết quả cần hiển thị: Báo sai mới có mã và được đưa vào hàng chờ; hồ sơ địa điểm chưa tự thay đổi.

## Task 4 — Kiểm thử và bằng chứng

**Spec ref:** UC-015 §10 và Verification Report; requirement chính §12.

- [ ] Kiểm thử AC-029: Given Khách chưa đăng nhập có nội dung hợp lệ; When gửi báo sai không kèm ảnh; Then tạo mã/trạng thái Mới mà không buộc đăng ký.
- [ ] Kiểm thử AC-030: Given Giá địa điểm bị khách báo sai; When báo sai được tiếp nhận; Then giá công khai chưa tự đổi trước kiểm tra/duyệt.

- [ ] Kiểm thử EX-01 quyền trực tiếp/thay id; EX-02 boundary nghiệp vụ; EX-03 lỗi đọc/ghi/đồng thời; EX-04 side effect lỗi theo tác động thực tế.
- [ ] Ghi kết quả theo FR/UC/AC, dữ liệu và cấu hình đã duyệt; chỉ chạy kiểm thử tải/restore ở hạng mục phù hợp, không ghi “passed” khi chưa chạy.
