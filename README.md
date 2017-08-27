# Awesome-Less 

## 概念

* Less CSS是一种动态样式语言，属于CSS预处理语言的一种，它使用类似CSS的语法，为CSS赋予了动态语言的特性，如变量、继承、运算、函数等，更方便CSS的编写和维护。
* Less CSS可以在多种语言、环境中使用，包括浏览器端、桌面客户端、服务端。

## 语法

### 注释Annotations

```css
// 模板注释，这里的注释转换成CSS后将删除
/* CSS 注释语法 转换为CSS后仍然保留 */
```

### 变量Variable

>   
1. 将需要重复使用或经常修改的值定义为变量，需要使用的地方引用
2. 变量具有计算功能
3. 定义一个变量名为变量
4. 变量范围采用就近原则

* less
```css
// @变量名:变量值
@backgroundColor: #f5f5f5;

// 变量计算
@nice-blue: #5b83ad;
@light-blue: @nice-blue + #666;

// 定义一个变量名为变量
@fontSize: 30px;
@bottomFontSize: "fontSize";

body {
    // css属性:@变量名;
    background-color: @backgroundColor;
    color: @light-blue;
    font-size:@@bottomFontSize;
}
```

* css
```css
body {
  background-color: #f5f5f5;
  color: #c1e9ff;
  font-size: 30px;
}
```

* tips
    * 在less中变量只能被定义一次，类似常量，之后定义的会覆盖前面定义的变量
    
### 嵌套Nested

> 如果在CSS中经常使用子代选择器，那么使用Less嵌套语法会更方便

* less

```css
.container {
  width: @containerWidth;
  height: 100px;
  > .row {
    height: 100px;
    width: 100px;
    background-color: @mainColor;
    a {
        color: #f40;
        &:hover {
          color: #f90;
      }
    }
  }
  div {
    .hello {
      background-color: pink;
      height: 30px;
    }
  }
}
```

* css

```css
.container {
  width: 1024px;
  height: 100px;
}
.container > .row {
  height: 100px;
  width: 100px;
  background-color: #999;
}
.container > .row a {
  color: #f40;
}
.container > .row a:hover {
  color: #f90;
}
.container div .hello {
  background-color: pink;
  height: 30px;
}
```

* tips
  * 在less中`&`区别很大，有&时解析的是同一个元素或此元素的伪类，没有&解析的是后代元素。
  
### 混合Mixin

> 混入也叫“混入参数”，混合其实就是一种嵌套，允许将一个类嵌入到另一个类中，而被嵌入的类称为一个变量（混合也像一个带有参数的function）

* less

```less
// 用一个类定义CSS,然后把整个类作为一个变量来使用，嵌入到另一个类中当属性
// 不定义任何参数 .roundedCorners()
.rounderdCorners(@radius: 20px){
  -moz-border-radius: @radius;
  -webkit-border-radius: @radius;
  border-radius: @radius;
  background-color: green;
}
@circleR: 80px;
.circle {
  width: @circleR;
  height: circleR;
  .roundedCorners(@circleR);
}
.circle1 {
  width: @circleR;
  height: @circleR;
  // 无参数方式可直接写.rounderCorners;
  .roundedCorners();
}
```

* css

```css
.circle {
  width: 80px;
  height: 80px;
  -moz-border-radius: 80px;
  -webkit-border-radius: 80px;
  border-radius: 80px;
  background-color: green;
}
.circle1 {
  width: 80px;
  height: 80px;
  border-radius: 20px;
  -moz-border-raidus: 20px;
  -webkit-border-raidus: 20px;
  background-color: green;
}

```

* tips
  * 任何CSS的类或ID下的样式都可以当作变量，使用混合模式用来当做另一个元素的属性值
  
> @arguments当Mixins引入这个参数时，将表示所有的变量，当不想处理个别参数时，这个参数将很有用

* less
```css
.boxShadow(@x: 0,@y: 0, @blur: 1px,@color: #000){
  -moz-box-shadow: @arguments;
  -webkit-box-shadow: @aruments;
  box-shadow: @arguments;
}
.bottom {
  .boxShadow(2px,2px,3px,red);
}
```

### import引用

> 通过@import将多个less文件合并

* less

```css
// main_p.less
.center {
  // @h在main_v.less定义
  height: @h;
  background-color: yellow;
}

@import url('main_v.less');

// main_v.less
@h: 100px;
```

