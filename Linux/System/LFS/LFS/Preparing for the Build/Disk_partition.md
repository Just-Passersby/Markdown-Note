# Disk Partition

LFS因為也是一個精簡的Linux系統，所以需要一個獨立分區

如果你建立系統的時候你不小心把硬碟都分配給了System，除了新增硬碟以外，如果你是使用LVM分區，可以透過縮小檔案系統和分區來實現空出硬碟空間給LFS

實作環境：Proxmox VE 8.4.14上使用Ubuntu Server 24.04

## Part I - Check Disk and Volume info
- 檢查邏輯卷(Logical Volume):
  ```bash
  sudo lvs
  ```

- 檢查Volume Group狀態：
  ```bash
  sudo vgs
  ```

- 檢查實體分區：
  ```bash
  sudo pvs
  ```

- file system usage
  ```bash
  df -h
  ```

## Part II - LVM Resize
Ubuntu Server如果要重新調整主系統(根目錄`/`)分區大小，需要透過Live CD處理

Ubuntu Server切換到Live CD的方法：
1. 選到右上角的help
2. 選擇"Enter shell"
3. 檢查檔案系統
   ```bash
   sudo e2fsck -f /dev/mapper/LVM-NAME
   ```
4. 縮小File System (以縮小成200G為例)
   ```bash
   sudo resize2fs /dev/mapper/LVM-NAME 200G
   ```
   這樣會將你的檔案系統縮小
5. 縮小邏輯卷，可以跟File System一樣大或者更大，不可更小 (以200G為例)
   ```bash
   sudo lvreduce -L 200G /dev/mapper/LVM-NAME
   ```
   > `resize2fs`不能用`-50G`，而`lvreduce`可以用`-L -50G`這種參數來控制大小，為了統一我都使用直接指定，不是用減法

## Part III - Create LVM and Partition for LVM
Ubuntu Server預設的Volume Group名稱是`ubuntu-vg`，以預設名稱為例，建立新的LVM，我稱為`lfs-lv`：
```bash
sudo lvcreate -n lfs-lv -L 50G ubuntu-vg
```
之後格式化即可(這裡選用ext4)
```bash
sudo mkfs -v -t ext4 /dev/mapper/ubuntu--vg-lfs--lv
```

