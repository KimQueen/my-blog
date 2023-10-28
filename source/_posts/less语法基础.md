---
title: less语法基础
categories: less
date: 2020-11-01 10:00:00
updated: 2020-11-01 10:00:00
tag:
  - 前端
  - less
  - 样式
toc: true
thumbnail: "/images/color.jpg"
---

## 变量
（1）可以将属性的值赋值给一个变量，变量为完全的常量，所以只能定义一次
```less
@pre-blue :  #5B83AD;
@after-blue :@nice-blue + #111；
#header{ color: @after-blue; }
```

<!--more -->
编译过的结果就是:
```
#header { color :#6c94be }
```
(2)也可以通过使用变量名定义为变量
```less
@fnord:'I am Kim'
@var: 'fnord';
content:@@var
```
编译过的结果就是:
```
content:'I am Kim';
```
##混合
将一些通用的属性放在一个class中，然后在例外一个class中去调用这些属性。任何的class和id或是元素都可以用这样的方式进行引入。
```less
.temp{
  border-top: dotted 1px black;
  border-bottom: solid 2px black;
}
#header a{
  color : #111;
  .temp;
}
#nav a{
  color : black;
  .temp;
}
```
编译过的结果就是:
```
#header a{
  color : #111;
  border-top: dotted 1px black;
  border-bottom: solid 2px black;
}
#nav a{
  color : black;
  border-top: dotted 1px black;
  border-bottom: solid 2px black;
}
```
##带参数的混合
（1）可以像函数一样定义一个带参数的属性集合
```less
.border-radius (@radius) {
        border-radius: @radius;
   -moz-border-radius: @radius;
-webkit-border-radius: @radius;
}

#header {
  .border-radius(4px);
}

.button {
  .border-radius(6px);  
}
```
（2）也可以给参数设置一个默认的值
```less
.border-radius (@radius : 5px) {
        border-radius: @radius;
   -moz-border-radius: @radius;
-webkit-border-radius: @radius;
}

#header {
  .border-radius(4px);
}
```
（3）也可以定义不带参数的属性集合，用于隐藏这个属性集合，不让它暴露到CSS中去。
```less
.wrap () {
  text-wrap: wrap;
  white-space: pre-wrap;
  white-space: -moz-pre-wrap;
  word-wrap: break-word;
}

pre { .wrap }
```
编译后的结果是：
```
pre {
  text-wrap: wrap;
  white-space: pre-wrap;
  white-space: -moz-pre-wrap;
  word-wrap: break-word;
}
```
（4）arguments包含了所有的传递进来的参数，不用单独处理每一个参数
```less
.box-shadow (@x: 0, @y: 0, @blur: 1px, @color: #000) {
  box-shadow: @arguments;
  -moz-box-shadow: @arguments;
  -webkit-box-shadow: @arguments;
}
.box-shadow(2px, 5px);
```
编译过后的结果是：
```less
.box-shadow{
          box-shadow: 2px 5px 1px #000;
     -moz-box-shadow: 2px 5px 1px #000;
  -webkit-box-shadow: 2px 5px 1px #000;
}
```
## 模式匹配
可以通过值的进行匹配，也可以通过参数的个数进行匹配 
```less
//让.mixin根据不同的@switch值而表现各异
.mixin (dark, @color) {
  color: darken(@color, 10%);
}
.mixin (light, @color) {
  color: lighten(@color, 10%);
}
.mixin (@_, @color) {
  display: block;
}

//运行
@switch: light;

.class {
  .mixin(@switch, #888);
}
```
经过编译后的颜色是：
```less
.class {
  color: #a2a2a2;
  display: block;
}
/*mixin就会得到传入颜色的浅色。如果@switch设为dark，就会得到深色。

具体实现如下：
第一个混合定义并未被匹配，因为它只接受dark做为首参
第二个混合定义被成功匹配，因为它只接受light
第三个混合定义被成功匹配，因为它接受任意值

只有被匹配的混合才会被使用。变量可以匹配任意的传入值，而变量以外的固定值就仅仅匹配与其相等的*/
```
## 导引表达式
（1）根据表达式进行匹配，而不是通过值和参数匹配
```less
//when关键字用以定义一个导引序列(此例只有一个导引)
.mixin (@a) when (lightness(@a) >= 50%) {
  background-color: black;
}
.mixin (@a) when (lightness(@a) < 50%) {
  background-color: white;
}
.mixin (@a) {
  color: @a;
}

//运行
.class1 { .mixin(#ddd) }
.class2 { .mixin(#555) }
```
编译后的结果是：
```
.class1 {
  background-color: black;
  color: #ddd;
}
.class2 {
  background-color: white;
  color: #555;
}
```
(2)导引中可用的全部的比较运算符有:> >= =< <。此外，关键字true只表示布尔真值，除去关键字true以外的值都被视为布尔假值。
(3)导引序列使用逗号‘，’分割，当且仅当所有的条件都符合的时候，才会被视为匹配成功。
```less
.minin(@a) when (@a >10),(@a < 100){...}
```
(4)导引可以没有参数，也可以对参数进行比较运算
```less
@media: mobile;

.mixin (@a) when (@media = mobile) { ... }
.mixin (@a) when (@media = desktop) { ... }

.max (@a, @b) when (@a > @b) { width: @a }
.max (@a, @b) when (@a < @b) { width: @b }
```
(5)如果想要基于值的类型进行匹配的话，可以使用is表达式进行判断
```less
.mixin (@a, @b: 0) when (isnumber(@b)) { ... }
.mixin (@a, @b: black) when (iscolor(@b)) { ... }
```
`常见的检测函式：iscolor、isnumber、isstring、iskeyword、isurl`
`判断一个值是纯数字，还是某个单位量，可以使用下列函式：ispixel、ispercentage、isem`
(6)使用and和not关键字实现与条件
```less
.mixin(@a) when (isnumber (@a) ) and (@a > 0) {...}
.mixin(@b) when not ( @b > 0){...}
```
## 嵌套规则
（1）以嵌套的方式写层叠样式
```
#header { color: black; }
#header .navigation {
  font-size: 12px;
}
#header .logo { 
  width: 300px; 
}
#header .logo:hover {
  text-decoration: none;
}
```
可以改写成用less语法进行,其中&符号用于写串联选择器，这一点对于伪类选择器有很大的作用。
```less
#header {
  color: black;

  .navigation {
    font-size: 12px;
  }
  .logo {
    width: 300px;
    &:hover { text-decoration: none }
  }
}
```
##  运算
（1）任何数字、颜色都可以参与运算
``` less
@base : 10%;
@filter : @base * 2;
@other : @base + @filter;

color : #888 / 4;
background-color : @base=color + #111;
height : 100% / 2 + filterl;
```
（2）less运算能够分辨颜色和单位
```less
@var  : 1px + 5; //6px
width  : (@var + 5 ) *2; //被允许使用括号
border: (@width * 2) solid black;  // 可以在符合属性中进行使用
```
## 函数
（1）less本身提供了一系列的颜色运算函数。颜色会被转化成为HSL色彩空间，然后在通道级别操作。
```less
lighten(@color, 10%);     // 返回一个比@color低10％更轻的颜色
darken(@color, 10%);      // 返回一个比@color高10％较暗的颜色
saturate(@color, 10%);    // 返回比@color多饱和度10％的颜色
desaturate(@color, 10%);  // 返回一个比@color少饱和度10％的颜色
fadein(@color, 10%);      // 返回一个比@color少10％透明度的颜色
fadeout(@color, 10%);     // 返回一个比@color多10％透明度的颜色
fade(@color, 50%);        // 返回一个颜色透明度为50％的颜色
spin(@color, 10);         // 返回色调比@color大10度的颜色
spin(@color, -10);        // 返回一个比@color小10度色调的颜色
mix(@color1, @color2);    // 返回一个混合@ color1和@ color2的颜色
```
举个小栗子：
```less
@base: #f04615;
.class {
  color: saturate(@base, 5%);
  background-color: lighten(spin(@base, 8), 25%);
}
```
（2）可以获取颜色的具体参数
```less
hue(@color)                     //获取色相
saturation(@color)             //获取饱和度
lightness(@color)              //获取明度
```
(3)也可以在一种颜色的通道上面创建另外一种颜色,@new 将会保持 @old的 色调, 但是具有不同的饱和度和亮度.
```
@new: hsl(hue(@old), 45%, 90%);
```
## Math函数
less提供了一组方便的数学函数，可以使用它们处理一些数字类型的值。
```less
round(1.67); // returns `2`
ceil(2.4);   // returns `3`
floor(2.6);  // returns `2`
percentage(0.5); // returns `50%`
```
## 命名空间
有时候，我们可能更好地组织CSS或是单纯的为了更好地封装，我们会将会一些变量或是混合模块进行打包操作，为了后续进行复用
```less
#bundle {
  .button () {
    display: block;
    border: 1px solid black;
    background-color: grey;
    &:hover { background-color: white }
  }
  .tab { ... }
  .citation { ... }
}
```
当我们想要在某一个地方引入button的样式的时候：
```less
#header a {
  color: orange;
  #bundle > .button;
}
```
## 作用域
less的作用域和其他编程语言十分的相似，首先在本地的变量和混合模块进行查找，如果没有找到的话，就会去父及作用域查找，直到找到为止。
```less
@var: red;

#page {
  @var: white;
  #header {
    color: @var; // white
  }
}

#footer {
  color: @var; // red  
}
```
## importing(导入)
我们如果想要引入less文件，.less的后缀可以有可以没有
```
@import "lib.less";
@import "lib";
```
但是想要导入一个CSS文件而且不希望less对它进行处理，只要加上CSS后缀就可以了。这样的话less就可以跳过，不去处理他了。
```
import "lib.css";
```
## 字符串插值
变量可以用类似ruby和php的方式嵌入到字符串中，像@{name}这样的结构
```less
@base-url: "http://assets.fnord.com";
background-image: url("@{base-url}/images/bg.png");
```
## 避免编译
有时候我们需要输出一些不正确的CSS语法或者使用一些 less不认识的专有语法。要输出这样的值我们可以在字符串前加上一个 ~，并将要避免编译的值用 “”包含起来。
```less
.class {
  filter: ~"ms:alwaysHasItsOwnSyntax.For.Stuff()";
}
```
结果是：
```
.class {
  filter: ms:alwaysHasItsOwnSyntax.For.Stuff();
}
```

## JavaScript表达式
（1）JavaScript 表达式也可以在.less 文件中使用. 可以通过反引号的方式使用:
```less
@var: `"hello".toUpperCase() + '!'`;  // @var :"HELLO!"
```
（2）也可以同时使用字符串插值和避免编译
```
@str: "hello";
@var: ~`"@{str}".toUpperCase() + '!'`; //@var: HELLO!;
```
（3）可以访问JavaScript的环境
```
@height: `document.body.clientHeight`;
```
（4）将一个JavaScript字符串解析成16进制的颜色值，可以使用 color 函数
```
@color: color(`window.colors.baseColor`);
@darkcolor: darken(@color, 10%);
```
参考：[less一种 动态 样式 语言](http://www.bootcss.com/p/lesscss/)