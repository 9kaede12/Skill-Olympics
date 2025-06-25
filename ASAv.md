# ASAvにサブインターフェースを設定する方法

1. 特権モードに移行
   <pre>enable</pre>
   `en` と入力しても特権モードに移行することができます。
   その時にパスワードの入力を促された場合は
   <pre>Cisco1@3</pre>
   と入力をしてください。
1. グローバルコンフィグレーションモードに移行
   <pre>configure terminal</pre>
   `conf t`と入力してもグローバルコンフィグレーションモードに移行することができます。
1. 対象のサブインタフェースを指定  
   例：GigabitEthernet0/0.10 を使う場合
   <pre>interface GigabitEthernet0/0.10</pre>
   `int g0/0.10`と入力しても対象のサブインターフェースを指定することができます。
1. vlanの設定  
   例：vlan 10を設定する場合
   <pre>vlan 10</pre>
1. インターフェース名の設定  
   例：DMZを設定する場合
   <pre>nameif DMZ</pre>
1. セキュリティーレベルの設定  
   例：セキュリティーレベルを10に設定する場合
   <pre>security-level 10</pre>
1. IPアドレスとサブネットマスクを設定  
   例：192.168.1.1/24 を設定する場合
   <pre>ip address 192.168.1.1 255.255.255.0</pre>
1. インタフェースを有効化
   <pre>no shutdown</pre>
1. グローバルコンフィグレーションモードに移行
   <pre>configure terminal</pre>
   `conf t`と入力してもグローバルコンフィグレーションモードに移行することができます。
1. 親インタフェースを指定  
   例：GigabitEthernet0/0.10 を使う場合
   <pre>interface GigabitEthernet0/0</pre>
   `int g0/0.10`と入力しても対象のサブインターフェースを指定することができます。  
   サブインターフェース「GigabitEthernet0/0.10」の場合は、「GigabitEthernet0/0」が親インタフェースになります。
1. インタフェースを有効化
   <pre>no shutdown</pre>
1. IPアドレスを設定できたか確認
   <pre>show interface ip brief</pre>
   `show int ip brief`で確認ができます。
1. ICMPインジェクションを有効にする
   <pre>
   policy-map global_policy
   class inspection_default
   inspect icmp
   </pre>
1. 設定に確認
   <pre>show run policy-map</pre>
   以下のように出力されればOK
   <pre>
   policy-map global_policy
   class inspection_default
   inspect icmp
   </pre>
