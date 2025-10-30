# Docker install

[toc]

# Docker Engine
如果要安裝在沒有GUI的系統（比如Ubuntu Server）安裝Docker Engine就好
## Ubuntu
如果你有安裝其他版本或來源的docker透過以下指令刪除
```bash
for pkg in docker.io docker-doc docker-compose docker-compose-v2 podman-docker containerd runc; do sudo apt-get remove $pkg; done
```
接著你希望手動控制版本，可以直接從官往下載`.deb`包，如果想自動從官網下載最新的，跟著以下流程

首先，新增Docker的Repository:
```bash
# Add Docker's official GPG key:
sudo apt-get update
sudo apt-get install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

# Add the repository to Apt sources:
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "${UBUNTU_CODENAME:-$VERSION_CODENAME}") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update
```
完成後安裝Docker Engine所需套件：
```bash
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```
安裝完成後就可以使用以下指令測試：
```bash
sudo docker run --rm hello-world
```
日後不想用Docker[教學在這](https://docs.docker.com/engine/install/ubuntu/#uninstall-docker-engine)

# Docker Desktop


# Docker安裝完成後的設定
把你要操作Docker的Users加入到Docker的群組
```bash
sudo usermod -aG docker $USER
```
然後登出再登入，再來執行指令測試：
```bash
docker run --rm hello-world
```

# Reference
[Install Docker Engine on Ubuntu](https://docs.docker.com/engine/install/ubuntu/)