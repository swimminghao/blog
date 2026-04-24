---
title: zsh必备插件
tags:
  - 工具
categories: 终端
abbrlink: 143a11e3
date: 2026-04-24 16:29:40
---
# zsh必备插件

```bash
#!/bin/bash
if [ "$EUID" -eq 0 ]; then
    echo "请勿使用 sudo 或以 root 身份运行此脚本。请以普通用户执行。"
    exit 1
fi
set -e  # 遇到错误立即退出

# 安装依赖
sudo yum install -y git zsh

# 修改当前用户的登录 shell
chsh -s "$(which zsh)"

# 自动安装 Oh My Zsh（非交互、不自动启动 zsh）
export RUNZSH=no
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)" "" --unattended


ZSH_CUSTOM_DIR="${ZSH_CUSTOM:-$HOME/.oh-my-zsh/custom}"
mkdir -p "$ZSH_CUSTOM_DIR/plugins"
# 克隆插件（浅克隆）
git clone --depth=1 https://github.com/zsh-users/zsh-syntax-highlighting.git "$ZSH_CUSTOM_DIR/plugins/zsh-syntax-highlighting"
git clone --depth=1 https://github.com/paulirish/git-open.git "$ZSH_CUSTOM_DIR/plugins/git-open"
git clone --depth=1 https://github.com/zsh-users/zsh-autosuggestions.git "$ZSH_CUSTOM_DIR/plugins/zsh-autosuggestions"

# 修改插件列表（使用转义后的 sed 模式）
sed -i.bak1 's/plugins=\([^)]*\)/plugins=(cp git zsh-autosuggestions zsh-syntax-highlighting sudo git-open brew extract z)/' ~/.zshrc

# 安装 powerlevel10k 主题
git clone --depth=1 https://github.com/romkatv/powerlevel10k.git "$ZSH_CUSTOM_DIR/themes/powerlevel10k"
echo 'source $ZSH_CUSTOM_DIR/themes/powerlevel10k/powerlevel10k.zsh-theme' >>~/.zshrc
source ~/.zshrc
# 启动 zsh（替换当前 shell）
exec zsh

```