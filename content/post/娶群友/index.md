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

本文提供了关于yunzaiQQBOT娶群友开发思路，适用于普通QQbot无按钮<br>
注:仅供参考,仅适用于有一点JavaScript基础的人看
<!-- more-->

## 娶群友到底是什么逻辑？
### 获取单个用户头像
他的逻辑只是单纯靠一个固定获取头像的链接,例如我常用的:
```js
const imgUrl = await e.member?.getAvatarUrl?.() || await e.friend?.getAvatarUrl?.() || `http://q2.qlogo.cn/headimg_dl?dst_uin=${e.user_id}&spec=5`
```
中间的e.user_id是用户的(此id并非QQid)id/QQ号，记住这个东西，获取用户头像的时候需要这个，我们只需要获取到用户QQ号把QQ号填入到这个括号内把{e.user_id}替换成{QQ号}这样就可以获取到***一个用户*** 的QQ头像√，随后bot输出你今天的老婆是\*\*\*[图片]看好他哦…………
### id绑定
随后把用户id和随机取到的另一个用户id进行存入绑定，用什么绑定都可以数据库,JSON什么的都行，如果同样的人再次发送娶群友命令，则检测他的id是否存在于数据库或者JSON文件等如果存在就告诉他你已经有老婆了什么巴拉巴拉~，不存在就随机抽一个群友呗
## 如何随机获取多个用户的头像呢？
很简单将全部id塞进去就刑惹~<br>
但是我们要怎么获取到用户的id呢？因为是QQBOT(**无按钮**)的原因我们可以用用户的QQ号去获取头像，哎?!突然有个人问:

<div style="
    background: #f5f5f5;
    border-radius: 8px;
    padding: 12px;
    margin: 12px 0;
    border-left: 4px solid #ddd;
    line-height: 1.6;
">
<div>
   <span style="color: blue; font-weight: bold;">B:</span>
   <span> 既然可以用QQ号那为什么不能直接用tx给的用户id去获取头像呢?方便快捷,使用QQ号的话获取很麻烦哎?(tx不允许获取用户个人信息,把id换成了一串乱码)</span>
</div>

<div>
   <span style="color: red; font-weight: bold;">A:</span>
   <span> 因为tx给的id直接填进去是无效的,填进去后获取的依然只能是自己的头像</span>
</div>

<div>
   <span style="color: red; font-weight: bold;">A:</span>
   <span> 嗯……tx给的id虽然可以实现但是也仅限有按钮权限的QQBOT</span>
</div>
</div>
我们只需要一个正则<strong>^娶群友(\d*)</strong>匹配用户输入的东西,例如:娶群友1145145,我们用这个正则娶匹配他将输入的QQ号存入到数据库 or JSON文件,同时放在一个默认的列表里面例如(这里我用JSON文件演示):

```js
>this.Marry_List.default.push(userId)
// JSON文件中呈现的结果是:default:["1145145"]
```

如果他是在群里输入这条命令的话则把群id也一起写入方便做群隔离

```js
>const groupId = 123456789 //假设群号是这个奥
>this.Marry_keys.push(groupId)
>this.Marry_keys[groupId].push(userId)
// JSON文件中呈现的结果是:"123456789":["1145145"]
```
这样就将用户的QQ号存下来当不同的用户再次发生**娶群友**时会从当前群列表内随机抽一个，这一步我们可以通过<span style="color: #76AF14">random</span>函数来完成，随后将这个QQ号放到获取头像的链接中即可
### 如何获取用户的id
很简单在用户第一次使用的时候顺便给他发个使用引导(例如:娶群友+QQ号)，告诉你这个东西怎么用，随后把他的id存入到数据库或者JSON文件，如果第二次使用的时候检测到用户存在则不发这条提示，这样就实现了**低效**获取用户id的方法了√<br>
### 啥？你问我高效获取?
单官bot确定不行，但是！咱不是还有野生bot麻~野生bot薅用户信息，把用户信息存入，官bot使用，如果你要麻烦点的话你人工收集也可以喽？
## 创建default的作用是什么？将群id也一并存入的作用是什么？
很简单就是方便做隔离以防群与群之间冲突，创建default的目的是做备用，可以理解为当当前群id不存在JSON文件或数据库的时候则直接从default中拿用户名单然后随机取一个用户，如果当前用户的环境是私聊也可以这么使用，如果你不想要群隔离可以只存入default