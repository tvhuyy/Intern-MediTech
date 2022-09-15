## Bonding interface network on Linux

- Bonding interface là một cơ chế cho phép cấu hình từ 2 đến nhiều interface vật lý kết hợp thành 1 interface ảo bằng cách sử dụng `module kernel bonding` trên Linux. Điều này mang lại các lợi ích như : tăng badwidth, HA, Load Balancing.
- Các mode bonding network :
    - **mode=0 (balance-rr)** : Mode này dựa trên Round-robin policy và nó là mode default. Mode này cung cấp các tính năng chịu lỗi và load balacing. Nó truyền các gói dữ liệu theo kiểu round-robin từ slave khả dụng đầu tiên đến slave cuối cùng.
    - **mode=1 (active-backup)** : Mode này dựa trên Active-backup policy. Chỉ có một slave được hoạt động và một slave khác sẽ active khi slave đầu bị lỗi. MAC address của bond chỉ có sẵn trên network adapter để tránh gây nhầm lẫn cho switch. Mode này cung cấp khả năng dự phòng.
    - **mode=2 (balance-xor)** : Mode này chuyển phát các gói tin dựa trên phép toán `XOR` (Source MAC)(Destination MAC). Thuật toán sẽ tự lựa chọn cổng NIC member cho mỗi gói tin dựa trên địa chỉ des MAC. Mode này sẽ cung cấp khả năng load balancing và chịu lỗi.
    - **mode=3 (broadcast)** : Mode này dựa trên broadcast policy, nó sẽ truyền mọi thứ trên tất cả các slave phụ. Nó cung cấp khả năng chịu lỗi.
    - **mode=4 (802.3ad)** : Mode này được gọi là `Dynamic Link Aggregation`, nó tạo ta các aggregation groups có cùng tốc độ. Mode này yêu cầu switch có hỗ trợ IEEE 802.3ad. Việc lựa chọn slave để lưu lượng gửi đi được thực hiện dựa trên phương thức hashing. Điều này có thể được thay đổi từ phương thức XOR thông qua tùy chọn `xmit_hash_policy`.
    - **mode=5 (balance-tlb)** : Mode này được gọi là `Adaptive transmit load balancing`. Lưu lượng gửi đi được phân phối dựa trên tải hiện tại trên mỗi slave và lưu lượng đến được nhận bởi slave hiện tại. Nếu traffic failed, slave nhận không thành công sẽ được thay thế bằng MAC address của slave khác. Mode này không yêu cầu bất kỳ switch đặc biệt nào.
    - **mode=6 (balance-alb)** : Mode này dựa trên `Active Load Balancing` policy để chịu lỗi và cân bằng tải. Bao gồm load balancing truyền và nhận cho traffic IPv4. Load balancing nhận vào được thực hiện thông qua ARP negotiation.

### Bonding on Centos 7

#### 1. Cài đặt module bonding

- Đầu tiên ta cần kiểm tra xem trên hệ thống Linux đã load module bonding cho tính năng bonding interface hay chưa, mặc định là chưa được load.
    ```
    lsmod | grep bonding
    ```
- Nếu chưa thì ta cần kích hoạt :
    ```
    modprobe --first-time bonding
    modinfo bonding
    ```
- Tạo cấu hính để load module `bonding` khi hệ thống khởi động lại. Ta cần tạo 1 file `bonding.conf` nằm trong thư mục `/etc/modprobe.d` :
    ```
    vi /etc/modprobe.d/bonding.conf
    alias bond0 bonding
    ```
- *Lưu ý* : Nếu mới chỉ tạo 1 bonding interface `bond0` thì cấu hình 1 dòng alias như trên.

#### 2. Cấu hình bonding interface network

#### 2.1 Khởi tạo cấu hình bonding interface MASTER

- Để khởi tạo được cổng bonding interface, ta cần tạo 1 file cấu hình có cú pháp `ifcfg-bondX` với `X` là số thứ tự của cổng interface bonding này.
- Nội dung cấu hình của file `ifcfg-bondX` vẫn dựa trên thông tin cấu hình 1 card mạng network bình thường. Điều quan trọng là ta sẽ có thêm 2 cấu hình gồm : `TYPE=Bond` và `BONDING_MASTER=yes`.
    ```
    # vi /etc/sysconfig/network-scripts/ifcfg-bond0
    DEVICE=bond0
    IPADDR=10.88.88.20
    NETMASK=255.255.255.0
    GATEWAY=10.88.88.2
    ONBOOT=yes
    BOOTPROTO=none
    USERCTL=no
    NM_CONTROLLED=no
    BONDING_OPTS="miimon=100 mode=6"
    ```
- Phần `mode` trong `BONDING_OPTS` có thể là 1 trong các lựa chọn sau :

    - mode = 0: balance-rr
    - mode = 1: active – backup
    - mode = 2: balance – xor
    - mode = 3: broadcast
    - mode = 4: 802.3ad
    - mode = 5: balance – tlb
    - mode = 6: balance – alb
    - `miimon` là giá trị tính bằng ms chỉ thời gian giám sát MII của NIC

