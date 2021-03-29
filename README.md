---
description: >-
  Drogon is a fast and efficient web application framework written in modern
  C++. It runs on all sort of devices; from embedded systems to large servers.
  These docs teaches you how to use it.
---

# About Drogon

## What is Drogon

Drogon is a community developed C++ web application framework. It provides an event driven, asynchronous, cross-platform and highly efficient environment for building web applications. Just like NodeJS, PHP and Golang. Furthermore, Drogon provides a PHP-like server side rendering system for JS-less and other scenarios.

Drogon's repository: [https://github.com/an-tao/drogon](https://github.com/an-tao/drogon)

Drogon's documentation: [https://drogon.docsforge.com](https://drogon.docsforge.com)

### Advantages of Drogon

1. Drogon is fast. While I their goal isn't being \#1, it is good to know that the framework can scale. Even if you don’t need the scale, you can run Drogon on resource-restricted hardware with a pretty decent performance.
2. Drogon has a very open development model. Every contribution is welcome, the developers are pretty open to every PR if it doesn’t affect performance very much.
3. Drogon is licensed under MIT. A very liberal and allows to use the framework for commercial projects.
4. Drogon is cross platform. It runs on Linux, Windows, OS X, \*BSD and ReactOS!

### Installation

#### Arch Linux

{% hint style="info" %}
Installation on different Linux distros requires a different process. Please search your distro's repository
{% endhint %}

```
$ yay -S drogon-git
```

#### Build from source \(\*NIX, including OS X\)

Please install the dependencies before running the commands. Required dependencies include: [jsoncpp](https://github.com/open-source-parsers/jsoncpp) and [zlib](https://zlib.net/). And optional ones include: sqlite3, mariadb/mysql, postgresql, brotli, [herdis](https://github.com/zond/herdis), [c-ares](https://c-ares.haxx.se/) and [openssl](https://www.openssl.org/).

```text
git clone https://github.com/an-tao/drogon --recursive
mkdir drogon/build
cd drogon/build
cmake ..
make -j
sudo make install
```

#### Windows \(vcpkg\)

```text
vcpkg install drogon[core, ctl]:x86-windows
vcpkg integrate install
```

### Verify Installation

Once you installed Drogon. You can open a terminal/command prompt and type `drogon_ctl version`. You should see the version of Drogon that is installed to your computer.

{% hint style="info" %}
This check doesn't work on Windows, installed via vcpkg. Please trust that you have installed everything correctly.
{% endhint %}

```text
❯ drogon_ctl version
     _                             
  __| |_ __ ___   __ _  ___  _ __  
 / _` | '__/ _ \ / _` |/ _ \| '_ \ 
| (_| | | | (_) | (_| | (_) | | | |
 \__,_|_|  \___/ \__, |\___/|_| |_|
                 |___/             

A utility for drogon
Version: 1.4.1
Git commit: d22ce4a8487463f6f43639eed945f6b9e58f1438
Compilation: 
  Compiler: /usr/bin/c++
  Compiler ID: GNU
  Compilation flags: -O3 -DNDEBUG -std=c++17 -I/usr/include
Libraries: 
  postgresql: yes  (batch mode: no)
  mariadb: yes
  sqlite3: yes
  openssl: yes
  brotli: yes
  boost: no
  hiredis: yes
  c-ares: yes
```

