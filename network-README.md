# commands for network

### activate interfaces
```bash
ifup eth1
ifdown eth1
```

### create a static route to 192.168.123.123
1) create file /etc/sysconfig/network-scripts/route-enp0s8
```bash
192.168.123.123/24 via 192.168.56.101 dev enp0s8
```

2) restart network
```bash
service network restart
```
