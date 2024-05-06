Index:
[toc]

# 甚麼是VLAN?
VLAN在概念上指的是透過Switch將單個**廣播**領域切成多個
比如一層樓同時有IT、銷售和主管部門，1台(企業級)常規的Switch通常有24個Port，而3個部門人數/崗位加起來也用不到24個Port，那這種情況每個部門放一個交換機是否很浪費
因此VLAN可以透過適當的規劃從1個LAN變切成多個LAN，因為這些多個LAN是用Switch切出來出來的，因此得名Virtual(虛擬)LAN
從效能的角度，適當的切割VLAN可以避免不必要的廣播封包傳入不須接收的主機
從資安的角度，可以避免廣播封包發送到黑客主機竊聽，或者一個部門有人的電腦中毒後避免透過廣播封包感染至其他部門
最後透過切出來的VLAN，再適當的規劃IP網段，一個標準的網路環境就此打下了良好基礎

(不要妄想用一個L3 Switch建一堆SVI跑Routing和切網段)
(我不希望在A教室看到別人能夠隨意地用B教室的IP位址)

# VLAN的類型與介紹

以Cisco Switch為例，VLAN可分為以下類型

- Data VLAN
  通常網管人員在設定的VLAN都是這種資料VLAN，用於傳輸各個終端設備的數據，其中又有幾個特殊用途的Data VLAN，常見的是以下3種
  - Default VLAN
  VLAN 1，預設的原生(Native)VLAN和所有Switch Port所在的VLAN位置，SVI則是預設關閉狀態，不可刪除
  - Native VLAN
  Switch中用於傳輸VLAN封包的VLAN
  若2台Switch的Native VLAN不同網路線接上後會在終端機上顯示Native VLAN不同的錯誤訊息，並且無法通訊，直到2台的Native VLAN相同
  - Management VLAN
  用於Cisco設備上設定網路服務（例如SSH、SNMP、HTTP等）的VLAN
- Voice VLAN
  Cisco有出過一種設備叫做VoIP電話，Voice VLAN就是專門切給這種設備使用

# VLAN協定

現今的VLAN封包定義於802.1q和802.1p，這2個就是定義VLAN封包的協定，前者是VLAN封包標準，後者則是VLAN封包的擴充標準，2個協定會共同運作與共用訊框
那Cisco還有出過一種專門的VLAN協定叫做ISL，由於802.1q的協定普及加上ISL的專有性導致ISL隨著時間被淘汰
而部分設備同時支援ISL和802.1q，這時要設定VLAN就需要指定使用的協定，在Switch中要設定專門傳輸VLAN封包(Cisco稱之為Trunk)的Port輸入以下指令:
```
(config-if)# switchport trunk encapsulation dot1q
```
這樣子就會將Auto模式改為只使用802.1q建立Trunk

# VLAN的一些簡單實作
VLAN的指定是透過在指定的Port指定VLAN
舉個例子: Fa 0/1設為VLAN10
實作:
```
(config)# int fa 0/1
(config-if)# switchport access vlan 編號
```
這樣子便指配完成，若指配的VLAN編號沒有在Switch的VLAN資料庫中會自動建立，並且SVI也會自動開啟
若是要從Fa 0/1變成從Fa 0/1到Fa 0/10都要指定為VLAN 10呢?
很簡單，只要將`(config)#int fa 0/1`改成`(config)# int range fa0/1-10`就好，剩下的指令也一樣(int range好用，可以批量設定Port狀態)
依照網路拓譜在適當的Port分發指定的VLAN建完成對終端的VLAN切割

