# macos中的四种配置文件

在 macOS 中，```.zprofile```、```.zshrc``` 、```.bash_profile``` 和 ```.bashrc```是四个重要的配置文件，分别用于不同的 shell 和场景。

以```profile```结尾的文件通常用于系统级别的配置，特点是在每次用户登录时都会被读取。而以```rc```结尾的文件则通常用于用户级别的配置，特点是在每次用户启动新的 shell 时都会被读取。

- .zprofile/.bash_profile：
  通常用于设置环境变量、路径和其他全局配置。
  
- .zshrc/.bashrc：
  通常用于设置 shell 提示符、别名和其他用户级别的配置。

**tips: 自 macOS Catalina (10.15) 以来，zsh 已成为 macOS 的默认 shell。**