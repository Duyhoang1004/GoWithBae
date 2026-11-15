# GoWithBae

Ngày 12 tháng 11 năm 2026 • Bản đề xuất

**\[Chú thích: (\*) nghĩa là phần này viết để người đọc dễ hiểu và dễ hình dung, chứ không cần làm vì đây chỉ là bản dùng thử đơn giản, chưa làm hoàn chỉnh\]**

# **1 Mục đích và phạm vi** Bối cảnh vấn đề: các cặp đôi khó khăn khi tìm địa điểm đi date mới, khi tìm trên facebook, tiktok,...thì không nhiều lựa chọn vì chỉ xuất hiện bài đăng viral, và các cặp đôi phải tự tổng hợp, tự so sánh giữa nhiều địa điểm.

GoWithBae là nền tảng giúp các cặp đôi **tìm địa điểm hẹn hò** theo ngân sách, vị trí, sở thích và thời gian; so sánh các lựa chọn; cùng chọn và lưu kế hoạch buổi hẹn. Sản phẩm gồm web và app dành cho khách hàng, trang quản trị nội bộ và trang đối tác ở giai đoạn mở rộng.

## **1.1 Phạm vi**

| Hạng mục | Bản đầu G1 | Mở rộng G2 (\*) |
| :---- | :---- | :---- |
| Khách hàng | Tìm, lọc, xem địa điểm, so sánh, lưu, cùng chọn, lập buổi hẹn | Đánh giá công khai, đặt chỗ, thanh toán  |
| Quản trị  | Địa điểm, nhãn lọc, tài khoản, báo lỗi, hỗ trợ, số liệu sử dụng | Đối tác, đơn đặt, hoàn tiền, đối soát  |
| Đối tác (\*) | Gửi thông tin cho nhân sự GoWithBae cập nhật | Đăng nhập quản lý địa điểm và đơn của mình  |
| Nền tảng | Đề xuất web dùng tốt trên điện thoại trước | App iOS và Android dùng chung tài khoản, dữ liệu |

## **1.2 Giả định sử dụng để thiết kế** 

* Thử nghiệm tại TP.HCM; ngôn ngữ tiếng Việt; tiền tệ VND; giờ hiển thị theo Việt Nam.  
* Một buổi hẹn phục vụ hai người. Các khoảng chi phí phải ghi rõ tính cho hai người và gồm những gì.  
* Chưa đưa vào phạm vi: mạng xã hội, chat riêng, ví tiền, chia tiền, ghép đôi tìm người yêu, gợi ý bằng AI và lịch trình nhiều điểm tự động.

# **2 Người dùng và màn hình**

## **2.1 Nhóm sử dụng**

| Nhóm | Quyền sử dụng |
| :---- | :---- |
| Khách chưa đăng nhập | Tìm, lọc, xem, so sánh; tham gia danh sách được mời bằng liên kết riêng |
| Thành viên | Quyền của khách; lưu danh sách, tạo lời mời, quản lý buổi hẹn, gửi hỗ trợ |
| Đối tác G2 (\*) | Quản lý các địa điểm được giao; gói dịch vụ, suất đặt, đơn và đối soát của mình |
| Quản trị nội bộ | Quyền theo **vai trò** tại mục 7; không mặc định mọi nhân viên có toàn quyền |

Mỗi người có một tài khoản riêng. Không bắt buộc liên kết tài khoản người yêu để sử dụng. Chưa xây hồ sơ “cặp đôi” cố định trong G1.

## **2.2 Danh sách màn hình khách hàng**

| Mã | Màn hình | Thành phần chính |
| :---- | :---- | :---- |
| UI01 | Khám phá | Tìm kiếm; khu vực; ngân sách; nhóm hoạt động; bộ sưu tập biên tập |
| UI02 | Kết quả | Bộ lọc; số kết quả; sắp xếp; thẻ địa điểm; chuyển danh sách và bản đồ |
| UI03 | Chi tiết địa điểm | Ảnh; giá; hoạt động; tiện ích; giờ; địa chỉ; lưu; so sánh; báo sai |
| UI04 | So sánh | Tối đa ba nơi; cùng các tiêu chí; thêm hoặc bỏ lựa chọn |
| UI05 | Đã lưu | Danh sách cá nhân; tạo, đổi tên, xóa; thêm và bỏ địa điểm |
| UI06 | Cùng chọn | Danh sách được mời; chọn thích hoặc bỏ qua; kết quả trùng |
| UI07 | Buổi hẹn | Địa điểm đã chốt; ngày giờ; ghi chú; chỉ đường; sửa hoặc hủy kế hoạch |
| UI08 | Tài khoản và hỗ trợ | Đăng nhập; hồ sơ; cài đặt; yêu cầu hỗ trợ; đăng xuất |
| UI09 G2  (\*) | Đặt chỗ và thanh toán | Gói; suất; thông tin khách; tổng tiền; chính sách; thanh toán |
| UI10 G2 (\*) | Chi tiết đơn | Xác nhận; mã sử dụng; hủy; hoàn tiền; lịch sử trạng thái |

