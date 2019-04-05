# commands for utility (commands a redhat sa would use)

### search for word in a file
```bash
grep -rnw -e "word"
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

### activate interfaces
```bash
ifup eth1
ifdown eth1
```
