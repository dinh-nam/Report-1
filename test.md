# Tổng quan về Linux
## Xem dung lượng Disk
1. Xem bằng ứng dụng mặc định trong ubuntu là “Disk Usage Analyzer” hoặc “Disks”
![Disk Usage Analyzer](images/disk.png)
1. Xem bằng GUI Tools của bên thứ 3
1. Dùng lệnh df (có thể kèm tham số -h cho các phần có thể đọc được)
1. Hiển thị bao gồm các thông tin chi tiết về dung lượng disk còn trống, dung lượng đang sử dụng, thông tin về disk đang chạy, nhiệt độ,…
## Xem phân vùng Disk
1. xem bằng GUI tools là Gparted
![Gparted](images/Gparted.png)
1. Dùng lệnh cfdisk hoặc fdisk (chạy ở quyền root)

Trong đó: 
* cfdisk cho phép list tạo xóa và resize phân vùng
* fidsk cho phép list ,tạo và xóa phân vùng (--help với các option khác nhau cho việc tương tác với các phân vùng)
```
$ sudo fdisk --help
```
![fdisk](images/fdisk.png)

## Xem thông tin phần cứng
_**Toàn bộ thông tin được dùng bằng command line**_
1. lscpu: cung cấp thông tin về cpu và các đơn vị xử lí liên quan
    ![lscpu](images/lscpu.png)
1. lspci: cung cấp thông tin các bus PCI và các linh kiện bên ngoài kết nối với chúng

    ![lspci](images/lspci.png)
1. lshw: cung cấp thông tin chi tiết, ngắn gọn về mọi linh kiện trong máy từ motherboard, cpu, RAM,… (với lệnh `lshw -short` sẽ hiển thị đơn giản hơn)

    ![lshw -short](images/lshw%20-short.png)
1. lsscsi: liệt kê các ổ disk đang sử dụng
1. lsusb: liệt kê các usb controller, hiển thị chi tiết về thông tin chúng kết nối với máy, mặc định sẽ ngắn gọn, để có nhiều thông tin chi tiết hơn dùng `lsusb -v`
1. kiểm tra lượng ram trên máy dùng lệnh `free`, để dễ hiểu hơn sử dụng lệnh `free -m`
* `ls option`: tổng hợp các option phổ biến khi dùng `ls`
![](images/ls%20option.png)
## Theo dõi chi tiết tiến trình
_**Cần thực hiện trên quyền root**_

* `$ sudo ps -a`: hiển thị mọi tiến trình có trên máy, kèm PID, thời gian thực thiện và lệnh thực hiện 
* `$ sudo ps -sa`: hiển thị ID tiến trình, ID unix của user, trạng trái thực thi, thời gian, lệnh thực hiện
* `$ sudo ps -u`: hiển thị user, ID tiến trình, % hệ thống hoạt động, trạng thái, thời gian và lện thực hiện
## Liệt kê danh sách file/folder

Dùng lệnh `ls` để liệt kê các file hiện có tại vị trí trỏ hiện tại cùng các option có điều kiện:

`$ ls [options] [file|dir]` để tìm ở 1 file được chỉ định trước

![ls option](images/ls_option.png)

## Tương tác với file/folder
* Sao chép: lệnh `cp` để sao chép file/ folder mới được chỉ định trước vào vị trí mới 

![copy file](images/copy.png)
syntax: `cp -R <source_folder> <destination_folder>`
* Với trường hợp sao chép vào phân vùng backup hay remotely host server dùng 2 lệnh sau:(yêu cầu thực hiện sudo apt-get install rsync )
```
rsync -ar <source folder/file> <destination_user>@<ipserver>:<destination folder/file/>
``` 
```
scp -r <source_folder> <destination_user>@<destination_host>:<ip server>
```
* Di chuyển: lệnh “mv” để di chuyển file/folder được chỉ định tới vị trí mới

![123](images/move.png)

```
mv [option] <source_file> </destination_folder>
```
Để đổi tên file dùng lệnh: `mv [option] <source> <destination>`