# Trunk模式
前面有提到Cisco設備傳輸帶有VLAN Tag的訊框的switch port模式稱為Trunk，而每家對於這種專門傳輸VLAN封包的switch port模式名稱都不一樣，像D Link就叫做Tag Port
那麼在[VLAN協定](#vlan協定)有提到若Cisco Switch同時支援ISL和802.1q該如何設定才能避免Trunk無法設定，這裡不再贅述
Cisco的Switch要設定Trunk很簡單，只要在要開啟Trunk模式的介面輸入`(config-if)# switchport mode trunk`就可以開啟Trunk模式
那設定Trunk模式的Switch Port就會依照編號每次發送不同VLAN編號的帶有VLAN Tag的訊框。
一般的電腦若連Trunk Port是否可以攔截一大堆不同VLAN的資料或者參與通訊?
可以在開啟Trunk的端口進行訊框交換，但是攔截封包不太行
造成不能不能攔截封包的原因是帶有802.1q的訊框會在原始訊框的Source MAC和Type中間插入一個Tag，整個標準Ethernet訊框最大只能到1518 Bytes，而加入Tag之後的訊框會額外加入2 Byte來到1520 Bytes，因此不符合標準的Ethernet封包大小而被電腦丟棄。
在[VLAN的類型與介紹](#vlan的類型與介紹)有提到用於專門傳輸帶有VLAN Tag訊框的VLAN - Native VLAN
前文提過Native VLAN不同會導致Trunk無法順利建立甚至連一般的Switch交換都會無法達成，那Native VLAN怎麼設?
只要進入到任意的Port裡面，輸入以下指令即可完成對Native VLAN的更改
```
(config-if)# switchport trunk native vlan 編號
```
如此一來就可以完成對Native VLAN的修改，那修改Native VLAN有甚麼作用?
首先，在網路的效能上可以得到優化，指定一個任何部門或者場域不會用到的VLAN編號可以避免這些帶有VLAN Tag的封包進入這些地方造成流量與性能上的損失。
第二個便是安全性，早期有種針對VLAN的攻擊叫做**Double Tag attack**，那這種攻擊以Cisco的設備來說基本上比較現代的設備或者有在持續更新的話基本不至於會被實現
不過還是有這2種攻擊可以被實施: 基於STP和基於DTP的攻擊
這2種攻擊基本上是基於這2者協定的特性從而產生的攻擊，而修改Native VLAN(並且攻擊者不知道Native VLAN的編號)會因為攻擊者與被攻擊者因Miss match Native VLAN從而導致攻擊失敗

# Dynamic Trunk Protocol
前面有提到未經過適當設定的Native VLAN可以透過DTP攻擊，那甚麼是DTP?
DTP是Cisco獨有的協定，這個協定可以讓Cisco的Switch連接之後自動將連線改成Trunk模式，需要注意的是若互相連接的Switch有一方不是Cisco那需要將該介面的DTP關掉

若想避免Switch被利用DTP特性的攻擊，除了前面所說修改Native VLAN之外，也可以將DTP關閉(只是Trunk都要手動開)

# Spanning-Tree Protocol
前面有提到利用STP特性來實施的攻擊，STP又是甚麼?
一個成熟的網路拓譜，會規劃重複的路線，這種路線的目的是為了保證網路的**容錯**性，避免設備出現問題從而導致斷線
然而Switch的特性，這種路線若規畫在Switch上面會導致網路迴圈，產生網路迴圈會讓封包一直在Switch之間互相轉送甚至因此不斷發送廣播封包，廣播風暴就此形成，所以這也直接造成網路性能的大幅下降甚至斷線，而STP就是為此誕生
STP最初修訂於802.1d，STP的出現讓第二層的網路也有容錯的能力，當Switch上的Port連線狀態改變時，就會發送BPDU(Bridge Protocol Data Units)，若收到的對方也是Switch就會互相交會BPDU，開始跑以下4個步驟防止網路迴圈的形成

1. **選舉Root bridge**
   首先所有戶相連接的Switch會透過BPDU的資訊來選舉出一個Root Bridge，而BPDU的資訊包含以下
    1. Bridge priority
    優先權的算法是VLAN + Priority，為什麼要加上VLAN，是因為Cisco Switch的STP是改良版的**PVST+**，在稍後會有更詳細的介紹
    Priority值越低，成為Root Bridge的機率越大，那沒設定的話預設都是327968，以VLAN1為例，各個Switch VLAN 1的Priority都是32768+1=32769
    Switch設定Priority的指令如下:
    ```
    (config)# spanning-tree vlan 編號 priority 優先權
    ```
    需要注意的是，Priority在設定時的數值必須是4096的倍數

    2. Switch MAC Address
   若各個Switch的優先權都一樣就會看各個Switch的MAC位址，由MAC位址最小的Switch來擔任Root bridge

   在選舉Root bridge時，建議由最穩定的Switch擔任Root bridge
   再次提醒，只要有新的Switch對Switch連線都有可能**重新選舉**Root bridge，因此這一步很重要
2. **選舉Root port**
   其他沒有成為Root bridge的Switch會選出一個Port連接root bridge，那這個Port稱為**root port**
   選舉Root port的過程會計算各個Port到Root bridge的**成本**，Port流量越大，成本越低，經過N台Switch，就是N個Port的成本
   成本參考:
   | 頻寬 | STP  | RSTP      |
   | :--- | :--- | :-------- |
   | 10G  | 2    | 2,000     |
   | 1G   | 4    | 20,000    |
   | 100M | 19   | 200,000   |
   | 10M  | 100  | 2,000,000 |

3. **選舉designated(委任) Port**
   Root Port到Root Bridge的路線中會經過的Port都叫Designated Port

4. **封鎖連接其他Switch的non-designated Port**
   STP的Link Port status非Root port的都將變為block(封鎖)，若是Designated Port或者連接非Switch的Port將會變成forwarding狀態，此時從第1步到這一步以預設設定來說大約花了30秒

因此基本上第一步相關的設定非常重要，若沒良好的設定將會造成網路的不穩定，畢竟從第1步到最後一步都要花了30秒，沒有新的Root bridge上任還好，若只要有個Switch接進來Root bridge就一直換那整個網路就又要花30秒來重新計算路線
即便新的連線不是Switch設備，也是需要花30秒跑完整個STP的狀態改變Port status
也因此，利用STP特性的攻擊方式就出來了
基於STP特性的攻擊實現方式便是透過提高自身優先權以及保證自己的MAC位址較小的情況下惡意的反覆改變連線狀態從而實現網路的癱瘓
參考下面的拓譜，假設旁邊的IE2000是攻擊者的Switch，他要接到整個拓譜右邊的Switch來實現攻擊
![Topology](../Pictures/2023-06-28%20191400.png)
假設最上面的Switch是Root Bridge，優先權為4096，而IE2000的優先權是0，IE2000接上後反覆改變Port的狀態(比如反覆插拔網路線或是反覆shutdown又no shutdown)將會導致IE2000反覆成為Bridge，整個拓譜要一直反覆重新計算路線，每次重新計算都要花上30秒才能讓整個網路順利通訊
重點，**反覆搶奪Root Bridge**，便是這一點造成網路癱瘓，因為一旦Root Bridge的角色改變，所有的Switch都要重新選舉Root Port和Designated port，這個過程會反覆改變連結狀態，從Listening到成為forwarding狀態(預設)都要花30秒改變

先說說如何防禦，除了前面提到避免使用預設的Native VLAN以及盡可能的保證Root Bridge有著較高的優先級之外，便是針對沒有要連接Switch的Port封鎖不必要的Switch連線
那麼防護方式如下:
- Active(主動/積極): BPDU Guard
  開啟方式:
  ```
  (config-if)# spanning-tree bpduguard enable
  ```
  開啟這種防護方式的Port一旦接收到BPDU就會立刻err-disable(若要重新enable需要手動shutdown再no shutdown)
- Passive(被動/消極): Guard Root
  開啟方式:
  ```
  (config-if)# spanning-tree guard root
  ```
  開啟這種防護方式的Port允許其他Switch互相連接以及參與STP運作，但是一旦連接該Port的Switch連接後參與選舉後變成了Root Bridge那該Port就會err-disable

※補充，若確定該Port只會連接電腦可以使用以下指令讓連結狀態永遠處於Forwarding
```
(config-if)# spannging-tree port-fast
```

那麼前面提到因為STP的運作Switch一旦有新的連線都會是Listen連接狀態，基於STP特性的攻擊便是一職改變Root Birdge，讓整個網路重新計算連接狀態(都會先變回Listening)，那甚麼是連接狀態?
STP的出現，讓Switch不只出現了開啟與關閉狀態，而是近一步將Switch的連線狀態劃分成了以下表格的狀態
| Port status | 說明                                                                                              |
| :---------: | :------------------------------------------------------------------------------------------------ |
|    block    | 不參與訊框交換，也不發送BPDU<div>一旦Forwarding狀態的Port沒有持續接收BPDU那麼20秒後便會變為該狀態 |
|  Listening  | 一旦網路線接通後，將會發送BPDU，通知鄰居自己要參與網路通訊，並且開始取得Root bridge資訊           |
|  Learning   | Listen完成後會持續與整個拓譜交換BPDU來學習路徑以及更新MAC table，但此時還不會轉送使用者的訊框     |
| Forwarding  | 成為Forwarding後，將被視為運作中的拓譜，這狀態下會轉送使用者訊框並且與整個拓譜的Switch交換BPDU    |
|  Disabled   | 被關閉的Port，不是管理員手動shutdown就是觸發保護機制變成err-disable，不運作                       |

預設情況下Listening與Learning各需花費15秒鐘，也就是每個連線或是Root Bridge改變都要連接30秒後才能開始正常的訊框交換，前30秒都算是離線狀態，這對於日漸龐大的網路來說逐漸不敷使用，因此Cisco早期更是憑藉著PVST+在Switch的領域也佔據了一席之地<del>(如果公司夠有錢首選Cisco)</del>，PVST+的特性造就高性能的網路拓譜，各大廠商也只能敲碗改良STP，因此IEEE之後修訂了802.1w，該協定又稱之為RSTP(Rapid Spanning-Tree Protocol)
接下來下表統整各個STP的特性:
|   STP變種   | 說明                                                                                                                                                                                     |
| :---------: | :--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
|     STP     | 修訂於802.1d，最原始的STP，目的為避免形成網路迴圈                                                                                                                                        |
|    PVST+    | 全稱為Pre Vlan Spanning-Tree+，雖然STP解決了網路迴圈的問題，但有著效率不佳、無安全性、無法附載平衡等問題<div>因此PVST+最主要目的正是解決該問題獨家改良版STP(PVST運作於ISL，因此已被淘汰) |
|    RSTP     | 修訂於802.1w，可視為收斂時間大幅縮短版STP，支援向下兼容                                                                                                                                  |
| Radip PVST+ | 基於802.1w的PVST+，有著RSTP以及PVST+的特性                                                                                                                                               |
| 802.1d-2004 | 針對802.1d與802.1w的協定更新                                                                                                                                                             |

※若需要進一步了解RSTP以及如何在Cisco的Switch上運作，可以參考[Cisco的這篇文章](https://www.cisco.com/c/zh_tw/support/docs/lan-switching/spanning-tree-protocol/24062-146.html)

# External
[Jan Ho - Virtual LAN](https://www.jannet.hk/virtual-lan-vlan-zh-hant/)