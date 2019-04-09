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
