# S1
```
!
hostname S1
!
no ip cef
!
interface Ethernet0/0
 switchport trunk encapsulation dot1q
 switchport mode trunk
!
interface Ethernet0/1
 switchport access vlan 127
 switchport mode access
!
interface Ethernet0/2
 switchport access vlan 127
 switchport mode access
!
```
# S2
```
!
hostname S2
!
no ip cef
!
interface Ethernet0/0
 switchport trunk encapsulation dot1q
 switchport mode trunk
!
interface Ethernet0/1
 switchport access vlan 128
 switchport mode access
!
interface Ethernet0/2
 switchport access vlan 128
 switchport mode access
!
```
# S3
```
!
hostname S3
!
no ip cef
!
interface Port-channel1
 no switchport
 ip address 172.16.0.1 255.255.255.252
!
interface Ethernet0/1
 no switchport
 no ip address
 duplex auto
 channel-group 1 mode active
!
interface Ethernet0/2
 no switchport
 no ip address
 duplex auto
 channel-group 1 mode active
!
interface Vlan127
 ip address 192.168.127.1 255.255.255.0
!
router rip
 version 2
 network 172.16.0.0
 network 192.168.127.0
```
# S4
```
!
hostname S4
!
no ip cef
!
interface Port-channel1
 no switchport
 ip address 172.16.0.2 255.255.255.252
!
interface Ethernet0/1
 no switchport
 no ip address
 duplex auto
 channel-group 1 mode active
!
interface Ethernet0/2
 no switchport
 no ip address
 duplex auto
 channel-group 1 mode active
!
interface Vlan128
 ip address 192.168.128.1 255.255.255.0
!
router rip
 version 2
 network 172.16.0.0
 network 192.168.128.0
!
```
# R1
```
!
hostname R1
!
```
