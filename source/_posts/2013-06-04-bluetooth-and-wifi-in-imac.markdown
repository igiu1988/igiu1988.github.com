---
layout: post
title: "iMac蓝牙与wifi信号"
date: 2013-06-04 13:42
comments: true
categories: 
---
在新公司终于使用上了iMac，甚是高兴，可是用着用着发现magic mouse总是掉帧，输入文字时偶尔也会变得很卡，鼠标的问题尤为严重，于是google之，的确有同样的问题。其中有一个帖子说是与其用的wifi信号频道（强度）有关系，于是将iMac连接到了公司的另一个wifi信号上，果然蓝牙设备不再掉帧了。具体原因我说不清，只提供一些证据

* iMac型号MD093CH/A，使用的无线芯片是Broadcom BCM43xx，集成了wifi与bluetooth
* 先前的wifi信号强度是-52dBm，后来的wifi信号强度是-26dBm
* 查看蓝牙设备的RSSI，基本都在-52dBm上下波动
