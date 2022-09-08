## Allow and Deny host or IP to login SSH

- Dưới đây là hướng dẫn cấu hình hạn chế SSH login tới server thông qua IP hoặc hostname.
- Để thực hiện điều này, ta sử dụng `TCP Wrappers` vì nó cung cấp khả năng lọc basic traffic của network traffic. TCP Wrappers về cơ bản sử dụng 2 file : `/etc/hosts.allow` và `/etc/hosts.deny`
- Nếu các file chưa tồn tại, ta có thể tạo nó : `sudo touch /etc/hosts.(allow,deny)`

### Deny all hosts

- Phương pháp tốt nhất là từ chối tất các cả các kết nối SSH đến.
- Mở file `/etc/hosts.deny` và chỉnh sửa :
    ```
    vi /etc/hosts.deny
    ```
- Thêm dòng dưới đây để chặn toàn bộ kết nối SSH đến server :
    ```
    sshd: ALL
    ```
- Lưu và đóng file.
- Kiểm tra :

    ![a](https://imgur.com/e3Ri5Kw.png)

### Allow IP address

- Tiếp theo ta sẽ cấu hình để địa chỉ IP cụ thể có thể đăng nhập SSH.
- Mở file `/etc/hosts.allow` và chỉnh sửa :
    ```
    vi /etc/hosts.allow
    ```
- Thêm dòng sau để cho phép IP được chỉ định kết nối SSH đến server (ví dụ này sử dụng IP 172.24.115.221) :
    ```
    sshd: 172.24.115.221
    ```
- Lưu và đóng file.

- File TCP Wrapper chấp nhận các argument được phân tách bằng dấu phẩy, nên ta có thể thêm các IP vào cùng một dòng này.
    ```
    sshd: 172.24.115.221,10.88.88.1,192.168.0.10
    ```
- Nó cũng cho phép cả một dải mạng (ví dụ này sử dụng `192.168.22.0/24`) :
    ```
    sshd: 192.168.22.
    ```
- Ta cũng có thể nhập như sau để trông clean hơn :
    ```
    sshd: localhost
    sshd: 192.168.22.
    sshd: 172.24.115.221
    ```
- Ta có thể cho phép hoạc từ chối IP, dải mạng hoặc hostname. Liệt kê danh sách từ rộng nhất đến cụ thể nhất. Quy trình là OS sẽ quét từ file hosts.allow bắt đầu từ trên xuống dưới, sau đó quét đến file hosts.deny.