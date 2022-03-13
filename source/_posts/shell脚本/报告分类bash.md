---
title: 报告分类bash
date: 2022-02-28 19:57:47
tags: [shell]
categories: 工具
---

# 报告分类bash

```java
for file in *.pdf; do mkdir -p -- "${file%%-*}" && \
    mv -- "$file" "${file%%-*}"; done
