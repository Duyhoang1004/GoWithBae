# UC-002: Xem hồ sơ địa điểm

**Version:** 1.1 — 15-11-2026  
**Status:** Draft, chờ duyệt nghiệp vụ; chưa tự coi các PROP/TBD là baseline.  
**Requirement chính:** [GWB-REQ-G1 v1.1](../requirements/GoWithBae_Requirements_G1_v1.1.md)  
**Nguồn khách hàng:** §4.1–4.3/F03  
**Truy vết:** FR-05; AC-003, AC-004.

## 1. Overview

| Trường | Nội dung |
|---|---|
| Goal | Đọc thông tin đã duyệt của một chi nhánh để cân nhắc buổi hẹn. |
| Primary Actor | Khách truy cập web |
| Secondary Actors | Máy chủ GoWithBae; dịch vụ email/bản đồ/tệp/lịch chỉ khi thao tác tương ứng cần tích hợp, theo requirement §7 |
| Trigger | Mở hồ sơ địa điểm |
| Scope IN | Xem hồ sơ địa điểm trên web G1 theo các FR liên kết |
| Scope OUT | Chức năng G2, đặt chỗ/thanh toán/app/AI/chat; quyền và thao tác ngoài mục tiêu UC. Điều hướng sang UC khác không tự thực hiện UC đó. |

## 2. Preconditions

- PRE-1: Mã địa điểm được cung cấp qua kết quả hoặc liên kết trực tiếp. Enforce tại bước 1; kiểm tra lại quyền/trạng thái liên quan tại bước 3–4 trước khi trả dữ liệu hoặc ghi.
- PRE-2: Địa điểm công khai hợp lệ; quyền đọc nơi ẩn trong lịch sử là trường hợp OQ-11, không mặc định công khai. Enforce tại bước 1; kiểm tra lại quyền/trạng thái liên quan tại bước 3–4 trước khi trả dữ liệu hoặc ghi.

## 3. Postconditions

- POST-1: Người xem nhận hồ sơ được phép, có nguồn/ngày cập nhật và không bị dẫn hiểu sai bởi dữ liệu thiếu. Được tạo tại bước 4 và phản hồi tại bước 5.
- POST-2: Quyền riêng tư/phạm vi được giữ; không thay dữ liệu ngoài mục tiêu hoặc thông báo thành công khi bước 4 thất bại. Enforce bước 1, 3–5 và EX-01–04.
- Nếu use case thất bại: không ghi kết quả nghiệp vụ dở dang như đã hoàn tất; dữ liệu đã lưu thành công không bị báo chưa lưu chỉ vì lỗi thông báo/analytics sau đó.

## 4. Main Flow

| Bước | Actor | Hành động | System Response |
|---|---|---|---|
| 1 | Khách truy cập web | Mở hồ sơ địa điểm | Kiểm tra mã và trạng thái công khai hoặc quyền xem lịch sử tương ứng. |
| 2 | Khách truy cập web | Xem các nhóm thông tin | Nạp ảnh hợp lệ, giá hai người, trải nghiệm, tiện ích, giờ, địa chỉ, lưu ý và ngày kiểm tra. |
| 3 | Khách truy cập web | Đọc giá và thuộc tính | Phân biệt known/unknown; không thay dữ liệu thiếu bằng không có, 0 đồng hoặc sao giả. |
| 4 | Khách truy cập web | Mở liên hệ hoặc chỉ đường | Trả liên hệ/địa chỉ có dữ liệu; thao tác lưu/so sánh/tạo kế hoạch chuyển UC tương ứng. |
| 5 | Khách truy cập web | Xem hồ sơ hoàn chỉnh | Hiển thị đúng thứ tự và hành động; ghi place_viewed theo quy tắc phân tích. |

Các xử lý kiểm tra, truy vấn/ghi, chuyển trạng thái, thông báo và analytics trong cột System Response do hệ thống thực hiện, không do actor tự gán trạng thái/quyền.

## 5. Alternate Flows

**AF-01 — Hướng đi hợp lệ khác:** Tại bước 2, một trường chưa xác minh → hiện “Chưa có thông tin”, tiếp tục bước 3; không chặn xem toàn bộ hồ sơ chỉ vì thiếu trường được phép unknown.

## 6. Exception Flows

**EX-01 — PRE/quyền không đạt.**
- Trigger: không đạt một trong PRE, danh tính/quyền/cấp quyền hết hiệu lực hoặc tài nguyên không được đọc.
- At step 1 hoặc kiểm tra lại bước 3–4: từ chối và không trả dữ liệu riêng; không chỉ ẩn nút phía client.
- Recovery: với thao tác tài khoản, đăng nhập lại khi cần; với khách nhận link, chỉ cần link/phiên khách hợp lệ, không bắt đăng nhập để xem. Xin mời lại/liên hệ người có quyền rồi bắt đầu bước 1; không lách bằng đổi id.

