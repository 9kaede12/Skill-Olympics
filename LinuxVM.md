# CMLのLinux VMで静的IPアドレスを設定する手順

1. インターフェース名の確認  
   <pre>ip a</pre>
   通常は `ensX` や `ethX` などが表示されます（例：`ens3`）。
1. 設定ファイルの編集  
   例：インターフェース名が `ens3` の場合  
   編集対象ファイル（例）: `/etc/netplan/50-cloud-init.yaml` または `/etc/netplan/01-netcfg.yaml`
   <pre>sudo nano /etc/netplan/50-cloud-init.yaml</pre>
1. 以下のように編集（例）
   <pre>
   network:
    version: 2
    ethernets:
      ens3:
        dhcp4: no
        addresses:
          - 192.168.1.100/24
        gateway4: 192.168.1.1
        nameservers:
          addresses:
            - 8.8.8.8
            - 1.1.1.1
   </pre>
1. 設定を反映
   <pre>sudo netplan apply</pre>
1. IPアドレスの確認
   <pre>
   ip a
   ip r
   </pre>
