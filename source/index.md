---
title: mtnb - MeiTuan Native Bridge

language_tabs:
  - javascript

toc_footers:
  - <a href='http://code.dianpingoa.com/ed-f2e/mtnb' target="about:_blank;">项目地址</a>
  - <a href='http://code.dianpingoa.com/ed-f2e/mtnb_h5_demo'>mtnb h5 demo</a>

search: true
---

# 概述

MTNB(meituan native bridge)，是用来在混血应用开发中打通客户端应用与网页应用信道的桥梁。MTNB也作为美团移动桥协议在webapp中的命名空间存在。
[详细文档](http://wiki.sankuai.com/display/DEVPUB/Meituan+Native+Bridge)

# 引入

使用MTNB需要通过BA认证，因此需要后端输出authInfo，前端使用authInfo鉴权，通过鉴权才可以使用jsbridge提供的功能。

## 后端
后端需要给前端页面提供鉴权信息，点评环境-商家端(e.dianping.com)如需使用可以直接调用商家平台的服务，详情请咨询朱凯(kevin.zhu)。
[详细的API文档](http://wiki.sankuai.com/display/DEVPUB/mtnb-auth-server++API+v1)

## 前端
mtnb模块目前仅支持通过Cortex通过CommonJS标准的方式引入。
require('mtnb');

# 初始化

请在页面onload之后调用init方法。
<aside class="warning">MTNB中所有和native相关的接口都需要在鉴权成功后使用。</aside>

```javascript
MTNB.init({
    "nonceStr":"qzpccxe1dgbhjjo",
    "ts":1433347371266,
    "url":"http://103.227.76.185/webview",          
    "sign":"a7b8aafa2750515638179c13e1923cd336b47e04"
}, function (res) {
    if (res.status === 0) {
        // 鉴权成功
    } else {
        // 鉴权失败
    }
});
```

# webview模块

## 关闭当前webview
type | name | 描述
--- | ---- | ----
string | anime | 关闭时执行的动画，如果不传，按照打开的反向动画执行

```javascript
MTNB.use('webview.close', {
	anime: "slideleft"
});
```
## 打开新的webview
type | name | 说明
--- | --- | ----
string | url	| 打开的地址
string	| anime	| 动画类型：swipeLeft, swipeRight 左右切换；fadeIn, fadeOut 淡入淡出

```javascript
MTNB.use('webview.open', {
	url: 'http://i.meiutan.com/',
	anime: "slideleft"
});
```

## 设置webview标题
type | name | 说明
--- | --- | ----
function	|callback	| 监听标题点击的回调
string | title	| 标题名称

```javascript
MTNB.use('webview.setTitle', {
    title: "一个很长很长的标题",
    callback: function () {
        // 标题被点击后的操作...
    }
});
```

## 设置复杂的webview标题
<aside class="warning">不支持换行</aside>
type | name | 说明
--- | --- | ----
string	| title | 标题名称，使用font来实现
function | callback | 监听标题点击的回调

```javascript
MTNB.use('webview.setHtmlTitle', {
    title: "<font size="4" face="arial" color="black">演唱会 </font><font size="2" color="black"> 北京</font><font size="1" color="black">▼</font>",
    callback: function () {
        // 标题被点击后的操作...
    }
});
```


## 设置角标

type | name | 说明
--- | --- | ----
object|	data|	附加数据，其中的callback为事件触发后的回调，参数是{}
string|	type|	icon类型，“text”文字，“share”分享
```javascript
// 分享
MTNB.use('webview.setIcon', {
    type: "share",
    channel: 385,
    url: location.href,
    title: document.title,
    pic: $('img').attr('src'),
    callback: function () {
        // 分享完成后的回调
    }
});
// 链接
MTNB.use('webview.setIcon', {
    type: "text",
    text: "退款帮助",
    url: "i.meituan.com/help/refund",
    callback: function () {
        // 点击分享的回调
    }
});
// 回调
MTNB.use('webview.setIcon', {
    type: "text",
    text: "设置提醒",
    callback: function () {
        // 点击提醒的回调
    },
});
// 设置多个icon
MTNB.use('webview.setIcon', [
    {
        type: "text",
        text: "设置提醒",
        callback: function () {
            // 交互
        }
    },
    {
        type: "text",
        text: "收藏",
        callback: function () {
            // 交互
        }
    }
]);
```

# UI模块

## alert

## confirm

## toast

## tooltip

## loading

