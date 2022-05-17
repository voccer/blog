# Nghịch vài trò hack Wifi - Phần 3: Phising

Ở phần thứ nhất và phần thứ 2 mình đã trình bày cách hack wifi bằng phương pháp từ điển - lợi dụng sơ hở trong quá trình xác thực bắt tay 4 bước, WPS - lợi dụng lỗ hổng WPS. Hai phương pháp chủ yếu áp dụng kĩ thuật để vét cạn, mà vét cạn thì có khả năng là không thể thực hiện được. Trong blog này, mình sẽ giới thiệu 1 phương pháp lợi dụng sơ hở của người dùng để chính họ cấp mật khẩu cho mình. Lưu ý, phương pháp này được xem như 1 sự lừa đảo liên quan tới pháp lý, chỉ dành mục đích học tập, không nên áp dụng 1 cách bừa bãi.

## Tổng quan

Với đại đa số người dùng phổ thông, không có sự hiểu biết nhiều về công nghệ và hay mất đi sự cảnh giác. Lợi dụng sơ hở này, dựa trên nguyên lý kẻ thứ 3, chúng ta tạo 1 AP ảo với tên giống hệt AP của victim, đồng thời tấn công liên tục vào AP victim để nó bị ngừng hoạt động. Khi đó, người dùng sẽ không thể kết nối với AP thật được nữa, và sẽ xuất hiện 1 AP ảo, nếu họ nhấn vào và nhập lại mật khẩu thì ola chúng ta đã thành công.

Phương pháp này gọi là Phishing

## Công cụ và thực hành

Nguyên lý cơ bản là thế, bây giờ chúng ta cùng thử xem nhé. Lưu ý chỉ áp dụng với modem của bản thân.
Có khá nhiều tool hỗ trợ việc tạo phising có thể kể tới như: WiFiSlax, WiFiPhisher ...
Ở đây mình sử dụng WiFiPhiser.
Công cụ:

- Air-crack: công cụ chuyển chế độ monitor
- WifiPhiser:
- mdk3: công cụ tấn công gói tin tới AP

Như cách tấn công bằng từ điển, chúng ta cần có 1 card wifi hỗ trợ monitor, ở đây mình vẫn chọn TP-link WN722N V1.
