## MAC系统安装
### 启动盘制作
```
sudo /Applications/Install\ OS\ X\ Yosemite.app/Contents/Resources/createinstallmedia --volume /Volumes/U盘名称 --applicationpath /Applications/Install\ OS\ X\ Yosemite.app --nointeraction
```

### Backup
- 关键目录 ```documents```、```github```

### Homebrew
```
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

### Homebrew-cask
- brew tap caskroom/cask

### 配置提速
- 在~/.curlrc文件中输入代理地址即可。如果你的服务端速度够快的话，brew的下载速度简直就是飞起= =。
```
socks5 = "127.0.0.1:49361" # lantern的地址
```

### iterm2 && oh-my-zsh
- brew cask install item2
- oh-my-zsh
```
sh -c "$(curl -fsSL https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
```
- 配置主题，编辑~/.zshrc
```
ZSH_THEME="ys"
```

### 正式安装软件
- brew cask sogouinput
- brew cask google-chrome
- brew cask iina
- brew cask the-unarchiver
- brew cask install cheatsheet
- brew cask isntall the-unarchiver
- brew cask install cleanmymac
- brew cask install thunder
- brew cask install atom
- brew cask install caskroom/versions/java8
- brew cask install dbvisualizer
- brew cask install pycharm
- brew cask install intellij-idea
- brew cask install microsoft-office
### SSH
```
ssh-keygen -t rsa -C lijiang
```

### Hosts
- vi /etc/hosts
```
0.0.0.0         account.jetbrains.com
```

### chrome SwitchyOmega
- Rule List URL ```https://raw.githubusercontent.com/gfwlist/gfwlist/master/gfwlist.txt```

### Python3 Install
- run ```brew install python3```

### Pip Install
- To install pip, securely download [get-pip.py](https://bootstrap.pypa.io/get-pip.py)
- run ```python get-pip.py```/ ```python get-pip.py --user```

### Ipython install
- run ```sudo pip install ipython```
    * python2.7 有问题 需要联网解决