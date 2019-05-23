# commands for sed

### edit configuration file that has spaces between key and value pair (does exact match of key name)

sample snip of auditd.conf before sed:
```bash
max_log_file_action = ROTATE
space_left = 75
```

bash script to edit file:
```bash
#!/bin/bash
SEARCH_KEY=space_left
NEW_VALUE=3866

sed -i "/#/!s/^\($SEARCH_KEY[[:space:]]*=[[:space:]]*\)\(.*\)/\1$NEW_VALUE/" /etc/audit/auditd.conf
```

sample snip of auditd.conf after sed:
```bash
max_log_file_action = ROTATE
space_left = 3866
```


### edit configuration file that has no spaces between key and value pair (does exact match of key name)

sample snip of network interface:
```bash
BOOTPROTO=dhcp
TYPE=Ethernet
```

bash script to edit file:
```bash
#!/bin/bash

SEARCH_KEY=BOOTPROTO
NEW_VALUE=none

sed -i "s/^$SEARCH_KEY.*/$SEARCH_KEY=$NEW_VALUE/g" /etc/sysconfig/network-scripts/ifcfg-enp0s8
```

sample snip of network interface:
```bash
BOOTPROTO=none
TYPE=Ethernet
```
