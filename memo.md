# 参考URLメモ

* 第2回自動運転AIチャレンジ  
https://www.jsae.or.jp/jaaic/index.html
* AIチャレンジ参加者用リポジトリ  
https://github.com/tier4/aichallenge_bringup
* markdownチートシート  
https://qiita.com/Qiita/items/c686397e4a0f4f11683d

# ubuntu16.04.6 LTSインストールメモ

* なんか日本語キーボード指定しても@使えなかったので、とりあえずパスワードは記号使わず簡潔に。

* IPアドレス固定
```
# vi /etc/network/interfaces
auto enp9s0
iface enp9s0 inet static
address 192.168.0.10
netmask 255.255.255.0
gateway 192.168.0.1
dns-nameservers 192.168.0.1
```



# autoware インストールメモ


# LGSVLシミュレータメモ
