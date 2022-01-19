## go mod 私有仓库

```bash
git config --global --add url."git@gitlab.sz.sensetime.com:".insteadOf "https://gitlab.sz.sensetime.com"
```



```bash
# $HOME/.netrc
machine gitlab.bj.sensetime.com
login sanghaifa
password Sensetime!@#12345

machine gitlab.sz.sensetime.com
login sanghaifa
password Sensetime!@#12345
>>> EOF
```

```bash
# $HOME/.gitconfig
[url "git@gitlab.bj.sensetime.com:"]
        insteadOf = https://gitlab.bj.sensetime.com/
[url "git@gitlab.sz.sensetime.com:"]
        insteadOf = https://gitlab.sz.sensetime.com/

```

