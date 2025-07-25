```bash
configure terminal
!
! インターフェースIPアドレス設定
interface GigabitEthernet0/0
 ip address 130.130.130.254 255.255.255.0
 no shutdown
exit

interface GigabitEthernet0/1
 ip address 100.99.2.6 255.255.255.252
 no shutdown
exit

interface GigabitEthernet0/2
 ip address 200.99.2.6 255.255.255.252
 no shutdown
exit
!
! BGP設定開始（AS番号400）
router bgp 400
  !
  ! eBGPピア設定
  neighbor 100.99.2.5 remote-as 100   ! isp1 (AS100)
  neighbor 200.99.2.5 remote-as 200   ! isp2 (AS200)
  !
  ! BGPでアドバタイズするネットワーク
  network 130.130.130.0 mask 255.255.255.0
  network 100.99.2.4 mask 255.255.255.252
  network 200.99.2.4 mask 255.255.255.252
exit
!
end

```
