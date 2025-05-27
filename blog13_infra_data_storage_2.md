## Data Storage - Phần 2
Ở phần 1, mình đã đi qua sơ lược về các khái niệm cơ bản của Data Storage, và cũng nói qua về một số kĩ thuât backup. Tiếp nối series này mình tiếp tục đi sâu hơn về phần backup và giới thiệu thêm 1 số khái niệm mới.

## Backup
### Full Backup
đây là kiểu backup rất phổ biến, hiêu đơn giản là cứ backup toàn bộ.
- Ưu điểm: Dễ dàng sao lưu và phục hồi
- Nhược điểm: backup rất tốn thời gian cũng như dung lượng, trên thực tế không nó backup nhiều thứ không cần thiết. Cách này ít được thực hiện nếu db quá lớn.

### Incremental backup
Nó sẽ backup lại những thứ gì thay đổi so với lần incremental backup gần nhất.
- Uu điểm: thời gian sao lưu nhanh, dung lượng cần ít
- Nhược điểm: khôi phục chậm

###  Differential backup
Backup lại những gì thay đổi so với lần full backup gần nhất.
- Ưu điểm: Thời gian backup, và dung lượng tiết kiệm hơn full backup, thời gian khôi phục thì nhanh hơn incremental backup
- Nhược điểm: Mỗi cái 1 thứ, 2 mang3

## NAS
NAS là từ viết tắt của Network Attached Storage, dịch tạm tiếng Việt là thiết bị lưu trữ gắn vào mạng.
NAS thường được sử dụng để lưu trữ, chia sẻ file và đặc biệt là streaming các dữ liệu đa phương tiện trong thời gian gần đây. Với các hệ thống NAS thì bạn có đi ra khỏi nhà, văn phòng vẫn truy cập được dữ liệu ở nhà một cách dễ dàng.
Các hệ thống NAS hiện đại cũng có thể hình dung như 1 máy chủ thu nhỏ bởi nó cũng có CPU, cũng có RAM và chạy những phiên bản hệ điều hành nhúng thu gọn (thường là Linux) cũng như có khả năng kết nối mạng qua cổng Ethernet hay thậm chí là kết nối không dây như Wi-Fi. Để lưu trữ dữ liệu thì NAS thường dùng ỗ gắn trong tuy nhiên một số thiết bị còn hỗ trợ kết nối với thiết bị gắn ngoài hay thậm chí là USB nhớ.
Một số NAS nâng cao khác lại hỗ trợ những tính năng như thiết lập máy chủ web, quản trị từ xa hay các thiết lập ổ cứng theo chế độ RAID.