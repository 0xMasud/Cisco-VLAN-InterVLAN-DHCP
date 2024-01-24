###

https://github.com/0xMasud

#Sw1

```
enable
configure terminal
hostname Sw1

no ip domain-lookup

line console 0
exec-timeout 0
end

write

show ip interface brief

```
### verify
```
show running-config

show vlan brief

```
### VLAN Naming

#Sw1

```
configure terminal

vlan 10
name HR
exit

vlan 20
name Accounts
exit

vlan 30
name Admin
exit

vlan 40
name Staffs
end

write

show vlan brief
```

### assign ports

### Sw1

```
enable
configure terminal

interface range fastEthernet 0/1-2
switchport mode access
switchport access vlan 10
exit

interface range fastEthernet 0/3-4
switchport mode access
switchport access vlan 20
exit

interface range fastEthernet 0/5-6
switchport mode access
switchport access vlan 30
exit

interface range fastEthernet 0/7-8
switchport mode access
switchport access vlan 40
end

write

show vlan brief
```

#Sw2

```
enable
configure terminal
hostname Sw2

no ip domain-lookup

line console 0
exec-timeout 0
end

write

show ip interface brief

```
### verify
```
show running-config

show vlan brief

```
### VLAN Naming

#Sw2

```
configure terminal

vlan 10
name HR
exit

vlan 20
name Accounts
exit

vlan 30
name Admin
exit

vlan 40
name Staffs
end

write

show vlan brief
```

### assign ports

### Sw2

```
configure terminal

interface range fastEthernet 0/1-2
switchport mode access
switchport access vlan 10
exit

interface range fastEthernet 0/3-4
switchport mode access
switchport access vlan 20
exit

interface range fastEthernet 0/5-6
switchport mode access
switchport access vlan 30
exit

interface range fastEthernet 0/7-8
switchport mode access
switchport access vlan 40
end

write

show vlan brief
```
### Sw2 lan port trunk'in
```
configure terminal

interface fastEthernet 0/9
switchport mode 
switchport trunk allowed vlan 10,20,30,40
end

write

show interface fastEthernet 0/9 switchport

```
### Sw1 lan port trunk'in
```
enable
configure terminal

interface fastEthernet 0/9
switchport mode 
switchport trunk allowed vlan 10,20,30,40
end

write

show interface fastEthernet 0/9 switchport

```
### Sw1 to R1 VLAN 

```
configure terminal
interface fastEthernet 0/10
switchport trunk allowed vlan all
end

write

```
### R1 config

```
enable 
configure terminal

no ip domain-lookup

line console 0
exec-timeout 0

interface gigabitEthernet 0/0
no shutdown

exit

interface gigabitEthernet 0/0.10

encapsulation dot1Q 10

ip address 192.168.10.1 255.255.255.128

exit


interface gigabitEthernet 0/0.20
encapsulation dot1Q 20

ip address 192.168.10.129 255.255.255.192

exit


interface gigabitEthernet 0/0.30
encapsulation dot1Q 30

ip address 192.168.10.193 255.255.255.224

exit


interface gigabitEthernet 0/0.40
encapsulation dot1Q 40

ip address 192.168.10.225 255.255.255.224

end

write

show ip interface brief

```
### dhcp pool'in #dramatically if you run pool'in script again in router it did fix/ complete DHCP-server. dont know why...

```
configure terminal

ip dhcp pool HR

network 192.168.10.0 255.255.255.128
default-router 192.168.10.254
dns-server 8.8.8.8
domain-name abc.com

exit


ip dhcp pool Accounts

network 192.168.10.128 255.255.255.192
default-router 192.168.10.254
dns-server 8.8.8.8
domain-name abc.com

exit

ip dhcp pool Admin

network 192.168.10.192 255.255.255.224
default-router 192.168.10.254
dns-server 8.8.8.8
domain-name abc.com

exit

ip dhcp pool Staffs

network 192.168.10.224 255.255.255.224
default-router 192.168.10.254
dns-server 8.8.8.8
domain-name abc.com

end

write

show running-config | section dhcp

```

