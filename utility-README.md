# commands for utility (commands a redhat sa would use)

### search for word in a file, get line number
```bash
grep -rnw -e "word"
```

### quick search, ignore capitalization
```bash
grep -i selinux /var/log/messages
```

### loop/error check/variables
```bash
#!/bin/bash

dnlist='dn-0109-18 dn-0109-20 dn-0109-26'

for i in $dnlist; do
	ssh -o StrictHostKeyChecking=no root@$i "service hadoop-hdfs-datanode restart"
	if [ $? -ne 0 ]; then
                echo "ERROR: the datanode service on $i did not restart"
                exit 1
        else
                echo "SUCCESS: the datanode service on $i was restarted"
        fi
done
```

### list all service units and whether they are activated at boot
```bash
systemctl list-unit-files --type=service
```

### examine network ports and user
```bash
lsof -Pni | grep zookeeper
```

### script with common loops,variables,cut
```bash
#!/bin/bash

if [ $# -gt 1 ]; then
  echo "Error: too many arguments"
  exit 1
fi

if [ $# -eq 0 ]; then
  username=$USER
else
  username=$1
fi

userinfo=$(getent passwd $username)

if [ $? -ne 0 ]; then
  echo "Error: cannot retrieve information for user $username"
  exit 2
fi

usershell=$(echo $userinfo | cut -f 7 -d ":")

echo "$username's shell is $usershell"
exit 0
```

### create md5 checksum of file and place checksums into file
```bash
md5sum test.txt > checksum.txt
```

### check the md5 checksums placed in the file for any modifications
```bash
md5sum -c checksum.txt
```

### if loop to test if file exists
```bash
#!/bin/bash

if [ -f /tmp/test.txt ]; then
  echo "test passed"
else
  echo "test failed"
fi
```

### for loop to go through users on system
```bash
#!/bin/bash

for i in $(getent passwd | cut -f 1 -d ":"); do
  echo "$i is a great user"
done
```

### to find out where a command comes from in yum
```bash
yum provides semanage
```

### send email from command line
```bash
echo -e "Subject: History of the World Volume 2 \n\n It all began in 1997." | sendmail -v root@localhost
```

### config file for secure apache
```bash
<Directory "/www/docs/big.dev.net/cubs">
    AuthType Basic
    AuthName "By Invitation Only"
    # Optional line:
    AuthBasicProvider file
    AuthUserFile /etc/httpd/passwd/passwords
    AuthGroupFile /etc/httpd/passwd/groups
    <RequireAll>
    Require group Cubs
    Require ip 192.168.56.104
    </RequireAll>
</Directory>

<VirtualHost *:80>
  ServerAdmin webmaster@big.dev.net
  DocumentRoot /www/docs/big.dev.net
  ServerName big.dev.net
  ErrorLog logs/big.dev.net-error_log
  TransferLog logs/big.dev.net-access_log
</VirtualHost>
```

### rhce postfix steps
```bash
1) setup postfix server (5 mys, 1 extra)
2) setup postfix null client (5 mys, 3 extra)
3) test with sendmail account to fred@dev.net
```

### rpm gpg keys
list rpm gpg keys:
```bash
rpm -q gpg-pubkey
```bash

remove an rpm gpg key:
```bash
rpm -e gpg-pubkey-f4a80eb5-53a7ff4b
```bash

add an rpm gpg key:
```bash
rpm --import RPM-GPG-KEY-CentOS-7
```
