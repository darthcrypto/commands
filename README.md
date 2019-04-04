# Commands for SSH Keys

## setup ssh keys
```bash
ssh-keygen
```


#ensure /etc/hosts has correct aliases

#add contents to ~/.ssh/config:
Host v1
User root
IdentityFile ~/.ssh/vf18
Port 22

Host v2
User root
IdentityFile ~/.ssh/vf18
Port 22

#chmod 700 .ssh
#chmod 600 authorized_keys

#use on redhat....
ssh-agent $SHELL

#add private key as identity file, adds passphrase to mac key manager....
ssh-add ~/.ssh/vf18
#public key

```bash
for i in v{1..3};do ssh -qex -t $i 'echo "";date;hostname;yum -y update;init 6';done
```

#verify your gitlab login
ssh -T git@gitlab.com

  git config --global user.email "you@example.com"
  git config --global user.name "Your Name"
