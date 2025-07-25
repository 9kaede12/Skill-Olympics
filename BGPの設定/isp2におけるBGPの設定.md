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
  neighbor 200.99.2.6 remote-as 400   ! isp3 (AS400)
  !
  ! BGPでアドバタイズするネットワーク
  network 200.99.1.0 mask 255.255.255.0
  network 200.99.2.0 mask 255.255.255.252
  network 200.99.2.4 mask 255.255.255.252
  !
  ! hq2に対してデフォルトルートと200.99.0.0/16をアドバタイズ
  default-information originate
  network 200.99.0.0 mask 255.255.0.0
exit
!
end

```
