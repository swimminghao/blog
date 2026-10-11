---
title: windows自带微软拼音中使用小鹤双拼方案
tags:
  - windows
categories: 工具
abbrlink: 3238d161
date: 2026-10-11 22:09:08
---

```
cmd输入:

reg add HKCU\Software\Microsoft\InputMethod\Settings\CHS /v UserDefinedDoublePinyinScheme0 /t REG_SZ /d "小鹤双拼*2*^*iuvdjhcwfg^xmlnpbksqszxkrltvyovt"

PowerShell输入:

Set-ItemProperty -LiteralPath 'HKCU:\Software\Microsoft\InputMethod\Settings\CHS' -Name 'UserDefinedDoublePinyinScheme0' -Type String -Value '小鹤双拼*2*^*iuvdjhcwfg^xmlnpbksqszxkrltvyovt'
```