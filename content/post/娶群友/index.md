+++
date = '2025-06-18T20:44:48+08:00'
draft = true
title = '娶群友'
description="分享一下自己写yunzaiQQBOT插件的思路。"
tags=[
	"yunzai",
	"娶群友"
]
categories = [
	"QQBOT",
	"娶群友",
	"思路分享"
]
image = "浏览器壁纸.png"
+++

本文提供了关于yunzaiQQBOT娶群友开发思路，适用于普通QQbot无按钮
注:仅供参考
<!-- more-->

##娶群友到底是什么逻辑？
他的逻辑只是单纯靠一个固定获取头像的链接例如:
```
const imgUrl = await e.member?.getAvatarUrl?.() || await e.friend?.getAvatarUrl?.() || `http://q2.qlogo.cn/headimg_dl?dst_uin=${e.user_id}&spec=5`
```