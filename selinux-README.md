# commands for selinux

### change the context for all /www and all files in it
```bash
semanage fcontext -a -t httpd_sys_content_t '/www(/.*)?'
```

### persist selinux changes
```bash
restorecon -R /www
```

### change the selinux user of a file (this will persist a reboot)
```bash
chcon -u system_u file1
```

### change an seboolean and persist the change
```bash
setsebool -P httpd_enable_homedirs 1
```

