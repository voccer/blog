# Nghịch vài trò hack Wifi - Phần 3: Phising

Ở phần thứ nhất và phần thứ 2 mình đã trình bày cách hack wifi bằng phương pháp từ điển - lợi dụng sơ hở trong quá trình xác thực bắt tay 4 bước, WPS - lợi dụng lỗ hổng WPS. Hai phương pháp chủ yếu áp dụng kĩ thuật để vét cạn, mà vét cạn thì có khả năng là không thể thực hiện được. Trong blog này, mình sẽ giới thiệu 1 phương pháp lợi dụng sơ hở của người dùng để chính họ cấp mật khẩu cho mình. Lưu ý, phương pháp này được xem như 1 sự lừa đảo liên quan tới pháp lý, chỉ dành mục đích học tập, không nên áp dụng 1 cách bừa bãi.

## I. Tổng quan

Với đại đa số người dùng phổ thông, không có sự hiểu biết nhiều về công nghệ và hay mất đi sự cảnh giác. Lợi dụng sơ hở này, dựa trên nguyên lý kẻ thứ 3, chúng ta tạo 1 AP ảo với tên giống hệt AP của victim, đồng thời tấn công liên tục vào AP victim để nó bị ngừng hoạt động. Khi đó, người dùng sẽ không thể kết nối với AP thật được nữa, và sẽ xuất hiện 1 AP ảo, nếu họ nhấn vào và nhập lại mật khẩu thì ola chúng ta đã thành công.

Phương pháp này gọi là Phishing.

## II. Nguyên lý hoạt động

Sau khi xác định được victim, hacker sẽ thưc hiện các tấn công <a href='https://www.pandasecurity.com/en/mediacenter/security/what-is-an-evil-twin-attack/'> evil twin </a>. Điều hướng toàn bộ yêu cầu HTTP (lưu ý là chỉ điều hướng http) đến 1 trang khác đã chuẩn bị sẵn để lừa đảo.

Quá trình tấn công sẽ diễn ra ở 3 giai đoạn:

- Victim bị ngắt kết nối và router bị dừng hoạt động
- Xuất hiện AP(access point) khác có tên tương tự AP cũ, victim ấn vào và đươc điều hướng tới trang có sẵn
- Victim nhập mật khẩu và sập bẫy.

## III. Chuẩn bị

Nguyên lý cơ bản là thế, bây giờ chúng ta cần chuẩn bị những thứ sau.

### 1. Công cụ

Có khá nhiều tool hỗ trợ việc tạo phising có thể kể tới như: WiFiSlax, WifiPhisher ...
Ở đây mình sử dụng <a href='https://github.com/wifiphisher/wifiphisher'>WifiPhisher</a>.

### 2. Dụng cụ

Như cách tấn công bằng từ điển, chúng ta cần có 1 card wifi hỗ trợ monitor, ở đây mình vẫn chọn TP-link WN722N V1.

<img src="https://raw.githubusercontent.com/Ducvoccer/blog/main/images/hack-wifi-p3/cardwifi.jpg">

## III. Thực chiến

### 1. Kiểm tra card wifi

```
$ ifconfig
```

<img src="https://github.com/Ducvoccer/blog/blob/main/images/hack-wifi-p3/ifconfig.png?raw=true">

Chúng ta có 1 card là wlan0

### 2. Start wifiphisher

Khởi động wifiphisher với card mạng là wlan0

```
# wifiphiser -i wlan0
```

<img src="https://github.com/Ducvoccer/blog/blob/main/images/hack-wifi-p3/start_wifiphisher.png?raw=true">

### 3. Choose victim

Sau khi khởi động wifiphiser, ta sẽ có list 1 danh sách các wifi trong vùng phủ sóng. Ở đây, mình chọn wifi có tên là `Voccer` (wifi cá nhân).

<img src="https://github.com/Ducvoccer/blog/blob/main/images/hack-wifi-p3/choose_victim.png?raw=true">

Ấn enter để chọn.

### 4. Choose method

