## RAID

- **RAID** là viết tắt của **Redundant Array of Independent Disks**.
- Mục đích ban đầu là một giải pháp dự phòng vì nó có thể ghi data lên nhiều ổ cứng cùng lúc. Về sau nó được cải tiến thêm chức năng giúp gia tăng tốc độ truy xuất dữ liệu từ ổ cứng.
- Có năm loại RAID được dùng phổ biến : 0,1,5,6,10

### RAID là gì ?

- RAID chỉ làm việc với các loại ổ cứng có dung lượng bằng nhau.
- Sử dụng RAID sẽ tốn số lượng ổ nhiều hơn bình thường, nhưng đổi lại dữ liệu sẽ an toàn hơn.
- RAID có thể dùng cho bất kỳ hệ điều hành nào
- Các loại RAID phổ biến :
    - RAID 0 bằng tổng dung lượng các ổ cộng lại.
    - RAID 1 chỉ duy trì dung lượng 1 ổ.
    - RAID 5 sẽ có dung lượng ít hơn 1 ổ (5 ổ dùng RAID 5 sẽ có dung lượng 4 ổ).
    - RAID 6 sẽ có dụng lượng ít hơn 2 ổ (5 ổ dùng RAID 6 sẽ có dung lượng 3 ổ).
    - RAID 10 sẽ chỉ tạo được khi số ổ là chắn, dung lượng bằng tổng số ổ chia đôi (10 ổ thì dung lượng sử dụng là 5 ổ).

#### RAID 0

- Đây là dạng RAID đang được người dùng ưa thích do khả năng nâng cao hiệu suất trao đổi dữ liệu của đĩa cứng. Đòi hỏi tối thiểu hai đĩa cứng, RAID 0 cho phép máy tính ghi dữ liệu lên chúng theo một phương thức đặc biệt được gọi là Striping. Ví dụ bạn có 8 đoạn dữ liệu được đánh số từ 1 đến 8, các đoạn đánh số lẻ (1,3,5,7) sẽ được ghi lên đĩa cứng đầu tiên và các đoạn đánh số chẵn (2,4,6,8) sẽ được ghi lên đĩa thứ hai. Để đơn giản hơn, bạn có thể hình dung mình có 100MB dữ liệu và thay vì dồn 100MB vào một đĩa cứng duy nhất, RAID 0 sẽ giúp dồn 50MB vào mỗi đĩa cứng riêng giúp giảm một nửa thời gian làm việc theo lý thuyết. Từ đó bạn có thể dễ dàng suy ra nếu có 4, 8 hay nhiều đĩa cứng hơn nữa thì tốc độ sẽ càng cao hơn. Tuy nghe có vẻ hấp dẫn nhưng trên thực tế, RAID 0 vẫn ẩn chứa nguy cơ mất dữ liệu. Nguyên nhân chính lại nằm ở cách ghi thông tin xé lẻ vì như vậy dữ liệu không nằm hoàn toàn ở một đĩa cứng nào và mỗi khi cần truy xuất thông tin (ví dụ một file nào đó), máy tính sẽ phải tổng hợp từ các đĩa cứng. Nếu một đĩa cứng gặp trục trặc thì thông tin (file) đó coi như không thể đọc được và mất luôn. Thật may mắn là với công nghệ hiện đại, sản phẩm phần cứng khá bền nên những trường hợp mất dữ liệu như vậy xảy ra không nhiều.

