# Implementation Tasks — UC-026: Xem báo cáo và xuất CSV

**Spec:** [UC-026-reports-and-export.md](./UC-026-reports-and-export.md)  
**FR:** FR-32, FR-33, FR-34  
**Điều kiện:** đây là backlog đề xuất. Chốt OQ-18, OQ-20, OQ-22 và yêu cầu dùng chung trước phần phụ thuộc. Không triển khai G2.

Mỗi task dưới đây là một PR dự kiến khi có codebase; có thể điều chỉnh kích thước sau thiết kế giải pháp. Không phải chỉ thị bắt đầu viết code trong lần lập tài liệu này.

## Task 1 — Dữ liệu và validation

**Spec ref:** UC-026 §2, §7–8; requirement chính §6.

- [ ] Xác định model/read model cần cho mục tiêu “Đo hành vi sử dụng/chất lượng dữ liệu và xuất kết quả đúng bộ lọc/quyền.”; tái dùng entity chung, không tạo entity trùng giữa các UC.
- [ ] Đặc tả và hiện thực validation: date_from/date_to: khoảng thời gian hợp lệ theo giờ Việt Nam.
- [ ] Đặc tả và hiện thực validation: area/type/platform: mã hợp lệ trong phạm vi quyền.
- [ ] Đặc tả và hiện thực validation: export: yêu cầu tùy chọn có quyền xuất, cùng bộ lọc; dữ liệu chỉ các trường được cho phép.
- [ ] Khi có database hiện hữu, lập migration và kiểm tra không phá tham chiếu/lịch sử; hiện dự án mới có tài liệu nên chưa có migration cụ thể.

## Task 2 — Quyền và xử lý nghiệp vụ

**Spec ref:** UC-026 §2–7; requirement chính §3/5/7.

- [ ] Kiểm tra từng PRE ở bước 1 và quyền/trạng thái lại ở bước 3–4.
- [ ] Bước 1: Kiểm tra quyền xem và phạm vi dữ liệu; không mặc định mọi nhân viên đều được xem/xuất.
- [ ] Bước 2: Giữ bộ lọc, xác định kỳ/cohort và thời điểm cập nhật.
- [ ] Bước 3: Kiểm tra khoảng ngày/mã/quyền xuất; loại trùng sự kiện, tách tài khoản/khách, không ghi dữ liệu nhạy cảm.
- [ ] Bước 4: Tính công thức §10, chất lượng dữ liệu; CSV dùng cùng phạm vi/bộ lọc và chỉ cột cho phép.
- [ ] Bước 5: Hiển thị dữ liệu cập nhật trong 24 giờ, mẫu số 0/chưa đủ 30 ngày là chưa có/đủ dữ liệu; kết quả đối chiếu được.
- [ ] Enforce RULE-21, RULE-25, RULE-26; xử lý Khoảng ngày/mã sai, xem vượt quyền, xuất không quyền hoặc dữ liệu xuất lỗi: không xuất phần ngoài phạm vi, báo lỗi/thử lại. Event lặp không tăng tổng; dữ liệu chưa có không biến thành tỷ lệ giả.
- [ ] Hiện thực AF-01 và EX-01–04, đặc biệt xác minh trạng thái trước retry ghi, không báo sai kết quả vì side effect lỗi.

## Task 3 — Interface và UI

**Spec ref:** UC-026 §4–6/8; requirement chính §7–8.

- [ ] Sau thiết kế giải pháp, chốt contract interface theo input/output của UC và lỗi validation/quyền/trạng thái; không tự chọn chính sách TBD bằng default framework.
- [ ] Xây giao diện thao tác của actor Nhân viên có quyền báo cáo, có loading/empty/error/retry, nhãn/lỗi theo trường và mobile/keyboard theo NFR.
- [ ] Tích hợp đúng các bước kiểm tra server; không dùng ẩn nút làm cơ chế phân quyền.
- [ ] Kết quả cần hiển thị: Báo cáo và CSV thống nhất cùng bộ lọc/quyền, theo đúng công thức và không lộ dữ liệu riêng.

## Task 4 — Kiểm thử và bằng chứng

**Spec ref:** UC-026 §10 và Verification Report; requirement chính §12.

- [ ] Kiểm thử AC-051: Given Dataset biết trước có 10 lượt tìm, 2 lượt không ra kết quả, event lặp; When xem báo cáo cùng kỳ; Then tỷ lệ không ra 20%, event lặp không cộng thêm và CSV khớp bộ lọc.
- [ ] Kiểm thử AC-052: Given Tổng lượt tìm bằng 0 hoặc cohort mới 7 ngày; When xem tỷ lệ không ra/quay lại 30 ngày; Then hiện chưa có/chưa đủ dữ liệu, không chia lỗi hoặc gán tỷ lệ sai.

- [ ] Kiểm thử EX-01 quyền trực tiếp/thay id; EX-02 boundary nghiệp vụ; EX-03 lỗi đọc/ghi/đồng thời; EX-04 side effect lỗi theo tác động thực tế.
- [ ] Ghi kết quả theo FR/UC/AC, dữ liệu và cấu hình đã duyệt; chỉ chạy kiểm thử tải/restore ở hạng mục phù hợp, không ghi “passed” khi chưa chạy.
