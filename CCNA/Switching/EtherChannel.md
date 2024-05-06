Index:
[Toc]

# 什麼是以太通道(EtherChannel)？
以太通道(EtherChannel)這項技術是Cisco獨有的叫法，但他有著更加通用的名字，叫做**鏈路聚合**(**Link Aggregation**)
這2個名字本質上是一樣的功能，前面我們有提到過VLAN為了容錯性IEEE制定了STP用於避免網路迴圈的形成
雖然避免了廣播風暴，但是卻讓整個流量限制住了，消耗了2個Port卻只有1個Port可以傳輸，造成性能浪費
而鏈路聚合/以太通道正是為了用於容錯的Port路線在保有容錯功能時，平時正常的時候可以用於附載平衡

具體的實現方式是將多個介面設定為1個Group，然後透過演算法將附載分配給同個Group的各個介面
那Group內某個介面掛掉並不會導致Group的錯誤，只會造成網路吞吐量降低

至於為何Cisco會自己又起一個**EtherChannel**這個名稱呢？當然是Cisco針對該功能也有獨家技術

# EtherChannel的限制
功能上聽起來很棒，但要注意有些限制
 - 介面不能混用，比如不能將1 Gbps和100 Mbps傳輸速率的介面設為同一群組
 - 單條鏈路不能同時拆分跟多個設備連線或多個Port，除非對面也是1個Group，只能1群對1群
 - 兩邊Port必須均是Switch Port
 - 兩邊的Port必須同時處於Trunk模式或是相同VLAN
 - 2邊的協定必須相同(PAgP、LACP、static)

# 協定名稱以及實現
前面有提到Cisco有自己的專有協定，除此之外也有公有協定，分別為以下2者：

## PAgP
PAgP是Cisco專有的聚合協定，而Cisco將這個動作、這項技術稱之為**以太通道**(**EtherChannel**)

PAgP有以下兩種模式:
 - Desirable
 - auto

Desirable狀態下的Group會主動與另一邊的Group進行協商並建立EtherChannel。
而auto則是會等待對面主動協商建立EtherChannel
因此，建立EtherChannel時候，除了協定相同外，必須至少有一方是Desirable才能成功建立EtherChannel

## LACP
LACP就是由IEEE定義於**802.3ad**的協定，而大家將該動作、技術稱之為**鏈路聚合**(**Link Aggregation**)

以Cisco Switch在設定狀態時為例，LACP有以下兩種模式:
 - active
 - passive

active模式的Group同PAgP的Desirable一樣，會主動協商並建立鏈路聚合a
而passive則同PAgP的auto一樣，等待對方協商建立鏈路聚合
因此在建立LACP的鏈路聚合時，必須至少有一方是active才可以成功建立鏈路聚合

# 建立EtherChannel
基本上用於建立EtherChannel的Port會使用連續號碼(比如Fa0/23-24)，因為可以直接使用`(config)# int range ...`來直接建立整個Port-Group
建立EtherChannel的過程如下：
1. 關閉兩邊介面
   `(config-if-range)# shutdown`
2. 建立Channel-Group
   指令為`(config-if-range)# channel-group 編號 mode 模式`
   模式的部分可以回去參考[協定名稱以及實現](#協定名稱以及實現)
3. 開啟兩邊介面
   `(config-if-range)# no shutdown`

這樣就成功建立EtherChannel了，每個Port-Group都被視為一個邏輯介面，因此可以用`(config)# int port-channel N`進入對應編號的Port-Group

# EtherChannel附載平衡與檢查
EtherChannel建立之後，要檢查EtherChannel的附載平衡模式可以用`# show etherchannel load-blance`檢查平衡模式
