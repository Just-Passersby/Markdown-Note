# DHCP概觀
在現今的網路部署基本上都會有DHCP Server，用於配發內部網路主機IP位址，而Cisco也可以在Router上面設定DHCP Server和DHCP relay。

# DHCP Server設定
在設定DHCP Server之前，請先確保該Router已經擁有一個固定IP

擁有固定IP之後，依照以下步驟建立DHCP服務

1. 排除(excluded)
   先將沒有要用於自動配發的IP（比如Default Gateway）的這些IP排除
   指令為`(config)# ip dhcp excluded-address 起始位址 [結束位址]`
   若要單一位址就只要給起始位址的參數，可多次輸入，直到要排除的位址都已排除

2. 定義與設定集區(pool)
   設定DHCP時配發IP位址的Server都會針對要配發的位置設定一個「集區(Pool)」，因此要要先建立集區
   建立和進入集區使用`(config)# ip dhcp pool 集區名稱`
   進入集區之後依據需求輸入下表指令（以下只收入常用到的指令）
   |                  Task                  | Command                                                        |
   | :------------------------------------: | :------------------------------------------------------------- |
   |    Publish subnet of address (Must)    | **network** *Network-Number* [ *mask* \| ( */prefix-Length* )] |
   | Default Gateway Address (Must Optinal) | **default-rotuer** *address* [ *address2*...*address8*]        |
   |   DNS Server address (Must Optinal)    | **dns-server** *address* [ *address2*...*address8*]            |
   |          Default Domain Name           | **domain-name** *domain*                                       |

3. 驗證集區運作
   驗證運作狀況的指令參照以下表格：
   |               Command               | Comment                                               |
   | :---------------------------------: | :---------------------------------------------------- |
   | show running-config \| section dhcp | 顯示路由器上DHCPv4資訊                                |
   |        show ip dhcp binding         | 顯示IPv4位址的清單，由 DHCPv4 服務                    |
   |   show ip dhcp server statistics    | 顯示有關 DHCPv4 訊息計算資訊的數目是 已經發送和接收。 |

經由上述步驟設定完成後Router會根據介面來發放位址

# DHCP Relay(Relay Agent)
若出於某些原因，需要將DHCP Server放在不同的網段運作，就會需要中間的Router設定DHCP Relay
Cisco Router啟用DHCP Relay很簡單，只需要先輸入`(config)# ip dhcp relay enable`後輸入`(config)# ip dhcp relay DHCP伺服器`，最多支援Relay 8個Server

# DHCP Relay(helper-address)
Cisco還有一種方法是在會需要負責中繼的「所有介面」上輸入`(config-if)# ip helper-address DHCP伺服器`來達到一樣的效果

# External
[透過指令行界面 (CLI) 在交換器上設定動態主機組態通訊協定 (DHCP) 中繼設定](https://www.cisco.com/c/zh_tw/support/docs/smb/switches/cisco-small-business-300-series-managed-switches/smb5567-configure-dynamic-host-configuration-protocol-dhcp-relay-set.html)