Điều hướng chính: Khám phá, Đã lưu, Buổi hẹn, Tài khoản. 

## **2.3 Quy tắc giao diện chung**

- Cho phép xem địa điểm trước khi đăng nhập. 

Mọi màn hình có dữ liệu cần có trạng thái đang tải, chưa có dữ liệu và lỗi kèm cách thử lại. Trên điện thoại, bộ lọc mở thành bảng riêng, có “Áp dụng” và “Xóa bộ lọc”. 

**2.4 Logo và phong cách giao diện**

**Logo:** ![][image1]

**Giao diện:** Thiết kế theo phong cách **trẻ trung, nhẹ nhàng và dễ thương**. Dùng xanh da trời và trắng làm màu chủ đạo, điểm chút cam nhạt theo logo. Các nút và thẻ địa điểm có thể bo góc nhẹ. 

# **3 Tìm kiếm và bộ lọc**

## **3.1 Tìm kiếm và hiển thị kết quả F01**

Khách nhập tên địa điểm, khu vực hoặc hoạt động. Hệ thống tìm trong dữ liệu đã được duyệt và đang hiển thị. Tìm kiếm không phân biệt chữ hoa, chữ thường và hỗ trợ nhập tiếng Việt không dấu. G1 chưa cần hiểu câu hỏi tự nhiên dài.

Mỗi thẻ kết quả gồm ảnh đại diện, tên, khu vực, chi phí dự kiến cho hai người, tối đa ba nhãn nổi bật và nút lưu, so sánh. Chỉ hiện khoảng cách khi có điểm xuất phát; ghi rõ là khoảng cách ước tính, không dùng thay thời gian di chuyển.

Mặc định sắp xếp theo mức khớp từ khóa \-\>  sau đó độ mới của thông tin đã xác minh \-\> dùng mã địa điểm để ổn định thứ tự khi bằng nhau. 

Có tùy chọn gần nhất và giá thấp đến cao. Giá chưa rõ xếp cuối khi sắp theo giá. Không ưu tiên trả phí trong G1.

## **3.2 Danh mục bộ lọc F02**

| Nhóm | Giá trị hoặc cách nhập |
| :---- | :---- |
| Khu vực | Chọn khu vực từ danh mục; hoặc chọn một điểm và bán kính |
| Ngân sách hai người | Miễn phí; dưới 200.000đ; 200.000 đến dưới 400.000đ; 400.000 đến 700.000đ; trên 700.000đ |
| Thời gian | Ngày và giờ dự định đến; đối chiếu lịch mở cửa và ngày nghỉ riêng |
| Hoạt động | Ăn uống, cà phê, vẽ tranh, làm gốm, làm bánh, xem phim, triển lãm, trò chơi, vận động |
| Không khí | Yên tĩnh, lãng mạn, sôi động, ấm áp, riêng tư |
| Phong cách | Vintage, tối giản, hiện đại, nghệ thuật, sân vườn |
| Cảnh quan | Nhiều cây, ven hồ, ven sông, rooftop, ngắm hoàng hôn |
| Đặc điểm | Có mèo tại quán, có chó tại quán, cho mang thú cưng, band nhạc, ngoài trời |
| Tiện ích | Khu không hút thuốc, tiếp cận xe lăn |
| Dịp hẹn | Hẹn đầu, cuối tuần, kỷ niệm, sinh nhật |

## **3.3 Quy tắc kết hợp**

Các nhóm lọc kết hợp theo điều kiện “và”. Nhiều lựa chọn trong nhóm hoạt động, phong cách, cảnh quan hoặc dịp hẹn được hiểu là “ít nhất một”. Các tiện ích, đặc điểm và yêu cầu không khí đã chọn phải cùng được đáp ứng. Giao diện ghi rõ quy tắc cạnh nhóm lọc.

Lọc giá theo mức chi phí dự kiến chuẩn cho hai người; luôn hiển thị thêm khoảng giá và nội dung bao gồm. Nơi chưa có giá không xuất hiện khi áp dụng lọc ngân sách. Nơi chưa có thông tin giờ không được coi là mở cửa.

Không có kết quả: giữ bộ lọc, báo rõ và đề xuất bỏ hoặc mở rộng từng điều kiện; chỉ thay đổi khi khách chọn. Từ chối quyền vị trí: cho nhập khu vực thủ công. Bản đồ lỗi: vẫn xem được danh sách.

# **4 Hồ sơ địa điểm và so sánh**

## **4.1 Dữ liệu địa điểm F03**

Mỗi chi nhánh có một mã địa điểm riêng.

