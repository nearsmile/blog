# Mac工具

## 终端工具

### 按照homebrew - 安装软件活插件神器

- 安装

```bash
ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

### zsh

- 安装[oh my zsh](https://github.com/robbyrussell/oh-my-zsh)

```bash
// curl
sh -c "$(curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
// wget
sh -c "$(wget https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh -O -)"
```

### autojump - 目录之间快速跳转

- brew install autojump - 需要先安装homebrew
- 在 .zshrc 中找到 plugins=，在后面添加`plugins=(git autojump)`
- 在 ./zshrc文件添加命令

```bash
[[ -s $(brew --prefix)/etc/profile.d/autojump.sh ]] && . $(brew --prefix)/etc/profile.d/autojump.sh
```

- 使用

```bash
// 触发跳转起作用 - 之前使用跳转过的的历史记录
autojump xxx
j xxx
```

### Chrome浏览器跨域问题

- [解决方案stackoverflow](https://stackoverflow.com/questions/3102819/disable-same-origin-policy-in-chrome)

```bash
// Win10

chrome.exe --user-data-dir="C:/Chrome dev session" --disable-web-security

// Mac
// 重启电脑缓存数据会丢失

open -n -a /Applications/Google\ Chrome.app --args --user-data-dir="/tmp/chrome_dev_session" --disable-web-security

// 需要保留终端窗口

/Applications/Google\ Chrome.app/Contents/MacOS/Google\ Chrome --disable-web-security --user-data-dir=~/ChromeUserData
```