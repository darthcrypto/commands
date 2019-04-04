# Commands for SSH Keys

## setup ssh keys
1) create ssh key pair (for this example, keys were named vf18 and vf18.pub)
```bash
ssh-keygen
```

2) ensure /etc/hosts has correct aliases
```bash
192.168.56.101 v1
192.168.56.102 v2
192.168.56.103 v3
```

3) add contents to ~/.ssh/config (the private key is the identity file)
```bash
Host v1
User root
IdentityFile ~/.ssh/vf18
Port 22

Host v2
User root
IdentityFile ~/.ssh/vf18
Port 22

Host v3
User root
IdentityFile ~/.ssh/vf18
Port 22
```

4) change permissions
```bash
chmod 700 .ssh
chmod 600 authorized_keys
```

5) on redhat systems activatge the ssh shell
```bash
ssh-agent $SHELL
```

6) add private key as identity file, adds passphrase to mac key manager....
```bash
ssh-add ~/.ssh/vf18
```

7) test functionality with for loop
```bash
for i in v{1..3};do ssh -qex -t $i 'echo "";date;hostname;yum -y update;init 6';done
```

#verify your gitlab login
ssh -T git@gitlab.com

  git config --global user.email "you@example.com"
  git config --global user.name "Your Name"