| Nhóm trường | Thông tin cần lưu |
| :---- | :---- |
| Nhận diện bắt buộc | Mã, tên, loại hình, địa chỉ, khu vực, tọa độ, mô tả ngắn |
| Giá bắt buộc có trạng thái | Đã biết hoặc chưa rõ; giá dự kiến chuẩn cho hai người; khoảng giá |
| Hoạt động và nhãn | Hoạt động chính; phong cách; không khí; cảnh quan; tiện ích; dịp phù hợp |
| Giờ hoạt động | Các khoảng mở cửa từng ngày; ngày nghỉ hoặc giờ ngoại lệ |
| Hình ảnh | Ảnh đại diện; thứ tự; mô tả; nguồn và quyền sử dụng |
| Liên hệ | Số điện thoại, website hoặc kênh liên hệ chính thức nếu có |
| Lưu ý | Thời lượng dự kiến, cần đặt trước |
| Quản lý | Trạng thái; người tạo/duyệt |

Giá trị chưa xác minh phải để “Chưa có thông tin”, không tự gán “Không có”.

## **4.2 Điều kiện xuất bản**

Phải có tên, loại hình, địa chỉ, tọa độ, mô tả, ít nhất một ảnh được phép sử dụng, hoạt động chính, trạng thái giá, trạng thái giờ mở cửa, nguồn và ngày kiểm tra. Người duyệt kiểm tra trùng chi nhánh trước khi xuất bản.

Trạng thái: Nháp → Chờ duyệt → Đang hiển thị. Có thể trả về nháp kèm lý do hoặc chuyển sang Tạm ẩn/Ngừng hoạt động. Không xóa lịch sử của địa điểm đã nằm trong buổi hẹn hoặc đơn hàng.

## **4.3 Trang chi tiết**

Hiển thị các nhóm dữ liệu trên theo thứ tự: ảnh và tên; giá; trải nghiệm; tiện ích; giờ và địa chỉ; lưu ý; ngày cập nhật. G1 có nút gọi, mở chỉ đường, lưu, so sánh, chọn cho buổi hẹn và báo sai. Không hiển thị số sao hoặc đánh giá giả khi chưa có dữ liệu.

## **4.4 So sánh F04**

Cho chọn hai hoặc ba địa điểm. So sánh theo giá hai người, khu vực/khoảng cách, giờ, hoạt động, không khí, tiện ích và yêu cầu đặt trước. Thiếu trường nào ghi “Chưa có thông tin”. Chặn thêm địa điểm thứ tư và hướng dẫn bỏ một nơi trước. Khách chọn một nơi từ bảng để tạo buổi hẹn.

# **5 Lưu địa điểm và cùng chọn**

## **5.1 Danh sách đã lưu F05**

Thành viên tạo, đổi tên, xóa danh sách; thêm hoặc bỏ địa điểm. Một địa điểm không được xuất hiện hai lần trong cùng danh sách. Danh sách mặc định riêng tư. Xóa danh sách không xóa địa điểm trong hệ thống hoặc buổi hẹn đã tạo trước đó.

Địa điểm tạm ẩn/ngừng hoạt động vẫn hiện tên và cảnh báo trong danh sách cũ; không được chốt cho buổi hẹn mới. Thông tin địa điểm được cập nhật theo dữ liệu hiện tại.

## **5.2 Cùng chọn F06**

1\. Người tạo chọn từ hai đến năm địa điểm đã lưu, tạo một phiên cùng chọn và lấy liên kết mời.

2\. Người nhận mở link, nhập tên hiển thị và tham gia; không bắt buộc đăng ký. Một phiên có người tạo và một người nhận.

3\. Mỗi người đánh dấu “Thích” hoặc “Bỏ qua” cho từng nơi. Chưa chọn được lưu là “Chưa trả lời”. Có thể sửa trước khi chốt.

4\. Hiển thị riêng các địa điểm cả hai cùng thích; các nơi còn lại vẫn xem được. Không có điểm trùng thì đề nghị thay lựa chọn hoặc chọn lại.

5\. Người tạo quyết định nơi cuối cùng và chuyển sang màn hình tạo buổi hẹn. Không tự chốt dựa trên số lượt thích.

Liên kết dùng mã khó đoán, không đưa vào kết quả tìm kiếm công khai. Bất kỳ ai có link hợp lệ đều có thể xem và chiếm suất người nhận; cần giải thích điều này trước khi chia sẻ. Người tạo được thu hồi link, loại người nhận và tạo link mới. Link đề xuất hết hạn sau bảy ngày.

Sau khi người nhận tham gia, danh sách lựa chọn được cố định. Muốn đổi địa điểm, người tạo tạo phiên mới; không làm mất nghĩa của lượt chọn cũ. Khi đã chốt, phiên chuyển sang chỉ xem. Người nhận dùng phiên truy cập riêng; mất phiên thì người tạo có thể cấp lại lời mời.

## **5.3 Buổi hẹn F07**

Thông tin gồm người tạo, địa điểm, ngày giờ, ghi chú và người nhận nếu có. Ngày giờ phải ở tương lai khi tạo. Nếu địa điểm có giờ đã xác minh và đóng cửa vào thời điểm chọn, chặn lưu và yêu cầu đổi giờ; chưa có giờ thì cảnh báo.

Trạng thái kế hoạch: Đã lên lịch, Đã hủy, Đã qua giờ. Khi qua giờ, hệ thống hỏi “Bạn đã đến đây chưa?” và ghi nhận Có/Không/Chưa trả lời riêng. Không tự coi kế hoạch đã qua giờ là khách đã đến.

