Index:
[toc]

# FHRP是什麼
First Hop Redundancy Protocols，中文譯做第一跳備份協定，一般上只要路由器設定完成後只要在電腦或是DHCP上面設定完成Default Gateway的資訊後便可以連上網際網路
但是如果直接設定Default Gateway會有一個問題：如果Gateway掛掉之後後面的所有終端設備都無法訪問網際網路
最初產生在RFC 1256中定義了IRDP，但IRDP本質上比較像是Gateway壞掉後透過ICMP收發確認是否有其他Gateway

# FHRP運作
FHRP如果要實現除了要準備2個以上的Router以外，一個網段也要用掉n+1個IP位址數量，n個路由器和1個給終端設備用的Default Gateway位址
FHRP設定成功後，會使用群組內設定的Virtual Gateway IP提供給終端設備之外，也會同時產生一個虛擬的實體位址

# FHRP類型
FHRP說白了，其實並不是一個協定，更偏向一個概念
因此以下表格說明了現今主流的FHRP協定:
|               FHRP Options                | 描述 ｜                                                                                          |
| :---------------------------------------: | :----------------------------------------------------------------------------------------------- |
|    Hot Standby Router Protocol (HSRP)     | Cisco專有協定，同時也是netacad網院中重點介紹的FHRP                                               |
| Virtual Router Redundancy Protocol (VRRP) | 現在的公有FHRP協定，目前主要使用的有v2和v3，其中v3同時支援IPv4和v6，並且支援跨廠商以及更具延展性 |
|  Gateway Load balancing Protocol (GLBP)   | Cisco專有協定，可以視為有附載平衡HSRP                                                            |

# HSRP介紹與實作
