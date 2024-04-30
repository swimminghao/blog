---
title: mac catalina 手动安装openssl@3
tags:
  - mac
categories: 工具
abbrlink: 243c12fc
date: 2024-04-30 23:30:00
---

# mac catalina 手动安装openssl@3

mac catalina 通过homebrew 安装 openssl 失败，被嫌弃系统太老，make test 测试不通过

I manually installed openssl@3 with the following commands

1. Download the latest version from `https://www.openssl.org/source/openssl-3.1.1.tar.gz`
2. Uncompress the file with double click and open the terminal. Find the created folder and cd openssl-3.1.1
3. Continue with
   `perl ./Configure --prefix=/usr/local --openssldir=/usr/local/openssl no-ssl3 no-ssl3-method no-zlib darwin64-x86_64-cc enable-ec_nistp_64_gcc_128`
4. `make`
5. `make test` (optional) I passed all tests
6. `sudo make install MANDIR=/usr/local/Cellar/openssl@3/3.1.1_1/share/man MANSUFFIX=ssl`
7. `openssl version` (optional - verification). It should report OpenSSL 3.1.1 30 May 2023 (Library: OpenSSL 3.1.1 30 May 2023)
8. `which -a openssl` (optional).
9. `brew link openssl@3`
10. `brew list` (optional) you should see the openssl@3 inside installed packages.