Người tạo được sửa giờ, ghi chú hoặc hủy; khi đổi địa điểm cần kiểm tra lại điều kiện như lúc tạo. Người nhận chỉ xem kế hoạch được chia sẻ. Khi chỉnh sửa, ghi thời điểm cập nhật và tạo thông báo cho tài khoản liên quan; khách nhận link xem thông tin mới khi mở lại.

G1 hỗ trợ mở chỉ đường và thêm vào lịch cá nhân. Trang buổi hẹn ghi rõ: “Kế hoạch này chưa giữ chỗ tại địa điểm”.

# **6 Tài khoản và hỗ trợ**

## **6.1 Tài khoản F08**

Phương án đăng nhập đề xuất cho G1: email và mã xác thực dùng một lần. Mã có hạn sử dụng, giới hạn gửi lại và số lần nhập sai; đội IT đề xuất cấu hình trước triển khai. Không yêu cầu người dùng khai tình trạng quan hệ hoặc thông tin của người yêu.

Hồ sơ gồm mã người dùng, tên hiển thị, email đã xác thực, ảnh đại diện tùy chọn, ngày tạo và trạng thái tài khoản. Cho sửa tên/ảnh, đăng xuất, yêu cầu xóa tài khoản. Đổi email phải xác thực email mới. Tài khoản bị khóa không được tạo hoặc chỉnh sửa dữ liệu.

Web và app dùng chung tài khoản, danh sách và buổi hẹn. Quyền xem và sửa được kiểm tra tại máy chủ, không chỉ ẩn nút trên màn hình.

## **6.2 Báo sai địa điểm F09**

Từ trang địa điểm, khách chọn lý do: sai giá, sai giờ, sai địa chỉ, đã đóng cửa, ảnh không đúng hoặc vấn đề khác. Cho nhập mô tả và gửi ảnh minh chứng tùy chọn. Khách chưa đăng nhập vẫn gửi được, có giới hạn chống gửi hàng loạt.

Hệ thống tạo mã báo lỗi, gắn mã địa điểm, nội dung, người gửi nếu có và thời gian. Báo lỗi không tự sửa dữ liệu công khai. Nhân sự kiểm tra, cập nhật nếu đúng và lưu kết quả xử lý.

Trạng thái: Mới → Đang kiểm tra → Đã xử lý hoặc Không đủ căn cứ. Đóng báo lỗi phải có ghi chú. Báo lỗi đã đóng có thể được mở lại bởi nhân sự có quyền.

## **6.3 Yêu cầu hỗ trợ F10**

Thành viên gửi yêu cầu theo nhóm: tài khoản, sử dụng tính năng, thông tin địa điểm; G2 bổ sung đặt chỗ và thanh toán. Mỗi yêu cầu có mã, chủ đề, nội dung, tệp đính kèm, người phụ trách, trạng thái và lịch sử phản hồi.

Khách xem được yêu cầu của mình và trả lời bổ sung. Nhân sự nhận việc, phản hồi và đóng yêu cầu. Trạng thái: Mới, Đang xử lý, Chờ khách phản hồi, Đã đóng. Không xây chat thời gian thực trong G1.

## **6.4 Thông báo và quyền riêng tư**

G1 có thông báo trong tài khoản về cập nhật buổi hẹn và hỗ trợ. Email dùng cho xác thực và thông báo xử lý cần thiết; nội dung tiếp thị có lựa chọn nhận riêng. Thông báo đẩy trên app thuộc phạm vi app.

Không hiển thị email, lịch sử tìm kiếm, danh sách riêng hoặc vị trí của người dùng khác. Không theo dõi vị trí liên tục. Khi thu hồi chia sẻ, link cũ phải mất quyền truy cập ngay. Yêu cầu xóa tài khoản cần thu hồi phiên đăng nhập và link riêng; chính sách lưu dữ liệu giao dịch ở G2 phải được thống nhất trước khi bật thanh toán.

# **7 Trang quản trị nội bộ**

## **7.1 Các phân hệ quản trị**

| Mã | Phân hệ | Thao tác cần có |
| :---- | :---- | :---- |
| A01 | Địa điểm | Tạo, sửa, xem trước, gửi duyệt, duyệt, ẩn; lọc theo khu vực và trạng thái; phát hiện trùng |
| A02 | Danh mục | Quản lý loại hình, nhãn, khu vực; ngừng dùng nhãn mà không làm mất dữ liệu cũ |
| A03 | Nội dung khám phá | Tạo bộ sưu tập, chọn địa điểm, ảnh, thứ tự và thời gian hiển thị |
| A04 | Tài khoản | Tìm tài khoản, xem trạng thái, khóa/mở khóa kèm lý do; quản lý vai trò nhân viên |
| A05 | Báo sai và hỗ trợ | Lọc hàng chờ, giao việc, phản hồi, cập nhật trạng thái và ghi chú nội bộ |
| A06 | Báo cáo | Chọn thời gian, khu vực; xem số liệu theo mục 10; xuất CSV theo quyền |
| A07 G2 | Đối tác và đơn | Duyệt đối tác, gán địa điểm, xem/xử lý đơn và lịch sử |
| A08 G2 | Tài chính | Tra giao dịch, xử lý hoàn tiền, lập kỳ đối soát, ghi nhận thanh toán đối tác |

