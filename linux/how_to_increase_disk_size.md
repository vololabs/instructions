# Как увеличить размер партиции на диске

1. Сделай бекап
2. Отмонтируй интересующий тебя раздел `sudo umount /dev/sd{a..z}{1..99}`
3. Заходим в parted: `sudo parted /dev/sd{a..z}`
4. Пишем `resize`, выбираем партицию и размер диска
```
vdm@sharestorage:~$  sudo parted /dev/sdb
GNU Parted 3.2
Using /dev/sdb
Welcome to GNU Parted! Type 'help' to view a list of commands.
(parted) resizepart
Partition number? 1
End?  [322GB]? 370GB
(parted) print
Model: VMware Virtual disk (scsi)
Disk /dev/sdb: 376GB
Sector size (logical/physical): 512B/512B
Partition Table: msdos
Disk Flags:

Number  Start   End    Size   Type     File system  Flags
 1      1049kB  370GB  370GB  primary  ext4

(parted) q

```
5. `sudo e2fsck -f /dev/sdb1`
```
e2fsck 1.44.1 (24-Mar-2018)
Pass 1: Checking inodes, blocks, and sizes
Pass 2: Checking directory structure
Pass 3: Checking directory connectivity
Pass 3A: Optimizing directories
Pass 4: Checking reference counts
Pass 5: Checking group summary information

/dev/sdb1: ***** FILE SYSTEM WAS MODIFIED *****
/dev/sdb1: 297643/19660800 files (1.2% non-contiguous), 55608220/78642944 blocks

```
6.  sudo resize2fs /dev/sdb1
```
vdm@sharestorage:~$ sudo resize2fs /dev/sdb1
resize2fs 1.44.1 (24-Mar-2018)
Resizing the filesystem on /dev/sdb1 to 90331775 (4k) blocks.
The filesystem on /dev/sdb1 is now 90331775 (4k) blocks long.
```
7. `sudo mount /dev/sdb1`
 
 
