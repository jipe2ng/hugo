---
title: Surge 2.5.3无限期试用教程
date: 2018-09-02
updated: 2018-09-02
author: yu-l.in
tags: 
- surge 2.5.3
- libfaketime
- 系统时间
copyright: true
---

本文仅以探讨 **[libfaketime](https://github.com/wolfcw/libfaketime)**的功能，请在surge试用期到期后，购买正版或卸載。
![b2](https://img02.unless.online/file/unless/andrew-neel-1332001.jpg)
# 介紹

根据README文档的描述，libfaketime拦截程序用于检索系統当前日期和时间的各类系統调用，然后将修改（或是伪造）的日期和时间（由用户自身指定）报告给这些程序，它允许修改程序看到的系统时间，而不需要在系统时间处更改时间，从而不影响其它应用对本地时间的调用。 以下內容來自直接拷贝于 **[libfaketime](https://github.com/wolfcw/libfaketime)**，详细了解，请点击联接。

```language
2、 Compatibility issues
-----------------------
- libfaketime is supposed to work on Linux and macOS.
  Your mileage may vary; some other *NIXes have been reported to work as well.
- libfaketime uses the library preload mechanism of your operating system's
  linker (which is involved in starting programs) and thus cannot work with
  statically linked binaries or binaries that have the setuid-flag set (e.g.,
  suidroot programs like "ping" or "passwd"). Please see you system linker's
  manpage for further details.
- libfaketime supports the pthreads environment. A separate library is built
  (libfaketimeMT.so.1), which contains the pthread synchronization calls. This
  library also single-threads calls through the time() intercept because
  several variables are statically cached by the library and could cause issues
  when accessed without synchronization.
  However, the performance penalty for this might be an issue for some
  applications. If this is the case, you can try using an unsynchronized time()
  intercept by removing the -DPTHREAD_SINGLETHREADED_TIME from the Makefile and
  rebuilding libfaketimeMT.so.1
* Java-/JVM-based applications work but you need to pass in an extra argument
  (DONT_FAKE_MONOTONIC).  See usage basics below for details.  Without this
  argument the java command usually hangs.
* libfaketime will eventually be bypassed by applications that dynamically load
  system libraries, such as librt, explicitly themselves instead of relying on
  the linker to do so at application start time. libfaketime will not work with
  those applications unless you can modify them.
* Applications can explicitly be designed to prevent libfaketime from working,
  e.g., by checking whether certain environment variables are set or whether
  libfaketime-specific files are present.

```

# 对surge的应用

## 安裝libfaketime

1、安裝Homebrew Mac下直接在终端下輸入下述命令

```
ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"

```

Homebrew安裝成功后，可通过**brew -v**显示Homebrew的版本号 2、Homebrew的基本操作 - 安裝软件：brew install <软件> - 搜索软件：brew search <软件> - 卸载软件：brew uninstall <软件> 3 、安裝libfaketime 通过终端安裝libfaketime和coreutils（由于依赖GNU时间，需要安裝coreutils），輸入下述命令

```
brew install libfaketime coreutils

```

安裝成功后，可在终端輸入**faketime**来查看libfaketime是否安裝成功。

## 修改Surge调用的系统时间

直接通过faketime来运行Surge，并且为程序单独伪造系统时间2018年9月1日。

```
faketime -f '@2018-09-01 00:00:00' /Applications/Surge.app/Contents/MacOS/Surge &

```

如此，便能看到Surge成功以2018年9月1日的时间运行。

## 其它说明的问题

*   此方法仅用于Surge2.5.3之前的各个版本的试用测试
*   若系统上已安裝过新版本或提示“试用期已过，请重新授权”之类的话，此方法无效
*   libfaketime可能会占用大量CPU资源，可尝试添加**FAKETIME_STOP_AFTER_SECONDS=10**(在程序启动后10秒，自动关闭libfaketime进程）
*   关闭libfaketime进程后，Surge的配置选项中，会显示剩余负数天数
*   参考链接中，更是给出了通过配合Alfred的Workflow的效果，本文不作讨论

    # libfaketime亦可对Docker的具体应用做伪造系统时间

参考链接：[子與孟子曰](https://www.daletan.win/2017/10/27/surge-for-mac-trial-crack/)