## **7.2 Phân quyền**

| Vai trò | Quyền chính | Giới hạn |
| :---- | :---- | :---- |
| Quản trị cao nhất | Cấu hình, cấp quyền, khóa tài khoản, xem nhật ký | Mọi thay đổi quyền phải ghi nhật ký |
| Biên tập | Tạo/sửa nháp, nhập dữ liệu, tiếp nhận báo sai | Không tự xuất bản hoặc xem tài chính |
| Duyệt nội dung | Duyệt, trả về, ẩn địa điểm, quản lý nhãn | Không hoàn tiền hoặc đổi tài khoản nhận tiền |
| Hỗ trợ và đối tác | Xử lý yêu cầu, quản lý đối tác/đơn trong phạm vi | Không cấp quyền quản trị hoặc tự chuyển tiền |
| Tài chính G2 | Đối soát, đề nghị/thực hiện giao dịch theo quyền | Không sửa nội dung để thay giá đơn đã tạo |

Một người có thể được gán nhiều vai trò. Đây là phân quyền hệ thống, không phải yêu cầu tuyển đủ từng người. Số nhân sự vận hành do phía kinh doanh quyết định theo khối lượng công việc.

## **7.3 Nhật ký và quản lý dữ liệu**

Ghi người thực hiện, thời gian, đối tượng, giá trị trước/sau và lý do cho các thao tác duyệt/ẩn, đổi giá, khóa tài khoản, cấp quyền, hoàn tiền, thay thông tin nhận tiền. Nhân viên không được sửa/xóa nhật ký qua giao diện.

G1 có mẫu nhập địa điểm CSV và kiểm tra thử trước khi lưu: báo dòng thiếu dữ liệu, mã trùng hoặc tọa độ sai. Dòng lỗi không được âm thầm bỏ qua. Ảnh tải lên riêng và gắn mã địa điểm. Đội kinh doanh chịu trách nhiệm cung cấp và xác minh nội dung; đội IT xây công cụ nhập, kiểm tra định dạng và duyệt.

# **8 Đối tác và đặt chỗ ở G2**

## **8.1 Trang đối tác B01**

Đối tác đăng nhập để quản lý đúng các địa điểm được gán. Nhân viên đối tác có quyền riêng cho nội dung, nhận đơn và xem đối soát. Sửa nội dung công khai phải qua duyệt; được đóng suất bán ngay để tránh nhận quá số lượng.

Mỗi gói có mã, địa điểm, mô tả dịch vụ, số người, giá, phần bao gồm, phụ phí, chính sách hủy, thời gian áp dụng và các suất. Mỗi suất có ngày giờ, sức chứa, số chỗ còn lại và trạng thái mở/đóng. Gói hai người phải thể hiện rõ số lượng mua một gói là hai người.

## **8.2 Quy trình đặt chỗ B02**

Đề xuất G2 ban đầu dùng suất được đối tác mở sẵn và xác nhận tự động. Mỗi đơn gồm một gói tại một địa điểm, một suất; chưa có giỏ nhiều địa điểm. Chỉ đặt được khi đối tác đã duyệt và gói đang bán.

1\. Khách chọn gói, suất và số lượng; nhập tên, số liên hệ; xem tổng tiền và chính sách.

2\. Máy chủ kiểm tra sức chứa, giá và hiệu lực gói. Tạo đơn Chờ thanh toán, giữ suất trong thời gian cấu hình; đề xuất 10 phút.

3\. Khách thanh toán. Khi hệ thống nhận xác nhận hợp lệ từ đơn vị thanh toán, chuyển đơn sang Đã xác nhận và cấp mã sử dụng.

4\. Đối tác kiểm tra mã, ghi nhận khách đến. Sau khi dịch vụ hoàn tất, đơn chuyển sang Hoàn tất.

5\. Khách hoặc đối tác hủy theo chính sách đã lưu trên đơn; xử lý tiền theo mục 9\.

## **8.3 Trạng thái và ngoại lệ**

Trạng thái đơn: Chờ thanh toán, Đã xác nhận, Đã sử dụng, Hoàn tất, Đã hủy, Hết hạn, Không đến. Đối tác xác nhận Đã sử dụng; chỉ nhân sự có quyền mới điều chỉnh trường hợp ghi sai, phải lưu lý do.

Giữ suất hết hạn mà chưa trả tiền: chuyển Hết hạn và trả suất. Thanh toán đến muộn: kiểm tra suất và trạng thái; không tự xác nhận nếu không thể cung cấp dịch vụ, tạo việc xử lý/hoàn tiền. Hai khách mua suất cuối cùng không được cùng thành công.

