```bash
configure terminal
!
! 名前付きEIGRPプロセス開始（インスタンス名：A2024）
router eigrp A2024
!
  ! IPv4ユニキャストアドレスファミリー + AS番号100
  address-family ipv4 unicast autonomous-system 100
    !
    ! EIGRPに参加させるネットワークを指定
    network 172.17.0.2 0.0.0.0     ! Gi0/0 (hq1接続)
    network 172.17.0.14 0.0.0.0    ! Gi1/0 (hq2接続)
    network 172.17.0.17 0.0.0.0    ! Vlan99
    !
    ! 端末接続セグメントのSVIをパッシブインターフェースに設定
    af-interface Vlan10
      passive-interface
    exit-af-interface
    af-interface Vlan11
      passive-interface
    exit-af-interface
  exit-address-family
exit
!
end
```
