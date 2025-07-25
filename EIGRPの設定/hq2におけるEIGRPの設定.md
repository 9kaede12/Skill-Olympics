# hq2ルータ用 EIGRP（IPv4）設定スクリプト

このスクリプトは、Cisco IOS環境でそのまま貼り付けて使用可能です。  
BGP・OSPFからの経路再配布、メトリック制御、優先順位制御を含みます。

---

```bash
configure terminal
!
! 名前付きEIGRPプロセス開始（インスタンス名：A2024）
router eigrp A2024
!
  ! IPv4ユニキャストアドレスファミリー + AS番号100 を定義
  address-family ipv4 unicast autonomous-system 100
    !
    ! ルータの各インターフェースをEIGRPに参加させる
    network 172.17.0.9 0.0.0.0        ! Gi0/1 (to cs2)
    network 172.17.0.22 0.0.0.0       ! Gi0/2.98
    network 172.17.100.253 0.0.0.0    ! Gi0/2.100
    network 172.17.0.13 0.0.0.0       ! Gi0/3 (to cs1)
    network 10.0.0.1 0.0.0.0          ! Tunnel0 (DMVPN)
    network 100.100.100.22 0.0.0.0    ! Loopback0 primary
    network 200.200.200.22 0.0.0.0    ! Loopback0 secondary
    !
    ! VLAN100インターフェースをpassiveに設定（Hello抑制）
    af-interface GigabitEthernet0/2.100
      passive-interface
    exit-af-interface
    !
    ! トポロジー設定（再配布とデフォルトメトリック）
    topology base
      !
      ! BGPからの再配布
      redistribute bgp 300 route-map BGP_TO_EIGRP_HQ2
      !
      ! OSPFからの再配布（プロセスID仮定：100）
      redistribute ospf 100 route-map OSPF_TO_EIGRP
      !
      ! 再配布経路のデフォルトメトリック設定
      default-metric 10000 100 255 1 1500
    exit-af-topology
  exit-address-family
exit
!
! --- ルートマップとプレフィックスリストの定義 ---
!
! デフォルトルート（0.0.0.0/0）：hq1より低優先に設定
route-map BGP_TO_EIGRP_HQ2 permit 10
  match ip address prefix-list DEFAULT_ROUTE_ONLY
  set metric 20000 200 255 1 1500
exit
!
! 200.99.0.0/16：通常優先のメトリックで再配布
route-map BGP_TO_EIGRP_HQ2 permit 20
  match ip address prefix-list PREFIX_200_99_0_16
  set metric 10000 100 255 1 1500
exit
!
! 100.99.0.0/16：hq2では再配布しない（hq1経由を優先）
route-map BGP_TO_EIGRP_HQ2 deny 30
  match ip address prefix-list PREFIX_100_99_0_16
exit
!
! その他のBGP経路：全て再配布許可
route-map BGP_TO_EIGRP_HQ2 permit 40
exit
!
! OSPFからの再配布経路：全て同一メトリック
route-map OSPF_TO_EIGRP permit 10
  set metric 10000 100 255 1 1500
exit
!
! --- プレフィックスリスト定義 ---
ip prefix-list DEFAULT_ROUTE_ONLY seq 5 permit 0.0.0.0/0
ip prefix-list PREFIX_100_99_0_16 seq 5 permit 100.99.0.0/16
ip prefix-list PREFIX_200_99_0_16 seq 5 permit 200.99.0.0/16
!
end
