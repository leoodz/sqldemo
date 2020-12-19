# sqldemo
demo site for sql injecttion

Hôm nay mình xin demo cho các bạn về hình thức tấn công 1 trang web bằng sql injection đơn giản.
LƯU Ý: Bài hướng dẫn này nhằm mục đích học tập, mình không chịu trách nhiệm nếu các bạn đi phá trang web
của người khác nhé

Các cuộc tấn công SQL Injection được thực hiện bằng cách gửi lệnh SQL độc hại đến các máy chủ cơ sở dữ liệu
thông qua các yêu cầu của người dùng mà website cho phép. Bất kỳ kênh input nào cũng có thể được sử dụng 
để gửi các lệnh độc hại, bao gồm các thẻ<input>, chuỗi truy vấn (query strings), cookie và tệp tin.

Để xem cách nó hoạt động, giả sử bạn có một form đăng nhập có 2 input username và password như dưới đây:

var query = "SELECT name FROM user where username = '" + username + "' and password = '" + password + "'";

Lệnh này sau đó được gửi lên máy chủ của website đó và được nó truy vấn trong cơ sở dữ liệu (ở đây mình
dùng sqllite)

Câu lệnh được gửi về server sẽ trông như thế này

SELECT * FROM users WHERE username='admin' AND password='admin123'

Nhưng nếu các chuỗi input ở phía máy chủ không được xác nhận rõ ràng thì những attacker có thể thay đổi cấu
trúc của câu lệnh gửi về server phía trên để làm nó hợp lệ mà không cần phải nhập đúng tên đăng nhập

SELECT * FROM users WHERE username='admin' OR 1=1; -- ' AND password='123456'

Câu lệnh phía trên đã được attacker gửi truy vấn lên máy chủ và sẽ trả về là đúng

Câu lệnh gửi về server có nội dung như sau
(Chọn tất cả từ user ở nơi mà username='admin' HOẶC 1=1; phần phía sau đã bị server bỏ qua vì nằm sau -- là comment)

1 thì chắc chắn luôn bằng 1 cho nên cơ sở dữ liệu cho rắng câu lệnh hợp lệ nên đã thực thi lệnh
