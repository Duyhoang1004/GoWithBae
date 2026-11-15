# Implementation Tasks — UC-022: Quản lý tài khoản và vai trò

**Spec:** [UC-022-admin-accounts.md](./UC-022-admin-accounts.md)  
**FR:** FR-27, FR-31  
**Điều kiện:** đây là backlog đề xuất. Chốt OQ-06, OQ-17, OQ-18 và yêu cầu dùng chung trước phần phụ thuộc. Không triển khai G2.

Mỗi task dưới đây là một PR dự kiến khi có codebase; có thể điều chỉnh kích thước sau thiết kế giải pháp. Không phải chỉ thị bắt đầu viết code trong lần lập tài liệu này.

## Task 1 — Dữ liệu và validation

**Spec ref:** UC-022 §2, §7–8; requirement chính §6.

- [ ] Xác định model/read model cần cho mục tiêu “Kiểm soát trạng thái tài khoản và quyền nhân viên có truy vết.”; tái dùng entity chung, không tạo entity trùng giữa các UC.
- [ ] Đặc tả và hiện thực validation: target_user_id: mã tài khoản đích tồn tại, không lấy quyền từ client.
- [ ] Đặc tả và hiện thực validation: action/roles: khóa/mở/gán/bỏ vai trò/xem nhật ký; mã vai trò hợp lệ.
- [ ] Đặc tả và hiện thực validation: reason: lý do bắt buộc khi khóa/mở và theo chính sách thay quyền.
- [ ] Khi có database hiện hữu, lập migration và kiểm tra không phá tham chiếu/lịch sử; hiện dự án mới có tài liệu nên chưa có migration cụ thể.

## Task 2 — Quyền và xử lý nghiệp vụ

**Spec ref:** UC-022 §2–7; requirement chính §3/5/7.

- [ ] Kiểm tra từng PRE ở bước 1 và quyền/trạng thái lại ở bước 3–4.
- [ ] Bước 1: Kiểm tra phiên và đúng quyền quản trị.
- [ ] Bước 2: Hiển thị trạng thái/quyền hiện tại, tiếp nhận lý do.
- [ ] Bước 3: Kiểm tra quyền, vai trò hợp lệ, lý do và chính sách tự đổi quyền/admin cuối theo OQ-18.
- [ ] Bước 4: Cập nhật đúng tài khoản, ghi audit người/thời gian/trước/sau/lý do; tác động phiên khóa theo OQ-17.
- [ ] Bước 5: Quyền mới được kiểm tra khi thao tác tiếp; nhân viên không sửa/xóa audit qua UI.
- [ ] Enforce RULE-17, RULE-21, RULE-27; xử lý Vai trò thấp gọi cấp quyền, lý do rỗng, role không hợp lệ hoặc vi phạm chính sách admin cuối: từ chối. Không che giấu thao tác thay quyền khỏi audit.
- [ ] Hiện thực AF-01 và EX-01–04, đặc biệt xác minh trạng thái trước retry ghi, không báo sai kết quả vì side effect lỗi.

## Task 3 — Interface và UI

**Spec ref:** UC-022 §4–6/8; requirement chính §7–8.

- [ ] Sau thiết kế giải pháp, chốt contract interface theo input/output của UC và lỗi validation/quyền/trạng thái; không tự chọn chính sách TBD bằng default framework.
- [ ] Xây giao diện thao tác của actor Quản trị cao nhất, có loading/empty/error/retry, nhãn/lỗi theo trường và mobile/keyboard theo NFR.
- [ ] Tích hợp đúng các bước kiểm tra server; không dùng ẩn nút làm cơ chế phân quyền.
- [ ] Kết quả cần hiển thị: Đúng tài khoản được thay đổi với nhật ký; không phát sinh quyền ngoài cấu hình được duyệt.

## Task 4 — Kiểm thử và bằng chứng

**Spec ref:** UC-022 §10 và Verification Report; requirement chính §12.

- [ ] Kiểm thử AC-043: Given Nhân viên hỗ trợ không có quyền cấp vai trò; When gọi trực tiếp gán quản trị; Then bị từ chối và quyền giữ nguyên.
- [ ] Kiểm thử AC-044: Given Quản trị có quyền khóa tài khoản với lý do; When lưu thao tác; Then tài khoản không tạo/sửa dữ liệu được và audit có trước/sau/lý do.

- [ ] Kiểm thử EX-01 quyền trực tiếp/thay id; EX-02 boundary nghiệp vụ; EX-03 lỗi đọc/ghi/đồng thời; EX-04 side effect lỗi theo tác động thực tế.
- [ ] Ghi kết quả theo FR/UC/AC, dữ liệu và cấu hình đã duyệt; chỉ chạy kiểm thử tải/restore ở hạng mục phù hợp, không ghi “passed” khi chưa chạy.
