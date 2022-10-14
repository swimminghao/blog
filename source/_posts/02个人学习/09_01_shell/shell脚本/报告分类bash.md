---
title: 报告分类bash
tags:
  - shell
categories: 工具
abbrlink: 9efd0db9
date: 2022-02-28 19:57:47
---

# 报告分类bash

```java
for file in *.pdf; do mkdir -p -- "${file%%-*}" && \
    mv -- "$file" "${file%%-*}"; done
