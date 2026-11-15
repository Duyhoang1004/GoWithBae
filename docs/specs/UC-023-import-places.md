# UC-023: Nhập địa điểm bằng CSV

**Version:** 1.1 — 15-11-2026  
**Status:** Draft, chờ duyệt nghiệp vụ; chưa tự coi các PROP/TBD là baseline.  
**Requirement chính:** [GWB-REQ-G1 v1.1](../requirements/GoWithBae_Requirements_G1_v1.1.md)  
**Nguồn khách hàng:** §7.3  
**Truy vết:** FR-30; AC-045, AC-046.

## 1. Overview

| Trường | Nội dung |
|---|---|
| Goal | Kiểm tra và ghi lô địa điểm có báo lỗi theo dòng, không bỏ lỗi âm thầm. |
| Primary Actor | Nhân viên có quyền nhập dữ liệu |
| Secondary Actors | Máy chủ GoWithBae; dịch vụ email/bản đồ/tệp/lịch chỉ khi thao tác tương ứng cần tích hợp, theo requirement §7 |
| Trigger | Mở nhập CSV và tải mẫu |
| Scope IN | Nhập địa điểm bằng CSV trên web G1 theo các FR liên kết |
| Scope OUT | Chức năng G2, đặt chỗ/thanh toán/app/AI/chat; quyền và thao tác ngoài mục tiêu UC. Điều hướng sang UC khác không tự thực hiện UC đó. |

## 2. Preconditions

- PRE-1: Phiên nhân viên có quyền nhập dữ liệu. Enforce tại bước 1; kiểm tra lại quyền/trạng thái liên quan tại bước 3–4 trước khi trả dữ liệu hoặc ghi.
- PRE-2: Mẫu CSV và chính sách commit/update/create-only theo OQ-19 được duyệt trước ghi thật. Enforce tại bước 1; kiểm tra lại quyền/trạng thái liên quan tại bước 3–4 trước khi trả dữ liệu hoặc ghi.

## 3. Postconditions

- POST-1: Kết quả lô rõ từng dòng và chỉ dữ liệu hợp lệ được ghi theo chính sách được duyệt, chưa tự xuất bản. Được tạo tại bước 4 và phản hồi tại bước 5.
- POST-2: Quyền riêng tư/phạm vi được giữ; không thay dữ liệu ngoài mục tiêu hoặc thông báo thành công khi bước 4 thất bại. Enforce bước 1, 3–5 và EX-01–04.
- Nếu use case thất bại: không ghi kết quả nghiệp vụ dở dang như đã hoàn tất; dữ liệu đã lưu thành công không bị báo chưa lưu chỉ vì lỗi thông báo/analytics sau đó.

## 4. Main Flow

| Bước | Actor | Hành động | System Response |
|---|---|---|---|
| 1 | Nhân viên có quyền nhập dữ liệu | Mở nhập CSV và tải mẫu | Kiểm tra quyền, trả mẫu theo phiên bản và quy tắc nhập. |
| 2 | Nhân viên có quyền nhập dữ liệu | Chọn tệp để kiểm tra thử | Đọc tệp theo định dạng/giới hạn, chưa tạo hồ sơ công khai. |
| 3 | Nhân viên có quyền nhập dữ liệu | Xem kiểm tra dữ liệu | Kiểm tra từng dòng: trường thiếu, mã trùng trong lô/hệ thống, tọa độ sai, quyền ảnh và định dạng. |
| 4 | Nhân viên có quyền nhập dữ liệu | Xác nhận ghi lô hợp lệ | Theo PROP OQ-19, chỉ commit lô khi toàn bộ hợp lệ; kiểm tra lại trùng trước ghi; lưu dữ liệu ở quy trình nháp/duyệt, không tự công khai. |
| 5 | Nhân viên có quyền nhập dữ liệu | Nhận kết quả nhập | Trả số dòng thành công/thất bại và lỗi theo dòng, truy nguyên batch; không âm thầm bỏ dòng lỗi. |

Các xử lý kiểm tra, truy vấn/ghi, chuyển trạng thái, thông báo và analytics trong cột System Response do hệ thống thực hiện, không do actor tự gán trạng thái/quyền.

## 5. Alternate Flows

**AF-01 — Hướng đi hợp lệ khác:** Tại bước 3, có lỗi → người nhập sửa file rồi trở lại bước 2; không chuyển lỗi thành thành công. Nếu PO chọn commit một phần phải cập nhật spec/AC trước triển khai.

## 6. Exception Flows

**EX-01 — PRE/quyền không đạt.**
- Trigger: không đạt một trong PRE, danh tính/quyền/cấp quyền hết hiệu lực hoặc tài nguyên không được đọc.
- At step 1 hoặc kiểm tra lại bước 3–4: từ chối và không trả dữ liệu riêng; không chỉ ẩn nút phía client.
- Recovery: với thao tác tài khoản, đăng nhập lại khi cần; với khách nhận link, chỉ cần link/phiên khách hợp lệ, không bắt đăng nhập để xem. Xin mời lại/liên hệ người có quyền rồi bắt đầu bước 1; không lách bằng đổi id.

