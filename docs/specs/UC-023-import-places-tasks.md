# Implementation Tasks — UC-023: Nhập địa điểm bằng CSV

**Spec:** [UC-023-import-places.md](./UC-023-import-places.md)  
**FR:** FR-30  
**Điều kiện:** đây là backlog đề xuất. Chốt OQ-19, OQ-21 và yêu cầu dùng chung trước phần phụ thuộc. Không triển khai G2.

Mỗi task dưới đây là một PR dự kiến khi có codebase; có thể điều chỉnh kích thước sau thiết kế giải pháp. Không phải chỉ thị bắt đầu viết code trong lần lập tài liệu này.

## Task 1 — Dữ liệu và validation

**Spec ref:** UC-023 §2, §7–8; requirement chính §6.

- [ ] Xác định model/read model cần cho mục tiêu “Kiểm tra và ghi lô địa điểm có báo lỗi theo dòng, không bỏ lỗi âm thầm.”; tái dùng entity chung, không tạo entity trùng giữa các UC.
- [ ] Đặc tả và hiện thực validation: csv_file: tệp đúng mẫu/encoding/giới hạn đã chốt; mỗi dòng có mã chi nhánh và các cột bắt buộc.
- [ ] Đặc tả và hiện thực validation: image_mapping: ảnh tải riêng gắn mã; ảnh/quyền theo quy định.
- [ ] Đặc tả và hiện thực validation: dry_run/batch_id: kiểm tra thử trước xác nhận ghi, giữ kết quả từng dòng.
- [ ] Khi có database hiện hữu, lập migration và kiểm tra không phá tham chiếu/lịch sử; hiện dự án mới có tài liệu nên chưa có migration cụ thể.

## Task 2 — Quyền và xử lý nghiệp vụ

**Spec ref:** UC-023 §2–7; requirement chính §3/5/7.

- [ ] Kiểm tra từng PRE ở bước 1 và quyền/trạng thái lại ở bước 3–4.
- [ ] Bước 1: Kiểm tra quyền, trả mẫu theo phiên bản và quy tắc nhập.
- [ ] Bước 2: Đọc tệp theo định dạng/giới hạn, chưa tạo hồ sơ công khai.
- [ ] Bước 3: Kiểm tra từng dòng: trường thiếu, mã trùng trong lô/hệ thống, tọa độ sai, quyền ảnh và định dạng.
- [ ] Bước 4: Theo PROP OQ-19, chỉ commit lô khi toàn bộ hợp lệ; kiểm tra lại trùng trước ghi; lưu dữ liệu ở quy trình nháp/duyệt, không tự công khai.
- [ ] Bước 5: Trả số dòng thành công/thất bại và lỗi theo dòng, truy nguyên batch; không âm thầm bỏ dòng lỗi.
- [ ] Enforce RULE-20, RULE-21, RULE-23, RULE-27, RULE-28; xử lý Tệp không đúng/không an toàn, thiếu trường, mã trùng, tọa độ ngoài biên hoặc lỗi commit: báo rõ dòng/lô, không bỏ qua. Nhập lại phải kiểm tra batch/kết quả hiện tại để tránh bản ghi trùng.
- [ ] Hiện thực AF-01 và EX-01–04, đặc biệt xác minh trạng thái trước retry ghi, không báo sai kết quả vì side effect lỗi.

## Task 3 — Interface và UI

**Spec ref:** UC-023 §4–6/8; requirement chính §7–8.

- [ ] Sau thiết kế giải pháp, chốt contract interface theo input/output của UC và lỗi validation/quyền/trạng thái; không tự chọn chính sách TBD bằng default framework.
- [ ] Xây giao diện thao tác của actor Nhân viên có quyền nhập dữ liệu, có loading/empty/error/retry, nhãn/lỗi theo trường và mobile/keyboard theo NFR.
- [ ] Tích hợp đúng các bước kiểm tra server; không dùng ẩn nút làm cơ chế phân quyền.
- [ ] Kết quả cần hiển thị: Kết quả lô rõ từng dòng và chỉ dữ liệu hợp lệ được ghi theo chính sách được duyệt, chưa tự xuất bản.

## Task 4 — Kiểm thử và bằng chứng

**Spec ref:** UC-023 §10 và Verification Report; requirement chính §12.

- [ ] Kiểm thử AC-045: Given Lô có một dòng thiếu tọa độ và một mã trùng; When chạy kiểm tra thử; Then báo đúng dòng/lý do và chưa tự ghi như thành công.
- [ ] Kiểm thử AC-046: Given CSV hợp lệ theo mẫu và chính sách commit đã duyệt; When xác nhận ghi; Then tạo dữ liệu ở quy trình nháp/duyệt và đối chiếu đủ số dòng.

- [ ] Kiểm thử EX-01 quyền trực tiếp/thay id; EX-02 boundary nghiệp vụ; EX-03 lỗi đọc/ghi/đồng thời; EX-04 side effect lỗi theo tác động thực tế.
- [ ] Ghi kết quả theo FR/UC/AC, dữ liệu và cấu hình đã duyệt; chỉ chạy kiểm thử tải/restore ở hạng mục phù hợp, không ghi “passed” khi chưa chạy.
