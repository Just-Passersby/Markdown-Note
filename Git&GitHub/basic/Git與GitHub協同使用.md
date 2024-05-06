# Git與GitHub協同使用

index:
[toc]

# 建立新專案
如果今天需要新建專案在GitHub頁面的右上角有個`+`號![](../Pictures/New_repository.png)
這時候輸入Repository的名稱不要跟你原本GitHub原有的Repository的撞名，然後使用ASCII所支援的字體![](../Pictures/New_repository_setting.png)
如果你有打算開源你的Source Code，需要注意License的選擇，不同的License在使用上有所區別

當你建立完成後，可以看到像是下圖的引導畫面![Quick setup](../Pictures/Repository_setup.png)
版本控制的操作與[透過git控制檔案版本.md](透過git控制檔案版本.md)一致，忘了可以回去看

當讓Git控制完成版本後由於此時檔案都還是在本機，所以需要將檔案push到Git Server，首先設定一個伺服器節點
```bash
REMOTE_PATH=https://github.com/Just-Passersby/Markdown-Note.git
git remote add origin $REMOTE_PATH
```
> 說明：裡面的`$REMOTE_PATH`在GitHub提供HTTPS和SSH，哪種傳輸方式差別不大，但注意若使用SSH的格式需要先設定好SSH key

不熟悉這條指令的話，我們逐一分析
首先`remote`參數代表跟遠端的操作有關，`add`指的是新增一個節點，最後的origin是一個「代名詞」，表示後面的GitHub的Repository位置

為和是`origin`？原因這是預設的遠端節點代稱，但這只是大家的共識，如果你今天想要你也可以自己取一個你喜歡的代稱，比如`Idontwant996`就像這樣：
```bash
git remote add Idontwant996 $REMOTE_PATH
```
總而言之，他只是一個遠端節點的代稱
設定完成之後，就可以把東西Push上去了
```bash
git push -u origin master
```


# Reference
[為你自己學Git - 十、遠端共同協作 - 使用 GitHub：Git教學：如何 Push 上傳到 GitHub？](https://gitbook.tw/chapters/github/push-to-github#google_vignette)
