# IOSvL2に静的IPアドレスを設定する手順

1. 特権モードに移行
   <pre>enable</pre>
   `en` と入力しても特権モードに移行することができます。
1. グローバルコンフィグレーションモードに移行
   <pre>configure terminal</pre>
   `conf t`と入力してもグローバルコンフィグレーションモードに移行することができます。
1. Vlanの設定
   例：vlan 100を使う場合
   <pre>vlan 100</pre>
   L2機器は基本的にvlanに対してIPアドレスを設定します。
1. Vlanインターフェースを指定
   例：vlan 100を指定する場合
   <pre>interface vlan 100</pre>
1. IPアドレスとサブネットマスクを設定  
   例：192.168.1.1/24 を設定する場合
   <pre>ip address 192.168.1.1 255.255.255.0</pre>
1. VLANに属するポートを設定
   例：GigabitEthernet0/1 を VLAN100 に属させる
   <pre>
    interface gigabitethernet0/1  
    switchport mode access  
    switchport access vlan 100  
    no shutdown  
    exit
   </pre>
1. 設定の確認
   <pre>show ip interface brief</pre>