Quét mã lần hai không tạo thêm lượt sử dụng. Mã không hợp lệ, sai đối tác hoặc đơn đã hủy phải bị từ chối. Đơn Không đến chỉ ghi sau thời gian quy định trong chính sách; không tự suy ra từ việc khách chưa đánh giá.

## **8.4 Đánh giá sau trải nghiệm B03**

Thành viên có thể gửi một đánh giá cho một địa điểm, chỉnh sửa được; lưu lịch sử. Nội dung gồm điểm tổng 1–5, nhận xét và ảnh tùy chọn. Chỉ gắn nhãn “Đã đặt và sử dụng” khi có đơn phù hợp; tự xác nhận đã đi không được dùng nhãn này. Có báo cáo vi phạm, duyệt/ẩn kèm lý do; không tự ẩn chỉ vì điểm thấp.

# **9 Thanh toán và đối soát ở G2**

## **9.1 Mô hình đề xuất P01**

Tài liệu dùng phương án khách trả trước toàn bộ giá trị gói qua đơn vị thanh toán tích hợp. GoWithBae hưởng hoa hồng theo thỏa thuận với đối tác. Chưa triển khai ví tiền, trả góp, chia tiền hoặc thu cọc. Nếu chọn khách trả tại quán thì quy trình thu hoa hồng phải được đặc tả lại trước khi xây.

Đơn phải lưu bản chụp giá, tên gói, phần bao gồm, chính sách hủy và tỷ lệ hoa hồng tại thời điểm mua. Sửa thông tin gói về sau không thay đổi đơn cũ. G1 không cần tích hợp thanh toán này.

## **9.2 Xử lý giao dịch**

Mỗi giao dịch có mã nội bộ, mã đơn, mã nhà cung cấp, số tiền, tiền tệ, trạng thái và thời gian. Tách trạng thái tiền khỏi trạng thái đặt chỗ: Chưa trả, Đang xử lý, Đã trả, Thất bại, Đang hoàn, Đã hoàn một phần, Đã hoàn toàn bộ.

Chỉ xác nhận đã trả sau thông báo máy chủ có xác thực hoặc tra soát trực tiếp với nhà cung cấp. Không tin riêng trang “Thanh toán thành công” trên thiết bị khách. Thông báo lặp không được ghi nhận hai lần; kiểm tra đúng mã đơn, số tiền và tiền tệ. Không lưu số thẻ đầy đủ hoặc mã bảo mật thẻ.

Khách đóng trình duyệt vẫn xem được trạng thái đơn khi quay lại. Giao dịch chưa rõ kết quả phải tra soát trước khi cho trả lại để hạn chế thu hai lần. Giao dịch thu thừa được đưa vào hàng xử lý hoàn tiền.

## **9.3 Hủy và hoàn tiền P02**

Hiển thị điều kiện hủy trước khi thanh toán và lưu cùng đơn. Khi yêu cầu hủy, hệ thống tính số tiền dự kiến được hoàn theo chính sách; khách xác nhận trước khi gửi. Nhân sự chỉ được hoàn trong số tiền đã thu trừ phần đã hoàn, phải có lý do và nhật ký.

Trạng thái “Đã hoàn” chỉ xuất hiện sau xác nhận của đơn vị thanh toán. Hoàn thất bại cần việc xử lý và thông báo cho khách, không đổi thành thành công bằng thao tác giao diện. G2 cần cấu hình ngưỡng hoàn tiền phải được người thứ hai duyệt.

## **9.4 Đối soát với đối tác P03**

Mỗi kỳ đối soát liệt kê từng đơn đủ điều kiện, giá trị dịch vụ, khoản hủy/hoàn, hoa hồng, phí/khuyến mãi do từng bên chịu, điều chỉnh và số tiền còn phải trả. Đơn tranh chấp hoặc chưa hoàn tất xử lý được tạm giữ ngoài kỳ thanh toán.

Trạng thái kỳ: Nháp → Chờ duyệt → Được duyệt → Đã thanh toán. G2 ban đầu cho chuyển tiền đối tác qua quy trình tài chính bên ngoài, nhập mã tham chiếu và chứng từ vào hệ thống. Đánh dấu “Đã thanh toán” không tự phát lệnh chuyển tiền.

Ví dụ chỉ minh họa, chưa tính phí/thuế: đơn 400.000đ, hoa hồng 10% thì hoa hồng gộp 40.000đ, phần đối tác 360.000đ. Không hiển thị tổng tiền khách trả thành doanh thu hoa hồng hoặc lợi nhuận. Tỷ lệ, lịch trả, trách nhiệm hóa đơn và cách hạch toán phải được phía kinh doanh/kế toán chốt.

# **10 Báo cáo và dữ liệu cần ghi nhận**

## **10.1 Báo cáo G1**

