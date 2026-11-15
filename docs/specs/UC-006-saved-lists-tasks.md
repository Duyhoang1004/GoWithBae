# Implementation Tasks — UC-006: Quản lý danh sách đã lưu

**Spec:** [UC-006-saved-lists.md](./UC-006-saved-lists.md)  
**FR:** FR-09, FR-22  
**Điều kiện:** đây là backlog đề xuất. Chốt OQ-11, OQ-21 và yêu cầu dùng chung trước phần phụ thuộc. Không triển khai G2.

Mỗi task dưới đây là một PR dự kiến khi có codebase; có thể điều chỉnh kích thước sau thiết kế giải pháp. Không phải chỉ thị bắt đầu viết code trong lần lập tài liệu này.

## Task 1 — Dữ liệu và validation

**Spec ref:** UC-006 §2, §7–8; requirement chính §6.

- [ ] Xác định model/read model cần cho mục tiêu “Tổ chức các địa điểm yêu thích trong danh sách riêng của mình.”; tái dùng entity chung, không tạo entity trùng giữa các UC.
- [ ] Đặc tả và hiện thực validation: list_id: bắt buộc với đổi tên/xóa/thêm/bỏ, kiểm tra chủ sở hữu.
- [ ] Đặc tả và hiện thực validation: list_name: chuỗi khi tạo/đổi tên, không rỗng; độ dài OQ-21.
- [ ] Đặc tả và hiện thực validation: place_id: mã tồn tại khi thêm/bỏ; cặp list_id–place_id duy nhất; dữ liệu nơi ẩn giữ trong danh sách cũ.
- [ ] Khi có database hiện hữu, lập migration và kiểm tra không phá tham chiếu/lịch sử; hiện dự án mới có tài liệu nên chưa có migration cụ thể.

## Task 2 — Quyền và xử lý nghiệp vụ

**Spec ref:** UC-006 §2–7; requirement chính §3/5/7.

- [ ] Kiểm tra từng PRE ở bước 1 và quyền/trạng thái lại ở bước 3–4.
- [ ] Bước 1: Kiểm tra phiên, trạng thái tài khoản và chỉ nạp danh sách của chủ.
- [ ] Bước 2: Nhận tên/thao tác/mã địa điểm, xác định đối tượng sở hữu.
- [ ] Bước 3: Kiểm tra quyền, tên và mã, không tạo trùng; nơi ẩn không cho chốt mới.
- [ ] Bước 4: Tạo/đổi tên/xóa list hoặc thêm/bỏ item; không xóa place/plan; giữ cảnh báo nơi không hoạt động.
- [ ] Bước 5: Trả dữ liệu hiện tại; chỉ ghi place_saved khi thêm mới thành công.
- [ ] Enforce RULE-07, RULE-01, RULE-22, RULE-17, RULE-25; xử lý Không sở hữu, tài khoản khóa, tên không hợp lệ hoặc mã không tồn tại: không thay đổi. Thêm lặp trả trạng thái đã lưu, không sinh item/sự kiện lưu mới.
- [ ] Hiện thực AF-01 và EX-01–04, đặc biệt xác minh trạng thái trước retry ghi, không báo sai kết quả vì side effect lỗi.

## Task 3 — Interface và UI

**Spec ref:** UC-006 §4–6/8; requirement chính §7–8.

- [ ] Sau thiết kế giải pháp, chốt contract interface theo input/output của UC và lỗi validation/quyền/trạng thái; không tự chọn chính sách TBD bằng default framework.
- [ ] Xây giao diện thao tác của actor Thành viên, có loading/empty/error/retry, nhãn/lỗi theo trường và mobile/keyboard theo NFR.
- [ ] Tích hợp đúng các bước kiểm tra server; không dùng ẩn nút làm cơ chế phân quyền.
- [ ] Kết quả cần hiển thị: Danh sách riêng được cập nhật không trùng và không phá địa điểm/kế hoạch liên quan.

## Task 4 — Kiểm thử và bằng chứng

**Spec ref:** UC-006 §10 và Verification Report; requirement chính §12.

- [ ] Kiểm thử AC-011: Given Một nơi đã trong danh sách; When thêm lại cùng nơi; Then chỉ có một item và không tăng lượt lưu thành công.
- [ ] Kiểm thử AC-012: Given Danh sách có nơi đã dùng tạo plan; When xóa danh sách; Then plan và hồ sơ địa điểm không bị xóa.

- [ ] Kiểm thử EX-01 quyền trực tiếp/thay id; EX-02 boundary nghiệp vụ; EX-03 lỗi đọc/ghi/đồng thời; EX-04 side effect lỗi theo tác động thực tế.
- [ ] Ghi kết quả theo FR/UC/AC, dữ liệu và cấu hình đã duyệt; chỉ chạy kiểm thử tải/restore ở hạng mục phù hợp, không ghi “passed” khi chưa chạy.
