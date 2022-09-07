## Set Up SSH Keys on Linux OS

- SSH Keys cung cấp một phương pháp đăng nhập đơn giản, an toàn để truy cập vào Linux server.

### Bước 1 : Tạo cặp khóa RSA

- Bước đầu tiên là cần tạo một cặp khóa trên máy Client. Theo mặc định, `ssh-keygen` sẽ tạo một cặp khóa RSA 2048 bit, đủ an toàn cho hầu hết các trường hợp sử dụng (có thể sử dụng option `-b 4096` để tạo cặp khóa 4096-bit).
    ```
    ssh-keygen 
    ```
- Sau khi nhập lệnh, màn hình sẽ hiển thị như sau :

    ![a](https://imgur.com/M7PlNPG.png)

- Nhấn `enter` để lưu key vào thư mục mặc định `.ssh/`, hoặc sửa đường dẫn theo ý muốn.
- Nếu trước đó có cặp SSH key đã được tạo, ta có thể thấy lời nhắc sau :

    ![a](https://imgur.com/zjGq7dA.png)

- Nếu chọn ghi đè key thì ta sẽ không thể xác thực được bằng key trước đó nữa. Vì vậy hãy cẩn thận khi chọn `yes`, đây là lựa chọn không thể hoàn tác.
- Tiếp theo ta sẽ thấy lời nhắc như bên dưới, tại đây ta có thể nhập thêm mật khẩu an toàn (tùy chọn), mật khẩu này sẽ thêm một lớp bảo mật nên ưu tiên đặt.

    ![a](https://imgur.com/6B3COB3.png)

- Sau đó, ta sẽ nhận được kết quả hiện thị tương tự bên dưới :

    ![a](https://imgur.com/poNZuuo.png)

- Bây giờ, ta đã có một cặp khóa để xác thực, tiếp theo ta cần lấy public key vào server để có thể sử dụng SSH key để đăng nhập.

### Bước 2 : Sao chép Public Key tới Linux Server

- Cách nhanh nhất để sao chép public key tới Linux server là sử dụng công cụ `ssh-copy-id`. Nếu không có `ssh-copy-id` sẵn trên máy client thì có thể sử dụng 2 phương pháp thay thế (sao chép qua SSH dựa trên password hoặc sao chép theo cách thủ công).

#### Sao chép Public key bằng ssh-copy-id

- Công cụ này thường có sẵn trong nhiều hệ điều hành. Để sử dụng phương pháp này, ta cần phải có quyền truy cập SSH dựa trên password server.
- Để sử dụng công cụ, ta chỉ cần chỉ định remote server muốn kết nối và tài khoản người dùng có quyền truy cập SSH bằng mật khẩu. Đây là tài khoản mà khóa SSH công khai sẽ được sao chép vào:
    ```
    ssh-copy-id username@remote_host
    ```
- Ta có thể nhìn thấy lời nhắc sau :

    ![a](https://imgur.com/kTZObhL.png)

- Điều này có nghĩa là máy client không nhận ra remote server. Điều này xảy ra lần đầu tiên khi kết nối với server mới. Nhập `yes` và nhấn `Enter` để tiếp tục.
- Tiếp theo, công cụ sẽ quét client account để tìm `id_rsa.pub` key mà ta đã tạo trước đó. Khi tìm thấy key, nó sẽ nhắc ta nhập password của user remote server :

    ![a](https://imgur.com/XKul3WU.png)

- Nhập password và nhấn Enter. Tool sẽ kết nối đến account trên remote server bằng password. Sau đó nó sẽ sao chép nội dung của `~/.ssh/id_rsa.pub` vào tệp của account remote `~/.ssh/authorized_keys`. Ta sẽ nhận được kết quả :

    ![a](https://imgur.com/tfufapY.png)

#### Sao chép khóa theo cách thủ công

- Ta sẽ nối nội dung của tệp `id_rsa.pub` vào `~/.ssh/authorized_keys` trên remote server theo cách thủ công.
- Đầu tiên ta xuất nội dung của `id_rsa.pub` ra màn hình :
    ```
    cat ~/.ssh/id_rsa.pub
    ```
- Copy nội dung vừa được xuất ra.
- Đăng nhập vào remote server và nối nội dung vừa copy vào file `authorized_keys` (thay thế "public_key_string" bằng nội dung đã copy) :
    ```
    echo public_key_string >> ~/.ssh/authorized_keys
    ```

- Điều này cũng được sử dụng khi quản trị viên muốn add thêm client có thể truy cập vào remote server.

### Bước 3 : Đăng nhập vào Linux Server bằng SSH Keys

- Bây giờ ta có thể đăng nhập vào remote server mà không cần đến mật khẩu của account.
- Câu lệnh để kết nối vẫn giống như khi sử dụng xác thực mật khẩu :
    ```
    ssh username@remote_host
    ```

### Bước 4 : Tắt xác thực mật khẩu trên Remote Server

- Bây giờ ta đã có thể đăng nhập vào remote server mà không cần mật khẩu. Tuy nhiên, cơ chế xác thực dựa trên mật khẩu vẫn đang hoạt động, có nghĩa là máy chủ vẫn có thể bị tấn công brute-force.
- Sau khi đăng nhập, hãy chuyển đổi sang tài khoản `root` hoặc tài khoản có quyền sudo. Sau đó mở file config SSH :
    ```
    sudo vi /etc/ssh/sshd_config
    ```
- Bên trong file này, hãy tìm dòng `PasswordAuthentication`. Dòng này có thể có dấu `#` ở đầu, hãy xóa dấu # và sửa `yes` thành `no`. Điều này sẽ tắt xác thực bằng mật khẩu khi login SSH.
- Sau khi sửa xong, hãy lưu file lại và restart lại `sshd`
    ```
    sudo systemctl restart sshd
    ```
