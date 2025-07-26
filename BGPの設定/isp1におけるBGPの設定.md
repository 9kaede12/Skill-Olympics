```bash
configure terminal
!
! インターフェースIPアドレス設定
interface GigabitEthernet0/0
 ip address 100.99.2.1 255.255.255.252
 no shutdown
exit

interface GigabitEthernet0/3
 ip address 100.99.2.5 255.255.255.252
 no shutdown
exit

interface Loopback0
 ip address 100.99.1.1 255.255.255.0
 no shutdown
exit
!
! BGP設定開始（AS番号100）
router bgp 100
  !
  ! eBGPピア設定
  neighbor 100.99.2.2 remote-as 300   ! hq1 (AS300)
  neighbor 100.99.2.6 remote-as 130   ! isp3 (AS130)
  !
  ! BGPでアドバタイズするネットワーク
  ! isp1ルータのLoopback0インターフェースに割り当てられているIPアドレス100.99.1.1/24 のネットワークアドレスです。
  network 100.99.1.0 mask 255.255.255.0

  ! isp1ルータのGigabitEthernet0/0インターフェースに割り当てられているIPアドレス100.99.2.1/30 のネットワークアドレスです。
  network 100.99.2.0 mask 255.255.255.252

  ! isp1ルータのGigabitEthernet0/3インターフェースに割り当てられているIPアドレス100.99.2.5/30 のネットワークアドレスです。
  network 100.99.2.4 mask 255.255.255.252
  !
  ! hq1に対してデフォルトルートと100.99.0.0/16をアドバタイズ
  default-information originate
  network 100.99.0.0 mask 255.255.0.0
exit
!
end
```
