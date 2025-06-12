# IOSvに静的IPアドレスを設定する手順

1. 特権モードに移行
   <pre>enable</pre>
   `en` と入力しても特権モードに移行することができます。
1. グローバルコンフィグレーションモードに移行
   <pre>configure terminal</pre>
   `conf t`と入力してもグローバルコンフィグレーションモードに移行することができます。
1. 対象のインタフェースを指定  
   例：GigabitEthernet0/0 を使う場合
   <pre>interface GigabitEthernet0/0</pre>
   `int g0/0`と入力しても対象のインターフェースを指定することができます。
1. IPアドレスとサブネットマスクを設定  
   例：192.168.1.1/24 を設定する場合
   <pre>ip address 192.168.1.1 255.255.255.0</pre>
1. インタフェースを有効化
   <pre>no shutdown</pre>
1. IPアドレスを設定できたか確認
   <pre>show ip interface brief</pre>
   `show ip int brief`でもIPアドレスの確認ができます。
