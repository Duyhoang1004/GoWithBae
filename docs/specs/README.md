# GoWithBae G1 — Danh mục use case và nghiệm thu

**Draft v1.1, 15-11-2026.** Đọc [requirement chính](../requirements/GoWithBae_Requirements_G1_v1.1.md) và [verification](../requirements/Verification_Report.md) trước dùng để triển khai. CR-001 xác nhận số lần đã đi, Bỏ qua lời nhắc chỉ đóng và xem link bằng khách không đăng nhập; các OQ còn lại chỉ chặn phần chưa rõ.

Mỗi UC có một mục tiêu/primary actor, tài liệu theo template skill, sequence Mermaid và task breakdown. UC có vòng đời còn có state diagram ở file riêng để mỗi file `.mermaid` hợp lệ với một loại sơ đồ. Nội dung diagram được nhúng lại trong UC để dễ xem.

| UC | Mục tiêu / actor | FR | AC | Diagram | Task |
|---|---|---|---|---|---|
| [UC-001](UC-001-discover-places.md) | Tìm địa điểm phù hợp / Khách truy cập web | FR-01, FR-02, FR-03, FR-04 | AC-001, AC-002 | [Sequence](UC-001-discover-places.mermaid) | [Tasks](UC-001-discover-places-tasks.md) |
| [UC-002](UC-002-view-place.md) | Xem hồ sơ địa điểm / Khách truy cập web | FR-05 | AC-003, AC-004 | [Sequence](UC-002-view-place.mermaid) | [Tasks](UC-002-view-place-tasks.md) |
| [UC-003](UC-003-compare-places.md) | So sánh địa điểm / Khách truy cập web | FR-06 | AC-005, AC-006 | [Sequence](UC-003-compare-places.mermaid) | [Tasks](UC-003-compare-places-tasks.md) |
| [UC-004](UC-004-authenticate.md) | Đăng nhập tài khoản / Người dùng muốn đăng nhập | FR-07 | AC-007, AC-008 | [Sequence](UC-004-authenticate.mermaid) | [Tasks](UC-004-authenticate-tasks.md) |
| [UC-005](UC-005-manage-profile.md) | Quản lý tài khoản cá nhân / Thành viên | FR-08, FR-22 | AC-009, AC-010 | [Sequence](UC-005-manage-profile.mermaid) | [Tasks](UC-005-manage-profile-tasks.md) |
| [UC-006](UC-006-saved-lists.md) | Quản lý danh sách đã lưu / Thành viên | FR-09, FR-22 | AC-011, AC-012 | [Sequence](UC-006-saved-lists.mermaid) | [Tasks](UC-006-saved-lists-tasks.md) |
| [UC-007](UC-007-create-choice.md) | Tạo phiên cùng chọn / Thành viên tạo lời mời | FR-10 | AC-013, AC-014 | [Sequence](UC-007-create-choice.mermaid) / [State](UC-007-create-choice-state.mermaid) | [Tasks](UC-007-create-choice-tasks.md) |
| [UC-008](UC-008-join-choice.md) | Tham gia phiên cùng chọn / Người nhận lời mời | FR-11, FR-22 | AC-015, AC-016, AC-055, AC-056 | [Sequence](UC-008-join-choice.mermaid) / [State](UC-008-join-choice-state.mermaid) | [Tasks](UC-008-join-choice-tasks.md) |
| [UC-009](UC-009-vote-choice.md) | Bỏ phiếu và xem lựa chọn trùng / Người tham gia phiên cùng chọn | FR-12, FR-22 | AC-017, AC-018 | [Sequence](UC-009-vote-choice.mermaid) / [State](UC-009-vote-choice-state.mermaid) | [Tasks](UC-009-vote-choice-tasks.md) |
| [UC-010](UC-010-finalize-choice.md) | Chốt địa điểm từ phiên cùng chọn / Người tạo phiên | FR-13 | AC-019, AC-020 | [Sequence](UC-010-finalize-choice.mermaid) / [State](UC-010-finalize-choice-state.mermaid) | [Tasks](UC-010-finalize-choice-tasks.md) |
| [UC-011](UC-011-manage-sharing.md) | Thu hồi hoặc cấp lại lời mời / Người tạo phiên | FR-14, FR-22 | AC-021, AC-022 | [Sequence](UC-011-manage-sharing.mermaid) / [State](UC-011-manage-sharing-state.mermaid) | [Tasks](UC-011-manage-sharing-tasks.md) |
| [UC-012](UC-012-create-date-plan.md) | Tạo kế hoạch buổi hẹn / Thành viên | FR-15, FR-22 | AC-023, AC-024 | [Sequence](UC-012-create-date-plan.mermaid) / [State](UC-012-create-date-plan-state.mermaid) | [Tasks](UC-012-create-date-plan-tasks.md) |
| [UC-013](UC-013-manage-date-plan.md) | Xem và quản lý kế hoạch / Người tạo kế hoạch | FR-16, FR-17, FR-22 | AC-025, AC-026 | [Sequence](UC-013-manage-date-plan.mermaid) / [State](UC-013-manage-date-plan-state.mermaid) | [Tasks](UC-013-manage-date-plan-tasks.md) |
| [UC-014](UC-014-report-visit.md) | Xử lý lời nhắc Đi chưa và xác nhận đã đi / Người được nhắc có quyền theo OQ-10 | FR-18, FR-35 | AC-027, AC-028, AC-057, AC-058 | [Sequence](UC-014-report-visit.mermaid) / [State](UC-014-report-visit-state.mermaid) | [Tasks](UC-014-report-visit-tasks.md) |
| [UC-015](UC-015-submit-place-report.md) | Báo thông tin địa điểm sai / Khách hoặc thành viên | FR-19 | AC-029, AC-030 | [Sequence](UC-015-submit-place-report.mermaid) | [Tasks](UC-015-submit-place-report-tasks.md) |
| [UC-016](UC-016-customer-support.md) | Gửi và theo dõi yêu cầu hỗ trợ / Thành viên | FR-20, FR-22 | AC-031, AC-032 | [Sequence](UC-016-customer-support.mermaid) | [Tasks](UC-016-customer-support-tasks.md) |
| [UC-017](UC-017-read-notifications.md) | Xem thông báo liên quan / Thành viên | FR-21, FR-22 | AC-033, AC-034 | [Sequence](UC-017-read-notifications.mermaid) | [Tasks](UC-017-read-notifications-tasks.md) |
| [UC-018](UC-018-edit-place-draft.md) | Biên tập và gửi duyệt địa điểm / Nhân viên biên tập | FR-23, FR-31 | AC-035, AC-036 | [Sequence](UC-018-edit-place-draft.mermaid) / [State](UC-018-edit-place-draft-state.mermaid) | [Tasks](UC-018-edit-place-draft-tasks.md) |
| [UC-019](UC-019-moderate-place.md) | Duyệt và điều chỉnh hiển thị địa điểm / Nhân viên duyệt nội dung | FR-24, FR-31 | AC-037, AC-038 | [Sequence](UC-019-moderate-place.mermaid) / [State](UC-019-moderate-place-state.mermaid) | [Tasks](UC-019-moderate-place-tasks.md) |
| [UC-020](UC-020-manage-taxonomy.md) | Quản lý danh mục và nhãn / Nhân viên có quyền danh mục | FR-25 | AC-039, AC-040 | [Sequence](UC-020-manage-taxonomy.mermaid) | [Tasks](UC-020-manage-taxonomy-tasks.md) |
| [UC-021](UC-021-manage-collections.md) | Biên tập bộ sưu tập khám phá / Nhân viên có quyền bộ sưu tập | FR-26 | AC-041, AC-042 | [Sequence](UC-021-manage-collections.mermaid) | [Tasks](UC-021-manage-collections-tasks.md) |
| [UC-022](UC-022-admin-accounts.md) | Quản lý tài khoản và vai trò / Quản trị cao nhất | FR-27, FR-31 | AC-043, AC-044 | [Sequence](UC-022-admin-accounts.mermaid) | [Tasks](UC-022-admin-accounts-tasks.md) |
| [UC-023](UC-023-import-places.md) | Nhập địa điểm bằng CSV / Nhân viên có quyền nhập dữ liệu | FR-30 | AC-045, AC-046 | [Sequence](UC-023-import-places.mermaid) | [Tasks](UC-023-import-places-tasks.md) |
| [UC-024](UC-024-handle-place-reports.md) | Xử lý báo sai địa điểm / Nhân viên được phân quyền xử lý báo sai | FR-28, FR-31 | AC-047, AC-048 | [Sequence](UC-024-handle-place-reports.mermaid) / [State](UC-024-handle-place-reports-state.mermaid) | [Tasks](UC-024-handle-place-reports-tasks.md) |
| [UC-025](UC-025-handle-support.md) | Xử lý yêu cầu hỗ trợ / Nhân viên hỗ trợ | FR-29 | AC-049, AC-050 | [Sequence](UC-025-handle-support.mermaid) / [State](UC-025-handle-support-state.mermaid) | [Tasks](UC-025-handle-support-tasks.md) |
| [UC-026](UC-026-reports-and-export.md) | Xem báo cáo và xuất CSV / Nhân viên có quyền báo cáo | FR-32, FR-33, FR-34 | AC-051, AC-052 | [Sequence](UC-026-reports-and-export.mermaid) | [Tasks](UC-026-reports-and-export-tasks.md) |
| [UC-027](UC-027-view-visit-count.md) | Xem số lần đã đi tại địa điểm / Thành viên | FR-35, FR-22 | AC-053, AC-054 | [Sequence](UC-027-view-visit-count.mermaid) | [Tasks](UC-027-view-visit-count-tasks.md) |

