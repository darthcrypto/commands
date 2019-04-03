```bash
for i in v{1..3};do ssh -qex -t $i 'echo "";date;hostname;yum -y update;init 6';done
```


