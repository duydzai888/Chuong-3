# Cấu hình phần cứng và phần cứng cốt lõi

### Hiểu vai trò của phần mềm cơ sở

### Khởi động BIOS

### Khởi động UEFI

### Giao diện thiết bị

Mỗi thiết bị kết nối với hệ thống Linux sử dụng 1 số loại giao thức tiêu chuẩn để giao tiếp với phần cứng hệ thống. 

#### #### Bo mạch PCI

Tiêu chuẩn kết nối PCI được phát triển năm 1993 như 1 phuwong pháp kết nối bo mạch phần cứng với bo mạch chủ PC.

#### Giao diện USB

Giao diện USB (Universal Serial Bus) phổ biến do tính dễ sử dụng và khả năng hỗ trợ truyền dữ liệu tốc độ cao. 

Phiên bản 1.0 hỗ trợ tốc độ truyền dữ liệu lên đến 12Mbps, chuẩn 2.0 tăng lên 480Mbps và 3.2 cho phép lên đến 20Gbps

Có nhiều thiết bị có thể sử dụng USB interface như : Bàn phím, máy in, chuột, ...

#### Giao diện GPIO

### Thư mục / dev
Linux kernel giao tiếp với 1 thiết bị trên 1 giao diện, nó phải có khả năng truyền dữ liệu đến và đi từ thiết bị đó. Điều này được thực hiện bằng các tệp thiết bị. Tệp thiết bị là tệp mà nhân Linux tạo trong thư mục đặc biệt `/dev` để giao tiếp với các thiết bị phần cứng. 

Khi thêm các thiết bị phần cứng như ổ USB, thẻ mạng hoặc ổ cứng vào hệ thống của mình, Linux sẽ tạo 1 tệp vào trong thư mục `/dev` đại diện cho thiết bị phần cứng đó. Các chương trình ứng dụng sau đo có thể tương tác trực tiếp với tệp đó để lưu trữ và truy xuất dữ liệu trên thiết bị. 

### Thư mục / proc

Thư mục `/proc` là 1 thư mục ảo mà kernel tự động điền để cung cấp quyền truy cập vào thông tin về cài đặt và trạng thái phần cứng hệ thống. 

#### Yêu cầu gián đoạn

IRQ cho phép các thiết bị phần cứng chỉ ra khi nào chúng có dữ liệu để gửi đến CPU. 

#### I/O Ports

Các cổng I/O của hệ thống là các vị trí trong bộ nhớ, nơi CPU có thể gửi dữ liệu đến và nhận dữ liệu từ thiết bị phần cứng. Có thể giám sát các cổng I/O được gán cho các thiết bị phần cứng trên hệ thống của mình bằng cách xem tệp /proc/ioports. 

#### Truy cập bộ nhớ trực tiếp

Việc sử dụng các cổng I/O để gửi dữ liệu đến CPU có thể hơi chậm, để tăng tốc độ, nhiều thiết bị sử dụng kênh truy cập bộ nhớ trực tiếp DMA. DMA gửi dữ liệu từ thiết bị phần cứng đến bộ nhớ hệ thống mà không cần chờ CPU, sau đó CPU có thể đọc các vị trí bộ nhớ đó để truy cập dữ liệu khi sẵn sàng. 

### Thư mục / sys

Một công cụ khác có sẵn để làm việc với các thiết bị là thư mục `/sys`. Thư mục `/sys` là 1 thư mục ảo tương tự `/proc`. Nó được tạo bởi kernel và nó cung cấp thông tin bổ sung về các thiết bị phần cứng mà người dùng nào trên hệ thống cũng có thể truy cập. 

### Làm việc với thiết bị

#### Tìm kiếm thiết bị
Sử dụng lệnh `lsdev` để hiển thị thông tin về các thiết bị phần cứng được cài đặt trên hệ thống. 

```
root@laiduy:~# lsdev
Device            DMA   IRQ  I/O Ports
------------------------------------------------
0000:00:01.1                   0170-0177   01f0-01f7   0376-0376   03f6-03f6   c0c0-c0cf
0000:00:01.2                   c040-c05f
0000:00:01.3                   0600-063f   0700-070f
0000:00:03.0                   c060-c07f
0000:00:04.0                   c080-c09f
0000:00:05.0                   c000-c03f
0000:00:06.0                   c0a0-c0bf
ACPI                             0600-0603     0604-0605     0608-060b   afe0-afe3
acpi                      9
ata_piix              14 15      0170-0177     01f0-01f7     0376-0376     03f6-03f6     c0c0-c0cf
cascade             4
dma                            0080-008f
dma1                           0000-001f
dma2                           00c0-00df
floppy              2     6    03f2-03f2   03f4-03f5   03f7-03f7
fpu                            00f0-00ff
fw_cfg_io                        0510-051b
i8042                  1 12
keyboard                       0060-0060   0064-0064
PCI                          0000-0cf7 0cf8-0cff 0d00-ffff
pic1                           0020-0021
pic2                           00a0-00a1
piix4_smbus                      0700-0708
QEMU0002:00                    0510-051b
rtc0                      8    0070-0071
serial                         03f8-03ff
timer                     0
timer0                         0040-0043
timer1                         0050-0053
uhci_hcd                         c040-c05f
uhci_hcd:usb1            11
virtio-pci-legacy                 c000-c03f     c060-c07f     c080-c09f     c0a0-c0bf
virtio0-config           26
virtio0-input.0          27
virtio0-output.0         28
virtio1-config           24
virtio1-virtqueues         25
virtio2-config           29
virtio2-req.0            30
```

