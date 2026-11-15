# GoWithBae — Backlog triển khai đề xuất G1

**Trạng thái:** Draft để lập kế hoạch, không phải cam kết chi phí/lịch. **Nguồn:** requirement chính và các use case. Scope web G1 + quản trị đã xác nhận; chưa có codebase ứng dụng.

## Cổng quyết định trước triển khai

1. PO chốt phạm vi chi tiết, các OQ chặn của module và ma trận quyền; không phải mọi OQ đều chặn mọi module.
2. Thiết kế/prototype có empty/error được duyệt trước xây UI. Đội giải pháp chọn công nghệ, data/API, nhà cung cấp và báo chi phí.
3. Mỗi task triển khai cần liên kết FR/UC/RULE/AC, có dữ liệu kiểm thử và người duyệt; các cấu hình đề xuất chưa được duyệt không dùng làm default chính sách.

## Thứ tự phụ thuộc đề xuất

| Đợt | Hạng mục | Đầu vào / điều kiện hoàn tất |
|---|---|---|
| 0 | Baseline và giải pháp | OQ-01/03–06/15/18/21–23 theo tác động; thiết kế màn hình, quyền, dịch vụ, dữ liệu; không tự ước lượng khi thiếu quyết định |
| 1 | Hạ tầng dùng chung, auth, quyền và kiểm soát dữ liệu | UC-004/005/022; NFR-04–08/12; audit, upload, email, môi trường; chính sách xóa/khóa theo OQ-17 |
| 2 | Dữ liệu nội dung và công cụ vận hành | UC-018–021/023; schema/mẫu CSV, nhãn, nguồn ảnh/giá/giờ, duyệt; không tự công khai dữ liệu import |
| 3 | Khám phá, chi tiết, so sánh, đã lưu | UC-001–003/006; bản đồ/giá/giờ/search đã chốt; dùng dữ liệu đã duyệt |
| 4 | Cùng chọn, chia sẻ, kế hoạch và số lần đã đi | UC-007–014/027; CR-001 đã CONF; chốt phần còn mở OQ-07–12/25; test phiên khách, grant thu hồi, dismiss không popup, count không trùng |
| 5 | Báo sai, hỗ trợ và thông báo | UC-015–017/024–025; OQ-13/14/16; liên kết quy trình duyệt địa điểm và quyền tệp |
| 6 | Analytics/báo cáo và nghiệm thu | UC-026; OQ-20/22; instrumentation phải đưa vào từng module từ đầu, không chờ cuối; dataset đối chiếu, NFR, restore và bàn giao |

Đợt là trình tự phụ thuộc, không phải sprint có thời lượng đã chốt. Các phần độc lập có thể do đội dự án sắp xếp song song sau khi có nhân sự; công việc phân tích này không khởi chạy agent hay lập trình song song.

## Task theo từng use case