## Tiêu chí Given/When/Then tập trung

| AC | UC | Given | When | Then |
|---|---|---|---|---|
| AC-001 | UC-001 | Có nơi A giá 300.000 cho hai người và đủ hai tiện ích, nơi B thiếu một tiện ích | tìm không dấu và lọc 200.000–<400.000 cùng hai tiện ích | chỉ nơi A đạt lọc, ghi đúng một lượt tìm |
| AC-002 | UC-001 | Bản đồ lỗi và người dùng không cấp vị trí | mở kết quả theo khu vực nhập thủ công | danh sách vẫn dùng được và bộ lọc không tự thay đổi |
| AC-003 | UC-002 | Nơi công khai có trạng thái giá và giờ chưa rõ | mở chi tiết | hiện rõ chưa có thông tin, không hiển thị miễn phí/đang mở hoặc số sao giả |
| AC-004 | UC-002 | Nơi bị ẩn và người xem không có quyền lịch sử | mở URL trực tiếp | không nhận nội dung địa điểm bị hạn chế |
| AC-005 | UC-003 | Đã chọn 3 nơi khác nhau | thêm nơi thứ tư | không thêm và hướng dẫn bỏ một nơi trước |
| AC-006 | UC-003 | Hai nơi có một tiện ích chưa biết | xem bảng so sánh | trường thiếu ghi chưa có thông tin và không bị biến thành Không có |
| AC-007 | UC-004 | Mã đúng, còn hạn và tài khoản hoạt động | xác minh mã | tạo phiên đúng tài khoản và mã không dùng lại được |
| AC-008 | UC-004 | Mã đã dùng hoặc hết hạn | xác minh lại | không cấp phiên mới, có hướng dẫn lấy mã hợp lệ |
| AC-009 | UC-005 | Email mới chưa xác minh | yêu cầu thay email | email cũ chưa bị thay và người dùng được hướng dẫn xác minh |
| AC-010 | UC-005 | Yêu cầu xóa đã được xử lý theo chính sách | dùng lại phiên hoặc link riêng cũ | không truy cập tiếp được bằng phiên/link đã thu hồi |
| AC-011 | UC-006 | Một nơi đã trong danh sách | thêm lại cùng nơi | chỉ có một item và không tăng lượt lưu thành công |
| AC-012 | UC-006 | Danh sách có nơi đã dùng tạo plan | xóa danh sách | plan và hồ sơ địa điểm không bị xóa |
| AC-013 | UC-007 | Chọn 3 nơi đã lưu khác nhau đủ điều kiện | tạo lời mời | sinh phiên/link riêng và cảnh báo quyền của người có link |
| AC-014 | UC-007 | Chọn 1 hoặc 6 nơi | tạo lời mời | không tạo phiên và yêu cầu chọn 2–5 nơi |
| AC-015 | UC-008 | Hai người khác nhau dùng link để tham gia cùng lúc | hai yêu cầu chiếm suất được xử lý | chỉ một người nhận thành công và tập lựa chọn không bị thay |
| AC-016 | UC-008 | Link đã thu hồi | mở hoặc gửi tham gia | bị chặn và không nhận nội dung phiên |
| AC-055 | UC-008 | Trình duyệt chưa đăng nhập và có link hợp lệ | mở link và tham gia bằng tên hiển thị | xem và tham gia bằng phiên khách, không bị đưa sang đăng nhập |
| AC-056 | UC-008 | Khách đang xem một nơi qua link hợp lệ | bấm Lưu yêu thích | yêu cầu đăng nhập cho thao tác cá nhân, quyền xem link không bị biến thành bắt buộc đăng nhập |
| AC-017 | UC-009 | Cả hai Thích A, một người chưa trả lời B | xem kết quả | A nằm trong nhóm trùng, B không được xem là cả hai thích |
| AC-018 | UC-009 | Phiên đã chốt hoặc quyền bị thu hồi | gửi sửa phiếu | phiếu không thay đổi và nhận hướng dẫn trạng thái/quyền |
| AC-019 | UC-010 | Chủ tạo chọn nơi hợp lệ và thời gian mở cửa tương lai | lưu kế hoạch từ phiên | tạo plan và phiên chỉ xem, không tự chọn theo phiếu |
| AC-020 | UC-010 | Lưu kế hoạch thất bại | thực hiện chốt | không báo chốt thành công và có thể kiểm tra/thử lại an toàn |
| AC-021 | UC-011 | Người nhận đang mở trang với phiên riêng | chủ tạo thu hồi cấp quyền rồi người nhận gửi yêu cầu tiếp | máy chủ từ chối cả URL cũ và phiên bị thu hồi |
| AC-022 | UC-011 | Một người không sở hữu phiên | gọi thu hồi lời mời | không thay đổi quyền chia sẻ |
| AC-023 | UC-012 | Nơi có giờ đã xác minh mở lúc hẹn và tài khoản hoạt động | tạo plan với ngày giờ tương lai | lưu Đã lên lịch và hiển thị chưa giữ chỗ |
| AC-024 | UC-012 | Nơi đóng lúc hẹn hoặc thời điểm đã qua | gửi tạo plan | không lưu và yêu cầu đổi thời gian |
| AC-025 | UC-013 | Người nhận có quyền xem plan | gọi trực tiếp thao tác sửa/hủy | bị từ chối và plan giữ nguyên |
| AC-026 | UC-013 | Chủ tạo đổi nơi sang nơi đã ẩn | lưu thay đổi | bị chặn, không thay thế địa điểm hiện tại |
| AC-027 | UC-014 | Plan đã đến giờ nhưng người dùng chưa trả lời | chỉ xem lời nhắc | visit_answer vẫn Chưa trả lời và count không tăng |
| AC-028 | UC-014 | Plan đã hủy trước giờ | tác vụ thời gian chạy sau giờ hẹn | plan vẫn Đã hủy, không hiện nhắc và không tự xác nhận đã đến/đã qua giờ |
| AC-057 | UC-014 | Kế hoạch chưa hủy đã đến scheduled_at, lời nhắc còn hiệu lực và người có quyền đang mở web | đến thời điểm nhắc | hiển thị Đi chưa tại giờ hẹn, không đợi quyết định mốc Past của plan |
| AC-058 | UC-014 | Lời nhắc Đi chưa đang hiện và câu trả lời là Chưa trả lời | bấm Bỏ qua lời nhắc rồi tải lại trang | lời nhắc biến mất và không tự hiện lại; không popup khác, không đổi lịch/địa điểm/visit_answer/count |
| AC-029 | UC-015 | Khách chưa đăng nhập có nội dung hợp lệ | gửi báo sai không kèm ảnh | tạo mã/trạng thái Mới mà không buộc đăng ký |
| AC-030 | UC-015 | Giá địa điểm bị khách báo sai | báo sai được tiếp nhận | giá công khai chưa tự đổi trước kiểm tra/duyệt |
| AC-031 | UC-016 | Thành viên có ticket đang Chờ khách phản hồi | bổ sung thông tin hợp lệ | lưu message của khách và chuyển xử lý theo quy trình được duyệt |
| AC-032 | UC-016 | Thành viên thay id sang ticket người khác | xem/trả lời trực tiếp | không đọc được ticket, tệp hay ghi chú của người khác |
| AC-033 | UC-017 | Có thông báo về plan đã bị thu hồi chia sẻ | mở liên kết trong thông báo | không xem được plan bằng thông báo cũ |
| AC-034 | UC-017 | Người dùng chưa chọn nhận tiếp thị | có sự kiện hỗ trợ cần thiết | không tự biến đồng ý nhận hỗ trợ thành đồng ý tiếp thị |
| AC-035 | UC-018 | Nơi unknown giá/giờ nhưng đủ trạng thái, nguồn và các trường xuất bản khác | gửi duyệt | không tự gán giá/giờ và vẫn chuyển Chờ duyệt khi đủ điều kiện |
| AC-036 | UC-018 | Ảnh không có quyền hoặc thiếu tọa độ | gửi duyệt | báo lỗi trường và không xuất bản |
| AC-037 | UC-019 | Biên tập không có quyền duyệt | gọi trực tiếp xuất bản | bị từ chối và nơi không ra công khai |
| AC-038 | UC-019 | Nơi đã nằm trong kế hoạch | nhân sự tạm ẩn nơi | nơi không xuất hiện trong tìm mới, tham chiếu kế hoạch cũ còn tồn tại |
| AC-039 | UC-020 | Nhãn đang gắn nhiều nơi | ngừng dùng nhãn | dữ liệu cũ vẫn tồn tại và không xóa địa điểm |
| AC-040 | UC-020 | Nhân viên không có quyền danh mục | gọi sửa nhóm nhãn | bị từ chối |
| AC-041 | UC-021 | Bộ sưu tập hợp lệ đang trong lịch hiển thị | khách mở Khám phá | xem các nơi còn công khai theo thứ tự đã cấu hình |
| AC-042 | UC-021 | Một nơi trong bộ sưu tập vừa bị ẩn | khách mở bộ sưu tập | không công khai lại nơi đó qua bộ sưu tập |
| AC-043 | UC-022 | Nhân viên hỗ trợ không có quyền cấp vai trò | gọi trực tiếp gán quản trị | bị từ chối và quyền giữ nguyên |
| AC-044 | UC-022 | Quản trị có quyền khóa tài khoản với lý do | lưu thao tác | tài khoản không tạo/sửa dữ liệu được và audit có trước/sau/lý do |
| AC-045 | UC-023 | Lô có một dòng thiếu tọa độ và một mã trùng | chạy kiểm tra thử | báo đúng dòng/lý do và chưa tự ghi như thành công |
| AC-046 | UC-023 | CSV hợp lệ theo mẫu và chính sách commit đã duyệt | xác nhận ghi | tạo dữ liệu ở quy trình nháp/duyệt và đối chiếu đủ số dòng |
| AC-047 | UC-024 | Báo sai đang kiểm tra | đóng mà không có ghi chú | bị chặn và báo cần ghi chú xử lý |
| AC-048 | UC-024 | Nhân sự có quyền mở lại báo sai đã đóng | mở lại | quay về kiểm tra và vẫn giữ kết quả/lịch sử trước |
| AC-049 | UC-025 | Nhân sự thêm ghi chú internal | khách xem lịch sử ticket | không thấy ghi chú hay tệp internal |
| AC-050 | UC-025 | Yêu cầu đang xử lý và có phản hồi cần khách bổ sung | nhân sự lưu phản hồi public, chuyển chờ khách | khách nhận thông báo phù hợp, lịch sử giữ phản hồi và trạng thái |
| AC-051 | UC-026 | Dataset biết trước có 10 lượt tìm, 2 lượt không ra kết quả, event lặp | xem báo cáo cùng kỳ | tỷ lệ không ra 20%, event lặp không cộng thêm và CSV khớp bộ lọc |
| AC-052 | UC-026 | Tổng lượt tìm bằng 0 hoặc cohort mới 7 ngày | xem tỷ lệ không ra/quay lại 30 ngày | hiện chưa có/chưa đủ dữ liệu, không chia lỗi hoặc gán tỷ lệ sai |
| AC-053 | UC-027 | Tài khoản có hai plan khác nhau tại cùng nơi đều xác nhận Đã đi | mở chi tiết nơi đó | hiển thị Đã đi 2 lần; nơi khác và tài khoản khác không bị cộng |
| AC-054 | UC-027 | Một plan đã được ghi Đã đi và count là 1 | gửi lại xác nhận hoặc tải lại nhiều lần | count vẫn 1, không tăng theo số lần click |
