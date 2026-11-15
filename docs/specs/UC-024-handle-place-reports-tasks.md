# Implementation Tasks — UC-024: Xử lý báo sai địa điểm

**Spec:** [UC-024-handle-place-reports.md](./UC-024-handle-place-reports.md)  
**FR:** FR-28, FR-31  
**Điều kiện:** đây là backlog đề xuất. Chốt OQ-14, OQ-18 và yêu cầu dùng chung trước phần phụ thuộc. Không triển khai G2.

Mỗi task dưới đây là một PR dự kiến khi có codebase; có thể điều chỉnh kích thước sau thiết kế giải pháp. Không phải chỉ thị bắt đầu viết code trong lần lập tài liệu này.

## Task 1 — Dữ liệu và validation

**Spec ref:** UC-024 §2, §7–8; requirement chính §6.

- [ ] Xác định model/read model cần cho mục tiêu “Kiểm tra thông tin phản ánh và lưu kết quả xử lý có căn cứ.”; tái dùng entity chung, không tạo entity trùng giữa các UC.
- [ ] Đặc tả và hiện thực validation: report_id: báo sai trong phạm vi; assignee: nhân viên hợp lệ khi giao việc.
- [ ] Đặc tả và hiện thực validation: status: Mới/Đang kiểm tra/Đã xử lý/Không đủ căn cứ.
- [ ] Đặc tả và hiện thực validation: resolution_note: ghi chú bắt buộc khi đóng; reopening: chỉ người có quyền.
- [ ] Khi có database hiện hữu, lập migration và kiểm tra không phá tham chiếu/lịch sử; hiện dự án mới có tài liệu nên chưa có migration cụ thể.

## Task 2 — Quyền và xử lý nghiệp vụ

**Spec ref:** UC-024 §2–7; requirement chính §3/5/7.

- [ ] Kiểm tra từng PRE ở bước 1 và quyền/trạng thái lại ở bước 3–4.
- [ ] Bước 1: Kiểm tra quyền/phạm vi, nạp trạng thái và người phụ trách.
- [ ] Bước 2: Chuyển Đang kiểm tra; hiển thị nội dung/ảnh theo quyền.
- [ ] Bước 3: Kiểm tra chuyển trạng thái, quyền mở lại, ghi chú bắt buộc; sửa nơi phải qua UC-018/019 đúng quyền.
- [ ] Bước 4: Ghi kết quả Đã xử lý/Không đủ căn cứ hoặc mở lại, người/thời gian/ghi chú và audit quan trọng.
- [ ] Bước 5: Giữ lịch sử, phản hồi/thông báo nếu chính sách đã duyệt; báo sai không tự sửa dữ liệu công khai.
- [ ] Enforce RULE-18, RULE-20, RULE-21, RULE-24; xử lý Đóng không ghi chú, sai quyền/trạng thái hoặc cố sửa công khai khi không có quyền duyệt: từ chối; bổ sung ghi chú/chuyển người có quyền rồi thử lại.
- [ ] Hiện thực AF-01 và EX-01–04, đặc biệt xác minh trạng thái trước retry ghi, không báo sai kết quả vì side effect lỗi.

## Task 3 — Interface và UI

**Spec ref:** UC-024 §4–6/8; requirement chính §7–8.

- [ ] Sau thiết kế giải pháp, chốt contract interface theo input/output của UC và lỗi validation/quyền/trạng thái; không tự chọn chính sách TBD bằng default framework.
- [ ] Xây giao diện thao tác của actor Nhân viên được phân quyền xử lý báo sai, có loading/empty/error/retry, nhãn/lỗi theo trường và mobile/keyboard theo NFR.
- [ ] Tích hợp đúng các bước kiểm tra server; không dùng ẩn nút làm cơ chế phân quyền.
- [ ] Kết quả cần hiển thị: Báo sai có kết quả/ghi chú và lịch sử; thay đổi địa điểm nếu có vẫn qua quy trình xuất bản.

## Task 4 — Kiểm thử và bằng chứng

**Spec ref:** UC-024 §10 và Verification Report; requirement chính §12.

- [ ] Kiểm thử AC-047: Given Báo sai đang kiểm tra; When đóng mà không có ghi chú; Then bị chặn và báo cần ghi chú xử lý.
- [ ] Kiểm thử AC-048: Given Nhân sự có quyền mở lại báo sai đã đóng; When mở lại; Then quay về kiểm tra và vẫn giữ kết quả/lịch sử trước.

- [ ] Kiểm thử EX-01 quyền trực tiếp/thay id; EX-02 boundary nghiệp vụ; EX-03 lỗi đọc/ghi/đồng thời; EX-04 side effect lỗi theo tác động thực tế.
- [ ] Ghi kết quả theo FR/UC/AC, dữ liệu và cấu hình đã duyệt; chỉ chạy kiểm thử tải/restore ở hạng mục phù hợp, không ghi “passed” khi chưa chạy.
