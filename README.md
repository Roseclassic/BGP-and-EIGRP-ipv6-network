# BGP-and-EIGRP-ipv6-network
network configuration btw 6 routers and 2 pcs with ipv6 , i´m using c7200 also maning this in gns3

- expectation
![PastedImage_v4kjx4yc0sf21xfhion5adbw6uwqynpz002](https://github.com/user-attachments/assets/e9395557-aedc-425e-9437-fc38e3048442)

- i have
  ![image](https://github.com/user-attachments/assets/f66f8ad4-1e83-49b9-b0ec-a572f9ff56ab)



***Configurations***

R1
R1> enable
R1# configure terminal
R1(config)# interface loopback 0
R1(config-if)# ipv6 address 2002:ABCD:10::1/128
R1(config-if)# exit
R1(config)# interface loopback 1
R1(config-if)# ipv6 address 2002:ABCD:10::2/128
R1(config-if)# exit
R1(config)# ipv6 unicast-routing
R1(config)# interface fastEthernet 0/0
R1(config-if)# ipv6 address 2002:ABCD:1::1/64
R1(config-if)# no shutdown
R1(config)# interface serial 1/1
R1(config-if)# ipv6 address 2002:ABCD:12::1/64
R1(config-if)# no shutdown
R1# do wr
R1(config)# router bgp 100
R1(config-router)# bgp router-id 1.1.1.1
R1(config-router)# no bgp default ipv4-unicast
R1(config-router)# neighbor 2002:ABCD:12::2 remote-as 200
R1(config-router)# neighbor 2002:ABCD:1::2 remote-as 100
R1(config-router)# address-family ipv6 unicast
R1(config-router-af)# neighbor 2002:ABCD:12::2 activate
R1(config-router-af)# neighbor 2002:ABCD:1::2 activate
R1(config-router)# network 2002:ABCD:10::1/128
R1(config-router)# network 2002:ABCD:10::2/128
R1(config-router)# network 2002:ABCD:12::/64
R1(config-router)# network 2002:ABCD:1::/64
R1(config-router-af)# exit


R2
R2> enable
R2# configure terminal
R2(config)# ipv6 unicast-routing
R2(config)# interface serial 1/1
R2(config-if)# ipv6 address 2002:ABCD:12::2/64
R2(config-if)# no shutdown
R2(config)# interface serial 1/0
R2(config-if)# ipv6 address 2002:ABCD:23::1/64
R2(config-if)# no shutdown
R2# do wr
R2(config)# router bgp 200
R2(config-router)# bgp router-id 2.2.2.2
R2(config-router)# no bgp default ipv4-unicast
R2(config-router)# neighbor 2002:ABCD:12::1 remote-as 100
R2(config-router)# address-family ipv6 unicast
R2(config-router-af)# neighbor 2002:ABCD:12::1 activate
R2(config-router)# network 2002:ABCD:12::/64
R2(config-router-af)# exit
R2(config)# ipv6 router eigrp 200
R2(config-router)# network 2002:ABCD:23::/64
R2(config-router)# redistribute bgp 500 metric 1000 1 255 1 mtu 1000
R2(config-router)# redistribute eigrp 200


R3
R3> enable
R3# configure terminal
R3(config)# ipv6 unicast-routing
R3(config)# interface serial 1/0
R3(config-if)# ipv6 address 2002:ABCD:23::2/64
R3(config-if)# no shutdown
R3(config-if)# ipv6 eigrp 1
R3# do wr
R3(config)# interface serial 1/1
R3(config-if)# ipv6 address 2002:ABCD:34::1/64
R3(config-if)# no shutdown
R3# do wr
R3(config)# ipv6 router eigrp 200
R3(config-router)# network 2002:ABCD:23::/64
R3(config-router)# network 2002:ABCD:34::/64
R3# do wr


R4
R4> enable
R4# configure terminal
R4(config)# ipv6 unicast-routing
R4(config)# interface serial 1/1
R4(config-if)# ipv6 address 2002:ABCD:34::2/64
R4(config-if)# no shutdown
R4# do wr
R4(config)# interface serial 1/0
R4(config-if)# ipv6 address 2002:ABCD:45::1/64
R4(config-if)# no shutdown
R4# do wr
R4(config)# ipv6 router eigrp 201
R4(config-router)# network 2002:ABCD:45::/64
R4(config-router)# network 2002:ABCD:34::/64
R4# do wr


R5
R5> enable
R5# configure terminal
R5(config)# ipv6 unicast-routing
R5(config)# interface serial 1/0
R5(config-if)# ipv6 address 2002:ABCD:45::2/64
R5(config-if)# no shutdown
R5(config)# interface serial 1/1
R5(config-if)# ipv6 address 2002:ABCD:56::1/64
R5(config-if)# no shutdown
R5# write memory
R5(config)# router bgp 200
R5(config-router)# bgp router-id 5.5.5.5
R5(config-router)# no bgp default ipv4-unicast
R5(config-router)# network 2002:ABCD:45::/64
R5(config-router)# network 2002:ABCD:56::/64
R5(config-router)# neighbor 2002:ABCD:45::1 remote-as 200
R5(config-router)# neighbor 2002:ABCD:56::2 remote-as 300
R5(config-router)# address-family ipv6 unicast
R5(config-router-af)# neighbor 2002:ABCD:45::1 activate
R5(config-router-af)# neighbor 2002:ABCD:56::2 activate
R5# do wr


R6
R6> enable
R6# configure terminal
R6(config)# ipv6 unicast-routing
R6(config)# interface loopback 0
R6(config-if)# ipv6 address 2002:ABCD:20::1/128
R6(config-if)# exit
R6(config)# interface loopback 1
R6(config-if)# ipv6 address 2002:ABCD:20::2/128
R6(config-if)# exit
R6(config)# interface fastEthernet 0/0
R6(config-if)# ipv6 address 2002:ABCD:6::1/64
R6(config-if)# no shutdown
R6(config)# interface serial 1/1
R6(config-if)# ipv6 address 2002:ABCD:56::2/64
R6(config-if)# no shutdown
R6# do wr
R6(config)# router bgp 300
R6(config-router)# bgp router-id 6.6.6.6
R6(config-router)# no bgp default ipv4-unicast
R6(config-router)# network 2002:ABCD:20::1/128
R6(config-router)# network 2002:ABCD:20::2/128
R6(config-router)# network 2002:ABCD:6::/64
R6(config-router)# neighbor 2002:ABCD:56::1 remote-as 200
R6(config-router)# neighbor 2002:ABCD:6::2 remote-as 300
R6(config-router)# address-family ipv6 unicast
R6(config-router-af)# neighbor 2002:ABCD:56::1 activate
R6(config-router-af)# neighbor 2002:ABCD:6::2 activate
R6# do wr
