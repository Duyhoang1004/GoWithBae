# GoWithBae — Phân tích khoảng trống và mâu thuẫn yêu cầu

**Cập nhật:** 15-11-2026, đồng bộ requirement v1.1. **Trạng thái:** Draft, ba hành vi CR-001 đã CONF. **Phương pháp:** READ_EXISTING → GAP_ANALYSIS của srs-agent; phân biệt sự thiếu rõ ràng với thiếu chức năng. Đây là phân tích từ tài liệu và phản hồi khách được người yêu cầu chuyển lại, không phải khảo sát ngoài dự án.

## 1. Bằng chứng và phạm vi phân tích

- Đã đọc toàn bộ phần văn bản của `GoWithBae_Yeu_cau_san_pham_v1.md` từ §1 đến §12; logo được nhận diện là tài sản tham chiếu có sẵn, chưa trích xuất hay thẩm định thiết kế hình ảnh.
- Workspace trước công việc này chỉ có tài liệu khách hàng, chưa có codebase ứng dụng để kiểm tra tái sử dụng entity/API hoặc mâu thuẫn implementation.
- Người yêu cầu xác nhận: web G1 + quản trị nội bộ, G2 định hướng; tài liệu cho cả khách hàng duyệt và đội phát triển.
- Áp dụng nhánh tài liệu có sẵn của [srs-agent](https://github.com/phong-baruby/ba-skills/blob/f26e0874f5bcf6e31d26044134cc4a562cb4fadf/skills/srs-agent/SKILL.md), commit `f26e0874f5bcf6e31d26044134cc4a562cb4fadf`. Không giả định đã thực hiện các vòng phỏng vấn khách hàng hoặc nhận duyệt toàn bộ requirement.

## 2. AS-IS / TO-BE và phần cần thay đổi

| Miền | AS-IS theo nguồn | TO-BE G1 theo nguồn | Khoảng cách / đáp ứng |
|---|---|---|---|
| Tìm địa điểm | Tìm rời rạc, dễ gặp bài viral | Danh mục đã duyệt, tìm/lọc theo nhu cầu | Chuẩn hóa dữ liệu và công cụ tìm; FR-01–05 |
| So sánh | Tự tổng hợp nhiều nơi | 2–3 nơi cùng tiêu chí và giá hai người | Dữ liệu thiếu biểu diễn đúng; FR-06 |
| Thống nhất với người yêu | Tự trao đổi qua kênh ngoài | 2–5 nơi, 2 người thích/bỏ qua, chủ tạo chốt | Danh tính/link/phiếu/trạng thái rõ; FR-10–14 |
| Lưu kế hoạch | Chưa có công cụ chung được mô tả | Danh sách riêng và kế hoạch một nơi | Quyền sở hữu, chia sẻ và lịch sử; FR-09, FR-15–18 |
| Chất lượng nội dung | Người dùng tự kiểm tra giá/giờ | Nguồn, ngày kiểm tra, duyệt, báo sai | Quy trình nội dung và phân quyền; FR-19, FR-23–31 |
| Vận hành | Chưa có hệ thống báo cáo được mô tả | Hỗ trợ có lịch sử và báo cáo sử dụng | Instrumentation, định nghĩa chỉ số và quyền; FR-20–22, FR-32–34 |

Không có số liệu nền về quy trình hiện tại, vì vậy không kết luận tiết kiệm bao nhiêu thời gian hoặc tăng tỷ lệ chốt bao nhiêu phần trăm.

## 3. Phát hiện và cách xử lý trong requirement

| Mã | Phát hiện / bằng chứng nguồn | Rủi ro nếu tự hiểu | Cách thể hiện trong bản phân tích | Quyết định / tác động |
|---|---|---|---|---|
| GAP-01 | §1 nói web và app, §1.1 đề xuất web trước; dấu (*) chỉ định hướng | Scope phình thành app/thanh toán | CONF web G1 + quản trị; G2 tách, không đưa backlog G1 | Đã rõ ở cấp phạm vi; chi tiết OQ-01 |
| GAP-02 | §1/10 có mục tiêu và chỉ số nhưng không target/baseline | KPI tự đặt thành cam kết | BR có chỉ số kiểm chứng; target kinh doanh TBD | OQ-02, PO trước kế hoạch/nghiệm thu |
| GAP-03 | §3 dùng giá chuẩn hai người nhưng chưa nêu giỏ dịch vụ/thuế/phụ phí | Cùng giá nhưng khác nội dung; lọc sai ý nghĩa | Dữ liệu phần bao gồm/nguồn; PROP validation giá; không tự tính giá từ khoảng | OQ-03, chặn dữ liệu giá/lọc |
| GAP-04 | “Miễn phí” và “dưới 200k” ở §3.2 có thể chồng nhau | Một giá 0 vào hai nhóm ngoài ý muốn | PROP miễn phí = 0, dưới 200k = (0,200k); nêu rõ cần duyệt | OQ-03, chặn boundary giá |
| GAP-05 | §3.1 chưa định nghĩa độ khớp, query rỗng, tìm theo từng từ hay cụm | Hai implementation cùng đọc nguồn ra thứ tự khác | Giữ ưu tiên relevance → xác minh mới → id; không chọn thuật toán | OQ-04, chặn chi tiết search |
| GAP-06 | §3/5 có giờ tuần/ngoại lệ, chưa có giờ qua đêm/biên đóng/ngày nghỉ | Đặt lúc đóng cửa hoặc loại nơi đang mở | Tách known/unknown, PROP ngoại lệ ưu tiên tuần; nêu bộ test qua ngày | OQ-04, chặn logic thời gian |
| GAP-07 | §3.2 cho khu vực hoặc tâm/bán kính, chưa công thức khoảng cách/giới hạn | “Gần nhất” bị diễn giải là thời gian đi | Ghi khoảng cách ước tính, không thời gian; nhập thủ công nếu từ chối vị trí | OQ-05, chặn bản đồ/radius |
| GAP-08 | §6.1 OTP là phương án đề xuất, chưa cấu hình; §11 yêu cầu 2 bước quản trị | Auth bị mặc định số lần/hạn hoặc thiếu 2FA | FR-07 đề xuất, NFR bắt buộc trước thật; không chọn JWT/mật khẩu hay số tùy ý | OQ-06, chặn production auth |
| GAP-09 | §5.2 chỉ cố định tập sau join, chưa rõ trước join/sửa/ẩn nơi | Phiếu bị mất nghĩa khi tập thay đổi | Cố định tập sau join; phiên mới để đổi; OQ cho chỉnh trước join và nơi ẩn | OQ-07/11, chặn ứng xử phiên |
| GAP-10 | §5.2 ai có link có thể chiếm một suất, chưa mô tả hai người join đồng thời | Có hơn một người nhận/nhầm quyền | DER kiểm tra slot tại lúc ghi; chỉ một request thành công | UC-008/AC-015; chính sách tên/session OQ-07/09 |
| GAP-11 | §5.2 nói xem kết quả trùng, chưa nêu thấy phiếu riêng hay phải trả lời hết | Tự xây cơ chế lộ phiếu/ép trả lời | Không khẳng định phiếu riêng công khai; không tự chốt, giữ OQ điều kiện chốt | OQ-07/08, chặn chi tiết bỏ phiếu/chốt |
| GAP-12 | Chốt rồi chuyển tạo plan ở §5.2, chưa rõ lúc nào khóa phiên | Phiên chỉ xem dù tạo plan lỗi; nhiều plan do retry | PROP chốt cùng lưu plan, tối đa 1 plan/phiên, kiểm tra trước retry | OQ-08, chặn UC-010 |
| GAP-13 | §5.2 link, phiên khách, hết hạn; §6.4 thu hồi phải mất quyền ngay | Thu hồi URL nhưng cookie khách cũ vẫn đọc được | DER cấp quyền ràng buộc phiên người nhận, kiểm tra lại; tách vòng đời grant/choice/plan | OQ-09, chặn quyền chia sẻ |
| GAP-14 | §5.3 có người nhận nếu có, chưa cơ chế chia sẻ plan trực tiếp | Tự công khai plan hoặc tự xây hồ sơ cặp đôi | Giữ recipient tùy chọn, quyền chỉ xem; không thêm public link tùy tiện | OQ-09, chặn nhánh share plan |
| GAP-15 | “Đã qua giờ” và sửa/hủy chưa giới hạn ở §5.3 | Hết giờ = đã đi; mở lại plan không chính sách | Plan status riêng VisitSelfReport; không tự suy ra sử dụng dịch vụ | OQ-10, chặn job/visit report |
| GAP-16 | §5.1 nêu nơi ẩn trong list, chưa nêu đầy đủ trong compare/choice/plan | Xóa lịch sử hoặc lộ nơi qua kênh phụ | Chặn chốt mới; giữ lịch sử theo quyền; chưa tự xóa plan cũ | OQ-11, chặn cảnh báo/truy cập cũ |
| GAP-17 | §5.3 thêm lịch không xác định file/OAuth/cập nhật | Tự thêm chi phí và đồng bộ hai chiều | PROP xuất một lần, chờ duyệt; lỗi lịch không xóa plan | OQ-12, chặn FR-17 cuối |
| GAP-18 | §6.3 chỉ tên trạng thái ticket, chưa mở lại/timeout chờ khách | Tự đóng hoặc cấm trả lời sai kỳ vọng | State gắn PROP/TBD, không tự đóng; public/internal tách | OQ-13, chặn trạng thái cuối ticket |
| GAP-19 | §6.4 chưa ma trận sự kiện/email/đã đọc | Tự thêm marketing, push hay nhắc hẹn | Chỉ thông báo plan/hỗ trợ; tiếp thị opt-in riêng; không tự thêm nhắc hẹn | OQ-14, chặn cấu hình thông báo |
| GAP-20 | §4.2/7 chưa bản sửa nơi công khai/khôi phục | Sửa trực tiếp bỏ duyệt hoặc nơi ẩn không có lối quay lại | PROP giữ bản cũ đến duyệt bản mới, khôi phục qua duyệt; diagram ghi rõ đề xuất | OQ-15, chặn moderation workflow |
| GAP-21 | §7 có vai trò kép, chưa chốt self-review, quyền report/collection/đọc riêng/admin cuối | Cấp quyền quá rộng hoặc hiểu mọi nhân viên có toàn quyền | Ma trận có TBD, default deny; audit thay quyền; không mặc định superadmin đọc riêng | OQ-18, chặn RBAC chi tiết |
| GAP-22 | §6.1/6.4 xóa/khóa chưa retention và quy trình xử lý | Nhận yêu cầu bị báo xóa xong; hoặc xóa phá audit/lịch sử | Tách nhận/xử lý xóa; bắt buộc thu hồi phiên/link, retention TBD | OQ-17, chặn deletion/lock policy |
| GAP-23 | §7.3 CSV chưa commit toàn lô/một phần/create/update | Bỏ lỗi âm thầm, ghi trùng, cập nhật sai nơi | Dry run + báo dòng; PROP toàn lô hợp lệ mới ghi, không tự xuất bản | OQ-19, chặn import commit |
| GAP-24 | §10 chưa identity guest→user, ngưỡng stale, mẫu số 0, late event | Báo cáo không đối chiếu, tỷ lệ giả | Công thức nguồn; DER mẫu số 0 N/A, cohort đủ 30 ngày; session 30 phút vẫn đề xuất | OQ-20, chặn công thức cuối |
| GAP-25 | §11.2 chưa giới hạn chuỗi/tệp/rate; template skill có ví dụ metric | Dùng số ví dụ như cam kết của khách | Ghi TBD, không đưa uptime 99.9%/1.000 CCU hay JWT từ template vào requirement | OQ-06/21/22, chặn validation/NFR cuối |
| GAP-26 | §11 hiệu năng/sao lưu là mục tiêu đề xuất; chưa cấu hình và chi phí | Hứa SLA chưa đo và không người vận hành | Giữ trạng thái đề xuất, test ghi cấu hình, cần người trực/dịch vụ dự án | OQ-16/22/23, trước production |
| GAP-27 | §12 có bàn giao nhưng chưa người/lịch/ước lượng | Requirement bị hiểu là đã ký hoặc đã có ngày giao | Draft, cổng duyệt module, CR, người duyệt TBD, backlog chưa ước lượng | OQ-02/23, trước baseline |
| GAP-28 | §8–9 nhiều chính sách G2 chưa chốt | Tự nhận thanh toán/đối soát trong G1 hoặc dùng tỷ lệ ví dụ | G2 chỉ định hướng; bảo toàn snapshot, chống trùng, tách tiền/đơn, không báo giá trị đơn là doanh thu | OQ-24, không chặn G1 |
| GAP-29 | CR-001 khách xác nhận có streak/X lần, trước đó chưa có trong v1.0 | Tự hiểu thành chuỗi ngày liên tiếp, tăng theo số click hoặc công khai lịch sử riêng | Thêm FR-35/RULE-29/UC-027; đếm lượt Đã đi hợp lệ theo tài khoản/chi nhánh, loại retry; không reset theo ngày/tuần | Có hiển thị số lần đã CONF; OQ-10/25 chỉ còn actor/sửa lượt/nhánh bổ sung |

## 4. Những điểm đã rõ, không hỏi lại một cách máy móc

Actor, mục tiêu, luồng chính, giới hạn so sánh/cùng chọn, quy tắc AND/OR, quyền khách chưa đăng nhập, danh sách riêng, điều kiện xuất bản, báo sai/hỗ trợ và chỉ số báo cáo đều có trong nguồn. Không lặp lại các câu WHO/WHAT/FLOW chung chỉ để hoàn thành checklist phỏng vấn. Các câu hỏi cần thiết tập trung vào khoảng trống phía trên và được đăng ký owner/mốc tại requirement §13.

## 5. Kết quả chuyển sang đặc tả

- Một requirement tổng thể bao gồm bối cảnh/nghiệp vụ/FR/RULE/data/UI/NFR/AC/OQ/G2/bàn giao.
- 27 UC theo một primary actor/mục tiêu, có PRE/POST, flow phản hồi hệ thống, AF/EX/Recovery, data/rules, 58 AC và verification.
- Sequence diagram từng UC; state diagram cho choice/grant/plan/place/report/ticket; grant không bị gộp sai thành trạng thái hủy plan.
- Backlog/task có tham chiếu ngược về § của UC, không tự chọn stack hoặc bắt đầu lập trình.
- Verification phân biệt kiểm tra cấu trúc/truy vết tài liệu và quyết định nghiệp vụ chưa có người duyệt; không ghi các giả định là Passed về nghiệp vụ.

## 6. Cập nhật đã xác nhận qua CR-001 ngày 15-11-2026

- GAP-13: việc có cần đăng nhập để xem link đã rõ: không cần; phiên khách theo link không phải phiên tài khoản. Hết hạn/thu hồi/quyền plan vẫn cần OQ-09, không gộp thành chưa chốt việc đăng nhập.
- GAP-15: thời điểm hiện nhắc là giờ hẹn; Bỏ qua chỉ đóng và không mở popup khác, không đổi lịch/quán. Trạng thái nhắc tách visit_answer; không tự ghi Không, không tăng count. Phần actor và chỉnh sửa lịch sử còn OQ-10.
- GAP-19: đã có lời nhắc trong web theo FR-18; không tự thêm email/push nhắc hẹn. OQ-14 chỉ còn ma trận các thông báo khác/đọc-chưa đọc/email.
- GAP-29: chức năng số lần đã đi được thêm vào G1 theo phản hồi mới, không ghi như chức năng có trong file gốc. Cách biểu diễn “Đã đi X lần” và loại lượt trùng được đặc tả, chưa tự thêm chuỗi liên tiếp/reset.

Các dòng GAP ở §3 lưu bối cảnh phát hiện ban đầu; trạng thái đã xác nhận ở mục này và requirement §13/CR-001 là căn cứ hiện hành. Không coi ba câu trả lời là phê duyệt toàn bộ các OQ khác.
