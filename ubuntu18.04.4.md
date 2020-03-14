# ubuntu 18.04.4 LTSでautowareをインストールするメモ

### ubuntu16.04.6 LTSインストールメモ
* 静的IPアドレス設定
```
vi /etc/netplan/50-cloud-init.yaml

network:
  version: 2
  renderer: networkd
  ethernets:
    enp0s3:
      dhcp4: no
      dhcp6: no
      addresses: [192.168.100.10/24]
      gateway4: 192.168.100.1
      nameservers:
        addresses: [192.168.100.1]
```

### CUDAインストール

## autoware インストール
## LGSVLシミュレータメモ