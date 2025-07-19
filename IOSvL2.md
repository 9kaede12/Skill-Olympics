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

## Etherchannelを動作させる方法
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
   1. インターフェースを Port-channel 1 に静的に所属させる
      <pre>channel-group 1 mode on</pre>
   1. インターフェースを有効化
      <pre>no shutdown</pre>
   1. 特権モードに移行
      <pre>end</pre>
   1. 設定が正しく適用されたかを確認する
      <pre>
      show etherchannel summary
      show interfaces Port-channel1 switchport
      show interfaces GigabitEthernet0/2 etherchannel
      show interfaces GigabitEthernet0/3 etherchannel
      </pre>
</details>

## STPを動作させる方法（RSTP (IEEE802.1w)）
<details>
   <summary>クリックで展開</summary>

   1. 特権モードに移行
      <pre>enable</pre>
      `en` と入力しても特権モードに移行することができます。
   1. グローバルコンフィグレーションモードに移行
      <pre>configure terminal</pre>
      `conf t`と入力してもグローバルコンフィグレーションモードに移行することができます。
   1. スイッチングモードを Rapid PVST+ (Rapid Per-VLAN Spanning Tree Plus) に設定
      <pre>spanning-tree mode rapid-pvst</pre>

   ### ルートブリッジの設定
   1. 特権モードに移行
      <pre>enable</pre>
      `en` と入力しても特権モードに移行することができます。
   1. グローバルコンフィグレーションモードに移行
      <pre>configure terminal</pre>
      `conf t`と入力してもグローバルコンフィグレーションモードに移行することができます。
   1. STP プライオリティを 4096 に設定
      <pre>spanning-tree vlan 10 priority 4096</pre>
      これは可能な最低値の一つであり、このVLANのルートブリッジになることを強く推奨します。

   ### セカンダリルートブリッジの設定
   1. 特権モードに移行
      <pre>enable</pre>
      `en` と入力しても特権モードに移行することができます。
   1. グローバルコンフィグレーションモードに移行
      <pre>configure terminal</pre>
      `conf t`と入力してもグローバルコンフィグレーションモードに移行することができます。
   1. STP プライオリティを 8192 に設定
      <pre>spanning-tree vlan 11 priority 8192</pre>
      これは cs2 のデフォルトプライオリティ 32768 より低く、かつ cs2 が VLAN 11 のルートブリッジとなるように設定する値（通常は 4096）より高く設定することで、セカンダリルートブリッジとしての役割を果たします。
</details>

## HSRPを設定する方法
<details>
   <summary>クリックで展開</summary>

   1. 特権モードに移行
      <pre>enable</pre>
      `en` と入力しても特権モードに移行することができます。
   1. グローバルコンフィグレーションモードに移行
      <pre>configure terminal</pre>
      `conf t`と入力してもグローバルコンフィグレーションモードに移行することができます。
   1. Vlanインターフェースを指定
      例：vlan 10を指定する場合
      <pre>interface vlan 10</pre>
   1. IPアドレスとサブネットマスクを設定 （まだ設定していない場合）
      例：192.168.1.1/24 を設定する場合
      <pre>ip address 192.168.1.1 255.255.255.0</pre>

   ### アクティブにする設定
   1. HSRPグループ10の仮想IPアドレスの設定
      <pre>standby 10 ip 172.17.10.254</pre>
      HSRPグループ番号を 10 とし、仮想IPアドレスを 172.17.10.254 に設定します。
   1. VLAN10でアクティブにする
      <pre>standby 10 priority 110</pre>
      アクティブにするため、デフォルトのプライオリティ 100 よりも高い 110 を設定します。
   1. 切り戻しの設定
      <pre>standby 10 preempt</pre>
      `アクティブ` がダウンして `スタンバイ` がアクティブになった後、 `元アクティブ` が復旧してより高いプライオリティでオンラインになった際に、`元アクティブ` にアクティブの役割を切り戻させるために必要です。

   ### スタンバイにする設定
   1. HSRPグループ10の仮想IPアドレスの設定
      <pre>standby 10 ip 172.17.10.254</pre>
      HSRPグループ番号を 10 とし、仮想IPアドレスを 172.17.10.254 に設定します。
   1. VLAN10でアクティブにする
      <pre>standby 10 priority 90</pre>
      スタンバイにするため、デフォルトのプライオリティ 100 よりも低い 90 を設定します。
   1. 切り戻しの設定
      <pre>standby 10 preempt</pre>
      `アクティブ` がダウンして `スタンバイ` がアクティブになった後、 `元アクティブ` が復旧してより高いプライオリティでオンラインになった際に、`元アクティブ` にアクティブの役割を切り戻させるために必要です。

   ### 設定の確認
   <pre>show standby brief</pre>
</details>

## EtherChannelの設定手順
<details>
   <summary>クリックで展開</summary>

   1. 特権モードに移行
      <pre>enable</pre>
      `en` と入力しても特権モードに移行することができます。
   1. グローバルコンフィグレーションモードに移行
      <pre>configure terminal</pre>
      `conf t`と入力してもグローバルコンフィグレーションモードに移行することができます。
   1. インターフェースを指定
      例：g0/2 を指定する場合
      <pre>interface g0/2</pre>
   1. トランクリンクで使用するカプセル化プロトコルを dot1q (IEEE 802.1Q) に明示的に設定
      <pre>switchport trunk encapsulation dot1q</pre>
      このコマンドは、`switchport mode trunk` コマンドを実行する前に必要です。
   1. 物理インターフェースをトランクモードとして動作させる
      <pre>switchport mode trunk</pre>
      これにより、複数のVLANのトラフィックがこのリンクを通過できるようになります。
   1. インターフェースを Port-channel 1 という論理グループに静的に所属させる
      <pre>channel-group 1 mode on</pre>
      mode on はネゴシエーションプロトコル（LACPやPAgP）を使用しないことを意味します。
   1. インターフェースを有効化
      <pre>no shutdown</pre>

   ### 論理インターフェース (Port-channel) の設定
   1. 論理インターフェースである Port-channel 1 のコンフィギュレーションモードに入る
      <pre>interface Port-channel1</pre>
   1. トランクリンクで使用するカプセル化プロトコルを dot1q (IEEE 802.1Q) に明示的に設定
      <pre>switchport trunk encapsulation dot1q</pre>
      このコマンドは、`switchport mode trunk` コマンドを実行する前に必要です。
   1. 物理インターフェースをトランクモードとして動作させる
      <pre>switchport mode trunk</pre>
      これにより、複数のVLANのトラフィックがこのリンクを通過できるようになります。
   1. インターフェースを有効化
      <pre>no shutdown</pre>

   ### 設定の確認
   1. EtherChannel の状態の概要を確認
      <pre>show etherchannel summary</pre>
      `Port-channel1` が「SU (Layer2, Up)」の状態になっていることを確認
   1. `Port-channel1` が `Operational Mode: trunk` になっていることを確認します。
      <pre>show interfaces Port-channel1 switchport</pre>
      また、「Trunking VLANs Enabled: ALL」または許可したいVLANがリストに含まれているか確認してください。
</details>
