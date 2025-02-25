# SSH金鑰驗證

[toc]

# SSH設定
對於要使用金鑰驗證的使用者，要先產生對應使用者的金鑰對，使用`ssh-keygen`即可生成金鑰對

在生成時會問你金鑰對要設什麼密碼，如果不設定直接Enter跳過就行

生成完成後就會在家目錄底下的`.ssh/`生成`id_rsa`和`id_rsa.pub`，兩者分別是**私鑰**和**公鑰**

生成完成後就可以去設定`sshd_config`，把`AuthorizedkeysFile`後面的$PATH改成`~/.ssh/id_rsa.pub`，像下圖這樣![](../Pictures/ssh-auth-key-path.png)

設定完成後儲存並重起SSH即可

# SSH金鑰驗證登入
設定完成後就可以試試看用金鑰驗證登入了，這裡有兩種方法可以完成

## 上傳公鑰
這裡的上傳公鑰指的是用於特定機器的特定使用者登入SSH的特定使用者時，該使用者的公鑰在該SSH使用者公鑰內，所以可以直接登入

你說看不懂？我懂，所以看以下範例

這裡有Alice@client和Bob@server

如果希望Alice@client在使用SSH連線至Bob@server時可以直接透過公鑰登入，Alice@client也執行前面步驟把金鑰對生出來後，把id_rsa.pub上傳至Bob@server，然後Bob@server再把該公鑰也加入到自己的id_rsa.pub(可以使用`cat alice@client_key.pub >> ~/.ssh/id_rsa.pub`)

這樣以用Alice@client透過SSH連線至Bob@server只要沒有強制使用密碼搭入都可以直接登入Bob@server

Windows的SSH key格式與OpenSSH的key格式不一樣，所以不能直接互通，如果找到Windows to Linux的方法，請記得通知我，感激不盡

## 取得私鑰
上傳公鑰的方法有侷限性，很適合1by1的方法，但如果你希望可以沒有電腦與使用者限制的登入，就是透過「私鑰」登入

方法非常簡單，把你希望存取的使用者的私鑰下載至你的電腦，然後在使用SSH連線時使用`-i id_rsa`參數引入對應user@server的私鑰就可以成功透過私鑰驗證登入，指令範例：
```bash
ssh user@server_IP -i path/id_rsa
```
