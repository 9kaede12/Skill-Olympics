# BGP構成設定スクリプト（AS300・hq2ルーター）

以下の設定は、Cisco IOSルーターに対してそのまま貼り付けて利用できるBGP構成です。hq2ルーターをAS300に所属させ、hq1とのiBGP、isp2とのeBGP、および経路集約とAS-Path制御を行います。

---

```bash
configure terminal
!
! BGPプロセス開始（AS300）
router bgp 300
  bgp log-neighbor-changes
  timers bgp 10 30

  ! 集約経路用の起点としてLoopback0をアドバタイズ
  network 100.100.100.22 mask 255.255.255.255
  network 200.200.200.22 mask 255.255.255.255

  ! 集約経路をsummary-onlyでアドバタイズ
  ! aggregate-address は、BGPで複数のルートを集約（サマライズ）するためのコマンドです。
  ! summary-only オプションを付けることで、集約後の経路のみをアドバタイズし、
  ! 個別経路（more specific routes）は広告しないという動作になります。
  aggregate-address 100.100.100.0 255.255.255.224 summary-only
  aggregate-address 200.200.200.0 255.255.255.224 summary-only

  ! hq1とのiBGPピア設定（Loopback0のプライマリアドレスを使用）
  neighbor 100.100.100.11 remote-as 300
  neighbor 100.100.100.11 update-source Loopback0
  neighbor 100.100.100.11 next-hop-self

  ! isp2とのeBGPピア設定
  neighbor 200.99.2.1 remote-as 200

  ! AS-Path Prepend用のルートマップをisp2へのアウトバウンドに適用
  neighbor 200.99.2.1 route-map PREPEND_100_TO_ISP2 out
exit
!
! ASパスアクセスリストの定義（任意：参照なしでもOK）
ip as-path access-list 1 permit _300$
!
! ルートマップの定義（100.100.100.0/27へのアクセスをisp1優先に）
route-map PREPEND_100_TO_ISP2 permit 10
  match ip address prefix-list PREFIX_100_100_100_27
  set as-path prepend 300 300 300
  exit
!
! プレフィックスリスト定義
ip prefix-list PREFIX_100_100_100_27 seq 5 permit 100.100.100.0/27
!
end
