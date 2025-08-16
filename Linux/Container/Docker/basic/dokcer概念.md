# docker概念

# Container(容器)介紹
一種Sandbox程序，隔離host(主機)的其他程序環境。在Linux中使用Kernel namespace和cgroup，Docker是簡化和增加功能行的套件。特點：
- 可執行image(鏡像)，透過可透過Docker操作
- 本地、雲端機器皆可運行
- 可以帶到不同環境執行
- 與其他容器隔離環境(包括執行檔、設定等)

同時，Container也可被稱為強化版`chroot`，比`chroot`有著更強的隔離能力
