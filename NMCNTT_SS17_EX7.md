BÀI TẬP TÌNH HUỐNG: PHÂN TÍCH QUY TRÌNH HOẠT ĐỘNG CỦA TRÌNH DUYỆT CAREPULSE
1. Xác định 4 lỗi kỹ thuật trong báo cáo của thực tập sinh
Lỗi 1: Bỏ qua bước DNS Lookup

Nhận định sai: Trình duyệt gửi URL trực tiếp đến Web Server để phân giải cổng.

Giải thích đúng: Trình duyệt cần thực hiện DNS Lookup để chuyển đổi tên miền thành địa chỉ IP của máy chủ.

DNS không có nhiệm vụ phân giải cổng mà giúp trình duyệt xác định địa chỉ máy chủ cần kết nối.

Lỗi 2: Bỏ qua TCP/TLS Handshake

Nhận định sai: HTTPS tải HTML ngay lập tức mà không cần thiết lập kết nối.

Giải thích đúng: Trước khi trao đổi dữ liệu HTTPS, trình duyệt cần thiết lập kết nối TCP và thực hiện TLS Handshake để tạo kết nối mã hóa an toàn.

Sau khi hoàn tất quá trình này, trình duyệt mới gửi HTTP Request và nhận HTTP Response.

Lỗi 3: Nhầm lẫn mã HTTP 404 và 500

Nhận định sai: Khi Database bị sập, máy chủ trả về HTTP 404 và thông báo hồ sơ bệnh nhân đã bị xóa vĩnh viễn.

Giải thích đúng:

404 Not Found: Tài nguyên hoặc URL được yêu cầu không tồn tại hoặc không tìm thấy trên máy chủ.

500 Internal Server Error: Máy chủ gặp lỗi nội bộ khi xử lý yêu cầu, chẳng hạn như lỗi Backend hoặc sự cố khi truy cập Database.

Vì vậy, lỗi Database không đồng nghĩa với việc hồ sơ bệnh nhân đã bị xóa.

Lỗi 4: Hiểu sai Rendering Pipeline

Nhận định sai: Trình duyệt bỏ qua DOM Tree và hiển thị trực tiếp mã JavaScript thô.

Giải thích đúng: Trình duyệt phải phân tích HTML để tạo DOM Tree, phân tích CSS để tạo CSSOM, sau đó thực hiện Layout và Paint để hiển thị nội dung lên màn hình.

JavaScript có thể thay đổi DOM trong quá trình hoạt động của trang web, nhưng không thay thế toàn bộ quy trình Rendering Pipeline.

2. Giải thích lỗi Backend và tác động đến bệnh nhân
Tại sao lỗi Backend nên trả về HTTP 500 thay vì HTTP 404?

Khi hệ thống CarePulse gặp sự cố trong quá trình truy vấn Database, máy chủ không thể xử lý yêu cầu lấy kết quả xét nghiệm.

Trong trường hợp này:

HTTP 500 phù hợp hơn vì lỗi xuất phát từ phía máy chủ.

HTTP 404 chỉ nên được sử dụng khi tài nguyên được yêu cầu không được tìm thấy.

Không thể kết luận hồ sơ bệnh nhân đã bị xóa chỉ dựa vào lỗi truy vấn Database.

Tác động tâm lý đối với bệnh nhân

Nếu hệ thống hiển thị thông báo sai rằng hồ sơ xét nghiệm đã bị xóa vĩnh viễn, bệnh nhân có thể:

Lo lắng vì nghĩ rằng kết quả xét nghiệm của mình đã bị mất.

Hoang mang về tình trạng sức khỏe do không thể xem kết quả.

Mất niềm tin vào độ tin cậy và tính an toàn của hệ thống CarePulse.

Gửi khiếu nại hoặc yêu cầu bệnh viện kiểm tra lại hồ sơ.

Kết luận: Trong hệ thống y tế, thông báo lỗi phải chính xác, rõ ràng và không gây hiểu lầm cho bệnh nhân.

3. Viết lại báo cáo đúng theo 4 bước trong vòng đời URL
Bước 1: DNS Lookup

Khi bệnh nhân nhấn nút "Xem Kết Quả Xét Nghiệm Máu", trình duyệt cần xác định địa chỉ IP tương ứng với tên miền của hệ thống CarePulse thông qua DNS Lookup.

DNS chuyển đổi tên miền thành địa chỉ IP.

Trình duyệt sử dụng địa chỉ IP này để xác định máy chủ cần kết nối.

Bước 2: TCP/TLS Handshake

Sau khi có địa chỉ IP, trình duyệt thiết lập kết nối với máy chủ.

TCP Handshake thiết lập kết nối giữa trình duyệt và máy chủ.

TLS Handshake xác thực và thiết lập kết nối mã hóa HTTPS.

Quá trình này giúp bảo vệ dữ liệu trao đổi giữa bệnh nhân và hệ thống.

Bước 3: HTTP Request/Response

Sau khi kết nối thành công:

Trình duyệt gửi HTTP Request đến máy chủ để yêu cầu kết quả xét nghiệm.

Backend tiếp nhận yêu cầu và truy vấn Database.

Nếu xử lý thành công, máy chủ trả về HTTP 200 OK cùng dữ liệu kết quả xét nghiệm.

Nếu xảy ra lỗi nội bộ phía máy chủ, hệ thống có thể trả về HTTP 500 Internal Server Error.

Nếu không tìm thấy tài nguyên được yêu cầu, hệ thống có thể trả về HTTP 404 Not Found.

Bước 4: Rendering Pipeline

Sau khi nhận được phản hồi từ máy chủ, trình duyệt tiến hành hiển thị nội dung:

HTML Parsing: Phân tích HTML để xây dựng DOM Tree.

CSS Parsing: Phân tích CSS để xây dựng CSSOM.

Layout: Tính toán vị trí và kích thước của các thành phần trên trang.

Paint: Vẽ nội dung lên màn hình để bệnh nhân có thể xem kết quả xét nghiệm.

4. Giải thích nguyên lý bộ nhớ: Vì sao chỉnh sửa Elements không cập nhật Database?
4.1. Bản chất của thao tác chỉnh sửa Elements

Khi thực tập sinh mở Chrome DevTools và chỉnh sửa nội dung thẻ <span> trong tab Elements, trình duyệt chỉ thay đổi cấu trúc DOM đang tồn tại trong bộ nhớ RAM của máy khách (Client-side).


4.3. Vì sao nhấn F5 sẽ mất nội dung đã chỉnh sửa?

Khi thực tập sinh chỉnh sửa Elements, trình duyệt chỉ thay đổi DOM tạm thời trong bộ nhớ.

Khi nhấn F5 để tải lại trang, trình duyệt gửi yêu cầu mới đến máy chủ.

Máy chủ trả về dữ liệu hiện có trong hệ thống.

Trình duyệt xây dựng lại DOM từ dữ liệu nhận được.

Vì vậy, nội dung chỉnh sửa thủ công trước đó sẽ biến mất.