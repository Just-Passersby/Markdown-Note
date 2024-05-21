# Easy RSA介紹

Index:
[toc]

# 什麼是Easy RSA?
Easy RSA是讓Linux作為PKI中的Certificate Authority(授權機構)的一種解決方案，雖然名叫**Easy** RSA，但仍舊有著不低的上手難度，但相比用內建的OpenSSL手搓CA...
恩...簡單多了

由於Easy RSA就是為了建立CA而生，所以依舊需要足夠的PKI知識（至少CA的這個部分），如果需要可以參考[Easy RSA解釋PKI](https://easy-rsa.readthedocs.io/en/latest/intro-to-PKI/)

同場加映：
雖然[這部影片](https://fb.watch/sc3H2LKS81/)主要在講TLS，但裡面也涉及到不少PKI知識，講的也很棒

# Easy RSA安裝
既然作為開源軟體，一定是能夠到[GitHub](https://github.com/OpenVPN/easy-rsa) clone下來然後編譯

而Debian/Ubuntu體系的Linux可以用以下指令安裝
```bash
apt install easy-rsa
```
