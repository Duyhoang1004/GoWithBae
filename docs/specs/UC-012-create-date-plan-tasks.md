# Implementation Tasks — UC-012: Tạo kế hoạch buổi hẹn

**Spec:** [UC-012-create-date-plan.md](./UC-012-create-date-plan.md)  
**FR:** FR-15, FR-22  
**Điều kiện:** đây là backlog đề xuất. Chốt OQ-04, OQ-09, OQ-10, OQ-11, OQ-21 và yêu cầu dùng chung trước phần phụ thuộc. Không triển khai G2.

Mỗi task dưới đây là một PR dự kiến khi có codebase; có thể điều chỉnh kích thước sau thiết kế giải pháp. Không phải chỉ thị bắt đầu viết code trong lần lập tài liệu này.

## Task 1 — Dữ liệu và validation

**Spec ref:** UC-012 §2, §7–8; requirement chính §6.

- [ ] Xác định model/read model cần cho mục tiêu “Lưu kế hoạch một địa điểm cho hai người tại ngày giờ tương lai.”; tái dùng entity chung, không tạo entity trùng giữa các UC.
- [ ] Đặc tả và hiện thực validation: place_id: một mã chi nhánh hợp lệ đang công khai.
- [ ] Đặc tả và hiện thực validation: scheduled_at: thời gian tương lai so với máy chủ, hiển thị giờ Việt Nam.
- [ ] Đặc tả và hiện thực validation: notes: chuỗi tùy chọn, giới hạn OQ-21; recipient: tùy chọn, chỉ cấp theo cơ chế đã duyệt OQ-09.
- [ ] Khi có database hiện hữu, lập migration và kiểm tra không phá tham chiếu/lịch sử; hiện dự án mới có tài liệu nên chưa có migration cụ thể.

## Task 2 — Quyền và xử lý nghiệp vụ

**Spec ref:** UC-012 §2–7; requirement chính §3/5/7.

- [ ] Kiểm tra từng PRE ở bước 1 và quyền/trạng thái lại ở bước 3–4.
- [ ] Bước 1: Kiểm tra đăng nhập, tài khoản hoạt động và mã nơi hiện tại.
- [ ] Bước 2: Hiển thị giá hai người, lịch đã xác minh và lưu ý chưa giữ chỗ.
- [ ] Bước 3: Kiểm tra tương lai, trạng thái nơi; đóng cửa đã xác minh thì chặn, giờ chưa rõ thì cảnh báo.
- [ ] Bước 4: Tạo plan của đúng chủ với trạng thái Đã lên lịch, người nhận chỉ khi cấp quyền hợp lệ.
- [ ] Bước 5: Trả ngày giờ/địa điểm/ghi chú/trạng thái, ghi date_plan_created không trùng; không tạo đơn/giữ chỗ.
- [ ] Enforce RULE-01, RULE-04, RULE-05, RULE-13, RULE-14, RULE-17, RULE-25; xử lý Thời gian quá khứ, nơi đã ẩn/ngừng hoạt động, xác minh đóng cửa, tài khoản khóa hoặc dữ liệu thiếu: không lưu; yêu cầu đổi trường/nơi/giờ. Bấm gửi lặp không được tạo nhiều plan do cùng thao tác.
- [ ] Hiện thực AF-01 và EX-01–04, đặc biệt xác minh trạng thái trước retry ghi, không báo sai kết quả vì side effect lỗi.

## Task 3 — Interface và UI

**Spec ref:** UC-012 §4–6/8; requirement chính §7–8.

- [ ] Sau thiết kế giải pháp, chốt contract interface theo input/output của UC và lỗi validation/quyền/trạng thái; không tự chọn chính sách TBD bằng default framework.
- [ ] Xây giao diện thao tác của actor Thành viên, có loading/empty/error/retry, nhãn/lỗi theo trường và mobile/keyboard theo NFR.
- [ ] Tích hợp đúng các bước kiểm tra server; không dùng ẩn nút làm cơ chế phân quyền.
- [ ] Kết quả cần hiển thị: Kế hoạch của đúng chủ được lưu ở tương lai, không làm khách hiểu đã đặt chỗ.

## Task 4 — Kiểm thử và bằng chứng

**Spec ref:** UC-012 §10 và Verification Report; requirement chính §12.

- [ ] Kiểm thử AC-023: Given Nơi có giờ đã xác minh mở lúc hẹn và tài khoản hoạt động; When tạo plan với ngày giờ tương lai; Then lưu Đã lên lịch và hiển thị chưa giữ chỗ.
- [ ] Kiểm thử AC-024: Given Nơi đóng lúc hẹn hoặc thời điểm đã qua; When gửi tạo plan; Then không lưu và yêu cầu đổi thời gian.

- [ ] Kiểm thử EX-01 quyền trực tiếp/thay id; EX-02 boundary nghiệp vụ; EX-03 lỗi đọc/ghi/đồng thời; EX-04 side effect lỗi theo tác động thực tế.
- [ ] Ghi kết quả theo FR/UC/AC, dữ liệu và cấu hình đã duyệt; chỉ chạy kiểm thử tải/restore ở hạng mục phù hợp, không ghi “passed” khi chưa chạy.
