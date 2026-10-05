SW1#sh run
Building configuration...

Current configuration : 9619 bytes
!
version 15.0
no service timestamps log datetime msec
no service timestamps debug datetime msec
service password-encryption
!
hostname SW1
!
enable secret 5 $1$mERr$QBeDfNlBwwgcSSgj1Y5Xn1
!
!
!
ip ssh version 2
no ip domain-lookup
ip domain-name gjwholesalers.local
!
username netadmin secret 5 $1$mERr$ifDae2VO/yFnNJejF9Cdn1
!
ip arp inspection vlan 10,20,30,40
!
ip dhcp snooping vlan 10,20,30,40
no ip dhcp snooping information option
ip dhcp snooping
!
spanning-tree mode pvst
spanning-tree extend system-id
!
interface FastEthernet0/1
 switchport access vlan 10
 ip dhcp snooping limit rate 15
 switchport mode access
 switchport nonegotiate
 switchport voice vlan 40
 switchport port-security
 switchport port-security maximum 3
 switchport port-security mac-address sticky 
 switchport port-security violation restrict 
 switchport port-security mac-address sticky 0002.16D9.9AE0
 switchport port-security mac-address sticky 0060.709B.9C23
 spanning-tree portfast
 spanning-tree bpduguard enable
!
interface FastEthernet0/2
 switchport access vlan 10
 ip dhcp snooping limit rate 15
 switchport mode access
 switchport nonegotiate
 switchport voice vlan 40
 switchport port-security
 switchport port-security maximum 3
 switchport port-security mac-address sticky 
 switchport port-security violation restrict 
 switchport port-security mac-address sticky 0001.972C.0ADA
 switchport port-security mac-address sticky 0003.E4CB.D598
 spanning-tree portfast
 spanning-tree bpduguard enable
!
interface FastEthernet0/3
 switchport access vlan 10
 ip arp inspection trust
 ip dhcp snooping limit rate 15
 switchport mode access
 switchport nonegotiate
 switchport port-security
 switchport port-security mac-address sticky 
 switchport port-security violation restrict 
 switchport port-security mac-address sticky 0030.F266.7740
 spanning-tree portfast
 spanning-tree bpduguard enable
!
interface FastEthernet0/4
 switchport access vlan 10
 ip dhcp snooping limit rate 15
 switchport mode access
 switchport nonegotiate
 switchport port-security
 switchport port-security maximum 5
 switchport port-security mac-address sticky 
 switchport port-security violation restrict 
 switchport port-security mac-address sticky 0060.5C01.E9E5
 switchport port-security mac-address sticky 00E0.F7CA.5D03
 spanning-tree portfast
 spanning-tree bpduguard enable
!
interface FastEthernet0/5
 switchport access vlan 20
 ip dhcp snooping limit rate 15
 switchport mode access
 switchport nonegotiate
 switchport voice vlan 40
 switchport port-security
 switchport port-security maximum 3
 switchport port-security mac-address sticky 
 switchport port-security violation restrict 
 switchport port-security mac-address sticky 0060.2F11.5467
 switchport port-security mac-address sticky 0060.2F8E.B503
 spanning-tree portfast
 spanning-tree bpduguard enable
!
interface FastEthernet0/6
 switchport access vlan 20
 ip arp inspection trust
 ip dhcp snooping limit rate 15
 switchport mode access
 switchport nonegotiate
 switchport port-security
 switchport port-security mac-address sticky 
 switchport port-security violation restrict 
 switchport port-security mac-address sticky 0004.9AD3.7129
 spanning-tree portfast
 spanning-tree bpduguard enable
!
interface FastEthernet0/7
 switchport access vlan 20
 ip dhcp snooping limit rate 15
 switchport mode access
 switchport nonegotiate
 switchport port-security
 switchport port-security maximum 5
 switchport port-security mac-address sticky 
 switchport port-security violation restrict 
 switchport port-security mac-address sticky 0000.0CAD.B98C
 switchport port-security mac-address sticky 0090.2B08.6611
 spanning-tree portfast
 spanning-tree bpduguard enable
!
interface FastEthernet0/8
 switchport access vlan 30
 ip dhcp snooping limit rate 15
 switchport mode access
 switchport nonegotiate
 switchport voice vlan 40
 switchport port-security
 switchport port-security maximum 3
 switchport port-security mac-address sticky 
 switchport port-security violation restrict 
 switchport port-security mac-address sticky 000A.F3E0.E2DE
 switchport port-security mac-address sticky 00D0.BC21.ADA5
 spanning-tree portfast
 spanning-tree bpduguard enable
!
interface FastEthernet0/9
 switchport access vlan 30
 ip arp inspection trust
 ip dhcp snooping limit rate 15
 switchport mode access
 switchport nonegotiate
 switchport port-security
 switchport port-security mac-address sticky 
 switchport port-security violation restrict 
 switchport port-security mac-address sticky 0001.4372.97B5
 spanning-tree portfast
 spanning-tree bpduguard enable
