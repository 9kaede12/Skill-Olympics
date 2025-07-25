# BGP構成設定スクリプト（AS300・hq1ルーター）

以下の設定は、Cisco IOSルーターにおいて `hq1`（AS300）でのBGP構成を定義しています。  
`hq2`や`isp1`とのピア確立、経路集約、AS-Path制御を含んでいます。

---

```bash
configure terminal
!
! BGPプロセス開始（AS300）
router bgp 300
  bgp log-neighbor-changes
  timers bgp 10 30

  ! 集約経路の起点としてLoopback0をアドバタイズ
  network 100.100.100.11 mask 255.255.255.255
  network 200.200.200.11 mask 255.255.255.255

  ! 集約経路をsummary-onlyでアドバタイズ
  aggregate-address 100.100.100.0 255.255.255.224 summary-only
  aggregate-address 200.200.200.0 255.255.255.224 summary-only

  ! hq2とのiBGPピア設定（Loopbackアドレスを使用）
  neighbor 200.200.200.22 remote-as 300
  neighbor 200.200.200.22 update-source Loopback0
  neighbor 200.200.200.22 next-hop-self

  ! isp1とのeBGPピア設定（AS100）
  neighbor 100.99.2.1 remote-as 100

  ! isp1へ200.200.200.0/27をアドバタイズする際にASパスを長くする
  neighbor 100.99.2.1 route-map PREPEND_200_TO_ISP1 out
exit
!
! ASパスアクセスリスト（オプション）
ip as-path access-list 1 permit _300$
!
! ルートマップの定義（200.200.200.0/27をisp1に向けてAS-Path Prepend）
route-map PREPEND_200_TO_ISP1 permit 10
  match ip address prefix-list PREFIX_200_200_200_27
  set as-path prepend 300 300 300
!
! 対象プレフィックスの指定
ip prefix-list PREFIX_200_200_200_27 seq 5 permit 200.200.200.0/27
!
end
