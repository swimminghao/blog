---
title: 使用 Clash 时使用的 Steam 规则
tags:
  - clash
categories: 科学上网
abbrlink: 4bec2351
date: 2026-05-16 20:23:14
---
# 使用 Clash 时使用的 Steam 规则

## 简介

在使用梯子时，Clash Verge 的自定义规则可能时常不好用，经常导致 Steam 连接到国外服务器，导致国内下载很慢 / 耗流量。

本文使用 Clash Verge 2.0.3 来编写 Steam 规则。



## 配置全局扩展脚本

我们可以通过 Clash 的 *全局扩展脚本*（通常在配置文件中名为 `rule-providers` 和 `rules` 部分）来精细控制 Steam 的走代理行为，避免它连接到国外服务器下载游戏。

在主界面找到【全局扩展脚本】，右键，编辑文件，复制下面内容覆盖原文件

```
const prependRules = [
  // lan ip
  "DOMAIN,steamcdn-a.akamaihd.net,DIRECT",
  "DOMAIN-SUFFIX,cm.steampowered.com,DIRECT",
  "DOMAIN-SUFFIX,steamserver.net,DIRECT",
  "DOMAIN-SUFFIX,steamcontent.com,DIRECT"
];

function main(config, profileName) {
  config.rules = prependRules.concat(config.rules);
  return config;
}
```

你可以将 Steam 社区和商店页面指定走代理，但下载相关域名设为直连。

这里的 DIRECT，可以替换为你所需要的，如果你想用代理下载，可以替换为你的策略组名字。

## 验证规则是否生效

在 Clash Verge 中打开「连接」界面，观察访问 `store.steampowered.com` 时是否走代理，而 `content.steampowered.com` 是否走直连即可。

你也可以查看 Clash 的日志输出，确认每个域名是否命中预期规则。

## 小贴士

- Steam CDN 域名大多为 `steamcontent.com` 和 `steamusercontent.com`，这部分建议走直连。
- `steamcommunity.com` 等社区功能如果不走代理可能无法加载头像或聊天功能。