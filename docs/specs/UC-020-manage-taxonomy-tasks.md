# Implementation Tasks — UC-020: Quản lý danh mục và nhãn

**Spec:** [UC-020-manage-taxonomy.md](./UC-020-manage-taxonomy.md)  
**FR:** FR-25  
**Điều kiện:** đây là backlog đề xuất. Chốt OQ-18 và yêu cầu dùng chung trước phần phụ thuộc. Không triển khai G2.

Mỗi task dưới đây là một PR dự kiến khi có codebase; có thể điều chỉnh kích thước sau thiết kế giải pháp. Không phải chỉ thị bắt đầu viết code trong lần lập tài liệu này.

## Task 1 — Dữ liệu và validation

**Spec ref:** UC-020 §2, §7–8; requirement chính §6.

- [ ] Xác định model/read model cần cho mục tiêu “Duy trì bộ phân loại phục vụ nhập dữ liệu và lọc địa điểm.”; tái dùng entity chung, không tạo entity trùng giữa các UC.
- [ ] Đặc tả và hiện thực validation: tag_id/group: mã và nhóm loại hình/nhãn/khu vực hợp lệ.
- [ ] Đặc tả và hiện thực validation: name/status: tên không rỗng, trạng thái dùng/ngừng dùng; gộp/xóa theo chính sách đã duyệt.
- [ ] Khi có database hiện hữu, lập migration và kiểm tra không phá tham chiếu/lịch sử; hiện dự án mới có tài liệu nên chưa có migration cụ thể.

## Task 2 — Quyền và xử lý nghiệp vụ

**Spec ref:** UC-020 §2–7; requirement chính §3/5/7.

- [ ] Kiểm tra từng PRE ở bước 1 và quyền/trạng thái lại ở bước 3–4.
- [ ] Bước 1: Kiểm tra quyền theo nhóm danh mục.
- [ ] Bước 2: Nhận tên, nhóm, mã và hiển thị nơi đang tham chiếu nếu cần.
- [ ] Bước 3: Kiểm tra nhóm, giá trị hợp lệ, trùng và ảnh hưởng tham chiếu; không đổi logic AND/OR bằng dữ liệu sai nhóm.
- [ ] Bước 4: Cập nhật/ngừng dùng mà không xóa dữ liệu cũ hoặc quan hệ lịch sử.
- [ ] Bước 5: Trả trạng thái; form nhập/lọc tuân chính sách nhãn ngừng dùng đã chốt.
- [ ] Enforce RULE-02, RULE-21, RULE-22; xử lý Sai quyền, nhóm không hợp lệ, tên rỗng hoặc xóa gây mất dữ liệu cũ: chặn và đề nghị sửa/ngừng dùng. Quy tắc trùng/gộp chưa duyệt không tự hợp nhất.
- [ ] Hiện thực AF-01 và EX-01–04, đặc biệt xác minh trạng thái trước retry ghi, không báo sai kết quả vì side effect lỗi.

## Task 3 — Interface và UI

**Spec ref:** UC-020 §4–6/8; requirement chính §7–8.

- [ ] Sau thiết kế giải pháp, chốt contract interface theo input/output của UC và lỗi validation/quyền/trạng thái; không tự chọn chính sách TBD bằng default framework.
- [ ] Xây giao diện thao tác của actor Nhân viên có quyền danh mục, có loading/empty/error/retry, nhãn/lỗi theo trường và mobile/keyboard theo NFR.
- [ ] Tích hợp đúng các bước kiểm tra server; không dùng ẩn nút làm cơ chế phân quyền.
- [ ] Kết quả cần hiển thị: Danh mục cập nhật đúng quyền và không làm mất lịch sử hoặc thay nghĩa bộ lọc.

## Task 4 — Kiểm thử và bằng chứng

**Spec ref:** UC-020 §10 và Verification Report; requirement chính §12.

- [ ] Kiểm thử AC-039: Given Nhãn đang gắn nhiều nơi; When ngừng dùng nhãn; Then dữ liệu cũ vẫn tồn tại và không xóa địa điểm.
- [ ] Kiểm thử AC-040: Given Nhân viên không có quyền danh mục; When gọi sửa nhóm nhãn; Then bị từ chối.

- [ ] Kiểm thử EX-01 quyền trực tiếp/thay id; EX-02 boundary nghiệp vụ; EX-03 lỗi đọc/ghi/đồng thời; EX-04 side effect lỗi theo tác động thực tế.
- [ ] Ghi kết quả theo FR/UC/AC, dữ liệu và cấu hình đã duyệt; chỉ chạy kiểm thử tải/restore ở hạng mục phù hợp, không ghi “passed” khi chưa chạy.
