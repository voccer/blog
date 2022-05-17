# Data storage
## Tổng quan
Khi phát triển 1 hệ thống, data storage là 1 khái niệm rất quan trọng. Data storage là nơi để lưu dữ liệu cho hệ thống như thông tin người dùng, thông tin sản phẩm, ảnh, log ....
Trong data storage có 3 khái niệm lớn: SQL, NoSQL và FILE.

## SQL là gì?
SQL là viết tắt của Structured Query Language - ngôn ngữ truy vấn dữ liệu. SQL là một ngôn ngữ, là tập hợp các lệnh để tương tác với cơ sở dữ liệu. Dùng để lưu trữ, thao tác và truy xuất dữ liệu được lưu trữ trong một cơ sở dữ liệu quan hệ. Trong thực tế, SQL là ngôn ngữ chuẩn được sử dụng hầu hết cho hệ cơ sở dữ liệu quan hệ. Tất cả các hệ thống quản lý cơ sở dữ liệu quan hệ (RDMS) như MySQL, MS Access, Oracle, Postgres và SQL Server… đều sử dụng SQL làm ngôn ngữ cơ sở dữ liệu chuẩn.
Tại sao nên sử dụng SQL:
- Cho phép người dùng truy cập dữ liệu trong các hệ thống quản lý cơ sở dữ liệu quan hệ.
- Cho phép người dùng mô tả dữ liệu.
- Cho phép người dùng xác định dữ liệu trong cơ sở dữ liệu và thao tác dữ liệu đó.
- Cho phép nhúng trong các ngôn ngữ khác sử dụng mô-đun SQL, thư viện và trình biên dịch trước.
- Cho phép người dùng tạo và thả các cơ sở dữ liệu và bảng.
- Cho phép người dùng tạo chế độ view, thủ tục lưu trữ, chức năng trong cơ sở dữ liệu.
- Cho phép người dùng thiết lập quyền trên các bảng, thủ tục và view.
## NoSQL là gì?
Thuật ngữ NoSQL được giới thiệu lần đầu vào năm 1998 sử dụng làm tên gọi chung cho các lightweight open source relational database (cơ sở dữ liệu quan hệ nguồn mở nhỏ) nhưng không sử dụng SQL cho truy vấn. Vào năm 2009, Eric Evans, nhân viên của Rackspace giới thiệu lại thuật ngữ NoSQL trong một hội thảo về cơ sở dữ liệu nguồn mở phân tán. Thuật ngữ NoSQL đánh dấu bước phát triển của thế hệ database mới: distributed (phân tán) + non-relational (không ràng buộc). Đây là 2 đặc tính quan trọng nhất.
NoSQL ra đời để giải quyết những vấn đề của SQL gặp phải. NoSQL phù hợp cho yêu cầu cần những database có khả năng lưu trữ dữ liệu với lượng cực lớn, truy vấn dữ liệu với tốc độ cao mà không đòi hỏi quá nhiều về năng lực phần cứng cũng như tài nguyên hệ thống và tăng khả năng chịu lỗi.
Tại sao nên sử dụng NoSQL:
- High Scalability: Gần như không có một giới hạn cho dữ liệu và người dùng trên hệ thống.
- High Availability: Do chấp nhận sự trùng lặp trong lưu trữ nên nếu một node (commodity machine) nào đó bị chết cũng không ảnh hưởng tới toàn bộ hệ thống.
- Atomicity: Độc lập data state trong các operation.
- Consistency: chấp nhận tính nhất quán yếu, có thể không thấy ngay được sự thay đổi mặc dù đã cập nhật dữ liệu.
- Durability: dữ liệu có thể tồn tại trong bộ nhớ máy tính nhưng đồng thời cũng được lưu trữ lại đĩa cứng.
- Deployment Flexibility: việc bổ sung thêm/loại bỏ các node, hệ thống sẽ tự động nhận biết để lưu trữ mà không cần phải can thiệp bằng tay. Hệ thống cũng không đòi hỏi cấu hình phần cứng mạnh, đồng nhất.
- Modeling flexibility: Key-Value pairs, Hierarchical data (dữ liệu cấu trúc), Graphs.
- Query Flexibility: Multi-Gets, Range queries (load một tập giá trị dựa vào một dãy các khóa).
Có 4 loại NoSQL chủ yếu: 
- Key-value stores
- Column-oriented databases (column-family)
- Graph databases
- Document Oriented databases

## So sánh SQL và NoSQL, khi nào dùng cái nào?
Cả SQL và NoSQL đều là nơi để lưu trữ dữ liệu, nhưng mỗi cái có 1 nét riêng phù hợp cho những trường hợp cụ thể riêng.
Với SQL phù hợp cho dữ liệu có cấu trúc, dữ liệu liên kết chặt chẽ, dễ dàng truy vấn nhưng lại khó mở rộng, chịu tải kém và hơi cứng nhắc.
NoSQL thì lại đi theo hướng ngược lại, dữ liệu phi cấu trúc, dễ dàng mở rộng, hiệu năng cao nhưng lại do quá lỏng lẻo nên khó kiểm soát và truy vấn cũng lâu.
Cứ nghĩ SQL và NoSQL là 2 cô gái, 1 cô thì ở trong gia đình gia giáo nghiêm khắc, khó tiếp cận với cổ  => phù hợp lấy làm vợ. Cô còn lại thì lại đủng đinh, nó cái gì cũng dạ dạ vâng vâng, dễ dàng => phù hợp để làm bồ :).

Trong 1 hệ thống lớn, người ta ưu tiên sử dụng cả 2 kiểu lưu trữ dữ liệu nhằm bổ trợ cho nhau.

## File là gì?
Trong hệ thống, không phải cái gì chúng ta cũng có lưu vào Database được. Thường những thông tin như log, ảnh hoặc các thông tin nặng được lưu vào File.
Một số File System phổ biến: FAT, NTFS, EXT, exFAT ....

## Sao lưu và khôi phục
### SQL
Sao lưu:
Các hệ quản trị cơ sở dữ liệu đều hỗ trợ chức năng backup. Ở đây, mình sử dụng ví dụ của My SQL.
```shell
sudo mysqldump -u [user] -p [database_name] > [filename].sql
```
Sau khi có file .sql chúng ta  có thể xem cái này như file và backup file - sẽ nói ở sau
Phục hồi:
Nếu cần khôi phục ta sử dụng lệnh: 
```shell
mysql -u [user] -p [database_name] < [filename].sql
```
### NoSQL
Tương tự SQL, NoSQL cũng hỗ trợ việc backup. Ở đây, mình sử dụng ví dụ trên mongoDB.
```shell
mongodump -d <database_name> -o <folder_will_contain_backup_file>
```

để khôi phục ta sử dụng lệnh:
```shell
mongorestore --drop -d <database_will_be_restored> <location_to_folder_containing_mongodb_database>
```
### File
File bao gồm cả các thành phần backup của SQL và NoSQL.
Để backup file chúng ta có nhiều cách, chúng ta có thể chiến lược lưu trữ trên cloud. 
Tạo 1 script chạy hàng ngày để backup file lên google drive.
Ở đây mình sử dụng rclone để backup.
```shell
#! /bin/bash
rclone copy -L --update --transfers 30 --checkers 8 --contimeout 60s --timeout 300s --retries 3 --low-level-retries 10 --stats 1s --stats-file-name-length 0 [folder-local] [folder-cloud]
```

Tạo 1 crontab để chạy script hàng ngày.
