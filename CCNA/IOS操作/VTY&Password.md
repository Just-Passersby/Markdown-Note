# 設定密碼與須知
語法: `(config)# enable secret 密碼`
雖然secret可以替換成`password`，但這麼做會導致密碼是以明文儲存，一旦config檔洩漏就可以取得設備密碼

# Tenlet
Telnet要開啟必須按照以下步驟
 1. 設定enable密碼(不然無法進入enable模式)
 2. `(config)# line vty 0 15`(進入所有VTY)
 3. 設定login密碼(`(config-line)# password 密碼`)或者設定使用者驗證(`(config)# username 使用者 secret 密碼`)
 4. 對終端機login(`(config-line)# login`)或是login local(`(config-line)# login local`)
 5. 設定完成終端機密碼或是使用者驗證後進行密碼加密(`(config)# service password-encryption`)

上述步驟跑完Telnet就會被打開了(沒有密碼是禁止Telnet的)

# login還是login local
在設定VTY(狀態為`(config-line)#`)時login指令可以直接`login`或`login local`，這2者有何區別?
`login`指的是在Telnet/VTY密碼設定完成後將其套用在終端機上面
而`login local`指的是透過使用者驗證登入終端機，並且同時如果原本使用VTY登入也會被關閉(就是只能2選1)
必須注意的是，`login local`即使沒有使用者也不會報錯，但會變成無法用VTY密碼登入(若沒再次`login`的話)

# SSH
SSH相比Telnet是相當安全的連線方式，特別是若金鑰長度夠長會難以破解
當然，設定SSH需要更多的精力與步驟
那在設定之前可以使用`# show ver`來確認設備是否支持SSH，只要看**K9**字樣表示可以
1. 設定Hostname(`(config)# hostname 主機名稱`)
2. 設定Domain name(`(config)# ip domain-name 網域名稱`)
3. 設定使用者(`(config)# username 使用者 secret 密碼`)
4. 設定非對稱金鑰對(`(config)# crypto key generate rsa `)
   - RSA金鑰對長度依照NIST的建議至少2048 bit
5. login local(`(config)# line vty 0 15` → `(config-line)# login local`)
6. (建議)強制使用SSH連線(`(config-line)# transport input ssh`)
7. (建議)使用SSH 2作為連線版本(`(config)# ip ssh version 2`)

SSH連線強制使用使用者驗證，因此不能使用VTY密碼驗證
Packet Tracer有個Bug，就是開啟SSH 2之後在顯示config時依舊顯示使用SSH 1.99
SSH版本1.99和2的區別除了版本號的不同(廢話)，更重要的是連線方式的不同，SSH 2有著更為先進的交握方式，SSH 1.99的存在更主要只是為了兼容老設備
