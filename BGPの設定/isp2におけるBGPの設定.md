```bash
configure terminal
!
! インターフェースIPアドレス設定
interface GigabitEthernet0/0
 ip address 200.99.2.1 255.255.255.252
 no shutdown
exit

interface GigabitEthernet0/1
 ip address 200.99.1.254 255.255.255.0
 no shutdown
exit

interface GigabitEthernet0/2
 ip address 200.99.2.5 255.255.255.252
 no shutdown
exit
!
! BGP設定開始（AS番号200）
router bgp 200
  !
  ! eBGPピア設定
  neighbor 200.99.2.2 remote-as 300   ! hq2 (AS300)
  neighbor 200.99.2.6 remote-as 130   ! isp3 (AS130)
  !
  ! BGPでアドバタイズするネットワーク
  ! isp2ルータのGigabitEthernet0/1インターフェースに割り当てられているIPアドレス200.99.1.254/24 のネットワークアドレスです。
  network 200.99.1.0 mask 255.255.255.0

  ! isp2ルータのGigabitEthernet0/0インターフェースに割り当てられているIPアドレス200.99.2.1/30 のネットワークアドレスです。
  network 200.99.2.0 mask 255.255.255.252

  ! isp2ルータのGigabitEthernet0/2インターフェースに割り当てられているIPアドレス200.99.2.5/30 のネットワークアドレスです。
  network 200.99.2.4 mask 255.255.255.252
  !
  ! hq2に対してデフォルトルートと200.99.0.0/16をアドバタイズ
  default-information originate
  network 200.99.0.0 mask 255.255.0.0
exit
!
end

```
