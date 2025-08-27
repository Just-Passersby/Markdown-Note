# docker概念

# Container(容器)介紹
一種Sandbox程序，隔離host(主機)的其他程序環境。在Linux中使用Kernel namespace和cgroup，Docker是簡化和增加功能行的套件。特點：
- 可執行image(鏡像)，透過可透過Docker操作
- 本地、雲端機器皆可運行
- 可以帶到不同環境執行
- 與其他容器隔離環境(包括執行檔、設定等)

同時，Container也可被稱為強化版`chroot`，比`chroot`有著更強的隔離能力

# 什麼是image
image是給容器運行隔離開的檔案系統。除了檔案系統還必須包含需要的二進制檔案、依賴項、腳本等，除此之外還可以透過其他設定，指定容器開啟時執行的指令、環境變數和metadata等東西。

有了image的加入，讓Container看起來很像小型系統，但它本身不是虛擬化，依賴的Kernel功能不一樣，請不要跟虛擬化混唯一談！

# Reference
[Docker Docs - Containerize an application](https://docs.docker.com/get-started/workshop/)