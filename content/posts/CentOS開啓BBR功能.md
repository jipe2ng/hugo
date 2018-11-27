---
title: TCP BBR擁塞控制算法
date: 2018-09-24
updated: 2018-09-24
author: yu-l.in
tags: 
- BBR
- 控制論
copyright: true
---

# TCP BBR擁塞控制算法
BBR是谷歌在2016年提出的TCP擁塞控制算法，極大地提高了TCP的吞吐量，在Linux4.9的patch中正式加入。[項目更新在這裏](https://github.com/google/bbr)
## 前提
### TCP基於丟包的擁塞控制
- 慢啓動階段：在建立新的TCP連接時，擁塞窗口（congestion window , 即cwnd）會初始化爲一個數據包大小，源斷按cwnd大小發送數據，每收到一個ACK確認，cwnd就增加一個數據包發送量，如此cwnd會隨着回路響應時間（RTT）的增加而增長。
- 擁塞避免階段：若源斷發現超時或收到3個相同的ACK時，便認爲網絡發生了擁塞，進入擁塞避免階段。慢啓動閾值（ssthresh）被設置爲cwnd的一半；如果超時，擁塞窗口被置1。當cwnd>ssthresh，TCP就會執行擁塞避免算法。
- 快速重傳階段：在這個階段中，接受方在收到失序的報文段後會馬上發出重複確認，而不要等到自己發送數據時捎帶確認。
- 快速恢復階段：



## BBR v2更新啦（2018）

## CentOS7下開啓BBR擁塞控制算法
- 查看Linux內核版本`unmae -a`
    開啓BBR的要求之一是內核版本必須在4.9以上
- 升級內核版本
  - 添加ELRepo GPG key`rpm --import https://www.elrepo.org/RPM-GPG-KEY-elrepo.org`
  - 添加源`rpm -Uvh http://www.elrepo.org/elrepo-release-7.0-2.el7.elrepo.noarch.rpm`
  - 安裝fastestmirror`yum install yum-plugin-fastestmirror`
  - 安裝最新Kernel`yum --enablerepo=elrepo-kernel install kernel-ml`
  - 切換到新內核並重啓`grub2-set-default 0`
- 開啓BBR
  - 在終端中輸入
      `vi /etc/sysctl.conf`，並加入
```
    net.core.default_qdisc=tcp_bbr
    net.ipv4.tcp_congestion_control=bbr
```
  - 檢查是否設置成功
```
    sysctl net.ipv4.tcp_available_congestion_control
    sysctl net.ipv4.tcp_congestion_control
```
- 檢查BBR是否正常運行`lsmod | grep tcp_bbr`
