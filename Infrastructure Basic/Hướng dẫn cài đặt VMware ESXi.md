## Hướng dẫn cài đặt VMware ESXi

- Tài liệu này cài đặt VMware ESXi bản 6.5

### Download ISO ESXi

- Đầu tiên ta cần đăng ký một account trên trang chủ VMware để có thể thực hiện download file cài đặt VMware ESXi
- Download tại [đây](https://customerconnect.vmware.com/downloads/#all_products)
- Sau khi đăng ký được account của VMware, ta login vào vmware để download file ISO.

### Hướng dẫn cài đặt

- Tiến hành boot device. Ta chọn `ESXi-6.5.0... Installer` :
- Bắt đầu tải bộ cài đặt :

    ![a](https://imgur.com/JSquyeX.png)

    ![a](https://imgur.com/2DvWmU4.png)

- Màn hình xuất hiện thông báo `Welcome to the VMware ESXi`, nhấn `Enter` để tiếp tục :

    ![a](https://imgur.com/PDfvNYM.png)

- Nhấn `F11` để đồng ý và tiếp tục :

    ![a](https://imgur.com/lbdvIdm.png)

- Chọn ổ cứng để tiến hành cài đặt ESXi vào ổ cứng đó. Lưu ý là data trên vùng ở cứng đó sẽ bị mất hết. Ấn `enter` :

    ![a](https://imgur.com/OO6GifU.png)
- Chọn ngôn ngữ hiển thị. Ngôn ngữ tiếng Anh sẽ là mặc định. Ấn `Enter` :

    ![a](https://imgur.com/Q1xa7V8.png)

- Cấu hình mật khẩu cho user `root`, nhập mật khẩu và ấn `Enter`

    ![a](https://imgur.com/C3P8wLi.png)

- Ổ cứng đã được tái phân vùng và cần xác nhận việc cài đặt. Ấn `F11` để tiếp tục :

    ![a](https://imgur.com/wrP2Cns.png)

- Bắt đầu quá trình cài đặt lên ổ cứng :

    ![a](https://imgur.com/WwaEjqW.png)

- Quá trình cài đặt hoàn tất. Gỡ USB hoặc CD rồi nhấn `Enter` để reboot server :

    ![a](https://imgur.com/QpGknD3.png)

- Hệ thống tiến hành reboot :

    ![a](https://imgur.com/I4kMy6W.png)

- Sau khi reboot hoàn tất, ta sẽ truy cập vào giao diện `Direct Console User Interface (DCUI)` :

    ![a](https://imgur.com/YoRDiS8.png)

- Nhấn `F2` để vào giao diện `DCUI`, hệ thống sẽ yêu cầu nhập thông tin user root và password :

    ![a](https://imgur.com/DcINeEy.png)

- Giao diện `DCUI` :

    ![a](https://imgur.com/oi6Xq8E.png)

### Cấu hình IP tĩnh cho server ESXi

- Trong menu `DCUI`, chọn phần `Configure Management Network` :

    ![a](https://imgur.com/wXbnJPr.png)

- Chọn `IPv4 Configuration` :

    ![a](https://imgur.com/dv0BeVV.png)

- Chọn `Set static IPv4...` để cấu hình IP tĩnh. Sau đó điền các thông tin địa chỉ IP, Subnetmask, Default Gateway :

    ![a](https://imgur.com/vJfLzcM.png)

- Nhấn `ESC` thoát ra và sẽ gặp thông báo xác nhận thay đổi cấu hình network, nhấn Y để lưu cấu hình :

    ![a](https://imgur.com/DRW4UF6.png)

### Truy cập ESXi Web Client

- Ta truy cập vào địa chỉ IP quản lý mà ta vừa thiết lập lúc nãy thông qua trình duyệt. Đăng nhập bằng user `root` và password đã được thiết lập trước đó :

    ![a](https://imgur.com/0kX8SMW.png)

- Giao diện quản lý của VMware ESXi :

    ![a](https://imgur.com/AiTHwiw.png)