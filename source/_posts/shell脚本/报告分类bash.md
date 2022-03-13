---
title: 报告分类bash
date: 2022-02-28 19:57:47
tags: [编程,感悟]
categories: 技术
---
# 报告分类bash

```java
for file in *.pdf; do mkdir -p -- "${file%%-*}" && \
    mv -- "$file" "${file%%-*}"; done
