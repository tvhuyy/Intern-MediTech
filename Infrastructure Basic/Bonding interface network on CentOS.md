## Bonding interface network on CentOS

- Bonding interface là một cơ chế cho phép cấu hình từ 2 đến nhiều interface vật lý kết hợp thành 1 interface ảo bằng cách sử dụng `module kernel bonding` trên Linux. Điều này mang lại các lợi ích như : tăng badwidth, HA, Load Balancing.

### 1. Cài đặt module bonding

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

### 2. Cấu hình bonding interface network

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

### 3. Khởi động card bond0

- Tiếp theo ta sẽ khởi động lại Network Service để up các card mạng cùng card bond0 mới được cấu hình lên. Trong một số trường hợp bạn cần khởi động lại hệ thống để có thể hoạt động các cổng interface đã cấu hình.
    ```
    service network restart
    ```