Sau khi chọn được victim, chúng ta sẽ chọn phương thức tấn công.
Cần phải nói qua về các phương thức tấn công:

- OAuth Login Page:
  Với tùy chọn này, chương trình sẽ đưa ra trang giả mạo giống trang login của Facebook
- Network Manger Connect
  Tùy chọn này sẽ đưa victim tới trang không thể truy cập của browser, sau đó yêu cầu nhập mật khẩu
- Firmware Upgrade Page
  Tùy chọn này sẽ đưa victim tới trang upgrade của firmware wifi, yêu cầu victim nhập mật khẩu để bắt đầu tiến trình

- Browser Plugin Update
  Tùy chọn này sẽ đưa victim tới trang upgrade của plugin trong browser, và tất nhiên cũng yêu cầu victim nhập mật khẩu.

<img src="https://github.com/Ducvoccer/blog/blob/main/images/hack-wifi-p3/choose_method_phishing.png?raw=true">

Ở đây, mình chọn phương án thứ 3 - Firmware Upgrade Page

### 5. Quá trình

Sau khi chọn method là Firmware Upgrade Page

<img src="https://github.com/Ducvoccer/blog/blob/main/images/hack-wifi-p3/after_select_victim.png?raw=true">

Lúc này chúng ta cùng chờ con mồi mắc bẫy.

Quay lại phía victim, ta nhận ra xuất hiện thêm 1 wifi nữa cũng tên là `Voccer` và không yêu cầu mật khẩu, và tất nhiên wifi chính bị disconnect

<img src="https://github.com/Ducvoccer/blog/blob/main/images/hack-wifi-p3/wifi_double.png?raw=true">

Sau khi victim kết nối với wifi giả mạo, và truy cập internet, sẽ xuất hiện trang Firmware Upgrade Page.

<img src="https://github.com/Ducvoccer/blog/blob/main/images/hack-wifi-p3/update_firmware_page.png?raw=true">

Victim ngây thơ nhập password vào ô và ấn upgrade =))
<img src="https://github.com/Ducvoccer/blog/blob/main/images/hack-wifi-p3/update_process.png?raw=true">

Hiển thị thời gian
<img src="https://github.com/Ducvoccer/blog/blob/main/images/hack-wifi-p3/update_time.png?raw=true">

Quay về với hacker, sau khi victim ấn gửi password, chúng ta sẽ có kết quả như sau:
<img src="https://github.com/Ducvoccer/blog/blob/main/images/hack-wifi-p3/rs.png?raw=true">

Tất nhiên, nếu victim gửi mật khẩu bậy lên thì cũng không biết =))

Và thế là hoàn thành việc phising, cũng đơn giản nhỉ.

## IV. Sơ kết

Kĩ thuật phishing là 1 kĩ thuật khá là cơ bản cũng như phổ biến trong các cuộc tấn công. Kĩ thuật này không đòi hỏi phải sử dụng quá nhiều kiến thức, dễ làm nhưng hiệu quả cho một số trường hợp nhất định.
Xin nhắc lại một lần nữa, phishing là phương pháp mang tính vi phạm pháp lý, do đó chỉ khuyến cáo sử dụng cho mục đích nguyên cứu chứ không nên lạm dụng cho các mục đích xấu.

Sau khi hiểu nguyên lý và cách thức hoạt động của phishing, chúng ta có thể đi tới một số lưu ý:

- Không ấn vào bất kì wifi lạ nào mà bạn không biết rõ nó là gì
- Không nhập mật khẩu vào bất kì trang nào lạ
- Luôn ưu tiên sử dụng các trang web https

## V. Tổng kết

Và như thế là mình đã chia sẻ cho mọi người 3 cách hack wifi phổ biến mình có biết. Mỗi cách đều có ưu và nhược điêm riêng, tùy vào tình huống cụ thể để sử dụng. Và điều quan trọng là chỉ sử dụng cho mục đích nghiên cứu học tập chứ không nên nghĩ về mục đích xấu.

<i>Người không chơi là người chiến thắng</i>
