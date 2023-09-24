# JSON Web Token (JWT) là gì ?

JSON web token (JWT) là một phương thức chuyển xác nhận quyền sở hữu an toàn với URL giữa hai bên. JWT mã hóa các xác nhận quyền sở hữu bằng ký hiệu đối tượng JavaScript và tùy ý cung cấp không gian cho chữ ký hoặc mã hóa toàn bộ. Tiêu chuẩn do JWT đề xuất đã bắt đầu được áp dụng rộng rãi hơn với các khung như OAuth 2.0 và các tiêu chuẩn như kết nối OpenID tận dụng JWT.

## JSON Web Tokens là gì?

JSON web token là JSON (JavaScript object notation) với một số cấu trúc bổ sung. JWT bao gồm tiêu đề và tải trọng sử dụng định dạng JSON. Tùy chọn, mã thông báo có thể được mã hóa hoặc ký bằng mã xác thực tin nhắn (MAC). Mã thông báo đã ký thường được gọi là chữ ký web JSON (JWS) và mã thông báo được mã hóa là mã hóa web JSON (JWE).

Để giao tiếp bằng lời nói, nhiều nhà phát triển đã phát âm JWT là “jot” hoặc “jawt”. Tiếp tục chủ đề đó, tôi đề xuất cách phát âm JWS là “hàm” và JWE là “jawa”.

JWT được hình thành bằng cách ghép JSON tiêu đề và JSON tải trọng bằng một dấu “.” và tùy ý nối thêm chữ ký. Toàn bộ chuỗi sau đó được mã hóa URL base64 .

## Ví dụ JSON Web Tokens

### Ví dụ về JWT

```
eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJMTSIsImlhdCI6MTYxOTQ3MDY4MiwiZXhwIjoxNjE5NDcxODg2LCJhdWQiOiJsb2dpY21vbml0b3IuY29tIiwic3ViIjoiTmVkIn0.mqWsk4fUZ5WAPYoY9bJHI7gD8Zwdtg9DUoCll-jXCMg
```

### Decoded Header

Bao gồm hai phần:
- Thuật toán ký đang được sử dụng.
- Loại mã thông báo, trong trường hợp này chủ yếu là “JWT”.

```
{
  "typ": "JWT",
  "alg": "HS256"
}
```

### Decoded Payload

- Chứa các xác nhận quyền sở hữu hoặc đối tượng JSON.
  
```
{
  "iss": "LM",
  "iat": 1619470682,
  "exp": 1619471886,
  "aud": "logicmonitor.com",
  "sub": "Ned"
}
```

### Signature

- Một chuỗi được tạo thông qua thuật toán mã hóa có thể được sử dụng để xác minh tính toàn vẹn của  JSON payload.
```
mqWsk4fUZ5WAPYoY9bJHI7gD8Zwdtg9DUoCll-jXCMg

```

## Khi nào nên sử dụng JWT


Trường hợp sử dụng phổ biến là dành cho các trường hợp cần duy trì trạng thái yêu cầu HTTP với máy khách. Điều này thường xảy ra với việc triển khai API RESTful. Về mặt ngữ nghĩa , dịch vụ web RESTful không thể giữ trạng thái máy khách trên máy chủ và vẫn tuân thủ nghiêm ngặt mô hình không trạng thái. Sử dụng JWT cho phép máy khách cung cấp thông tin trạng thái cho máy chủ cho mỗi yêu cầu. Điều này đặc biệt hữu ích trong các dịch vụ web RESTful được bảo mật yêu cầu một số hình thức xác thực ứng dụng khách và kiểm soát ủy quyền.

Không sử dụng JWT khi mã thông báo phiên sẽ hoạt động. Nói chung, mã thông báo phiên là một phương pháp được thiết lập tốt để quản lý quyền truy cập của khách hàng và chúng không gây ra bất kỳ lỗ hổng tiềm ẩn nào được đề cập dưới đây. JWT vượt trội trong môi trường yêu cầu phương thức xác thực không trạng thái, môi trường đăng nhập một lần là một ví dụ điển hình. Tuy nhiên, có các phương pháp truyền dữ liệu phiên nội bộ khác có thể cung cấp khả năng bảo mật tốt hơn với hiệu suất tương đương. 

## Quy ước yêu cầu JWT

Bạn có thể nhận thấy rằng trong ví dụ JWT (do Google phát hành) ở trên, tải trọng JSON có tên trường không rõ ràng. Họ sử dụng sub, iat, audv.v.:

- iss : Nhà phát hành mã thông báo (trong trường hợp này là Google)
- azp và aud : ID khách hàng do Google cấp cho ứng dụng của bạn. Bằng cách này, Google biết trang web nào đang cố gắng sử dụng dịch vụ đăng nhập của mình và trang web đó biết rằng JWT được cấp riêng cho họ.
- sub : ID người dùng Google của người dùng cuối
- at_hash : Hàm băm của mã thông báo truy cập. Mã thông báo truy cập OAuth khác với JWT ở chỗ nó là mã thông báo không rõ ràng. Mục đích của mã thông báo truy cập là để ứng dụng khách có thể truy vấn Google để hỏi thêm thông tin về người dùng đã đăng nhập.
- email : ID email của người dùng cuối
- email_verified : Người dùng đã xác minh email của họ hay chưa.
- iat : Thời gian (tính bằng mili giây kể từ kỷ nguyên) JWT được tạo.
- exp : Thời gian (tính bằng mili giây kể từ kỷ nguyên) JWT sẽ hết hạn.
- nonce : Có thể được ứng dụng khách sử dụng để ngăn chặn các cuộc tấn công lặp lại.
- hd : Miền G Suite được lưu trữ của người dùng
Lý do sử dụng các khóa đặc biệt này là để tuân theo quy ước ngành về tên của các trường quan trọng trong JWT. Việc tuân theo quy ước này cho phép các thư viện máy khách ở các ngôn ngữ khác nhau có thể kiểm tra tính hợp lệ của JWT do bất kỳ máy chủ xác thực nào cấp. Ví dụ: nếu thư viện máy khách cần kiểm tra xem JWT đã hết hạn hay chưa, thì nó chỉ cần tìm trường đó iat.

