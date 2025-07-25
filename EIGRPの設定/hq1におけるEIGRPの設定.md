# hq1ルータ用 EIGRP（IPv4）設定スクリプト

以下は、Cisco IOSの**名前付きEIGRP**構文による設定スクリプトです。  
BGPからの再配布、ルートマップ、プレフィックスリストを含みます。

---

```bash
configure terminal
!
! 名前付きEIGRPプロセスの開始（インスタンス名：A2024）
router eigrp A2024
!
  ! IPv4ユニキャストアドレスファミリー + AS番号100 の定義
  address-family ipv4 unicast autonomous-system 100
    !
    ! EIGRPに参加させるインターフェースのIPアドレスを指定
    network 172.17.0.1 0.0.0.0        ! Gi0/1 (to cs1)
    network 172.17.0.21 0.0.0.0       ! Gi0/2.98
    network 172.17.100.254 0.0.0.0    ! Gi0/2.100
    network 172.17.0.5 0.0.0.0        ! Gi0/3 (to cs2)
    network 100.100.100.11 0.0.0.0    ! Loopback0 (primary)
    network 200.200.200.11 0.0.0.0    ! Loopback0 (secondary)
    !
    ! Gi0/2.100 を passive に設定（Hello送出抑制）
    af-interface GigabitEthernet0/2.100
      passive-interface
    exit-af-interface
    !
    ! トポロジーベース設定モードへ
    topology base
      !
      ! BGPからの経路をルートマップに従って再配布
      redistribute bgp 300 route-map BGP_TO_EIGRP_HQ1
      !
      ! 再配布される経路のデフォルトメトリックを設定
      default-metric 10000 100 255 1 1500
    exit-af-topology
  exit-address-family
exit
!
! --- ルートマップとプレフィックスリストの定義 ---
!
! DEFAULT_ROUTE_ONLY（0.0.0.0/0）用メトリック設定
route-map BGP_TO_EIGRP_HQ1 permit 10
  match ip address prefix-list DEFAULT_ROUTE_ONLY
  set metric 10000 100 255 1 1500
exit
!
! 100.99.0.0/16 を再配布（同じメトリック）
route-map BGP_TO_EIGRP_HQ1 permit 20
  match ip address prefix-list PREFIX_100_99_0_16
  set metric 10000 100 255 1 1500
exit
!
! 200.99.0.0/16 は再配布禁止
route-map BGP_TO_EIGRP_HQ1 deny 30
  match ip address prefix-list PREFIX_200_99_0_16
exit
!
! その他の経路は全て再配布許可
route-map BGP_TO_EIGRP_HQ1 permit 40
exit
!
! --- プレフィックスリストの定義 ---
ip prefix-list DEFAULT_ROUTE_ONLY seq 5 permit 0.0.0.0/0
ip prefix-list PREFIX_100_99_0_16 seq 5 permit 100.99.0.0/16
ip prefix-list PREFIX_200_99_0_16 seq 5 permit 200.99.0.0/16
!
end
