# Nghịch vài trò hack Wifi - Phần 3: Phising

Ở phần thứ nhất và phần thứ 2 mình đã trình bày cách hack wifi bằng phương pháp từ điển - lợi dụng sơ hở trong quá trình xác thực bắt tay 4 bước, WPS - lợi dụng lỗ hổng WPS. Hai phương pháp chủ yếu áp dụng kĩ thuật để vét cạn, mà vét cạn thì có khả năng là không thể thực hiện được. Trong blog này, mình sẽ giới thiệu 1 phương pháp lợi dụng sơ hở của người dùng để chính họ cấp mật khẩu cho mình. Lưu ý, phương pháp này được xem như 1 sự lừa đảo liên quan tới pháp lý, chỉ dành mục đích học tập, không nên áp dụng 1 cách bừa bãi.

## Tổng quan

Với đại đa số người dùng phổ thông, không có sự hiểu biết nhiều về công nghệ và hay mất đi sự cảnh giác. Lợi dụng sơ hở này, dựa trên nguyên lý kẻ thứ 3, chúng ta tạo 1 AP ảo với tên giống hệt AP của victim, đồng thời tấn công liên tục vào AP victim để nó bị ngừng hoạt động. Khi đó, người dùng sẽ không thể kết nối với AP thật được nữa, và sẽ xuất hiện 1 AP ảo, nếu họ nhấn vào và nhập lại mật khẩu thì ola chúng ta đã thành công.

Phương pháp này gọi là Phishing.

## Nguyên lý hoạt động

Sau khi xác định được victim, hacker sẽ thưc hiện các tấn công <a href='https://www.pandasecurity.com/en/mediacenter/security/what-is-an-evil-twin-attack/'> evil twin </a>. Điều hướng toàn bộ yêu cầu HTTP (lưu ý là chỉ điều hướng http) đến 1 trang khác đinh chuẩn bị sẵn để lừa đảo.

Quá trình tấn công sẽ diễn ra ở 3 giai đoạn:

- Victim bị ngắt kết nối và router bị dừng hoạt động
- Xuất hiện AP(access point) khác có tên tương tự AP cũ, victim ấn vào và đươc điều hướng tới trang có sẵn
- Victim nhập mật khẩu và sập bẫy.

## Thực chiến

Nguyên lý cơ bản là thế, bây giờ chúng ta cùng thử xem nhé. Lưu ý chỉ áp dụng với modem của bản thân.

### Công cụ

Có khá nhiều tool hỗ trợ việc tạo phising có thể kể tới như: WiFiSlax, WiFiPhisher ...
Ở đây mình sử dụng <a href='https://github.com/wifiphisher/wifiphisher'>trang chủ</a>.

### Dụng cụ

Như cách tấn công bằng từ điển, chúng ta cần có 1 card wifi hỗ trợ monitor, ở đây mình vẫn chọn TP-link WN722N V1.

<img src="https://raw.githubusercontent.com/Ducvoccer/blog/main/images/hack-wifi-p3/cardwifi.jpg">

### Kiểm tra card wifi

```
$ ifconfig
```

<img src="https://github.com/Ducvoccer/blog/blob/main/images/hack-wifi-p3/ifconfig.png?raw=true">

Chúng ta có 1 card là wlan0

### Start wifiphisher

Khởi động wifiphisher với card mạng là wlan0

```
# wifiphiser -i wlan0
```

<img src="https://github.com/Ducvoccer/blog/blob/main/images/hack-wifi-p3/start_wifiphisher.png?raw=true">

### Choose victim

Sau khi khởi động wifiphiser, ta sẽ có list 1 danh sách các wifi trong vùng phủ sóng. Ở đây, mình chọn wifi có tên là `Voccer` (wifi cá nhân)
<img src="https://github.com/Ducvoccer/blog/blob/main/images/hack-wifi-p3/choose_victim.png?raw=true">

### Choose method

<img src="https://github.com/Ducvoccer/blog/blob/main/images/hack-wifi-p3/choose_method_phishing.png?raw=true">

after choose victim
<img src="https://github.com/Ducvoccer/blog/blob/main/images/hack-wifi-p3/after_select_victim.png?raw=true">

wifi fake
<img src="https://github.com/Ducvoccer/blog/blob/main/images/hack-wifi-p3/wifi_double.png?raw=true">

update firmware page
after choose victim
<img src="https://github.com/Ducvoccer/blog/blob/main/images/hack-wifi-p3/update_firmwarge_page.png?raw=true">

update process
<img src="https://github.com/Ducvoccer/blog/blob/main/images/hack-wifi-p3/update_process.png?raw=true">

update time
<img src="https://github.com/Ducvoccer/blog/blob/main/images/hack-wifi-p3/update_time.png?raw=true">

mật khẩu gửi lên server
<img src="https://github.com/Ducvoccer/blog/blob/main/images/hack-wifi-p3/rs.png?raw=true">

## Sơ kết

Kĩ thuật phishing là 1 kĩ thuật khá là cơ bản cũng như phổ biến trong các cuộc tấn công. Kĩ thuật này không đòi hỏi phải sử dụng quá nhiều kiến thức, dễ làm nhưng hiệu quả cho một số trường hợp nhất định.
Xin nhắc lại một lần nữa, phishing là phương pháp mang tính vi phạm pháp lý, do đó chỉ khuyến cáo sử dụng cho mục đích nguyên cứu chứ không nên lạm dụng cho các mục đích xấu.

Sau khi hiểu nguyên lý và cách thức hoạt động của phishing, chúng ta có thể đi tới một số lưu ý:

- Không ấn vào bất kì wifi lạ nào mà bạn không biết rõ nó là gì
- Không nhập mật khẩu vào bất kì trang nào lạ
- Luôn ưu tiên sử dụng các trang web https

## Tổng kết

Và như thế là mình đã chia sẻ cho mọi người 3 cách hack wifi phổ biến mình có biết. Mỗi cách đều có ưu và nhược điêm riêng, tùy vào tình huống cụ thể để sử dụng. Và điều quan trọng là chỉ sử dụng cho mục đích nghiên cứu học tập chứ không nên nghĩ về mục đích xấu.

_Người không chơi là người chiến thắng_
