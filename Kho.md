# Kho

## Khái niệm cơ bản về lưu trữ

### Các loại ổ đĩa
Có 3 loại kết nối ổ đĩa ta sẽ gặp phải với hệ thống Linux. 

- Parallel Advanced Technology Attachment (PATA)
- Serial Advanced Technology Attachment (SATA)
- Small	Computer System Interface (SCSI)

### Phân vùng đĩa

Hầu hết các hệ điều hành cho phép phân vùng ổ đĩa thành nhiều phần. 

### Phát hiện ổ đĩa tự động

Hệ thống linux phát hiện ra các ổ đĩa và phân vùng tại thời điểm khởi động và gán cho mỗi ổ đĩa một tên tệp thiết bị duy nhất. 

Hầu hết các hệ thống Linux hiện nay đều sử dụng ứng dụng udev luôn chạy ở chế độ nền và tự động phát hiện phần cứng mới được kết nối với hệ thống Linux đang chạy. 

## Giải pháp thay thế lưu trữ

### Multipath

Một số kỹ thuật quản lý lưu trữ: 

- `dm-multipath`: 
- `multipath`: 
- `multipathd`: 

### Trình quản lý khối lượng hợp lý

LVM là công nghệ giúp quản lý các thiết bị lưu trữ dữ liệu trên các hệ điều hành linux. Nó cho phép người dùng gom các ổ cứng vật lý lại và tách chúng thành những phân vùng nhỏ hơn, dễ dàng mở rộng khi cần thiết. 

Một bố cục LVM bao gồm : Hard drives, partitions, physical volume, volume group, logical volume, file systems.

- Physical Volumes: Một ổ đĩa vật lý có thể phân chia thành nhiều phân vùng vật lý gọi là Physical Volumes
- Volume Group: là 1 nhóm bao gồm nhiều Physical Volume trên 1 hoặc nhiều ổ đĩa khác nhau được liên kết lại thành 1 Volume group
- Logical volume: Một volume group được chia nhỏ ra thành nhiều logical volume, được format với các chuẩn định dạng Ext3, Ext4
- File Systems: Hệ thống tập tin quản lý các file và thư mục trên ổ đĩa, được mount tới các logical volumes

### Sử dụng công nghệ RAID

- RAID chỉ nên làm việc với các loại ổ cứng dung lượng bằng nhau.
- Sử dụng RAID sẽ tốn số lượng ổ nhiều hơn bình thường, nhưng đổi lại là dữ liệu sẽ an toàn hơn.

Một số phiên bản RAID thường được sử dụng: 

- RAID 0 bằng tổng dung lượng các ổ cộng lại.
- RAID 1 chỉ duy trì dung lượng 1 ổ.
- RAID 5 sẽ có dung lượng ít hơn 1 ổ (5 ổ dùng raid 5 sẽ có dung lượng 4 ổ).
- RAID 6 sẽ có dung lượng ít hơn 2 ổ (5 ổ dùng raid 6 sẽ có dung lượng 3 ổ).
- RAID 10 sẽ chỉ tạo được khi số ổ là chẵn, phải có tối thiểu từ 2 ổ trở lên. Dung lượng bằng tổng số ổ chia đôi (10 ổ thì dung lượng sử dụng là 5 ổ).