Có thể di chuyển từng file, nhiều file hoặc tất cả file có cùng định dạng 

    -i :để xác nhận ghi đè hay không
    -f :để tắt thông báo xác nhận ghi đè (trường hợp thực hiện nhiều file liên tục)
    -n :để chặn ghi đè file đã tồn tại
    -b :để sao lưu file đã có trước đó
    -v :để hiện thông báo tiến trình di chuyển từng file

* Tìm: dùng lệnh “find” kèm với đường dẫn vào file cần tìm để ra kết quả chính xác nhất bằng đệ quy

```
find /path/ -type f -name file-to-search
```
    /path: nơi bắt đầu tìm
    -type [option]: định dạng file cần tìm 
    (f-regular file, d-directory, b-block, l-symbolic link, c-character device)
    -name: tên file cần tìm 
Có thể kết hợp lệnh `mv` và `rm`(remove) để lọc file tốt hơn

Ngoài ra lệnh `locate` cũng được dùng tương đương lệnh `find`, nhưng thời gian nhanh hơn do dùng cơ sở dữ liệu xây dựng từ trước, trong khi `find` quét hệ thống theo thời gian thực 

![find](images/find.png)
## Phân quyền cơ bản và phân quyền nâng cao

```
$ sudo ls -l <file>
```
cấu trúc của 1 phân quyền file bao gồm :

![](images/file-permission.jpg)

1. file type: trong đó
    1. "-" regular file
    1. "d" directory
    1. "i" link
1. user: người sở hữu
1. group: nhóm user
1. others: bất kì đối tượng không thuộc nhóm và không phải user sỡ hữu file
- 'r' : read giá trị = 4
- 'w' : write giá trị = 2
- 'x' : execute giá trị = 1
- '-' : deny giá trị = 0

![](images/example.png)

Giá trị quyền lúc này của file task 1.odt
|  file type |   user   |    group  | others |
|------------|----------|-----------|--------|
|     -      |   rw-    |    rw-    |    r-- |
|regular file|  6       |   6       |   4    |

Để thay đổi giá trị phân quyền tiêu chuẩn có 2 cách:
- Chế độ symbolic
- chế độ kí hiệu bát phân (octal nonation)

ví dụ: để thay đổi quyền của file task 1.odt thành tất cả chỉ đọc và chỉ người sở hữu có thể thực thi, sử dụng lệnh (octal nonation)
```
chmod 744 task 1.odt 
```
hoặc `chmod u=rwx,g=r,o=r task 1.odt` (symbolic mode)

![chmod](images/chmod.png)

Để thây đổi phân quyền nâng cao , sử dụng lệnh `chown`, cụ thể có thể thay được group và owner ban đầu (yêu cầu quyền root)

```
$ sudo chown [new-user] [file]
$ sudo chgrp [new-group] [file]
```
![](images/permission.png)

## Mount và Umount
Để kiểm tra phân vùng ổ cứng, dùng lệnh `lsblk` để hiển thị toàn bộ phân vùng đang có trên máy

Có 2 cách mount ổ cứng bằng device file hoặc UUID của ổ cứng đó
- Cách 1: device file
```
$ sudo mount [/dev/disk-name] [/mount-file-name]
```
Là hình thức mount tạm thời, sẽ bị mất sau khi reset hoặc shutdown máy
- Cách 2: mount bằng UUID

Để ổ có thể mount tự động sau khi mở máy, sử dụng UUID của ổ đó để mount
sử dụng `blkid` hoặc list toàn bộ UUID phân vùng ổ cứng như sau:
```
lsblk -o NAME,UUID,SIZE
```
Sau khi lấy được UUID ổ cần tìm, cần chỉnh sửa file "/etc/fstab"
thêm lệnh mount vào cuối file hoặc dùng `echo` thay thế
```
UUID =[uuid của ổ cần mount] [/file-to-mount] ext4 default 0 0
```
Sau đó lưu lại chỉnh sửa file và dùng lệnh ` $ sudo mount -a` để hoàn tất