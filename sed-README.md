# commands for sed

### edit key and value pair in a configuration file (does exact match of key name)

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

