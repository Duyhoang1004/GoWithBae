# Verification Report — GoWithBae G1

**Cập nhật:** 15-11-2026, requirement v1.1. **Phạm vi kiểm tra:** tài liệu yêu cầu, use case, sơ đồ và truy vết; không có phần mềm ứng dụng để chạy kiểm thử chức năng/hiệu năng/bảo mật.

**Kết luận:** đã cấu trúc hóa được requirement web G1 + quản trị và đầu ra theo skill. Bộ tài liệu ở trạng thái **Draft để khách hàng review**; chưa được coi là sẵn sàng lập trình toàn bộ do còn quyết định nghiệp vụ ảnh hưởng trực tiếp quy tắc xử lý.

## 1. Quy trình skill đã thực hiện

| Bước srs-agent cho tài liệu sẵn có | Bằng chứng |
|---|---|
| READ_EXISTING | Đọc nguồn §1–12; kiểm tra workspace chưa có codebase; lưu hash và phiên bản skill trong requirement |
| GAP_ANALYSIS | [Gap_Analysis.md](Gap_Analysis.md), 29 phát hiện, AS-IS/TO-BE và cập nhật CR-001, không tự thêm yêu cầu từ thị trường |
| STRUCTURE | [Requirement chính v1.1](GoWithBae_Requirements_G1_v1.1.md), [27 UC](../specs/README.md); PRE/POST/Main Flow/AF/EX/Rules/Data/OQ/Notes |
| VERIFY | Rà logic bên dưới, kiểm tra cấu trúc/link/truy vết tự động và parse Mermaid; kết quả máy đọc tại [validation-results.json](validation-results.json) |
| OUTPUT | Requirement tổng thể, UC, sequence/state Mermaid, task breakdown từng UC và backlog tổng |

Người yêu cầu đã xác nhận phạm vi/đối tượng đọc và ngày 15-11-2026 chuyển lại ba phản hồi khách hàng trong CR-001. Đã ghi ba hành vi đó CONF, không giả lập phỏng vấn trực tiếp hoặc coi là duyệt các chính sách mở khác. Dùng nhánh READ_EXISTING → GAP_ANALYSIS; mọi câu hỏi còn thiếu có owner và mốc tại requirement §13.

## 2. Blocking Issues — cần quyết định trước phần triển khai phụ thuộc

| Nhóm | Vì sao chặn | OQ / bước bị ảnh hưởng |
|---|---|---|
| Giá chuẩn và boundary miễn phí | Không đủ để tính giá lọc/kiểm tra giá nhập; nếu chọn sai, cùng dataset sẽ trả kết quả khác nhau | OQ-03; UC-001/018 bước 3–4 |
| Matching, giờ qua đêm và vị trí | Chưa công thức relevance, quy tắc giờ/ngoại lệ/biên và radius/khoảng cách để QA có expected result cuối | OQ-04–05; UC-001/012 bước 3–4 |
| Auth, trạng thái khóa/xóa và tệp | Chưa phương án/cấu hình OTP/phiên/2 bước, retention, giới hạn và xử lý tệp | OQ-06/17/21; UC-004/005/022, NFR |
| Cùng chọn và chốt | Chưa đủ điều kiện chốt, thời điểm khóa phiên, một hay nhiều plan, công khai phiếu và thay người nhận | OQ-07–08; UC-007–010 bước 3–4 |
| Chia sẻ và thu hồi | Chưa tác động hết hạn lên phiên đã join, quyền plan sau thu hồi và chia sẻ plan trực tiếp | OQ-09; UC-008/011/013 bước 1/3–4 |
| Buổi hẹn qua giờ / nơi biến động / lịch | CR-001 đã chốt nhắc tại giờ hẹn và Bỏ qua chỉ đóng. Còn mốc trạng thái Past, sửa/hủy sau giờ, actor tự báo, nơi ẩn và cơ chế lịch | OQ-10–12/25; UC-010/012–014/027 |
| Duyệt nội dung và RBAC | Chưa versioning nội dung công khai, khôi phục, vai trò kép self-review và ma trận quyền chi tiết | OQ-15/18; UC-018–022 bước 1/3–4 |
| Hỗ trợ và thông báo | Chưa điều kiện mở lại/đóng ticket đang chờ, nội dung email cần thiết/đã đọc | OQ-13–14; UC-016/017/025 |
| Import và chỉ số | Chưa chính sách commit/update lô, định danh/cohort/ngưỡng dữ liệu cũ và chi tiết CSV | OQ-19–20; UC-023/026 bước 3–4 |
| Cam kết vận hành/nghiệm thu | Chưa người duyệt, lịch, môi trường đo và đồng ý mục tiêu NFR/dịch vụ/chi phí/bảo trì | OQ-01/02/16/22/23; cổng baseline/production |