* css
```css
.center{
  height: 100px;
  background-color: yellow;
}
```

### Functions & Operations

> Operations 可以让我们对元素的属性值、颜色进行加减乘除运算，主要针对任何数字、颜色、变量的操作
Functions就像JavaScript中的function一样可以进行我们想要的值操作，主要针对Color functions，Less提供了多种变化颜色的功能

* less
```
@header-border: 4px;
@base-color: #111;
@red: #842210;

#header {
  height: 100px;
  color: @base-color * 3;
  border: 5px solid desaturate(@red, 100%);
  border-width: @header-border @header-border * 2 @header-border *3 @header-border;
  border-color: desaturate(@red, 100%) @lighten(@red, 10%) darken(@red, 30%);
}
```

* css
```
#header {
  height: 100px;
  color: #333333;
  border: 5px solid #4a4a4a;
  border-width: 4px 8px 12px 4px;
  border-color: #4a4a4a #842210 #b12e16 #000000;
}
```

### Operation使用

* less

```css
#nav {
  color: #888 / 4;
  background-color: green;
  
  // 简单四则运算 同单位操作
  @base: 5%;
  @filter: @base * 2;
  @other: @base + @filter;
  height: 100% / 2 + @other;
  
  // 不同单位操作
  @borderW: 1px + 6;
  border: @borderW solid yellow;
  
  // 使用()改变运算的先后顺序
  @width: 20px;
  width: (@width + 5) * 3;
}
```

* css

```css
#nav {
    color: #222222;
    background-color: green;
    height: 65%;
    border: 7px solid yellow;
    width: 75px;
}
```

### Color Functions

> 提供多种变换颜色的函数，先把颜色转换成HSL色，然后在此基础上进行操作

* lightn: 将一个颜色变亮
  * `lighten(#000, 10%) = #1a1a1a`
* darken: 将一个颜色变暗
  * `darken(#000, 10%) = #e6e6e6`
  
### 命名空间Namespaces

> 将公共的内容封装起来，在其他地方调用，类似如下样式#common> .button

* less
```css
#common {
  .button {
    display: block;
    border: 1px solid black;
    width: 100px;
    height: 30px;
    background-color: grey;
    &:hover {
      background-color: white;
    }
  }
}
#header {
  .button {
    #common > .button;
  }
}
#nav {
  a {
    #common > .button;
  }
}
```

* css
```css
#common .button {
  display: block;
  border: 1px solid black;
  width: 100px;
  height: 30px;
  background-color: grey;
}
#common .button:hover {
  background-color: white;
}
#header .button {
  display: block;
  border: 1px solid black;
  width: 100px;
  height: 30px;
  background-color: grey;
}
#header .button:hover {
  background-color: white;
}
#nav a {
  display: block;
  border: 1px solid black;
  width: 100px;
  height: 30px;
  background-color: grey;
}
#nav a:hover {
  background-color: white;
}
```

### Loops

> 在less中，混合可以调用它自身。所以，less中的混合递归调用自己，就形成了循环结构


* less
```css
@counter: 5;

.loop(@counter) when(@counter > 0) {
  .loop(@counter - 1); // 递归调用自身
  width: (20px * @counter); // 每次调用时产生的样式代码
}

div.loop{
  background-color: green;
  height: 50px;
  .loop(5);
}
```

* css
```css
div.loop{
  background-color: green;
  height: 50px;
  width: 20px;
  width: 40px;
  width: 60px;
  width: 80px;
  width: 100px;
}
```

> 使用递归循环最常见的情况就是生成栅格系统的CSS

* less
```css
div.row {
  height: 100px;
}
.generate-columns(4);
.generate-columns(@n, @i:1) when(@i =< @n){
  .column-@{i} {
    width: (@i * 100% / @n);
    height: 100%;
    float: left;
    background-color: #369 + @i * 15;
    .generate-columns(@n, (@i + 1));
  }
}
```

* css
```css
div.row {
  height: 100px;
}
.column-1 {
  width: 25%;
  height: 100%;
  float: left;
  background-color: #4275a8;
}
.column-2 {
  width: 50%;
  height: 100%;
  float: left;
  background-color: #5184b7;
}
.column-3 {
  width: 75%;
  height: 100%;
  float: left;
  background-color: #6093c6;
}
.column-4 {
  width: 100%;
  height: 100%;
  float: left;
  background-color: #6fa2d5;
}
```