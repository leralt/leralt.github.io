---
tags:
  - 工具
  - zsh
---
## 安装zsh & oh my zsh
```bash
sudo apt install zsh
```
```bash
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```
## 安装插件
### zsh-autosuggestions
```bash
git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
```
在.zshrc中plugins中添加zsh-autosuggestions
### zsh-syntax-highlighting
```bash
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting
```
在.zshrc中plugins中添加zsh-syntax-highlighting

## 配置补全快捷键
将`shift + tab`配置为接受补全建议
```bash
echo "bindkey '^[[Z' autosuggest-accept" >> ~/.zshrc
```
## 解决autosuggestion显示为白色
```bash
echo "export TERM=xterm-256color" >> ~/.zshrc
```
最后一定要`source`才会生效
```bash
source ~/.zshrc
```