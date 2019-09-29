# Study SNMP Trap packets of VyOS

## Link Up/Down
`vyos-link.pcap`

Commands

```
vyos@vyos-1# set interfaces ethernet eth1 disable
[edit]
vyos@vyos-1# commit
[edit]
vyos@vyos-1# delete interfaces ethernet eth1 disable
[edit]
vyos@vyos-1# commit
[edit]
```

Output of tcpdump

```
$ tcpdump -ttttnn -r vyos-link.pcap
reading from file vyos-link.pcap, link-type EN10MB (Ethernet)
2019-09-29 22:56:34.293243 IP 172.16.178.130.33031 > 172.16.178.131.162:  V2Trap(201)  .1.3.6.1.2.1.1.3.0=2867800 .1.3.6.1.6.3.1.1.4.1.0=.1.3.6.1.6.3.1.1.5.3 .1.3.6.1.2.1.2.2.1.1.3=3 .1.3.6.1.2.1.2.2.1.2.3="VMware VMXNET3 Ethernet Controller" .1.3.6.1.2.1.2.2.1.3.3=6 .1.3.6.1.2.1.2.2.1.7.3=2 .1.3.6.1.2.1.2.2.1.8.3=2 .1.3.6.1.6.3.1.1.4.3.0=.1.3.6.1.4.1.8072.3.2.10
2019-09-29 22:56:54.299261 IP 172.16.178.130.33031 > 172.16.178.131.162:  V2Trap(201)  .1.3.6.1.2.1.1.3.0=2869800 .1.3.6.1.6.3.1.1.4.1.0=.1.3.6.1.6.3.1.1.5.4 .1.3.6.1.2.1.2.2.1.1.3=3 .1.3.6.1.2.1.2.2.1.2.3="VMware VMXNET3 Ethernet Controller" .1.3.6.1.2.1.2.2.1.3.3=6 .1.3.6.1.2.1.2.2.1.7.3=1 .1.3.6.1.2.1.2.2.1.8.3=1 .1.3.6.1.6.3.1.1.4.3.0=.1.3.6.1.4.1.8072.3.2.10
```

Output of snmptrapd

```
=====
vyos-1
UDP: [172.16.178.130]:33031->[172.16.178.131]:162
DISMAN-EVENT-MIB::sysUpTimeInstance 0:7:57:58.00
SNMPv2-MIB::snmpTrapOID.0 IF-MIB::linkDown
IF-MIB::ifIndex.3 3
IF-MIB::ifDescr.3 VMware VMXNET3 Ethernet Controller
IF-MIB::ifType.3 ethernetCsmacd
IF-MIB::ifAdminStatus.3 down
IF-MIB::ifOperStatus.3 down
SNMPv2-MIB::snmpTrapEnterprise.0 NET-SNMP-TC::linux
=====
vyos-1
UDP: [172.16.178.130]:33031->[172.16.178.131]:162
DISMAN-EVENT-MIB::sysUpTimeInstance 0:7:58:18.00
SNMPv2-MIB::snmpTrapOID.0 IF-MIB::linkUp
IF-MIB::ifIndex.3 3
IF-MIB::ifDescr.3 VMware VMXNET3 Ethernet Controller
IF-MIB::ifType.3 ethernetCsmacd
IF-MIB::ifAdminStatus.3 up
IF-MIB::ifOperStatus.3 up
SNMPv2-MIB::snmpTrapEnterprise.0 NET-SNMP-TC::linux
=====
```

## BGP Peer Up/Down
`vyos-bgp.pcap`

Commands

```
vyos@vyos-1# set protocols bgp 65001 neighbor 192.168.47.2 shutdown
[edit]
vyos@vyos-1# commit
[edit]
vyos@vyos-1# delete protocols bgp 65001 neighbor 192.168.47.2 shutdown
[edit]
vyos@vyos-1# commit
[edit]
```

Output of tcpdump