#### 2.2 Cấu hình card mạng SLAVE

– Sau khi cấu hình cổng `bonding interface` xong thì cổng bonding đó sẽ được coi là MASTER (chính) và các cổng interface khác được gom tham gia vào hoạt động bond interface sẽ được coi là các SLAVE (phụ). Lúc này ta cần lưu ý khi cấu hình các cổng interface chính đang có trong hệ thống phải thêm 2 giá trị `MASTER` và `SLAVE` để chỉ rõ vai trò thuộc tính của các interface này trong quá trình tham gia hoạt động bonding.
- Tiếp theo ta sẽ cấu hình cho 2 interface tham gia vào bonding, ví dụ là `eno1` và `eno2`. Nội dung file cấu hình `eno1` và `eno2` như sau :
    ```
    # vi /etc/sysconfig/network-scripts/ifcfg-eno1
    DEVICE=eno1
    TYPE=Ethernet
    ONBOOT=yes
    BOOTPROTO=none
    NM_CONTROLLED=no
    MASTER=bond0
    SLAVE=yes
    ```

    ```
    # vi /etc/sysconfig/network-scripts/ifcfg-eno2
    DEVICE=eno2
    TYPE=Ethernet
    ONBOOT=yes
    BOOTPROTO=none
    NM_CONTROLLED=no
    MASTER=bond0
    SLAVE=yes
    ```
- *Lưu ý* : slave=yes và master=bond0

#### 3. Khởi động card bond0

- Tiếp theo ta sẽ khởi động lại Network Service để up các card mạng cùng card bond0 mới được cấu hình lên. Trong một số trường hợp bạn cần khởi động lại hệ thống để có thể hoạt động các cổng interface đã cấu hình.
    ```
    service network restart
    ```

### Bonding on Centos 8

- List các network device : `nmcli device`

    ![a](https://imgur.com/Oz5Hlh6.png)

- Thêm bonding device với tên bất kì (ví dụ này sử dụng tên bond0). Lưu ý đặt `mode number` cho phù hợp (mode=) (ví dụ này chọn mode `balance-alb`).
    ```
    nmcli connection add type bond ifname bond0 con-name bond0 bond.options "mode=balance-alb"
    ```

    ![a](https://imgur.com/HyAp6Bm.png)

- Thêm các member devices cho bonding :
    ```
    nmcli connection add type ethernet ifname eno1 master bond0
    ```

    ![a](https://imgur.com/MYtZbhj.png)

- Kiểm tra lại các network device đã add : `nmcli device` - `nmcli connection`

    ![a](https://imgur.com/s5cLSXt.png)

    ![a](https://imgur.com/kMJUiM0.png)

- Đặt IP address cho bonding device và restart :
    ```
    nmcli connection modify bond0 ipv4.addresses 10.88.88.20/24
    nmcli connection modify bond0 ipv4.gateway 10.88.88.2
    nmcli connection modify bond0 ipv4.dns "8.8.8.8 8.8.4.4"
    nmcli connection modify bond0 ipv4.dns-search "google.com"
    nmcli connection modify bond0 ipv4.method manual
    nmcli connection down bond0
    nmcli connection up bond0
    ```

    ![a](https://imgur.com/HigmknR.png)

- Xác nhận lại trạng thái của bond : `cat /proc/net/bonding/bond0`

    ![a](https://imgur.com/jY90eKt.png)

- File cấu hình được lưu ở `/etc/sysconfig/network-scripts`

### Bonding on Ubuntu 22.04

- Theo mặc định từ bản Ubuntu 18.04 trở đi, Ubuntu sẽ sử dụng config mạng bằng công cụ `Netplan`. Netplan lưu cấu hình bằng file `yaml` được lưu tại `etc/netplan/(file yaml)`.
- Để cấu hình Bonding trên Ubuntu ta truy cập vào file yaml để chỉnh sửa. Lưu ý : Nhớ backup file config để roll-back khi cần `cp /etc/netplan/(file yaml) /etc/netplan/(file yaml).bak`
- final netplan config :
    ```
    network:
        renderer: networkd
        ethernets:
            ens33:
                dhcp4: no
            ens34:
                dhcp4: no
    bonds:
        bond0:
            interfaces: [ens33, ens34]
            addresses: [10.88.88.22/24]
            routes:
                - to: default
                via: 10.88.88.2
            nameservers:
                search: [google.com]
                addresses: [8.8.8.8, 8.8.4.4]
            parameters:
                mode: balance-alb
                mii-monitor-interval: 100
    version: 2
    ```
- áp dụng cấu hình : `sudo netplan apply`
- dùng lệnh `ip a` để kiểm tra trạng thái của các interface SLAVE và MASTER.
- xem thông tin của bond trong `/proc/net/bonding/bond0`

    ![a](https://imgur.com/BTZneX7.png)

    ![a](https://imgur.com/WdTMVAs.png)