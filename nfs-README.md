# commands for setting up nfs on rhel 7.0

### server config
```bash
yum install nfs-utils -y
systemctl start nfs-server;systemctl enable nfs-server
yum install rpcbind -y
systemctl start rpcbind; systemctl enable rpcbind
firewall-cmd --permanent --zone=public --add-rich-rule="rule family=ipv4 source address=192.16
8.56.0/24 service name=nfs accept"
firewall-cmd --permanent --zone=public --add-rich-rule="rule family=ipv4 source address=192.16
8.56.0/24 service name=mountd accept"
firewall-cmd --permanent --zone=public --add-rich-rule="rule family=ipv4 source address=192.16
8.56.0/24 service name=rpc-bind accept"
config exports file (see below)
vi /etc/sysconfig/nfs     RPCNFSDARGS="-V 4.2"
systemctl restart nfs
systemctl start nfs-secure; systemctl enable nfs-secure
exportfs -rv
selinux the shared folders
```

### exports config file:
```bash
/secure 192.168.56.0/24(rw,sync,sec=krb5p)
/public 192.168.56.0/24(ro,sync)
```