**EX-02 — Vi phạm đầu vào hoặc quy tắc nghiệp vụ.**
- Trigger: Tệp không đúng/không an toàn, thiếu trường, mã trùng, tọa độ ngoài biên hoặc lỗi commit: báo rõ dòng/lô, không bỏ qua. Nhập lại phải kiểm tra batch/kết quả hiện tại để tránh bản ghi trùng.
- At step 3; kiểm tra biên/trạng thái lại trước ghi tại bước 4. Không ghi kết quả sai; giải thích trường hoặc điều kiện không đạt.
- Recovery: sửa đầu vào ở bước 2, hoặc nạp trạng thái mới từ bước 1 rồi thử lại; OQ chặn phải được PO quyết định, không tự đặt mặc định.

**EX-03 — Đọc/ghi hoặc tích hợp phục vụ nghiệp vụ thất bại.**
- Trigger: timeout, dịch vụ cần thiết không đáp ứng, thao tác đồng thời làm dữ liệu thay đổi hoặc kết quả ghi chưa rõ.
- At step 4: không trả “thành công” khi chưa có kết quả chắc chắn; hiển thị mã tra cứu và giữ input an toàn có thể dùng lại.
- Recovery: kiểm tra trạng thái hiện tại trước retry thao tác ghi để không tạo lặp; đọc có thể thử lại; lỗi kéo dài chuyển vận hành tra mã. Với tích hợp có fallback cụ thể, dùng AF-01/requirement §7 thay vì tự bịa dữ liệu.

**EX-04 — Phản hồi/side effect sau thành công gặp lỗi.**
- Trigger: mất kết nối khi trả kết quả, lỗi thông báo/email/analytics cần thiết sau khi kết quả nghiệp vụ đã được xác lập.
- At step 5: giữ kết quả đã lưu, không báo sai thành thất bại nghiệp vụ và không lộ bí mật trong log.
- Recovery: tải lại/tra trạng thái; vận hành theo dõi và retry phần gửi/ghi sự kiện với mã chống trùng. Không tự tạo lại đối tượng nghiệp vụ chỉ để gửi lại thông báo.

## 7. Business Rules

Định nghĩa đầy đủ tại requirement chính §5. Quy tắc áp dụng vào bước và handler dưới đây; exception phải giữ đúng hậu điều kiện riêng của UC, không coi mọi lỗi đều rollback thao tác đã hoàn tất.

| Rule ID | Description và xử lý khi vi phạm | Applies At Step / handler |
|---|---|---|
| RULE-20 | Xuất bản cần tên, loại hình, địa chỉ, tọa độ, mô tả, ảnh có quyền, hoạt động chính, trạng thái giá/giờ, nguồn/ngày kiểm tra và kiểm tra trùng Khi vi phạm: Thiếu thì giữ nháp/chờ duyệt, liệt kê lỗi; không bắt buộc biết giá/giờ thực nếu đã ghi chưa rõ | Bước 1/3–4; EX-01/EX-02/EX-03 |
| RULE-21 | Biên tập không tự xuất bản; quản lý quyền, duyệt/ẩn/giá/khóa có nhật ký Khi vi phạm: Từ chối thao tác vượt quyền; thay đổi quan trọng không được mất dấu vết; vai trò kép OQ-18 | Bước 1/3–4; EX-01/EX-02/EX-03 |
| RULE-23 | CSV có kiểm tra thử và báo lỗi dòng, mã trùng, tọa độ; ảnh gắn mã riêng Khi vi phạm: Không âm thầm bỏ dòng lỗi; chưa ghi lô chưa hợp lệ theo chính sách OQ-19 | Bước 3–4; EX-02/EX-03 |
| RULE-27 | Tệp giới hạn định dạng/kích thước, kiểm tra nội dung; không công khai token/khóa; nhân viên dùng 2 bước trước vận hành Khi vi phạm: Tệp bị từ chối không tạo liên kết công khai; lỗi có mã tra cứu, không trả bí mật | Bước 1/3–4; EX-01/EX-02/EX-03 |
| RULE-28 | Chỉ dữ liệu có quyền sử dụng được công bố; minh họa phải gắn nhãn và tách môi trường thật Khi vi phạm: Không xuất bản ảnh không có quyền; không dùng dữ liệu giả làm số liệu kinh doanh | Bước 1/3–4; EX-01/EX-02/EX-03 |

## 8. Data Requirements

**Input và validation:**

- csv_file: tệp đúng mẫu/encoding/giới hạn đã chốt; mỗi dòng có mã chi nhánh và các cột bắt buộc.
- image_mapping: ảnh tải riêng gắn mã; ảnh/quyền theo quy định.
- dry_run/batch_id: kiểm tra thử trước xác nhận ghi, giữ kết quả từng dòng.

