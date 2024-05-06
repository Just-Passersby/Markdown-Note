# 淺談經過verify的commit

Index:
[toc]

# Signature的重要性
你知道嗎，你在`~/.gitconfig`裡面設定成像是底下的內容
```bash
[user]
    email = torvalds@osdl.org
    name = torvalds

```
好了，接下來你所有丟到Github上面的push和commit都會被會是托瓦茲的狀態了（而且點進去也確實是托瓦茲的Github）
Emm...也就是說，你可以發現Github上面要冒充身份十分容易，因次設定好Signature其實是十分重要的事，因為這樣你所有丟到Github內容只要看有沒有verify，基本可以確定這個是不是你本人上傳的內容（只要你的Private key沒有洩漏），同時保證自己品行良好，沒有冒充別人😅

# How signature and verify
又能夠產生verify的commit會需要GPG這個套件的協助
macOS使用[GPG Suite](https://gpgtools.org)這個套件，若有安裝Homebrew可以使用以下指令安裝：
```bash
brew install gpg
```
Windows用戶則是使用[Gpg4win](https://www.gpg4win.org/)這個套件

# Reference
[五分鐘簽署認證你的 GIT COMMIT / SIGNING YOUR GIT COMMITS](https://useme.medium.com/五分鐘認證你的-git-commit-265b002ce71b)
[Github Docs: Signing commits](https://docs.github.com/en/authentication/managing-commit-signature-verification/signing-commits)
[macOS下使用GPG](https://tourcoder.com/gpg-on-macos/)
