# IOSvL2に静的IPアドレスを設定する手順

<details>
   <summary>クリックで展開</summary>
   
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
</details>

# IOSvL2でVTPの設定をする方法

<details>
   <summary>クリックで展開</summary>
   
   1. 特権モードに移行
      <pre>enable</pre>
      `en` と入力しても特権モードに移行することができます。
   1. グローバルコンフィグレーションモードに移行
      <pre>configure terminal</pre>
      `conf t`と入力してもグローバルコンフィグレーションモードに移行することができます。
   1. VTPドメイン名を設定（※任意）
      <pre>vtp domain TESTDOMAIN</pre>
      ドメイン名は同一でなくてもよいですが、設定しておくのが一般的です。
   1. トランスペアレントモードに設定（この場合はトランスペアレントモードに設定しますが、環境に合わせて変えてください。）
      <pre>vtp mode transparent</pre>
      以下のようなメッセージが表示されれば成功
      <pre>Setting device to VTP TRANSPARENT mode.</pre>
   1. 設定の確認
      <pre>show vtp status</pre>
</details>

# IOSvL2でポートをトランクモードにする方法

<details>
   <summary>クリックで展開</summary>

   1. 特権モードに移行
      <pre>enable</pre>
      `en` と入力しても特権モードに移行することができます。
   1. グローバルコンフィグレーションモードに移行
      <pre>configure terminal</pre>
      `conf t`と入力してもグローバルコンフィグレーションモードに移行することができます。
   1. 設定したいインターフェースへ移行
      <pre>interface g0/0</pre>
   1. トランクリンクで使用するカプセル化プロトコルを指定する
      <pre>switchport trunk encapsulation dot1q</pre>
   1. 指定したポートをトランクモードとして動作させる
      <pre>switchport mode trunk</pre>
   1. 特権モードに移行
      <pre>end</pre>
   1. 設定が正しく適用されたかを確認する
      <pre>show interfaces g0/0 switchport</pre>
      
</details>
