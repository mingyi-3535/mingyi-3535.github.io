+++
date = '2025-06-18T20:44:48+08:00'
draft = false
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

本文提供了关于yunzaiQQBOT娶群友开发思路，适用于普通QQbot无按钮<br>
注:仅供参考,仅适用于有一点JavaScript基础的人看
<!-- more-->

## 娶群友到底是什么逻辑？
### 获取单个用户头像
他的逻辑只是单纯靠一个固定获取头像的链接,例如我常用的:
```
const imgUrl = await e.member?.getAvatarUrl?.() || await e.friend?.getAvatarUrl?.() || `http://q2.qlogo.cn/headimg_dl?dst_uin=${e.user_id}&spec=5`
```
中间的e.user_id是用户的(此id并非QQid)id/QQ号，记住这个东西，获取用户头像的时候需要这个，我们只需要获取到用户QQ号把QQ号填入到这个括号内把{e.user_id}替换成{QQ号}这样就可以获取到***一个用户*** 的QQ头像√，随后bot输出你今天的老婆是\*\*\*[图片]看好他哦…………
### id绑定
随后把用户id和随机取到的另一个用户id进行存入绑定，用什么绑定都可以数据库,JSON什么的都行，如果同样的人再次发送娶群友命令，则检测他的id是否存在于数据库或者JSON文件等如果存在就告诉他你已经有老婆了什么巴拉巴拉~，不存在就随机抽一个群友呗
### 如何获取多个用户的头像呢？
很简单将全部id塞进去就刑惹~<br>
但是我们要怎么获取到用户的id呢？因为是QQBOT(**无按钮**)的原因我们可以用用户的QQ号去获取头像，哎?!突然有个人问:
```
B:既然可以用QQ号那为什么不能直接用tx给的用户id去获取头像呢?方便快捷,使用QQ号
只能让用户输入QQ号,QQBOT没办法直接获取口牙?
A:因为tx给的id直接填进去是无效的,填进去后获取的依然只能是自己的头像
A:嗯……tx给的id虽然可以实现但是也仅限有按钮权限的QQBOT
```