## JWT có an toàn không?

JSON web signature (JWS) vốn không cung cấp bảo mật. Tính bảo mật hoặc lỗ hổng bảo mật của dịch vụ xuất hiện từ quá trình triển khai. Các lỗ hổng phổ biến nhất do triển khai JWT/JWS tạo ra có thể dễ dàng tránh được nhưng không phải lúc nào cũng rõ ràng. Một số lỗ hổng phổ biến hơn phát sinh từ việc triển khai cho phép các giá trị tiêu đề JWT thúc đẩy quá trình xác thực JWT.

Nếu chức năng xử lý JWT tuân thủ một cách mù quáng loại thuật toán được khai báo trong tiêu đề và tiêu đề “alg” được đặt thành “none”, thì JWT sẽ được xử lý là hợp lệ mà không có bất kỳ biện pháp bảo vệ nào mà chữ ký cung cấp. Điều này cho phép các tác nhân tấn công đặt các giá trị xác nhận quyền sở hữu trong nội dung JWT thành bất kỳ giá trị nào phù hợp với mục đích của chúng. Việc triển khai chặt chẽ chức năng xử lý sẽ chỉ cho phép một tập hợp các giá trị tiêu đề nghiêm ngặt và chỉ xác thực JWT đáp ứng các hạn chế đó. Đây là một cách khai thác phổ biến tại thời điểm này và hầu hết các thư viện và chức năng trợ giúp JWT hiện đại sẽ không còn dễ bị tấn công bởi nó nữa.

Ngay cả khi JWT được ký, vẫn có một số cạm bẫy cần lưu ý. Có ba loại thuật toán chữ ký cơ bản được sử dụng phổ biến. Thuật toán “không”, như đã thảo luận ở trên, thuật toán đối xứng và thuật toán bất đối xứng. Nếu dịch vụ sử dụng JWT/JWS cho phép kết hợp nhiều thuật toán khác nhau thì chức năng xử lý cần phải đề phòng một cách khai thác phổ biến được sử dụng để chống lại việc triển khai JWS không đồng bộ.

Hãy xem xét một ví dụ về hai thuật toán mã hóa, mã xác thực tin nhắn dựa trên hàm băm (HMAC) và Rivest-Shamir-Adleman (RSA). HMAC sử dụng khóa bí mật chung để tạo và xác thực chữ ký. RSA, một thuật toán bất đối xứng, sử dụng khóa riêng để tạo chữ ký và khóa chung để xác thực chữ ký. Hệ thống sử dụng thuật toán RSA sẽ ký bằng khóa riêng và xác thực bằng khóa chung. Nếu JWT được ký bằng thuật toán HMAC và khóa RSA công khai làm đầu vào thì hệ thống nhận có thể bị lừa khi cố gắng sử dụng khóa RSA công khai mà nó có như một cách để xác thực chữ ký bằng thuật toán HMAC. Kiểu tấn công này hoạt động khi hệ thống nhận cho phép tiêu đề JWT điều khiển xác thực mã thông báo. Trong trường hợp này.

## Ưu và nhược điểm của JWT

Có khá nhiều lợi ích khi sử dụng JWT:

- Bảo mật : JWT được ký điện tử bằng cách sử dụng cặp khóa bí mật (HMAC) hoặc cặp khóa công khai/riêng (RSA hoặc ECDSA) để bảo vệ chúng khỏi bị khách hàng hoặc kẻ tấn công sửa đổi.
- Chỉ được lưu trữ trên máy khách : Bạn tạo JWT trên máy chủ và gửi chúng cho máy khách. Sau đó, khách hàng sẽ gửi JWT với mọi yêu cầu. Điều này tiết kiệm không gian cơ sở dữ liệu.
- Hiệu quả / Không trạng thái : Việc xác minh JWT nhanh chóng vì nó không yêu cầu tra cứu cơ sở dữ liệu. Điều này đặc biệt hữu ích trong các hệ thống phân tán lớn.

Tuy nhiên, một số nhược điểm là:

- Không thể hủy ngang : Do tính chất khép kín và quy trình xác minh không trạng thái, có thể khó thu hồi JWT trước khi nó hết hạn một cách tự nhiên. Do đó, những hành động như cấm người dùng ngay lập tức không thể thực hiện dễ dàng. Nói như vậy, có một cách để duy trì danh sách đen/từ chối JWT và thông qua đó, chúng ta có thể thu hồi chúng ngay lập tức.
- Phụ thuộc vào một khóa bí mật : Việc tạo JWT phụ thuộc vào một khóa bí mật. Nếu khóa đó bị xâm phạm, kẻ tấn công có thể tạo JWT của riêng chúng mà lớp API sẽ chấp nhận. Điều này ngụ ý rằng nếu khóa bí mật bị xâm phạm, kẻ tấn công có thể giả mạo danh tính của bất kỳ người dùng nào. Chúng ta có thể giảm thiểu rủi ro này bằng cách thay đổi khóa bí mật thường xuyên.
Tóm lại, JWT hữu ích nhất cho các ứng dụng quy mô lớn không yêu cầu các hành động như cấm người dùng ngay lập tức.