**Output / state:** Kết quả lô rõ từng dòng và chỉ dữ liệu hợp lệ được ghi theo chính sách được duyệt, chưa tự xuất bản. Dữ liệu nghiệp vụ chi tiết tại requirement §6; giới hạn chưa có số phải theo OQ-21, không tự gán giá trị.

**Sequence diagram:** [file Mermaid](./UC-023-import-places.mermaid).

```mermaid
sequenceDiagram
    actor Person as Nhân viên có quyền nhập dữ liệu
    participant Web as Web
    participant Server as Máy chủ nghiệp vụ
    participant Data as Dữ liệu
    Person->>Web: Mở nhập CSV và tải mẫu
    Web->>Server: Yêu cầu và danh tính hoặc cấp quyền
    Server->>Server: Bước 1, kiểm tra PRE và quyền
    alt EX-01, không có quyền hoặc sai điều kiện
        Server-->>Web: Từ chối, không lộ dữ liệu riêng
    else Đủ điều kiện
        Person->>Web: Chọn tệp để kiểm tra thử
        Web->>Server: Input nghiệp vụ
        Server->>Server: Bước 3, validation và RULE
        alt EX-02, vi phạm quy tắc
            Server-->>Web: Lỗi cụ thể, giữ dữ liệu có thể sửa
        else Hợp lệ
            Server->>Data: Bước 4, đọc hoặc ghi theo quyền
            alt EX-03, xử lý hoặc lưu thất bại
                Data-->>Server: Không thành công hoặc cần kiểm tra trạng thái
                Server-->>Web: Không báo thành công, mã tra cứu và cách thử lại
            else Thành công
                Data-->>Server: Kết quả nhất quán
                Server-->>Web: Bước 5, kết quả và thông báo phù hợp
                Web-->>Person: Kiểm tra và ghi lô địa điểm có báo lỗi theo dòng, không bỏ lỗi âm thầm.
            end
        end
    end
```

**State machine:** UC này không thêm vòng đời trạng thái riêng. Nếu tham chiếu một entity có trạng thái, áp dụng state machine ở requirement §7; không tạo trạng thái giả chỉ để vẽ sơ đồ.

## 9. Open Questions

OQ-19, OQ-21

Owner, mốc quyết định và mức chặn được ghi tại requirement §13. UC chỉ được baseline sau khi các câu hỏi ảnh hưởng tới nhánh triển khai đã được trả lời hoặc nhánh đó được PO loại khỏi phạm vi rõ ràng.

## 10. Notes và Acceptance Criteria

- Đây là đặc tả yêu cầu, chưa xây phần mềm; nội dung CR-001 mang nhãn CONF, phần PROP/TBD khác vẫn chưa được duyệt; không có bằng chứng kiểm thử runtime.
- Không chọn framework/API/token cụ thể; task tham chiếu hành vi và validation thay vì khóa giải pháp sớm.
- Source/DER/PROP/TBD tuân quy ước requirement chính; cần đọc OQ trước khi dùng làm input coding agent.

| Mã | Given | When | Then |
|---|---|---|---|
| AC-045 | Lô có một dòng thiếu tọa độ và một mã trùng | chạy kiểm tra thử | báo đúng dòng/lý do và chưa tự ghi như thành công |
| AC-046 | CSV hợp lệ theo mẫu và chính sách commit đã duyệt | xác nhận ghi | tạo dữ liệu ở quy trình nháp/duyệt và đối chiếu đủ số dòng |


Ngoài các ví dụ Given/When/Then này, QA phải kiểm thử EX-01–04, các RULE áp dụng và boundary sau khi OQ đã được quyết định; danh sách này không thay thế toàn bộ kế hoạch kiểm thử. Các nội dung CR-001 đã CONF, chỉ chi tiết còn mở mới cần OQ.

## Verification Report — UC-023

### Blocking Issues / Open Questions cần unblock

OQ-19, OQ-21

Các OQ có mức chặn ở requirement §13 ảnh hưởng trực tiếp bước 3–4. Không kết luận “ready to code” khi chưa chốt chính sách liên quan.

### Warnings

- Giới hạn input, cấu hình dịch vụ/quyền/retention phải duyệt theo phạm vi; task chưa phải ước lượng ngày công hay giải pháp cuối.
- Sơ đồ diễn tả luồng chính và nhánh lỗi; chi tiết AF/EX bằng văn bản là một phần đặc tả bắt buộc.

### Passed — kiểm tra tài liệu, không phải test hệ thống

- Mỗi PRE có kiểm tra ở bước 1 và kiểm tra lại trước kết quả tại bước 3–4.
- POST-1 có xử lý ở bước 4–5; POST-2 được giữ bởi kiểm tra quyền và EX-01–04.
- RULE có bước áp dụng và handler; AF có điểm quay lại/kết thúc; EX có Recovery.
- Một primary actor và một mục tiêu; không có bước thực thi chức năng G2.
- Trạng thái có điều kiện vào/ra hoặc trạng thái kết thúc có chủ ý; PROP/TBD không được coi là chính sách đã duyệt.
