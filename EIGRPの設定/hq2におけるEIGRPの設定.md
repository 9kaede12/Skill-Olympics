configure terminal
!
router eigrp A2024
 address-family ipv4 unicast autonomous-system 100
  network 172.17.0.9 0.0.0.0
  network 172.17.0.22 0.0.0.0
  network 172.17.100.253 0.0.0.0
  network 172.17.0.13 0.0.0.0
  network 10.0.0.1 0.0.0.0
  network 100.100.100.22 0.0.0.0
  network 200.200.200.22 0.0.0.0
  !
  af-interface GigabitEthernet0/2.100
   passive-interface
  exit-af-interface
  !
  topology base
   redistribute bgp 300 route-map BGP_TO_EIGRP_HQ2
   redistribute ospf 100 route-map OSPF_TO_EIGRP
   default-metric 10000 100 255 1 1500
  exit-af-topology
 exit-address-family
exit
!
route-map BGP_TO_EIGRP_HQ2 permit 10
 match ip address prefix-list DEFAULT_ROUTE_ONLY
 set metric 20000 200 255 1 1500
exit
!
route-map BGP_TO_EIGRP_HQ2 permit 20
 match ip address prefix-list PREFIX_200_99_0_16
 set metric 10000 100 255 1 1500
exit
!
route-map BGP_TO_EIGRP_HQ2 deny 30
 match ip address prefix-list PREFIX_100_99_0_16
exit
!
route-map BGP_TO_EIGRP_HQ2 permit 40
exit
!
route-map OSPF_TO_EIGRP permit 10
 set metric 10000 100 255 1 1500
exit
!
ip prefix-list DEFAULT_ROUTE_ONLY seq 5 permit 0.0.0.0/0
ip prefix-list PREFIX_100_99_0_16 seq 5 permit 100.99.0.0/16
ip prefix-list PREFIX_200_99_0_16 seq 5 permit 200.99.0.0/16
!
end