```
$ tcpdump -ttttnn -r vyos-bgp.pcap
reading from file vyos-bgp.pcap, link-type EN10MB (Ethernet)
2019-09-29 22:57:41.023689 IP 172.16.178.130.33031 > 172.16.178.131.162:  V2Trap(130)  .1.3.6.1.2.1.1.3.0=2874473 .1.3.6.1.6.3.1.1.4.1.0=.1.3.6.1.4.1.3317.1.2.2.0.2 .1.3.6.1.2.1.15.3.1.14.192.168.47.2=00_00 .1.3.6.1.2.1.15.3.1.2.192.168.47.2=6 .1.3.6.1.6.3.1.1.4.3.0=.1.3.6.1.4.1.3317.1.2.2
2019-09-29 22:57:41.024004 IP 172.16.178.129.46222 > 172.16.178.131.162:  V2Trap(130)  .1.3.6.1.2.1.1.3.0=2873402 .1.3.6.1.6.3.1.1.4.1.0=.1.3.6.1.4.1.3317.1.2.2.0.2 .1.3.6.1.2.1.15.3.1.14.192.168.47.1=06_02 .1.3.6.1.2.1.15.3.1.2.192.168.47.1=6 .1.3.6.1.6.3.1.1.4.3.0=.1.3.6.1.4.1.3317.1.2.2
2019-09-29 22:57:58.157009 IP 172.16.178.130.33031 > 172.16.178.131.162:  V2Trap(130)  .1.3.6.1.2.1.1.3.0=2876186 .1.3.6.1.6.3.1.1.4.1.0=.1.3.6.1.4.1.3317.1.2.2.0.1 .1.3.6.1.2.1.15.3.1.14.192.168.47.2=00_00 .1.3.6.1.2.1.15.3.1.2.192.168.47.2=6 .1.3.6.1.6.3.1.1.4.3.0=.1.3.6.1.4.1.3317.1.2.2
2019-09-29 22:57:58.157263 IP 172.16.178.129.46222 > 172.16.178.131.162:  V2Trap(130)  .1.3.6.1.2.1.1.3.0=2875115 .1.3.6.1.6.3.1.1.4.1.0=.1.3.6.1.4.1.3317.1.2.2.0.1 .1.3.6.1.2.1.15.3.1.14.192.168.47.1=00_00 .1.3.6.1.2.1.15.3.1.2.192.168.47.1=6 .1.3.6.1.6.3.1.1.4.3.0=.1.3.6.1.4.1.3317.1.2.2
```

Output of snmptrapd

```
=====
vyos-1
UDP: [172.16.178.130]:33031->[172.16.178.131]:162
DISMAN-EVENT-MIB::sysUpTimeInstance 0:7:59:04.73
SNMPv2-MIB::snmpTrapOID.0 GNOME-PRODUCT-ZEBRA-MIB::bgpd.0.2
BGP4-MIB::bgpPeerLastError.192.168.47.2 "00 00 "
BGP4-MIB::bgpPeerState.192.168.47.2 established
SNMPv2-MIB::snmpTrapEnterprise.0 GNOME-PRODUCT-ZEBRA-MIB::bgpd
=====
vyos-2
UDP: [172.16.178.129]:46222->[172.16.178.131]:162
DISMAN-EVENT-MIB::sysUpTimeInstance 0:7:58:54.02
SNMPv2-MIB::snmpTrapOID.0 GNOME-PRODUCT-ZEBRA-MIB::bgpd.0.2
BGP4-MIB::bgpPeerLastError.192.168.47.1 "06 02 "
BGP4-MIB::bgpPeerState.192.168.47.1 established
SNMPv2-MIB::snmpTrapEnterprise.0 GNOME-PRODUCT-ZEBRA-MIB::bgpd
=====
vyos-1
UDP: [172.16.178.130]:33031->[172.16.178.131]:162
DISMAN-EVENT-MIB::sysUpTimeInstance 0:7:59:21.86
SNMPv2-MIB::snmpTrapOID.0 GNOME-PRODUCT-ZEBRA-MIB::bgpd.0.1
BGP4-MIB::bgpPeerLastError.192.168.47.2 "00 00 "
BGP4-MIB::bgpPeerState.192.168.47.2 established
SNMPv2-MIB::snmpTrapEnterprise.0 GNOME-PRODUCT-ZEBRA-MIB::bgpd
=====
vyos-2
UDP: [172.16.178.129]:46222->[172.16.178.131]:162
DISMAN-EVENT-MIB::sysUpTimeInstance 0:7:59:11.15
SNMPv2-MIB::snmpTrapOID.0 GNOME-PRODUCT-ZEBRA-MIB::bgpd.0.1
BGP4-MIB::bgpPeerLastError.192.168.47.1 "00 00 "
BGP4-MIB::bgpPeerState.192.168.47.1 established
SNMPv2-MIB::snmpTrapEnterprise.0 GNOME-PRODUCT-ZEBRA-MIB::bgpd
=====
```