!
interface FastEthernet0/10
 switchport access vlan 30
 ip dhcp snooping limit rate 15
 switchport mode access
 switchport nonegotiate
 switchport port-security
 switchport port-security maximum 5
 switchport port-security mac-address sticky 
 switchport port-security violation restrict 
 switchport port-security mac-address sticky 0001.644D.E759
 switchport port-security mac-address sticky 0001.9730.E5C8
 spanning-tree portfast
 spanning-tree bpduguard enable
!
interface FastEthernet0/11
 switchport access vlan 50
 ip dhcp snooping limit rate 15
 switchport mode access
 switchport nonegotiate
 switchport port-security
 switchport port-security mac-address sticky 
 switchport port-security violation restrict 
 switchport port-security mac-address sticky 000C.CF1A.1D17
 spanning-tree portfast
 spanning-tree bpduguard enable
!
interface FastEthernet0/12
 switchport access vlan 50
 ip dhcp snooping limit rate 15
 switchport mode access
 switchport nonegotiate
 switchport port-security
 switchport port-security mac-address sticky 
 switchport port-security violation restrict 
 switchport port-security mac-address sticky 0060.3E0D.BADC
 spanning-tree portfast
 spanning-tree bpduguard enable
!
interface FastEthernet0/13
 switchport access vlan 50
 ip dhcp snooping limit rate 15
 switchport mode access
 switchport nonegotiate
 switchport port-security
 switchport port-security mac-address sticky 
 switchport port-security violation restrict 
 switchport port-security mac-address sticky 000C.852B.A816
 spanning-tree portfast
 spanning-tree bpduguard enable
!
interface FastEthernet0/14
 switchport access vlan 50
 ip dhcp snooping limit rate 15
 switchport mode access
 switchport nonegotiate
 switchport port-security
 switchport port-security mac-address sticky 
 switchport port-security violation restrict 
 switchport port-security mac-address sticky 0004.9AB0.99C4
 spanning-tree portfast
 spanning-tree bpduguard enable
!
interface FastEthernet0/15
 switchport access vlan 50
 ip dhcp snooping limit rate 15
 switchport mode access
 switchport nonegotiate
 switchport port-security
 switchport port-security mac-address sticky 
 switchport port-security violation restrict 
 switchport port-security mac-address sticky 0060.4754.E269
 spanning-tree portfast
 spanning-tree bpduguard enable
!
interface FastEthernet0/16
 switchport access vlan 50
 ip dhcp snooping limit rate 15
 switchport mode access
 switchport nonegotiate
 switchport port-security
 switchport port-security mac-address sticky 
 switchport port-security violation restrict 
 switchport port-security mac-address sticky 0030.A3E2.ED87
 spanning-tree portfast
 spanning-tree bpduguard enable
!
interface FastEthernet0/17
 switchport access vlan 50
 ip dhcp snooping limit rate 15
 switchport mode access
 switchport nonegotiate
 switchport port-security
 switchport port-security mac-address sticky 
 switchport port-security violation restrict 
 switchport port-security mac-address sticky 0001.9764.36C5
 spanning-tree portfast
 spanning-tree bpduguard enable
!
interface FastEthernet0/18
 switchport access vlan 50
 ip dhcp snooping limit rate 15
 switchport mode access
 switchport nonegotiate
 switchport port-security
 switchport port-security mac-address sticky 
 switchport port-security violation restrict 
 switchport port-security mac-address sticky 0001.C731.B670
 spanning-tree portfast
 spanning-tree bpduguard enable
!
interface FastEthernet0/19
 switchport access vlan 998
 switchport mode access
 switchport nonegotiate
 shutdown
!
interface FastEthernet0/20
 switchport access vlan 998
 switchport mode access
 switchport nonegotiate
 shutdown
!
interface FastEthernet0/21
 switchport access vlan 998
 switchport mode access
 switchport nonegotiate
 shutdown
!
interface FastEthernet0/22
 switchport access vlan 998
 switchport mode access
 switchport nonegotiate
 shutdown
!
interface FastEthernet0/23
 switchport access vlan 998
 switchport mode access
 switchport nonegotiate
 shutdown
!
interface FastEthernet0/24
 switchport access vlan 998
 switchport mode access
 switchport nonegotiate
 shutdown
!
interface GigabitEthernet0/1
 switchport trunk native vlan 999
 switchport trunk allowed vlan 10,20,30,40,50,99
 ip arp inspection trust
 ip dhcp snooping trust
 switchport mode trunk
 switchport nonegotiate
!
interface GigabitEthernet0/2
 switchport access vlan 998
 switchport mode access
 switchport nonegotiate
 shutdown
!
interface Vlan1
 no ip address
 shutdown
!
interface Vlan99
 ip address 192.168.99.2 255.255.255.0
!
ip default-gateway 192.168.99.1
!
banner motd ^C Authorised access only. Unauthorised access is prohibited ^C
!
!
!
access-list 10 permit 192.168.99.0 0.0.0.255
access-list 10 permit 192.168.10.0 0.0.0.255
line con 0
 logging synchronous
 login local
 exec-timeout 15 0
!
line vty 0 4
 access-class 10 in
 logging synchronous
 login local
 transport input ssh
line vty 5 15
 access-class 10 in
 logging synchronous
 login local
 transport input ssh
!
!
!
ntp server 192.168.99.1
!
end