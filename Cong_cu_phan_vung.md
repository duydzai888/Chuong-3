# Công cụ phân vùng

### Làm việc với `fdisk`

Sử dụng lệnh `fdisk` để tạo partition, nó cho phép ta tạo, xem sửa, xóa và sửa đổi phân vùng trên bất kỳ ổ đĩa nào sử dụng phương pháp lập chỉ mục phân vùng MBR

```
[root@client ~]# fdisk /dev/sdb
Welcome to fdisk (util-linux 2.23.2).

Changes will remain in memory only, until you decide to write them.
Be careful before using the write command.

Device does not contain a recognized partition table
Building a new DOS disklabel with disk identifier 0xaaa2e28d.

Command (m for help): n
Partition type:
   p   primary (0 primary, 0 extended, 4 free)
   e   extended
Select (default p): p
Partition number (1-4, default 1): 1
First sector (2048-20971519, default 2048):
Using default value 2048
Last sector, +sectors or +size{K,M,G} (2048-20971519, default 20971519): +1G
Partition 1 of type Linux and of size 1 GiB is set

Command (m for help): w
The partition table has been altered!

Calling ioctl() to re-read partition table.
Syncing disks.
```
MBR không thể xử lý ổ đĩa có dung lượng lớn hơn 2TB, chỉ hỗ trợ 4 phân vùng chính.

### Làm việc với `gdisk`

Sử dụng lệnh `gdisk` để tạo, sửa, xóa các phân vùng. 

```
[root@client ~]# gdisk /dev/vdb
GPT fdisk (gdisk) version 0.8.10

Partition table scan:
  MBR: not present
  BSD: not present
  APM: not present
  GPT: not present

Creating new GPT entries.

Command (? for help): n
Partition number (1-128, default 1): 1
First sector (34-62914526, default = 2048) or {+-}size{KMGTP}: +5G
Last sector (10487808-62914526, default = 62914526) or {+-}size{KMGTP}: +5G
Current type is 'Linux filesystem'
Hex code or GUID (L to show codes, Enter = 8300):
Changed type of partition to 'Linux filesystem'

Command (? for help): w

Final checks complete. About to write GPT data. THIS WILL OVERWRITE EXISTING
PARTITIONS!!

Do you want to proceed? (Y/N): y
OK; writing new GUID partition table (GPT) to /dev/vdb.
The operation has completed successfully.
```

GPT cho phép lên đến 128 phân vùng, Kích thước giới hạn phụ thuộc vào hệ điều hành và hệ thống tập tin của hệ điều hành. 

### Lệnh chia tách GNU

Lệnh `parted` cũng cho phép tạo, và quản lý các phân vùng. Nó cho phép tạo nhiều loại label như: `aix`, `amiga`, `bsd`, `dvh`, `gpt`, `mac`, `msdos`, `pc98`, `sun`, `loop`.

### Công cụ đồ họa

`gparted` cho phép tạo phân vùng sử dụng đồ họa 