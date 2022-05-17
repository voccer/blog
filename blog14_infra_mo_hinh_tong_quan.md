Giải thích:
- Khi có 1 request từ client tới, hệ thống cân bằng tải load balancer sẽ xem xét điều hướng nó đi đâu
- Ở server sẽ có 2 chức năng CI/CD và ansible cấu hình cho hệ thống
- Logging được tích hợp và có thể lưu thông tin vào Database cũng như File.
- Database và File sẽ được backup định kì qua 1 trigger. 
- Sau khi backup sẽ được tự động lưu vào NAS hoặc Cloud.
- Hệ thống restore sẽ được kích hoạt khi cân restore cái gì đó.