**EX-02 — Vi phạm đầu vào hoặc quy tắc nghiệp vụ.**
- Trigger: Mã không tồn tại/không được xem không trả nội dung riêng. Ảnh thiếu quyền sử dụng không được công bố; liên hệ thiếu không tạo số điện thoại hay đường dẫn giả.
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
| RULE-01 | Chỉ địa điểm đã duyệt và đang hiển thị được khám phá/tìm/chốt mới Khi vi phạm: Loại khỏi kết quả; chốt thất bại yêu cầu chọn lại; lịch sử giữ tên/cảnh báo theo quyền | Bước 1/3–4; EX-01/EX-02/EX-03 |
| RULE-04 | Không có dữ liệu giá/giờ/tiện ích không đồng nghĩa bằng 0/đang mở/không có Khi vi phạm: Hiển thị “Chưa có thông tin”; không thỏa bộ lọc cần chứng minh thuộc tính đó | Bước 1/3–4; EX-01/EX-02/EX-03 |
| RULE-28 | Chỉ dữ liệu có quyền sử dụng được công bố; minh họa phải gắn nhãn và tách môi trường thật Khi vi phạm: Không xuất bản ảnh không có quyền; không dùng dữ liệu giả làm số liệu kinh doanh | Bước 1/3–4; EX-01/EX-02/EX-03 |
| RULE-25 | Sự kiện có mã chống trùng; báo cáo tách khách/tài khoản, tự báo/đơn; không ghi dữ liệu nhạy cảm đã cấm Khi vi phạm: Loại sự kiện lặp khỏi tổng; không hợp nhất xuyên thiết bị khi chưa có căn cứ | Bước 3–5; EX-02/EX-03/EX-04 |

## 8. Data Requirements

**Input và validation:**

- place_id: mã chi nhánh bắt buộc, tồn tại và được phép xem.
- contact_action: gọi/chỉ đường tùy chọn, chỉ dùng liên hệ chính thức đã có.

**Output / state:** Người xem nhận hồ sơ được phép, có nguồn/ngày cập nhật và không bị dẫn hiểu sai bởi dữ liệu thiếu. Dữ liệu nghiệp vụ chi tiết tại requirement §6; giới hạn chưa có số phải theo OQ-21, không tự gán giá trị.

**Sequence diagram:** [file Mermaid](./UC-002-view-place.mermaid).

```mermaid
sequenceDiagram
    actor Person as Khách truy cập web
    participant Web as Web
    participant Server as Máy chủ nghiệp vụ
    participant Data as Dữ liệu
    Person->>Web: Mở hồ sơ địa điểm
    Web->>Server: Yêu cầu và danh tính hoặc cấp quyền
    Server->>Server: Bước 1, kiểm tra PRE và quyền
    alt EX-01, không có quyền hoặc sai điều kiện
        Server-->>Web: Từ chối, không lộ dữ liệu riêng
    else Đủ điều kiện
        Person->>Web: Xem các nhóm thông tin
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
                Web-->>Person: Đọc thông tin đã duyệt của một chi nhánh để cân nhắc buổi hẹn.
            end
        end
    end
```

**State machine:** UC này không thêm vòng đời trạng thái riêng. Nếu tham chiếu một entity có trạng thái, áp dụng state machine ở requirement §7; không tạo trạng thái giả chỉ để vẽ sơ đồ.

## 9. Open Questions

OQ-11

Owner, mốc quyết định và mức chặn được ghi tại requirement §13. UC chỉ được baseline sau khi các câu hỏi ảnh hưởng tới nhánh triển khai đã được trả lời hoặc nhánh đó được PO loại khỏi phạm vi rõ ràng.

## 10. Notes và Acceptance Criteria

- Đây là đặc tả yêu cầu, chưa xây phần mềm; nội dung CR-001 mang nhãn CONF, phần PROP/TBD khác vẫn chưa được duyệt; không có bằng chứng kiểm thử runtime.
- Không chọn framework/API/token cụ thể; task tham chiếu hành vi và validation thay vì khóa giải pháp sớm.
- Source/DER/PROP/TBD tuân quy ước requirement chính; cần đọc OQ trước khi dùng làm input coding agent.

| Mã | Given | When | Then |
|---|---|---|---|
| AC-003 | Nơi công khai có trạng thái giá và giờ chưa rõ | mở chi tiết | hiện rõ chưa có thông tin, không hiển thị miễn phí/đang mở hoặc số sao giả |
| AC-004 | Nơi bị ẩn và người xem không có quyền lịch sử | mở URL trực tiếp | không nhận nội dung địa điểm bị hạn chế |


Ngoài các ví dụ Given/When/Then này, QA phải kiểm thử EX-01–04, các RULE áp dụng và boundary sau khi OQ đã được quyết định; danh sách này không thay thế toàn bộ kế hoạch kiểm thử. Các nội dung CR-001 đã CONF, chỉ chi tiết còn mở mới cần OQ.

## Verification Report — UC-002

### Blocking Issues / Open Questions cần unblock

OQ-11

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