Đây là blocker **quyết định nghiệp vụ/triển khai**, không phải lỗi cú pháp đã được “fix” bằng giả định. Có thể triển khai module độc lập sau khi phần baseline của module được duyệt; không cần coi OQ-24 của G2 là blocker G1.

## 3. Warnings

- Các con số 7 ngày link, 30 phút analytics session, p95 2 giây/1.000 nơi/100 người, backup 30 ngày và RPO/RTO là đề xuất/mục tiêu nguồn, chưa phải cam kết đã ký.
- Sơ đồ mô tả hành vi, chưa quyết định kiến trúc. Máy chủ và kho dữ liệu trong sequence là ranh giới logic, không buộc microservices hoặc loại database.
- Mỗi UC có ít nhất hai AC Given/When/Then; không được hiểu 58 AC là toàn bộ test suite. EX-01–04, RULE, boundary và NFR vẫn cần test chi tiết sau khi OQ được chốt.
- Backlog có task tham chiếu UC/section; chưa có code để kiểm tra API/schema hiện hữu và chưa có căn cứ ước lượng ngày công/chi phí.
- Bản phân tích chưa có prototype, visual QA giao diện, kiểm thử người dùng, số liệu KPI nền, kiểm thử restore hoặc đo tải thật. Đây là các hạng mục triển khai/nghiệm thu tiếp theo.
- Kiểm tra link/regex/parse chỉ chứng minh tính nhất quán cơ học/cú pháp, không chứng minh yêu cầu nghiệp vụ đúng ý khách hàng hoặc sơ đồ đã tối ưu trình bày.

## 4. Passed — rà soát logic tài liệu

### Preconditions ↔ Main Flow

Mỗi UC ghi PRE và bước kiểm tra đầu vào/quyền tại bước 1, kiểm tra lại bước 3–4 trước đọc/ghi. Luồng thành công không dựa vào việc nút bị ẩn. Tạo plan/cùng chọn/list/ticket gắn chủ từ phiên máy chủ; không tin id chủ do client gửi. Các OQ về chính sách quyền vẫn là blocker, không được đánh dấu đã duyệt.

### Postconditions ↔ Main Flow

Mỗi UC gắn kết quả đọc/ghi ở bước 4 và phản hồi bước 5. Chốt từ phiên đề xuất thực hiện cùng lưu plan để tránh hậu điều kiện “đã chốt” mà không có plan; chính sách này ghi PROP/OQ-08. Lỗi thông báo sau lưu không đảo ngược hoặc báo sai kết quả nghiệp vụ đã xác lập.

### Business Rules ↔ Exception handling

Mỗi bảng RULE của UC có bước áp dụng và handler EX-01 (quyền/PRE), EX-02 (validation/rule), EX-03 (đọc/ghi/đồng thời), EX-04 (phản hồi/side effect sau thành công). Các trường hợp đặc thù nằm trong Trigger của EX-02, không chỉ có câu chung “xảy ra lỗi”. Có Recovery về bước sửa input, tải trạng thái mới hoặc nhờ vận hành.

### State transitions và actor

- Tách ChoiceSession, ShareGrant và DatePlan; hết hạn/thu hồi grant không tự hủy plan hoặc xóa phiếu.
- Place có Nháp → Chờ duyệt → Công khai, nhánh trả về/ẩn/ngừng; lối khôi phục là PROP cần duyệt. Published là trạng thái vận hành có chủ ý, không phải orphan.
- Choice Waiting/ Voting là trạng thái đang hoạt động; link có vòng đời riêng; Finalized chỉ đọc theo quyền. Không tự đóng phiên hoặc xóa lựa chọn khi hết hạn link.
- Plan Đã hủy/Đã qua giờ là kết thúc theo mô hình hiện tại, không phải lỗi không có lối ra. VisitSelfReport tách riêng, không có trạng thái giả “đã sử dụng” của G2.
- DateReminder tách khỏi plan/visit_answer. Shown → Dismissed chỉ đóng nhắc và lưu thời điểm; không gán Không, không hủy hoặc đổi lịch, không mở popup gợi ý. Đã đi → Answered chỉ cộng một lượt hợp lệ; retry không cộng thêm.
- Báo sai có mở lại theo quyền; hỗ trợ có chuyển chờ khách/phản hồi, các chuyển chưa rõ ghi PROP/TBD. Không tự thêm timeout đóng.
- Mỗi UC có một primary actor và một mục tiêu; hệ thống chịu trách nhiệm các xử lý trong cột System Response. Người nhận plan là actor phụ có nhánh chỉ xem; không có quyền sửa.

