## Live-Migration Hyper-V VMs on Windows Server Cluster

- **Mục đích tài liệu** : Hướng dẫn cách cấu hình hệ thống Windows server cluster, giúp tăng tính dự phòng cho hệ thống dịch vụ chạy trong các `Hyper-V VM`. Khi một Host Hyper-V đang chạy bị gặp sự cố, các VMs trên host đó sẽ được chuyển sang chạy trên các Host Hyper-V khác trong cùng Cluster.
- **Sơ đồ tổng quan** :

    ![a](https://imgur.com/w6HDgc5.png)

- **Chuẩn bị môi trường**:
    - 3 máy chủ Windows Server (2012 trở lên) được cài đặt sẵn:
        - 2 card mạng int và ext, cấu hình IP theo mô hình.
        - join vào cùng một miền AD.
        - Cài đặt Roles Hyper-V.
        - Cài đặt Features Failover Clustering.
    - 1 máy chủ iSCSI-Server có sẵn 2 phân vùng cho cluster windows.

### Cấu hình Storage cho các Hyper-V Host

- Tham khảo các tài liệu dựng iSCSI server và tạo Virtual disk, iSCSI target,... để tạo 2 disk Witness và Storage cho các Hyper-V Host.
- Kết quả sau khi các Host kết nối thành công đến storage của iSCSI server:

    ![a](https://imgur.com/d4vswa9.png)

### Cấu hình Cluster

- Cấu hình này có thể được thực hiện trên 1 host bất kì trong 3 host.
- Trên giao diện `Server Manager` > chọn `Tools` > Chọn `Failover Cluster Manager`

    ![a](https://imgur.com/u2U6uwM.png)

- Tại `Action` chọn `Create Cluster`

    ![a](https://imgur.com/ZbkpqOf.png)

- Ấn `Next`, ở bước chọn Server, nhấn `Browse` rồi nhập tên server > nhấn `OK`

    ![a](https://imgur.com/biUfk56.png)

- Ấn `Next` chuyển sang phần `Validation`, chọn `Yes` để kiểm tra các thiết bị có tương thích không > `Next`

    ![a](https://imgur.com/wI56Lzq.png)

- Tại bước `Testing Options` > để mặc định `Run all tests` > `Next`

    ![a](https://imgur.com/OGCJpLX.png)

- Xác thực lại thông tin > `Next`
- Nhấn `Finish`
- Nhập `Cluster Name` và `Virtual IP` > `Next`
    
    ![a](https://imgur.com/mhc346A.png)

- Nhấn `Next` để confirm thông tin

    ![a](https://imgur.com/MMVn92K.png)

- Kiểm tra trạng thái các thành phần của cluster

    ![a](https://imgur.com/qsGt86F.png)

    ![a](https://imgur.com/rvI9SVQ.png)

    ![a](https://imgur.com/mMPTJkK.png)

### Cấu hình Hyper-V Manager và tạo V-Switch

- Trên giao diện `Server Manager` > `Tools` > `Hyper-V Manager`
- Chuột phải vào `Hyper-V Manager` > chọn `Connect to Server`

    ![a](https://imgur.com/96qOA1H.png)

- Chọn `Browse...` rồi thêm các host chưa hiển thị vào (thêm lần lượt từng Host)

    ![a](https://imgur.com/gzZInDl.png)

- Sau khi đã hiển thị cả 3 Host, chuột phải vào Host và chọn Virtual Switch Manager để tạo thêm V-Switch

    ![a](https://imgur.com/L0TGlta.png)

- Chọn `New virtual network switch` > `External` > `Create Virtual Switch` (Các loại switch Ext,Int,Private tìm hiểu thêm về Hyper-V)

    ![a](https://imgur.com/GP4abS7.png)

- Nhập tên Switch, chọn card > `OK`

    ![a](https://imgur.com/4oG3XBz.png)

- Làm tương tự với 2 Host còn lại

### Tạo máy ảo trên Cluster

- Tại `Failover Cluster Manager` -> chuột phải vào `Role`-> `New Virtual Machines`

    ![a](https://imgur.com/ObHLvJv.png)

- Chọn 1 trong 3 server (lưu ý, nên chọn server đang là Owner của Disk)

    ![a](https://imgur.com/ah3eLZ3.png)

- Đặt tên và trỏ đường dẫn tới ổ đĩa chung mà ta connect từ iSCSI server

    ![a](https://imgur.com/PKWWfY9.png)

- Điền dung lượng RAM cho máy ảo

    ![a](https://imgur.com/6f1XP5T.png)

- Chọn card mạng cho máy ảo

    ![a](https://imgur.com/i6CTixZ.png)

- Điền dung lượng disk cho máy ảo

    ![a](https://imgur.com/TzbhFXC.png)

- Trỏ đường dẫn đến file ISO > Next

    ![a](https://imgur.com/oGKpflK.png)

- Nhấn `Finish` sau đó start máy ảo lên và cài đặt hệ điều hành như bình thường.

### Kiểm tra cơ chế dự phòng máy ảo trong Cluster

- Để theo dõi chi tiết hơn thì ta nên cấu hình IP cho máy ảo và thực hiện ping liên tục từ một máy bên ngoài đến.

#### Cơ chế dự phòng thủ công

- Tình huống máy ảo đang chạy trên Host `Hyper-V-Host2`, ta thực hiện move sang `Hyper-V-Host3` và theo dõi trạng thái máy ảo, ping.

    ![a](https://imgur.com/gWDidO4.png)

    ![a](https://imgur.com/Jk3k2Sw.png)

- Quá trình move thành công và ping chỉ mất 1 gói

    ![a](https://imgur.com/EdpuHoo.png)

#### Cơ chế dự phòng tự động

- Ta tiến hành shutdown máy chủ `Hyper-V-Host3` (giả định trường hợp mất điện hoặc sự cố đột ngột)

    ![a](https://imgur.com/ZYEWZSK.png)

- Máy ảo lập tức được khởi tạo vào chạy trên Host khác, quá trình này chỉ mất vài giây.

    ![a](https://imgur.com/pmXdCPN.png)