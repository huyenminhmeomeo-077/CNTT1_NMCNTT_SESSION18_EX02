**4\. Yêu cầu đầu ra**

***Phần 1\. Phân tích***

* So sánh sự đánh đổi giữa dung lượng Cache (**MB/GB**) và Cookies (**\~4KB**) trong việc lưu trữ dữ liệu người dùng.

| Tiêu chí | Cookies | Browser Cache / Local Storage |
| :---- | :---- | :---- |
| Dung lượng tối đa | Rất nhỏ  | Lớn hơn nhiều : Vài MB đến vài GB  |
| Mục đích chính | Lưu trữ thông tin định danh để xác thực với máy chủ. | Lưu trữ tài nguyên tĩnh hoặc dữ liệu ứng dụng lớn. |
| Luồng truyền dữ liệu | Tự động gửi kèm theo mọi HTTP request lên máy chủ. | Chỉ nằm ở phía Client, không tự động gửi lên server trừ khi code JavaScript chủ động gọi. |
| Sự đánh đổi | Giới hạn dung lượng nghiêm ngặt | Dung lượng thoải mái nhưng không phù hợp để chứa thông tin bảo mật nhạy cảm vì dễ bị truy cập qua JavaScript  |

- 

***Phần 2\. Triển khai***

* Mô tả cấu trúc mẩu tin Cookie cần thiết (bao gồm Token, Max-Age, và các cờ bảo mật).  
- auth\_token=...: Chuỗi mã hóa định danh phiên làm việc của shipper  
- Max-Age=604800: Định nghĩa thời gian sống của Cookie bằng giây. Giúp cookie tồn tại ngay cả khi shipper đóng trình duyệt hay tắt máy  
- Path=/: Cho phép cookie có hiệu lực trên toàn bộ các đường dẫn của tên miền GreenLogistics.  
- HttpOnly: Ngăn chặn hoàn toàn mã JavaScript phía Client đọc hoặc ghi giá trị của cookie này, bảo vệ tài khoản shipper khỏi bị đánh cắp  
- Secure: Đảm bảo cookie chỉ được truyền tải qua các kết nối được mã hóa an toàn, chống nghe lén dữ liệu trên đường truyền mạng  
- SameSite=Strict: Ngăn chặn tấn công CSRF bằng cách giới hạn cookie chỉ được gửi trong các yêu cầu xuất phát từ chính nội bộ trang web GreenLogistics

***Phần 3\. Kiểm thử***

* Nếu shipper thay đổi giao diện nhưng trình duyệt vẫn hiện màu cũ, hãy hướng dẫn thao tác Hard Reload để xóa Cache đệm.

Khi shipper thay đổi giao diện hoặc cập nhật mã nguồn giao diện mới nhưng trình duyệt vẫn hiển thị màu sắc cũ. Để trình duyệt tải lại toàn bộ tệp mới nhất từ máy chủ, shipper hoặc kỹ thuật viên kiểm thử cần thực hiện thao tác Hard Reload 

Trên macOS : Cmd \+ Shift \+ R.

