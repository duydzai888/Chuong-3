# Filesystems

## Định dạng hệ thống tập tin

### Các loại hệ thống tập tin phổ biến

#### Hệ thống

Có các hệ thống tệp như: 

- btrfs: Hệ thống tệp hiệu suất cao, hỗ trợ tệp lên đến 16EB và tổng kích thước hệ thống tệp là 16EB

- encryptfs: 

- ext3: Nó hỗ trợ tệp lên đến 2 TB với tổng kích thước tệp là 16EB

- ext4: Nó hỗ trợ các tệp lên đến 16TB, với tổng kích thước của hệ thống tệp là 1EB

- reiserFS: Thường sử dụng trên các hệ thống linux cũ, nó cung cấp các tính năng hiện có trong ext3 và ext4

- swap: Cho phép tạo bộ nhớ ram ảo cho hệ thống của mình bằng cách sử dụng không gian trên ổ đĩa vật lý. 

#### Hệ thống tệp không phải Linux

Danh sách các hệ thống tệp mà linux có thể xử lý:

- CIFS
- exFAT
- HFS
- ISO-9660
- NFS
- NTFS
- SMB
- UDF
- VFAT
- XFS
- ZFS


### Tạo hệ thống tập tin

Sau khi sử dụng `fdisk` hoặc `gdisk` để tạo file pattern, ta sử dụng `mkfs` để chỉ định loại tệp sẽ được sử dụng. 

```
[root@client ~]# mkfs -t ext4 /dev/vdb1
mke2fs 1.42.9 (28-Dec-2013)
Filesystem label=
OS type: Linux
Block size=4096 (log=2)
Fragment size=4096 (log=2)
Stride=0 blocks, Stripe width=0 blocks
655360 inodes, 2621440 blocks
131072 blocks (5.00%) reserved for the super user
First data block=0
Maximum filesystem blocks=2151677952
80 block groups
32768 blocks per group, 32768 fragments per group
8192 inodes per group
Superblock backups stored on blocks:
        32768, 98304, 163840, 229376, 294912, 819200, 884736, 1605632

Allocating group tables: done
Writing inode tables: done
Creating journal (32768 blocks): done
Writing superblocks and filesystem accounting information: done
```

## Gắn hệ thống tập tin

### Gắn thiết bị theo cách thủ công

Để tạm thời gắn kết 1 hệ thống tệp vào thư mục trên linux, ta sử dụng lênh mount. 

```
[root@client]# mount -t ext4 /dev/vdb1 /media/disk1
```

```
[root@client]# lsblk
NAME            MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
sr0              11:0    1  918M  0 rom
vda             252:0    0   20G  0 disk
├─vda1          252:1    0    1G  0 part /boot
└─vda2          252:2    0   19G  0 part
  ├─centos-root 253:0    0   17G  0 lvm  /
  └─centos-swap 253:1    0    2G  0 lvm  [SWAP]
vdb             252:16   0   30G  0 disk
└─vdb1          252:17   0   10G  0 part /media/disk1
```

### Tự động gắn thiết bị

Để 1 tệp được tự động mount khi máy được bật, ta phải thêm trực tiếp hệ thống tệp vào file `/etc/fstab`. 

VD: Để mount `vdb1` vào thư mục `/media/disk1/` ta thêm đoạn sau vào cuối file `/etc/fstab`: `/dev/vdb1 /media/disk1/ ext4 defaults 0 0`
```
[root@client ~]# cat /etc/fstab

#
# /etc/fstab
# Created by anaconda on Sun Jun 14 04:36:01 2020
#
# Accessible filesystems, by reference, are maintained under '/dev/disk'
# See man pages fstab(5), findfs(8), mount(8) and/or blkid(8) for more info
#
/dev/mapper/centos-root /                       xfs     defaults        0 0
UUID=f3315d0f-4fd4-45de-8460-96908cf37860 /boot                   xfs     defaults        0 0
/dev/mapper/centos-swap swap                    swap    defaults        0 0
/dev/vdb1 /media/disk1/ ext4 defaults 0 0
```

## Quản lí hệ thống tập tin

Một số tiện ích có sẵn để quản lý hệ thống tệp trên linux 

### Truy xuất thống kê hệ thống tệp

Một số lệnh để theo dõi dung lượng ổ cứng: 

- df : Hiển thị mức sử dụng disk theo phân vùng

- du : Hiển thị mức sử dụng disk theo thư mục

- iostat: hiển thị biểu đồ thống kê đĩa theo phân vùng theo thời gian thực

- lsblk: Hiển thị kích thước phân vùng hiện tại và các điểm được mount. 

### Công cụ hệ thống tập tin

- `blkid` : Hiển thị thông tin của các pattions device

- `chattr`: Thay đổi thuộc tính trên hệ thống file

- `debugfs`: Xem và sửa đổi cấu trúc hệ thống file theo cách thủ công.

- `dumpe2fs`: Hiển thị thông tin khối 

- `e2label`: Thay đổi nhãn trên hệ thống tệp

- `resize2fs`: Mở rộng hoặc thu nhỏ hệ thống tệp

- `tune2fs`: Sửa đổi thông số hệ thống tệp

- `xfs_admin`: Hiển thị hoặc thay đổi các thông số hệ thống tệp như nhãn hoặc UUID được chỉ định

- `xfs_db`: Kiểm tra và gỡ lỗi hệ thống tệp XFS

- `xfs_fsr`: Cải thiện tổ chức của hệ thống tệp tin gắn kết

- `xfs_info`: Hiển thị thông tin về hệ thống tệp được mount

- `xfs_repair`: Sửa chữa tệp XFS bị hỏng
`fsck` là tiện ích dòng lệnh cho phép thực hiện kiểm tra tính nhất quán và sửa chữa tương tác trên 1 hoặc nhiều hệ thống tệp Linux. 