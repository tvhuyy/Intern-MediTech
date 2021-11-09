## Tìm hiểu, sử dụng VMware Workstation

## 1. Lý thuyết

### 1.1 Tìm hiểu 3 chế độ card mạng, đường đi của các chế độ :
**Chế độ Bridge** :

- Ở chế độ này, card mạng trên máy ảo được gắn vào VMnet0, VMnet0 này liên kết trực tiếp đến card mạng trên máy thật.
- Máy ảo lúc này sẽ kết nối internet thông qua card mạng vật lý và có chung dải mạng với máy thật.

![a](https://i.imgur.com/QRqjiQp.png)

**Chế độ NAT** :
 
 - Ở chế độ này, card mạng của máy ảo liên kết với VMnet8 cho phép đi ra internet thông qua card mạng vật lý bằng cơ chế NAT.
 - Lúc này lớp mạng của máy ảo khác hoàn toàn với card mạng vật lý bên ngoài, hai mạng hoàn toàn tách biệt.

![a](https://i.imgur.com/WvaGSFz.png)

**Chế độ Host-Only** :

- Ở chế độ này, máy ảo được kết nối với VMnet có tính năng host-only. Khi tạo một card mạng ảo trên VMware thì cũng sẽ tạo ra card mạng ảo tương ứng trên máy thật.
- Chế độ này, máy ảo không có kết nối vào mạng vật lý bên ngoài hay internet thông qua máy thật, có nghĩa là mạng VMnet Host-only tách biệt hoàn toàn với mạng vật lý.

![a](https://i.imgur.com/0O0z3PU.png)

### 1.2 Port Forwarding, NAT Advanced :

- Port Forwarding là quá trình chuyển tiếp một port cụ thể từ mạng này đến mạng khác, cho phép người dùng bên ngoài có thể truy cập vào mạng bên trong bằng cách sử dụng port đó từ bên ngoài thông qua bộ định tuyến (đã mở NAT).
- Port Forwarding trong VMware là thêm một cổng để chuyển tiếp cổng. Với Forwarding thì các yêu cầu qua TCP hoặc UDP đến sẽ được gửi đến máy ảo cụ thể trong dải mạng được cung cấp bởi thiết bị NAT. Các thông số cấu hình Port Forwarding :

	- Host Port : Là số cổng TCP hoặc UDP đến.
	- Virtual machine IP address : Địa chỉ IP máy ảo mà bạn muốn Forward yêu cầu đến.
	- Virtual machine port : Là số cổng để sử dụng cho các yêu cầu trên máy ảo được chỉ định.
	- Description : Là tùy chọn để bạn có thể miêu tả chức năng của dịch vụ được chuyển tiếp.
- NAT advanced là các tùy chọn nâng cao trong cài đặt NAT :
	- Allow active FTP : tùy chọn có cho phép chỉ chế độ FTP passive trên thiết bị NAT hay không.
	- Allow any Organizationally Unique Identifier : Chọn cài đặt này nếu bạn thay đổi phần định danh duy nhất có tổ chức (OUI) của địa chỉ MAC cho máy ảo và sau đó không thể sử dụng NAT với máy ảo.

### 1.3 LAN Segment trên VMware
- LAN Segment giúp cho các máy ảo có thể giao tiếp được với nhau mà không cần đến các virtual switch bằng cách add các máy ảo vào một Team rồi tạo LAN Segment cho team. Mỗi môt LAN segment hoàn toàn cô lập với máy thật và các LAN segment khác. Trên 1 máy có thể tạo ra nhiều NIC rồi nối các NIC này với LAN Segment hoặc các Virtual Switch để tạo ra các mạng LAN theo ý muốn.

### 1.4 Tìm hiểu 4 cách phân vùng ổ cứng khi cài đặt Ubuntu

- `Guided - use entire disk` : máy tính sẽ tự động phân vùng cho ổ cứng.
- `Guided - use entire disk and with set up LVM` : Tự động phân vùng bằng LVM (Logical Volume Management)
- `Guided - use entire disk and with set encrypted up LVM` : giống lựa chọn trên nhưng ổ cứng sẽ được mã hóa để tăng tính bảo mật.
- `Manual` : phân vùng thủ công.

### 1.5 Kỹ thuật Clone máy ảo
- Clone máy ảo là kỹ thuật dùng để nhân bản một máy ảo nào đó được tạo ra trong VMware thành nhiều bản nữa giống hệt máy ảo ban đầu với mục đích tiết kiệm thời gian cho việc lặp đi lặp lại cài đặt các máy ảo có cấu hình giống nhau. Clone máy ảo có 2 loại :
	- `Full Clone` : Là bản sao máy ảo đầy đủ, tất cả các dữ liệu được chép vào ổ đĩa ảo mới (độc lập với máy ảo gốc). Các clone này cũng hoạt động độc lập với gốc của nó.
	- `Linked Clone` : Là bản sao mới của máy ảo nhưng ổ đĩa ảo này sử dụng là của máy ảo ban đầu, nghĩa là khi thao tác trên máy ảo mới thì sẽ tác động tới máy ảo gốc (ghi, xóa dữ liệu).


## 2. Thực hành

- Gắn hai card mạng cho một máy ảo trên Vmware, một để Nat, một để Briged :
![a](https://i.imgur.com/KBRYInt.png)


![a](https://prnt.sc/1ymhkod.png)

![a](https://i.imgur.com/hoUplWL.png)






		