- Có thể thấy RAID 0 thực sự thích hợp cho những người dùng cần truy cập nhanh khối lượng dữ liệu lớn, ví dụ các game thủ hoặc những người chuyên làm đồ hoạ, video số.

    ![a](https://imgur.com/t79KhAG.png)

#### RAID 1

- Đây là dạng RAID cơ bản nhất có khả năng đảm bảo an toàn dữ liệu. Cũng giống như RAID 0, RAID 1 đòi hỏi ít nhất hai đĩa cứng để làm việc. Dữ liệu được ghi vào 2 ổ giống hệt nhau (Mirroring). Trong trường hợp một ổ bị trục trặc, ổ còn lại sẽ tiếp tục hoạt động bình thường. Bạn có thể thay thế ổ đĩa bị hỏng mà không phải lo lắng đến vấn đề thông tin thất lạc. Đối với RAID 1, hiệu năng không phải là yếu tố hàng đầu nên chẳng có gì ngạc nhiên nếu nó không phải là lựa chọn số một cho những người say mê tốc độ. Tuy nhiên đối với những nhà quản trị mạng hoặc những ai phải quản lý nhiều thông tin quan trọng thì hệ thống RAID 1 là thứ không thể thiếu. Dung lượng cuối cùng của hệ thống RAID 1 bằng dung lượng của ổ đơn (hai ổ 80GB chạy RAID 1 sẽ cho hệ thống nhìn thấy duy nhất một ổ RAID 80GB).

    ![a](https://imgur.com/Ps8Gtrv.png)

#### RAID 0+1

- Có bao giờ bạn ao ước một hệ thống lưu trữ nhanh nhẹn như RAID 0, an toàn như RAID 1 hay chưa? Chắc chắn là có và hiển nhiên ước muốn đó không chỉ của riêng bạn. Chính vì thế mà hệ thống RAID kết hợp 0+1 đã ra đời, tổng hợp ưu điểm của cả hai “đàn anh”. Tuy nhiên chi phí cho một hệ thống kiểu này khá đắt, bạn sẽ cần tối thiểu 4 đĩa cứng để chạy RAID 0+1. Dữ liệu sẽ được ghi đồng thời lên 4 đĩa cứng với 2 ổ dạng Striping tăng tốc và 2 ổ dạng Mirroring sao lưu. 4 ổ đĩa này phải giống hệt nhau và khi đưa vào hệ thống RAID 0+1, dung lượng cuối cùng sẽ bằng ½ tổng dung lượng 4 ổ, ví dụ bạn chạy 4 ổ 80GB thì lượng dữ liệu “thấy được” là (4*80)/2 = 160GB.

#### RAID 5

- Đây có lẽ là dạng RAID mạnh mẽ nhất cho người dùng văn phòng và gia đình với 3 hoặc 5 đĩa cứng riêng biệt. Dữ liệu và bản sao lưu được chia lên tất cả các ổ cứng. Nguyên tắc này khá rối rắm. Chúng ta quay trở lại ví dụ về 8 đoạn dữ liệu (1-8) và giờ đây là 3 ổ đĩa cứng. Đoạn dữ liệu số 1 và số 2 sẽ được ghi vào ổ đĩa 1 và 2 riêng rẽ, đoạn sao lưu của chúng được ghi vào ổ cứng 3. Đoạn số 3 và 4 được ghi vào ổ 1 và 3 với đoạn sao lưu tương ứng ghi vào ổ đĩa 2. Đoạn số 5, 6 ghi vào ổ đĩa 2 và 3, còn đoạn sao lưu được ghi vào ổ đĩa 1 và sau đó trình tự này lặp lại, đoạn số 7,8 được ghi vào ổ 1, 2 và đoạn sao lưu ghi vào ổ 3 như ban đầu. Như vậy RAID 5 vừa đảm bảo tốc độ có cải thiện, vừa giữ được tính an toàn cao. Dung lượng đĩa cứng cuối cùng bằng tổng dung lượng đĩa sử dụng trừ đi một ổ. Tức là nếu bạn dùng 3 ổ 80GB thì dung lượng cuối cùng sẽ là 160GB.

    ![a](https://imgur.com/tJD1HvJ.png)

#### JBOD

- JBOD (Just a Bunch Of Disks) thực tế không phải là một dạng RAID chính thống, nhưng lại có một số đặc điểm liên quan tới RAID và được đa số các thiết bị điều khiển RAID hỗ trợ. JBOD cho phép bạn gắn bao nhiêu ổ đĩa tùy thích vào bộ điều khiển RAID của mình (dĩ nhiên là trong giới hạn cổng cho phép). Sau đó chúng sẽ được “tổng hợp” lại thành một đĩa cứng lớn hơn cho hệ thống sử dụng. Ví dụ bạn cắm vào đó các ổ 10GB, 20GB, 30GB thì thông qua bộ điều khiển RAID có hỗ trợ JBOD, máy tính sẽ nhận ra một ổ đĩa 60GB. Tuy nhiên, lưu ý là JBOD không hề đem lại bất cứ một giá trị phụ trội nào khác: không cải thiện về hiệu năng, không mang lại giải pháp an toàn dữ liệu, chỉ là kết nối và tổng hợp dung lượng mà thôi.

#### Một số loại RAID khác

- Ngoài các loại được đề cập ở trên, bạn còn có thể bắt gặp nhiều loại RAID khác nhưng chúng không được sử dụng rộng rãi mà chỉ giới hạn trong các hệ thống máy tính phục vụ mục đích riêng, có thể kể như: Level 2 (Error-Correcting Coding), Level 3 (Bit-Interleaved Parity), Level 4 (Dedicated Parity Drive), Level 6 (Independent Data Disks with Double Parity), Level 10 (Stripe of Mirrors, ngược lại với RAID 0+1), Level 7 (thương hiệu của tập đoàn Storage Computer, cho phép thêm bộ đệm cho RAID 3 và 4), RAID S (phát minh của tập đoàn EMC và được sử dụng trong các hệ thống lưu trữ Symmetrix của họ). Bên cạnh đó còn một số biến thể khác, ví dụ như Intel Matrix Storage cho phép chạy kiểu RAID 0+1 với chỉ 2 ổ cứng hoặc RAID 1.5 của DFI trên các hệ BMC 865, 875. Chúng tuy có nhiều điểm khác biệt nhưng đa phần đều là bản cải tiến của các phương thức RAID truyền thống.

### Yêu cầu để chạy RAID

- Để chạy được RAID, bạn cần tối thiểu một card điều khiển và hai ổ đĩa cứng giống nhau về dung lượng. Đĩa cứng có thể ở bất cứ chuẩn nào, từ ATA, Serial ATA hay SCSI, tốt nhất chúng nên hoàn toàn giống nhau vì một nguyên tắc đơn giản là khi hoạt động ở chế độ đồng bộ như RAID, hiệu năng chung của cả hệ thống sẽ bị kéo xuống theo ổ thấp nhất nếu có. Ví dụ khi bạn bắt ổ 160GB chạy RAID với ổ 40GB (bất kể 0 hay 1) thì coi như bạn đã lãng phí 120GB vô ích vì hệ thống điều khiển chỉ coi chúng là một cặp hai ổ cứng 40GB mà thôi (ngoại trừ trường hợp JBOD như đã đề cập). Yếu tố quyết định tới số lượng ổ đĩa chính là kiểu RAID mà bạn định chạy. Chuẩn giao tiếp không quan trọng lắm, đặc biệt là giữa SATA và ATA. Một số BMC đời mới cho phép chạy RAID theo kiểu trộn lẫn cả hai giao tiếp này với nhau. Điển hình như MSI K8N Neo2 Platinum hay dòng DFI Lanparty NForce4.
- Bộ điều khiển RAID (RAID Controller) là nơi tập trung các cáp dữ liệu nối các đĩa cứng trong hệ thống RAID và nó xử lý toàn bộ dữ liệu đi qua đó. Bộ điều khiển này có nhiều dạng khác nhau, từ card tách rời cho dến chip tích hợp trên BMC.
- Đối với các hệ thống PC, tuy chưa phổ biến nhưng việc chọn mua BMC có RAID tích hợp là điều nên làm vì nói chung đây là một trong những giải pháp cải thiện hiệu năng hệ thống rõ rệt và rẻ tiền nhất, chưa tính tới giá trị an toàn dữ liệu của chúng. Trong trường hợp BMC không có RAID, bạn vẫn có thể mua được card điều khiển PCI trên thị trường với giá không cao lắm.
- Một thành phần khác của hệ thống RAID không bắt buộc phải có nhưng đôi khi là hữu dụng, đó là các khay hoán đổi nóng ổ đĩa. Nó cho phép bạn thay các đĩa cứng gặp trục trặc trong khi hệ thống đang hoạt động mà không phải tắt máy (chỉ đơn giản là mở khóa, rút ổ ra và cắm ổ mới vào). Thiết bị này thường sử dụng với ổ cứng SCSI và khá quan trọng đối với các hệ thống máy chủ vốn yêu cầu hoạt động liên tục.