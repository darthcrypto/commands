# commands for gitlab

### setting up signing gpg keys
```bash
https://docs.gitlab.com/ee/user/project/repository/gpg_signed_commits/
```


### verify your gitlab login 
```bash
ssh -T git@gitlab.com
```

### setup global accounts
```bash
git config --global user.email "you@example.com" 
git config --global user.name "Your Name"
```

### troubleshooting "can't sign errors"
```bash
export GPG_TTY=$(tty)
echo $GPG_TTY
```
