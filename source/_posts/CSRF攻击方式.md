---
title: CSRF攻击方式
categories: 安全
date: 2020-11-01 10:00:00
updated: 2020-11-01 10:00:00
tag:
  - 前端
  - 安全
  - HTTP
toc: true
thumbnail: "/images/safe.jpg"
---

## CSRF 是什么？

`CSRF`（`Cross-site request forgery`），中文名称：跨域请求伪造。

## CSRF 可以做什么？

你这可以这么理解 CSRF 攻击：`攻击者盗用了你的身份，以你的名义发送恶意请求`。`CSRF`能够做的事情包括：以你名义发送邮件，发消息，盗取你的账号，甚至于购买商品，虚拟货币转账......造成的问题包括：个人隐私泄露以及财产安全。

<!--more-->

## CSRF 的原理

我们下面根据一个时序图来模拟一下这样的思想：

![CSRF攻击](https://upload-images.jianshu.io/upload_images/13681871-2f2d98599487a693.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

从上面的图中我们可以看出，要完成一次`CSRF`的攻击，受害者必须要完成两个步骤： 1.登录受信任的网站`A`，并在本地生成了`cookie` 2.在不登出`A`的情况下，访问危险网站`B`
但是我们看到了，如果我们："不满足上面的两个条件的一个，就不会受到`CSRF`的攻击"。是的，就是这样的，但是我们不能保证下面的情况不会发生。 1.我们不能保证我们登录一个网站之后，不再打开`tab`页面访问另外的一个网站。 2.不能保证关闭浏览器之后，本地的`cookie`马上过期，我上次的会话已经过期（事实上，关闭浏览器并不意味着结束了一个会话，但大多数人都会认为关闭浏览器等于退出\结束一个会话）。 3.上面的攻击一个网站，可能是一个存在其他漏掉的可信任的被人经常访问的网站。

## 示例 1

银行网站 A，它以 GET 请求来完成银行，如：`http://www.mybank.com/Transfer.php?toBankId=11&money=1000`
而对于危险的网站，它里面有一段 HTML 的代码如下：

```
　<img src=http://www.mybank.com/Transfer.php?toBankId=11&money=1000>
```

我们看一下，首先我们登录银行网站`A`，然后访问了危险网站`B`，这个时候我们发现我们的银行账户里面的钱少了 1000 元。
我们看一下原因：因为银行`A`网站违反了`HTTP`规范，使用了`GET`请求，请求了更新资源，在访问危险网站`B`之前，我们已经登录了`A`网站，而`B`的`<img>`用`GET`的方式请求了第三方的资源(这里的第三方资源就是指的是银行网站，原来一个十分合法的请求，被不合法的利用了)，所以我们的浏览器就会带上在网站`A`的 Cookie 发出的 Get 请求，去获取资源("`http://www.mybank.com/Transfer.php?toBankId=11&money=1000.`")然后银行资源获取到请求之后，认为这是一个更新资源的操作，所以就转账了，钱飞了~~~~

## 示例 2

为了杜绝上面的问题，采取了`POST`的凡是进行转账的操作
比如说银行网站`A`的`web`表单如下：

```
<form action="Transfer.php" method="POST">
　　　　<p>ToBankId: <input type="text" name="toBankId" /></p>
　　　　<p>Money: <input type="text" name="money" /></p>
　　　　<p><input type="submit" value="Transfer" /></p>
</form>
```

后台处理页面`Transfer.php`如下：

```
<?php
　　　　session_start();
　　　　if (isset($_REQUEST['toBankId'] &&　isset($_REQUEST['money']))
　　　　{
　　　　    buy_stocks($_REQUEST['toBankId'],　$_REQUEST['money']);
　　　　}
?>
```

危险网站`B`，仍然只是包含那句`HTML`代码：

```
　<img src=http://www.mybank.com/Transfer.php?toBankId=11&money=1000>
```

首先我们登录银行网站`A`，然后访问了危险网站`B`，这个时候我们发现我们的银行账户里面的钱又少了 1000 元。这次事故的原因是：银行的后台系统使用了`$_REQUEST`来获取请求的的数据，然而`$_REQUEST`既可以获取 GET 请求的数据，也可以获取 POST 请求的数据，这样就造成了后台处理程序没有办法分辨到底是`GET`请求还是`POST`请求，在`PHP`中，可以使用`$_GET`和`$_POST`分别获取`GET`请求和`POST`请求的数据。在`JAVA`中，用于获取请求数据`request`一样存在不能区分`GET`请求数据和`POST`数据的问题。

## 示例 3

经过前面 2 个惨痛的教训，银行决定把获取请求数据的方法也改了，改用`$_POST`，只获取`POST`请求的数据，后台处理页面`Transfer.php`代码如下：

```
<?php
　　　　session_start();
　　　　if (isset($_POST['toBankId'] &&　isset($_POST['money']))
　　　　{
　　　　    buy_stocks($_POST['toBankId'],　$_POST['money']);
　　　　}
?>
```

然而，危险网站`B`与时俱进，它改了一下代码:

```
<html>
　　<head>
　　　　<script type="text/javascript">
　　　　　　function steal()
　　　　　　{
          　　　　 iframe = document.frames["steal"];
　　     　　      iframe.document.Submit("transfer");
　　　　　　}
　　　　</script>
　　</head>

　　<body onload="steal()">
　　　　<iframe name="steal" display="none">
　　　　　　<form method="POST" name="transfer"　action="http://www.myBank.com/Transfer.php">
　　　　　　　　<input type="hidden" name="toBankId" value="11">
　　　　　　　　<input type="hidden" name="money" value="1000">
　　　　　　</form>
　　　　</iframe>
　　</body>
</html>
```

如果用户仍是继续上面的操作，很不幸，结果将会是再次不见 1000 块......因为这里危险网站`B`暗地里发送了 POST 请求到银行!

总结一下上面 3 个例子，`CSRF`主要的攻击模式基本上是以上的`3`种，其中以第`1,2`种最为严重，因为触发条件很简单，一个`<img>`就可以了，而第`3`种比较麻烦，需要使用`JavaScript`，所以使用的机会会比前面的少很多，但无论是哪种情况，只要触发了`CSRF`攻击，后果都有可能很严重。

理解上面的 3 种攻击模式，其实可以看出,`CSRF`攻击是源于`WEB`的隐式身份验证机制！`WEB`的身份验证机制虽然可以保证一个请求是来自于某个用户的浏览器，但却无法保证该请求是用户批准发送的！

## CSRF 的防御机制

根据看到的资料，`CSRF`的防御可以从服务端和客户端两方面着手，防御效果是从服务端着手效果比较好，现在一般的`CSRF`防御也都在服务端进行。
服务端的`CSRF`方式方法由很多中，但是思想都是一样的，就是在客户端增加伪随机数
(1)`Cookie Hashing`(所有的表单都包含同一个伪随机值)
这个方法虽然简陋，但是很有效，因为攻击者不能获取第三方的`Cookie`（理论上），所以表单数据也就构造失败了。

```
　　<?php
　　　　//构造加密的Cookie信息
　　　　$value = “DefenseSCRF”;
　　　　setcookie(”cookie”, $value, time()+3600);
　　?>
```

在表单里增加`Hash`值，以认证这确实是用户发送的请求。

```
　<?php
　　　　$hash = md5($_COOKIE['cookie']);
　　?>
　　<form method=”POST” action=”transfer.php”>
　　　　<input type=”text” name=”toBankId”>
　　　　<input type=”text” name=”money”>
　　　　<input type=”hidden” name=”hash” value=”<?=$hash;?>”>
　　　　<input type=”submit” name=”submit” value=”Submit”>
　　</form>
```

然后在服务器端进行`Hash`值验证:

```
  <?php
　　      if(isset($_POST['check'])) {
     　　      $hash = md5($_COOKIE['cookie']);
          　　 if($_POST['check'] == $hash) {
               　　 doJob();
　　           } else {
　　　　　　　　//...
          　　 }
　　      } else {
　　　　　　//...
　　      }
      ?>
```

 这个方法大概已经可以杜绝`99%`的`CSRF`攻击了，那还有`1%`呢....由于用户的`Cookie`很容易由于网站的`XSS`漏洞而被盗取，这就另外的`1%`。一般的攻击者看到有需要算`Hash`值，基本都会放弃了，某些除外，所以如果需要`100%`的杜绝，这个不是最好的方法。
(2)验证码
这个方案的实例是：每一次的用户提交都需要用户在表单上填写一个验证码，这个方案可以完全的解决`CSRF`，但是在用户体现上不交差，而且还会产生一些被称为`MHTML`的`Bug`
(3).`One-Time Tokens`(不同的表单包含一个不同的伪随机值)
在实现`One-Time Tokens`时，需要注意一点：就是“并行会话的兼容”。如果用户在一个站点上同时打开了两个不同的表单，`CSRF`保护措施不应该影响到他对任何表单的提交。考虑一下如果每次表单被装入时站点生成一个伪随机值来覆盖以前的伪随机值将会发生什么情况：用户只能成功地提交他最后打开的表单，因为所有其他的表单都含有非法的伪随机值。必须小心操作以确保`CSRF`保护措施不会影响选项卡式的浏览或者利用多个浏览器窗口浏览一个站点。可以添加当前时间的时间戳或是随机数字。
盗一段代码：

- 先是令牌生成函数`(gen_token()):`

```
<?php
     function gen_token() {
 　　　//这里我是贪方便，实际上单使用Rand()得出的随机数作为令牌，也是不安全的。
　　　　//这个可以参考我写的Findbugs笔记中的《Random object created and used only once》
          $token = md5(uniqid(rand(), true));
          return $token;
     }
```

- 然后是 Session 令牌生成函数`(gen_stoken())`

```
<?php
     function gen_stoken() {
　　　　　$pToken = "";
　　　　　　if($_SESSION[STOKEN_NAME]  == $pToken){
　　　　　　　　//没有值，赋新值
　　　　　　　　$_SESSION[STOKEN_NAME] = gen_token();
　　　　　　}
　　　　　　else{
　　　　　　　　//继续使用旧的值
　　　　　　}
     　　}
     ?>
```

- `WEB`表单生成隐藏输入域的函数

```
 <?php
　　     function gen_input() {
     　　     gen_stoken();
　　          echo “<input type=\”hidden\” name=\”" . FTOKEN_NAME . “\”
          　　     value=\”" . $_SESSION[STOKEN_NAME] . “\”> “;
     　　}
     ?>
```

- `WEB`表单结构

```
<?php
          session_start();
          include(”functions.php”);
     ?>
     <form method=”POST” action=”transfer.php”>
          <input type=”text” name=”toBankId”>
          <input type=”text” name=”money”>
          <? gen_input(); ?>
          <input type=”submit” name=”submit” value=”Submit”>
     </FORM>
```

5).服务端核对令牌：
这个很简单，这里就不再啰嗦了。
[这篇文章还不错](https://somefuture.iteye.com/blog/2282894)