Sử dụng lệnh `lsblk` để liệt kê các thiết bị đang chạy trên hệ thống. Nó cũng chỉ ra sự liên kết giữa các khối như với các ổ LVM được ánh xạ thiết bị và ổ cứng vật lý được liên kết. 

```
root@laiduy:~# lsblk
NAME                         MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
sr0                           11:0    1 1024M  0 rom
vda                          252:0    0   40G  0 disk
└─vda1                       252:1    0   40G  0 part
  ├─US1804--x8664--vg-root   253:0    0   39G  0 lvm  /
  └─US1804--x8664--vg-swap_1 253:1    0  980M  0 lvm  [SWAP]
```

Ta có thể sử dụng tùy chọn `-s` để chỉ xem thông tin các thiết bị khối `SCSI` trên hệ thống. 

```
root@laiduy:~# lsblk -s
NAME                     MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
sr0                       11:0    1 1024M  0 rom
US1804--x8664--vg-root   253:0    0   39G  0 lvm  /
└─vda1                   252:1    0   40G  0 part
  └─vda                  252:0    0   40G  0 disk
US1804--x8664--vg-swap_1 253:1    0  980M  0 lvm  [SWAP]
└─vda1                   252:1    0   40G  0 part
  └─vda                  252:0    0   40G  0 disk
```

#### Làm việc với thẻ PCI

Lệnh `lspci` cho phép xem các thẻ PCI và PCIe hiện được cài đặt và công nhận trên hệ thốn Linux.

```
root@laiduy:~# lspci
00:00.0 Host bridge: Intel Corporation 440FX - 82441FX PMC [Natoma] (rev 02)
00:01.0 ISA bridge: Intel Corporation 82371SB PIIX3 ISA [Natoma/Triton II]
00:01.1 IDE interface: Intel Corporation 82371SB PIIX3 IDE [Natoma/Triton II]
00:01.2 USB controller: Intel Corporation 82371SB PIIX3 USB [Natoma/Triton II] (rev 01)
00:01.3 Bridge: Intel Corporation 82371AB/EB/MB PIIX4 ACPI (rev 03)
00:02.0 VGA compatible controller: Device 1234:1111
00:03.0 Ethernet controller: Red Hat, Inc Virtio network device
00:04.0 Communication controller: Red Hat, Inc Virtio console
00:05.0 SCSI storage controller: Red Hat, Inc Virtio block device
00:06.0 Unclassified device [00ff]: Red Hat, Inc Virtio memory balloon
```

#### Làm việc với thiết bị USB

Lệnh `lsusb` chp phép xem thông tin cơ bản về các thiết bị USB được kết nối với hệ thống Linux. 

```
root@laiduy:~# lsusb
Bus 001 Device 002: ID 0627:0001 Adomax Technology Co., Ltd
Bus 001 Device 001: ID 1d6b:0001 Linux Foundation 1.1 root hub
```

### Mô-đun Phần cứng

#### Liệt kê các Mô-đun đã Cài đặt
`lsmod` là 1 tiện ích dòng lệnh hiển thị thông tin về các module kernel đã được tải. 

```
root@laiduy:~# lsmod
Module                  Size  Used by
kvm_intel             212992  0
kvm                   598016  1 kvm_intel
irqbypass              16384  1 kvm
crct10dif_pclmul       16384  0
crc32_pclmul           16384  0
ghash_clmulni_intel    16384  0
pcbc                   16384  0
aesni_intel           188416  0
aes_x86_64             20480  1 aesni_intel
crypto_simd            16384  1 aesni_intel
glue_helper            16384  1 aesni_intel
cryptd                 24576  3 crypto_simd,ghash_clmulni_intel,aesni_intel
joydev                 24576  0
input_leds             16384  0
serio_raw              16384  0
mac_hid                16384  0
qemu_fw_cfg            16384  0
sch_fq_codel           20480  2
ip_tables              28672  0
x_tables               40960  1 ip_tables
autofs4                40960  2
hid_generic            16384  0
usbhid                 49152  0
psmouse               147456  0
hid                   118784  2 usbhid,hid_generic
virtio_blk             20480  2
virtio_net             45056  0
floppy                 77824  0
i2c_piix4              24576  0
pata_acpi              16384  0
```

#### Nhận Thông tin Mô-đun

Xem thêm thông tin về module sử dụng `modinfo`: 

```
root@laiduy:~# modinfo kvm
filename:       /lib/modules/4.15.0-45-generic/kernel/arch/x86/kvm/kvm.ko
license:        GPL
author:         Qumranet
srcversion:     8EBD075A1652D2BABA0BBEC
depends:        irqbypass
retpoline:      Y
intree:         Y
name:           kvm
vermagic:       4.15.0-45-generic SMP mod_unload
signat:         PKCS#7
signer:
sig_key:
sig_hashalgo:   md4
parm:           ignore_msrs:bool
parm:           report_ignored_msrs:bool
parm:           min_timer_period_us:uint
parm:           kvmclock_periodic_sync:bool
parm:           tsc_tolerance_ppm:uint
parm:           lapic_timer_advance_ns:uint
parm:           vector_hashing:bool
parm:           halt_poll_ns:uint
parm:           halt_poll_ns_grow:uint
parm:           halt_poll_ns_shrink:uint
```

#### Cài đặt Mô-đun mới

Để cài đặt 1 module mới, bạn có thể sử dụng 2 lệnh là: `insmod` và `modprobe`. 

#### Xóa Mô-đun

Để xóa 1 module, ta có thể sử dụng lệnh `modprobe` với tùy chọn `-r`. 