| UC | Task breakdown | Phụ thuộc cần quyết định |
|---|---|---|
| UC-001 | [Tìm địa điểm phù hợp](../specs/UC-001-discover-places-tasks.md) | OQ-03, OQ-04, OQ-05, OQ-20 |
| UC-002 | [Xem hồ sơ địa điểm](../specs/UC-002-view-place-tasks.md) | OQ-11 |
| UC-003 | [So sánh địa điểm](../specs/UC-003-compare-places-tasks.md) | OQ-11 |
| UC-004 | [Đăng nhập tài khoản](../specs/UC-004-authenticate-tasks.md) | OQ-06, OQ-17 |
| UC-005 | [Quản lý tài khoản cá nhân](../specs/UC-005-manage-profile-tasks.md) | OQ-06, OQ-17, OQ-21 |
| UC-006 | [Quản lý danh sách đã lưu](../specs/UC-006-saved-lists-tasks.md) | OQ-11, OQ-21 |
| UC-007 | [Tạo phiên cùng chọn](../specs/UC-007-create-choice-tasks.md) | OQ-07, OQ-11 |
| UC-008 | [Tham gia phiên cùng chọn](../specs/UC-008-join-choice-tasks.md) | OQ-07, OQ-09, OQ-21 |
| UC-009 | [Bỏ phiếu và xem lựa chọn trùng](../specs/UC-009-vote-choice-tasks.md) | OQ-07, OQ-08, OQ-09 |
| UC-010 | [Chốt địa điểm từ phiên cùng chọn](../specs/UC-010-finalize-choice-tasks.md) | OQ-08, OQ-10, OQ-11 |
| UC-011 | [Thu hồi hoặc cấp lại lời mời](../specs/UC-011-manage-sharing-tasks.md) | OQ-07, OQ-09 |
| UC-012 | [Tạo kế hoạch buổi hẹn](../specs/UC-012-create-date-plan-tasks.md) | OQ-04, OQ-09, OQ-10, OQ-11, OQ-21 |
| UC-013 | [Xem và quản lý kế hoạch](../specs/UC-013-manage-date-plan-tasks.md) | OQ-09, OQ-10, OQ-11, OQ-12 |
| UC-014 | [Xử lý lời nhắc Đi chưa và xác nhận đã đi](../specs/UC-014-report-visit-tasks.md) | OQ-10, OQ-25 |
| UC-015 | [Báo thông tin địa điểm sai](../specs/UC-015-submit-place-report-tasks.md) | OQ-21 |
| UC-016 | [Gửi và theo dõi yêu cầu hỗ trợ](../specs/UC-016-customer-support-tasks.md) | OQ-13, OQ-21 |
| UC-017 | [Xem thông báo liên quan](../specs/UC-017-read-notifications-tasks.md) | OQ-09, OQ-14 |
| UC-018 | [Biên tập và gửi duyệt địa điểm](../specs/UC-018-edit-place-draft-tasks.md) | OQ-03, OQ-04, OQ-15, OQ-18, OQ-21 |
| UC-019 | [Duyệt và điều chỉnh hiển thị địa điểm](../specs/UC-019-moderate-place-tasks.md) | OQ-11, OQ-15, OQ-18 |
| UC-020 | [Quản lý danh mục và nhãn](../specs/UC-020-manage-taxonomy-tasks.md) | OQ-18 |
| UC-021 | [Biên tập bộ sưu tập khám phá](../specs/UC-021-manage-collections-tasks.md) | OQ-01, OQ-18, OQ-21 |
| UC-022 | [Quản lý tài khoản và vai trò](../specs/UC-022-admin-accounts-tasks.md) | OQ-06, OQ-17, OQ-18 |
| UC-023 | [Nhập địa điểm bằng CSV](../specs/UC-023-import-places-tasks.md) | OQ-19, OQ-21 |
| UC-024 | [Xử lý báo sai địa điểm](../specs/UC-024-handle-place-reports-tasks.md) | OQ-14, OQ-18 |
| UC-025 | [Xử lý yêu cầu hỗ trợ](../specs/UC-025-handle-support-tasks.md) | OQ-13, OQ-14, OQ-18, OQ-21 |
| UC-026 | [Xem báo cáo và xuất CSV](../specs/UC-026-reports-and-export-tasks.md) | OQ-18, OQ-20, OQ-22 |
| UC-027 | [Xem số lần đã đi tại địa điểm](../specs/UC-027-view-visit-count-tasks.md) | OQ-10, OQ-11, OQ-25 |

## Các task xuyên suốt không được bỏ sót

- Kiểm tra quyền từng tài nguyên, upload/tải tệp, thu hồi phiên/link, 2 bước nhân viên và bí mật không nằm trong log (FR-22/31, NFR-04–07).
- Thiết lập analytics event chống trùng ngay tại bước thành công; không thu ghi chú riêng/OTP, tách guest/user; kiểm tra số tổng bằng dữ liệu biết trước (FR-32–34).
- CR-001: nhắc tại giờ hẹn; Bỏ qua chỉ lưu dismissed và đóng, không tự ghi Không/đổi lịch/popup gợi ý; số lần FR-35 từ lượt Đã đi không trùng; người nhận link dùng phiên khách không cần tài khoản.
- Kiểm tra mobile 360 px/desktop/trình duyệt/bàn phím, loading/empty/error và bấm lặp theo NFR-02/03/08.
- Đo tìm kiếm p95 trên cấu hình đã chốt, chạy khôi phục backup, giám sát/email/bản đồ/jobs và hướng dẫn xử lý sự cố (NFR-01/10/11).
- Chuẩn bị tài khoản dự án, hướng dẫn cài đặt/nhập dữ liệu/duyệt/phân quyền/báo cáo/restore, chi phí dịch vụ, kết quả kiểm thử/lỗi và phạm vi bảo trì (requirement §14).

Không có task thanh toán/đặt chỗ/đối soát/app/AI trong backlog G1. Ước lượng ngày công, chi phí và deadline: TBD, owner PO + đội triển khai sau thiết kế giải pháp.
