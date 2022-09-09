## Config NIC Teaming on Windows Server 2019

- Tài liệu dưới đây sẽ hướng dẫn cách gộp nhiều card mạng vào một NIC Teaming interface trên Windows Server 2019. Theo mặc định NIC Teaming sẽ disable.
- Để enable, mở `Server Manager`, chọn `Local Server` và chọn `NIC Teaming: Disabled` trong tab `Properties`.

    ![a](https://imgur.com/DML7Svb.png)

- Tiếp theo, chọn `Tasks` -> `New Team` :

    ![a](https://imgur.com/vfuDw8B.png)

- Nhập tên Team và chọn các card mạng muốn add vào group, ta có thể chọn các tùy chọn cho Team :

    ![a](https://imgur.com/fNfKQX1.png)

- Các tùy chọn này sẽ thiết lập rules và perfomance cho NIC Teaming, hãy xem xét các tùy chọn này :
    - **Teaming Mode** : Tùy chọn này sẽ thiết lập cách mà team tương tác với mạng chuyển mạch.
        - `Static Teaming (IEEE 802.3ad)` : kiểu cấu hình này yêu cầu cấu hình trên cả switch và host để xác định liên kết nào sẽ tham gia vào card mạng gộp. Vì cấu hình bằng tay, nên không cần thêm giao thức để hỗ trợ switch và host trong việc xác định đường truyền và xác định các lỗi trong quá trình thiết lập. Kiểu này được hỗ trợ bởi các server-class switch. Kiểu này được sử dụng nhiều trong việc chia tải giữa các card mạng trong hệ thống.
        - `Switch Independent` : các card mạng được kết nối vào các switch khác nhau, cung cấp các đường truyền dự phòng cho hệ thống mạng.
        - `LACP`: giao thức để tạo EthernetChanel.
    - **Load balancing mode** :
        - Address Hash : dùng cho máy thật
        - Hyper-V Port : dùng cho máy ảo
    - Standby Adapter : đây là chức năng Fault Tolerance.

- Lựa chọn các thiết lập và ấn OK, một NIC Team mới sẽ được tạo.
- Mở danh sách network connections. Kiểm tra xem card NIC Team đã được tạo chưa.

    ![a](https://imgur.com/VAn06nK.png)

- Cấu hình thêm thông tin của card mạng :

    ![a](https://imgur.com/NKZPXcU.png)

- Card mạng mà ta đã thêm vào team sẽ không còn địa chỉ riêng nữa :

    ![a](https://imgur.com/HcPspos.png)

- Nếu NIC Teaming bị xóa, cài đặt card mạng trước đó sẽ được khôi phục.