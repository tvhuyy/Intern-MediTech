## Enable Firewall and Allow Specific IP to Remote Desktop Server

- Tài liệu này hướng dẫn cách mở firewall và thêm rule để cho phép các IP được chỉ định có thể Remote Desktop đến Server.

- Trên giao diện Windows Server, chọn Tools và lựa chọn Firewall

    ![a](https://imgur.com/0cCyBxY.png)

- Chọn `Inbound Rules` :

    ![a](https://imgur.com/tKcgeLK.png)

- Chọn `New Rule` :

    ![a](https://imgur.com/flE8WBa.png)

- Chọn `Custom` rồi chọn Next :

    ![a](https://imgur.com/Jw9lutp.png)

- Chọn `All programs` rồi chọn Next :

    ![a](https://imgur.com/IJ5Ci8h.png)

- Cấu hình như hình bên dưới và nhấn Next :

    ![a](https://imgur.com/EdIilyr.png)

- Tích chọn `These IP addresses` trong phần remote IP, sau đó chọn `Add`.
- Nhập IP hoặc dải IP sau đó nhấn OK.
- Chọn `Allow the connection` rồi nhấn Next.
- Ở trang profile, tích chọn cả 3 option  rồi nhấn Next.

    ![a](https://imgur.com/4nl3WKe.png)

- Ở bước cuối cùng, ta đặt tên cho Rule (ví dụ : Remote Desktop - IP Restriction Rule) rồi ấn Finish.


- tham khảo thêm : 

    ![a](https://imgur.com/0vIjS0y.png)