# CMLのLinuxServerVMに静的IPアドレスを設定する手順
1. IPアドレスを設定する  
   例：IPアドレスに192.168.1.10/24を設定する場合
   <pre>
    sudo ifconfig eth0 192.168.1.100 netmask 255.255.255.0 up
    sudo route add default gw 192.168.1.1
   </pre>
   ここで、eth0 はインターフェース名です。必要に応じて ifconfig や ip link で確認してください。
1. 設定したIPアドレスの確認
   <pre>ifconfig</pre>