| Chỉ số | Định nghĩa sử dụng |
| :---- | :---- |
| Người dùng hoạt động | Số tài khoản hoặc mã khách ẩn danh có hoạt động trong kỳ; tách hai nhóm |
| Lượt tìm kiếm | Mỗi lần gửi truy vấn hoặc áp dụng bộ lọc; không tính từng ký tự đang gõ |
| Tỷ lệ tìm không ra | Lượt tìm có 0 kết quả chia tổng lượt tìm trong kỳ |
| Lượt xem địa điểm | Sự kiện mở chi tiết; báo cáo thêm số người xem khác nhau |
| Lượt lưu | Số lần thêm địa điểm vào danh sách thành công; tách lượt và số người |
| Tỷ lệ chốt từ tìm | Phiên có tìm kiếm và tạo buổi hẹn chia số phiên có tìm kiếm |
| Thời gian đến lúc chốt | Trung vị thời gian từ tìm lần đầu đến tạo buổi hẹn đầu tiên trong cùng phiên |
| Quay lại 30 ngày | Người có phiên đầu trong nhóm theo dõi và có hoạt động vào ngày 1–30 sau đó; chỉ tính nhóm đã đủ 30 ngày |
| Chất lượng dữ liệu | Địa điểm thiếu giá/giờ, quá hạn xác minh, báo sai đang mở |

Đề xuất phiên kết thúc sau 30 phút không hoạt động. Không khẳng định đo được một người duy nhất trên nhiều thiết bị khi chưa đăng nhập. Ghi nhận việc khách tự xác nhận đã đi riêng với đơn đã sử dụng.

## **10.2 Báo cáo G2**

Theo dõi đơn đã xác nhận, hoàn tất, hủy, không đến; giá trị giao dịch; tiền đã thu; tiền đã hoàn; hoa hồng gộp; khoản phải trả đối tác; khoản đã trả và giao dịch cần tra soát. Có bảng chi tiết để đối chiếu số tổng với từng đơn.

Tỷ lệ hủy tính trên nhóm đơn đã xác nhận, loại đơn hết hạn chưa trả tiền. Tách ngày tạo đơn, ngày dùng dịch vụ và ngày nhận tiền bằng bộ lọc rõ ràng; không cộng lẫn các mốc. Khi có hoàn tiền, báo cáo phải phản ánh điều chỉnh theo quy tắc kỳ được thống nhất.

## **10.3 Sự kiện và bộ lọc báo cáo**

Ghi sự kiện: search\_submitted, results\_shown, place\_viewed, place\_saved, comparison\_opened, invite\_created, vote\_submitted, date\_plan\_created, visit\_self\_reported; G2 bổ sung booking\_created, payment\_confirmed, booking\_used và refund\_confirmed.

Mỗi sự kiện có mã riêng để chống trùng, thời gian, mã phiên, nền tảng web/app, mã người dùng hoặc khách, mã đối tượng và thuộc tính liên quan. Không ghi mã xác thực, thông tin thẻ hoặc ghi chú riêng của buổi hẹn vào dữ liệu phân tích.

Báo cáo lọc theo khoảng ngày, khu vực, loại hình và nền tảng. G1 chấp nhận cập nhật báo cáo trong 24 giờ; trạng thái đơn và thanh toán phải cập nhật theo kết quả xử lý, không chờ báo cáo ngày. Xuất CSV phải áp dụng đúng bộ lọc và quyền truy cập hiện tại.

# **11 Yêu cầu chất lượng và vận hành**

## **11.1 Hiệu năng và tương thích**

Mục tiêu đề xuất để đội IT xác nhận: tìm kiếm trả kết quả trong 2 giây ở 95% yêu cầu, với 1.000 địa điểm và 100 người dùng đồng thời trong môi trường kiểm thử thống nhất. Báo cáo kiểm thử phải ghi cấu hình, dữ liệu và điều kiện mạng; không dùng con số này như cam kết trước khi thống nhất.

Web hiển thị tốt từ màn hình điện thoại rộng 360 px đến máy tính; kiểm tra Chrome, Safari và Edge hiện hành tại thời điểm bàn giao. App cần chốt phiên bản iOS/Android tối thiểu riêng. Không để chữ, nút hoặc bảng quan trọng tràn khỏi màn hình.

Ô nhập có nhãn; lỗi chỉ rõ trường cần sửa; nút có trạng thái khi đang xử lý để tránh bấm lặp. Dùng được các thao tác chính bằng bàn phím trên web, ảnh có mô tả cần thiết, chữ có độ tương phản dễ đọc.

## **11.2 Bảo mật và dữ liệu**

Kiểm tra quyền tại máy chủ cho mọi danh sách, buổi hẹn, yêu cầu hỗ trợ, đơn và dữ liệu đối tác. Người dùng đổi mã đối tượng trên đường dẫn không được xem dữ liệu người khác. Quản trị nội bộ dùng xác thực hai bước trước khi vận hành thực tế.

Mã mời, mã đăng nhập và khóa tích hợp không được ghi công khai. Tệp tải lên phải giới hạn loại/kích thước và kiểm tra nội dung; cấu hình được thống nhất trước phát hành. Có giới hạn yêu cầu để hạn chế dò mã và gửi hàng loạt.

## **11.3 Sao lưu và giám sát**

