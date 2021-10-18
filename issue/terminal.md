## 解决Mac下Terminal、Iterm2启动慢

替换 startup shell
将 startup shell 从默认的 /usr/bin/login 改为 /bin/bash -l 或者 /usr/bin/zsh ，Terminal 和 iTerm 2 就可以秒开了。

对于 Terminal，要修改 Preferences → Startup: 从 Default login shell 改为 /bin/bash -l。

如果用的是 iTerm 2，要修改 Preferences → Profiles → General → Command: 从 Login Shell 改为 /bin/bash -l。
