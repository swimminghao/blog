---
title: nas定时任务
tags:
  - nas
categories: 工具
abbrlink: b2fba1f
date: 2022-11-03 14:31:00
---

# nas定时任务



```bash
0 23 * * * /opt/bin/cp -rf ~/.zshrc /share/CACHEDEV1_DATA/zsh
0 23 * * * /opt/bin/cp -rf ~/.zsh_history /share/CACHEDEV1_DATA/zsh
0 23 * * * /opt/bin/cp -rf ~/.oh-my-zsh /share/CACHEDEV1_DATA/zsh
```

