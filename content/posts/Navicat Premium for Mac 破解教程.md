---
title: Navicat Premium for Mac 破解教程
date: 2018-09-01
updated: 2018-09-01
author: yu-l.in
tags: 
- Navicat
- Mac
- RSA公匙
copyright: true
featured_image: https://img02.unless.online/file/unless/andrew-neel-1332001.jpg
---
![藏宝图](https://i.loli.net/2020/02/14/jEBbMIF463PGfu8.jpg)
本文仅以探讨破解思路，请支持正版
![b2](https://img02.unless.online/file/unless/andrew-neel-1332001.jpg)

# 破解思路

替换RSA加密算法公匙（Navicat Premium for Mac中的公匙位于程序包目录的rpk文件中）

# 生成自己的RSA公匙私匙对

使用open ssl工具生成，当然也可以使用其它工具[在线生成工具](http://web.chacuo.net/netrsakeypair)生成，這里需要注意的是`密匙是2048位，PKCS#8格式`

# 替换RSA公匙

*   版本12.0.24之前，公匙存储在**Navicat Premium.app/Contents/Resources/rpk**，用文本编辑器打开它，替换上述生成的公匙
*   12.0.24之后，公匙不再存储在**Navicat Premium.app/Contents/Resources/rpk**中，而是放在了Navicat的二进制执行文件**Navicat Premium.app/Contents/MacOS/Navicat Premium**中，通过搜索`-----BEGIN PUBLIC KEY-----`找到它

    # 算出有效的Mac版序列号密匙

*   序列号是一个16字节长度的字符串，是经过Base32编码的，其实际存储长度为80位（8字节），输入密匙后程序仍然会用Base33编码，以二进制形式存储比对。 正常的Base32编码表顺序是 `char EncodeTable[]=‘ABCDEFGHIJKLMNOPQRSTUVWXYZ234567’` 80位长的二进制数据，可转换成为10个十六进制，如 
 * 第一个8位二进制数据：68 /原作者表示不知是什么，但不能改变； 
 * 第二个8位二进制数据：2A /原作者表示不知是什么，但不能改变； 
 * 第三个8位二进制数据：00 /原作者表示不知是什么，但可任意改变； 
 * 第四个8位二进制数据：00 /原作者表示不知是什么，但可任意改变； 
 * 第五个8位二进制数据：00 /原作者表示不知是什么，但可任意改变； 
 * 第六个8位二进制数据：AA /第六个和第七个组合使用； 
 * 第七个8位二进制数据：99 /第六个和第七个组合使用，目前已知0xAC 0x88代表英文版，0xCE 0x32代表简体中文版，0xAA 0x99代表繁体中文版，0xAD 0x82代表日本语，0xBB 0x55代表Polski，0xAE 0x10代表Español，0xFA 0x20代表Français，0xB1 0x60代表Deutsch，0xB5 0x60代表한국어，0xEE 0x16代表Русский，0xCD 0x49代表Português； 
 * 第八个8位二进制数据：65 /代表商业许可类型，0x65代表企业版，0x66代表教育版，0x67代表精简版；（以上数据为Navicat12版本） 
 * 第九个8位二进制数据：C0 /这个8位数据的前4位必须是1100，转换为十进制就是12，代表版本12，数据的后4位不知道代表什么，但是可以延迟激活使用时间，后4位可以是0000或0001；（以上数据为Navicat12版本） 
 * 第十个8位二进制数据：FF /代表許可的期限权利类型，0xFB代表30天不可转售许可，0xFC代表90天不可转售许可，0xFD代表365天不可转售许可，0xFE代表不可转售许可，0xFF代表站点许可； 
 * 如此，得到繁体中文版密匙的原始数据：`68 2A 00 00 00 AA 99 65 C0 FF`

*   对密匙後8个8位数据進行DES对称加密，使用DES加密算法，并采用ECB模式。这里，需要加密的是为`00 00 00 AA 99 65 C0 FF`，DES加密密匙是`unsigned char DESKey = { 0x64, 0xAD, 0xF3, 0x2F, 0xAE, 0xF2, 0x1A, 0x27 }`，计算得出加密后的序列号密匙数据，并将原始数据转换为二进制，按每5位一组，进行Base32编码，转为十进制，按找Base32编码表，便能得到了相应版本的密匙。

    # 解密激活请求码，生成激活码

*   启动程序后，提示注册，**断开网络，阻止程序联网**，然后点击注册

*   根据提示，点击手动激活，弹出离线激活请求，得到了程序给出的请求码（这是一个Base64编码的字符串，代表的是长度为256字节的数据，这是离线激活请求信息被激活公匙加密的密文

*   经过RSA配对的私匙解密后，能得到激活请求信息（json风格） `{ “K“: “xxxxxxxxxxxxxxxx”， “P”: “Mac 10.13”， “DI”: “xxxxxxxxxxxxxxxxxxxx” }`这里包含了3个key：“K”、“DI”、“P”，分別代表序列号、设备识别号（与电脑硬件信息相关）和平台（操作系統类型）

*   仍然通过RSA配对的私钥，使用上述的“K”和“DI”信息的json风格 `{ “DI” : “xxxxxxxxxxxxxxxxxxxx”, “T” : “1535808471.925012”, “K” : “xxxxxxxxxxxxxxxx”, “N” : “DoubleLabyrinth”, “O” : “Shadow” }`这里包含了5个key：“K”、“DI”、“N”、“T” 、“O”，分別代表序列号、设备识别号（与电脑硬件信息相关）、注册名、授权时间、组织。**其中“T”是必须的，且代表的时间必须位于当前时间-1~+4天之內，采用的是unix时间戳**

*   输入得到的离线激活回复信息，激活完毕。

參考鏈接： [Double Sine](https://github.com/DoubleLabyrinth/navicat-keygen.git)
