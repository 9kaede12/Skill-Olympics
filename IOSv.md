# IOSvに静的IPアドレスを設定する手順
<details>
   <summary>設定手順</summary>
   
   1. 特権モードに移行
      <details open>
         <summary>コード</summary> 
      
          enable

      `en` と入力しても特権モードに移行することができます。
      </details>
      
   1. グローバルコンフィグレーションモードに移行
      <details open>
         <summary>コード</summary> 
      
          configure terminal

      `conf t`と入力してもグローバルコンフィグレーションモードに移行することができます。
      </details>
      
   1. 対象のインタフェースを指定  
      例：GigabitEthernet0/0 を使う場合
      <details open>
         <summary>コード</summary> 
      
          interface GigabitEthernet0/0

      `int g0/0`と入力しても対象のインターフェースを指定することができます。
      </details>
      
   1. IPアドレスとサブネットマスクを設定  
      例：192.168.1.1/24 を設定する場合
      <details open>
         <summary>コード</summary> 
      
          ip address 192.168.1.1 255.255.255.0
      
      </details>
   1. インタフェースを有効化
      <details open>
         <summary>コード</summary> 
      
          no shutdown
      
      </details>

   1. IPアドレスを設定できたか確認
      <details open>
         <summary>コード</summary>
      
          show ip interface brief

      `show ip int brief`でもIPアドレスの確認ができます。
      </details>

</details>

# DHCPサーバとして動作させるときの設定手順
<details>
   <summary>設定手順</summary>
   
   1. DHCPプールの設定
      <details open>
         <summary>コード</summary>
   
          conf t
          ip dhcp pool MYPOOL
          network 130.130.130.0 255.255.255.0
          default-router 130.130.130.254

      コンフィグレーションモードで設定を行います。  
      `MYPOOL`という名前にしていますが、任意の名前で大丈夫なのでわかりやすい名前にすると良いです。  
      今回は`130.130.130.0/24`の配布するIPアドレスとして設定をしています。  
      デフォルトルートは`default-router 130.130.130.254`で設定を行っています。  
      </details>

   1. DHCPで配布しないIPアドレス（静的IP用）の設定
      <details open>
         <summary>コード</summary>
         
          ip dhcp excluded-address 130.130.130.1 130.130.130.10
          ip dhcp excluded-address 130.130.130.254

      1行目に関しては任意ですが、同一ネットワーク内でIPアドレスが重複する可能性をなくすために、配布しない範囲をあらかじめ決めておくと良いです。  
      2行目に関してはデフォルトルートとして使用しているIPアドレスのため、配布しないようにします。
      </details>      

   1. DHCPサーバ側（IPリース状況）の確認
      <details open>
         <summary>コード</summary>

          show ip dhcp binding
      
      </details>
   
</details>

# DHCPクライアントとして動作させる時の設定手順
<details>
   <summary>設定手順</summary>

   1. DHCPクライアントとして設定する
      <details open>
         <summary>コード</summary>
         
          conf t
          interface GigabitEthernet0/0
          ip address dhcp
          no shutdown

      コンフィグレーションモードで設定を行います。  
      インターフェースはIPアドレスをDHCPで設定したいものを指定します。  
      必ず`no shutdown`をするようにしてください。
      </details>

   1. クライアント側（IP取得確認）の確認
      <details open>
         <summary>コード</summary>
         
          show ip int brief
          show ip route
      
      </details>
      
</details>

# br1_clとbr2_clで疎通確認ができるようにする設定
<details>
   <summary>設定手順</summary>

   1. br1の設定
      <details open>
         <summary>コード</summary>
      
          ip route 10.1.0.0 255.255.255.0 130.130.130.11
      
      130.130.130.11の部分はDHCPによって取得するため ` show ip int brief `でIPアドレスを確認して、環境にあったものを指定してください。
      </details>
      
   1. br2の設定
      <details open>
         <summary>コード</summary>
      
          ip route 10.2.0.0 255.255.255.0 130.130.130.12

      130.130.130.12の部分はDHCPによって取得するため ` show ip int brief `でIPアドレスを確認して、環境にあったものを指定してください。
      </details>
            
</details>