### Scope và dữ liệu

Không có bước thanh toán/đặt chỗ/app/AI trong luồng G1. G2 được bảo toàn ở mức định hướng có điều kiện riêng. Dữ liệu unknown không tự đổi thành No/0/Open; bộ lọc yêu cầu thuộc tính phải có căn cứ đạt. Nhãn/địa điểm có lịch sử không bị xóa phá tham chiếu; danh sách riêng không bị lộ toàn bộ qua link cùng chọn.

## 5. Kiểm tra tự động

Kết quả chạy lại bản v1.1 được ghi tại `validation-results.json` và `mermaid-validation.json`; số liệu v1.0 không dùng làm bằng chứng cho bản cập nhật. Hash file khách gốc phải giữ nguyên. Đây là kiểm tra tài liệu, không phải chứng nhận phần mềm đã hoạt động.

**Đã chạy ngày 15-11-2026:** 17 nhóm kiểm tra tài liệu đạt, 0 nhóm lỗi; 61 file Markdown, 35 FR, 30 RULE, 12 NFR, 25 nhóm OQ (có nhóm đã chốt một phần), 27 UC và 58 AC. Mermaid 12.3.0 parse thành công 97 lượt sơ đồ, gồm 39 file riêng và 58 sơ đồ nhúng (bao gồm bản v1.0 được giữ để đối chiếu). Kiểm tra mới CR-001 xác nhận truy vết và nội dung hợp đồng của ba hành vi; không coi kiểm tra chuỗi văn bản là kiểm thử runtime.

Kết quả thực thi cụ thể và phiên bản công cụ được ghi ở [validation-results.json](validation-results.json). Kiểm tra gồm:

1. Tính toàn vẹn SHA-256 file nguồn không thay đổi.
2. Đủ 35 FR/30 RULE/12 NFR/25 nhóm OQ và ID liên tục; 27 UC, 58 AC duy nhất; FR/RULE đều có UC tham chiếu, không có mã treo. OQ-09/10 đã đóng một phần, không hỏi lại hành vi được CONF.
3. Mỗi UC đủ 10 mục template, Main Flow có Actor/Action/System Response, PRE/POST, AF có hướng tiếp tục, EX có Recovery, RULE có handler, link task/diagram tương ứng.
4. Link file Markdown trong `docs` có đích tồn tại; code fence đóng đủ; file Mermaid có một root diagram.
5. Parse các sequence/state/flow/ERD bằng Mermaid, bao gồm các sơ đồ nhúng trong requirement và UC, khi công cụ được cài; parse không thay thế render/đánh giá thẩm mỹ.

Chỉ ghi `passed` cho các check thực sự chạy thành công. Không có kết quả test runtime của web, API, email, bản đồ hoặc bảo mật sản phẩm trong báo cáo này.

## 6. Bước review tiếp theo

CR-001 đã được đưa vào FR-11/18/21/22/35, RULE-15/29/30, UC-008/009/014/027 và AC-P11–13/AC-053–058. Không chờ xác nhận lại ba hành vi khách đã trả lời. Các giả định về actor sửa lượt/lượt ngoài kế hoạch/nhắc lại sau thay giờ chỉ ảnh hưởng nhánh ở OQ-10/25; không phủ nhận chức năng số lần và Bỏ qua đã chốt.

PO/nội dung xác nhận trước cách tính giá, giờ và dữ liệu thử; PO/đội giải pháp xác nhận auth/quyền/chia sẻ/cùng chọn; PO/vận hành chốt nhập CSV/chỉ số/hỗ trợ/NFR. Ghi câu trả lời vào OQ tương ứng, tăng phiên bản/baseline phần đã duyệt và cập nhật UC/diagram/AC/task bị ảnh hưởng trước coding.