Đề xuất sao lưu dữ liệu hằng ngày, giữ 30 ngày; kiểm tra khôi phục trước phát hành. G1 đặt mục tiêu mất tối đa 24 giờ dữ liệu và khôi phục trong 8 giờ. G2 phải có phương án sao lưu/khôi phục tốt hơn cho giao dịch, được chốt trước khi nhận tiền.

Theo dõi lỗi hệ thống, lỗi gửi email, bản đồ, đặt chỗ, thanh toán và công việc nền. Lỗi phải có mã truy vết để hỗ trợ tra cứu. Đội IT đề xuất kênh cảnh báo, người nhận và thời gian phản hồi sự cố; cần có người chịu trách nhiệm sau bàn giao.

## **11.4 Tích hợp và chi phí duy trì**

Đội IT liệt kê dịch vụ ngoài dự kiến dùng: bản đồ, email xác thực, lưu ảnh, máy chủ, giám sát; G2 thêm thanh toán; app thêm thông báo đẩy. Nêu chi phí khởi tạo, định kỳ và theo lượng sử dụng. Tài khoản dịch vụ và tên miền cần thuộc quyền quản lý của dự án.

Không tự lấy ảnh, bài đánh giá hoặc dữ liệu của nền tảng khác làm dữ liệu chính thức khi chưa thống nhất quyền sử dụng. Dữ liệu thật do phía kinh doanh cung cấp hoặc cho phép thu thập; dữ liệu minh họa phải được gắn nhãn và tách khỏi môi trường thật.

# **12 Nghiệm thu và quyết định cần chốt**

## **12.1 Các tình huống nghiệm thu tối thiểu**

| Mã | Tình huống | Kết quả cần đạt |
| :---- | :---- | :---- |
| T01 | Tìm không dấu, lọc đồng thời ngân sách và tiện ích | Đúng quy tắc mục 3; không trả nơi thiếu điều kiện |
| T02 | Không có kết quả hoặc từ chối vị trí | Giữ bộ lọc; có cách chọn khu vực hoặc nới điều kiện |
| T03 | Giá/giờ chưa rõ; địa điểm tạm ẩn | Hiển thị đúng trạng thái; không tạo thông tin thay thế |
| T04 | So sánh và lưu lặp | Tối đa ba nơi; không trùng trong cùng danh sách |
| T05 | Hai người chọn; hết hạn hoặc thu hồi link | Kết quả trùng chính xác; link mất hiệu lực bị chặn |
| T06 | Sửa buổi hẹn và đổi mã truy cập | Người có quyền sửa được; người khác không truy cập được |
| T07 | Biên tập tự duyệt hoặc hỗ trợ xem tài chính | Bị từ chối theo quyền; ghi nhật ký thao tác cần thiết |
| T08 | Báo cáo trên dữ liệu kiểm thử biết trước | Số tổng khớp dữ liệu chi tiết, xuất CSV đúng bộ lọc |
| T09 G2 | Hai khách đặt suất cuối; thanh toán lặp/muộn | Không bán quá suất, thu/ghi nhận trùng; có xử lý ngoại lệ |
| T10 G2 | Hủy, hoàn một phần, hoàn thất bại, đối soát | Đúng chính sách lưu trên đơn; số tiền không vượt tiền thu |

## **12.2 Các quyết định cần thống nhất**

Trước thiết kế: phạm vi G1; web hay web và app cùng đợt; khu vực thử nghiệm; logo/màu sắc; danh mục lọc; cách tính chi phí chuẩn cho hai người; phương thức đăng nhập. Trước nghiệm thu: cấu hình link mời, tải tệp, hiệu năng, thời gian hỗ trợ và trách nhiệm bảo trì.

Trước G2: nhà cung cấp thanh toán; chính sách hủy/không đến; thời hạn giữ suất; tỷ lệ hoa hồng; phí/khuyến mãi do bên nào chịu; kỳ đối soát; người duyệt hoàn tiền; điều kiện ghi Hoàn tất; chính sách dữ liệu và chứng từ giao dịch. Các mục này không được tự thay bằng chính sách mặc định của đội lập trình.

## **12.3 Bàn giao**

* Thiết kế màn hình và luồng có thể bấm thử, gồm cả trạng thái trống/lỗi; được thống nhất trước khi xây giao diện.  
* Mã nguồn, hướng dẫn cài đặt, cấu trúc dữ liệu, mô tả giao tiếp hệ thống và cấu hình dịch vụ ngoài; tách môi trường thử với môi trường thật.  
* Tài khoản quản trị thuộc dự án; hướng dẫn nhập địa điểm, phân quyền, xem báo cáo, sao lưu và khôi phục.  
* Kết quả kiểm thử theo mã yêu cầu; danh sách lỗi còn lại; phạm vi bảo hành/bảo trì và chi phí duy trì.

Đội IT phản hồi bằng bảng hạng mục G1/G2, nội dung đã bao gồm/chưa bao gồm, phụ thuộc cần phía kinh doanh cung cấp, ước lượng thời gian và chi phí. Mọi thay đổi phạm vi sau khi thống nhất cần được ghi lại trước khi triển khai.

[image1]: <data:image/png;base64,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>