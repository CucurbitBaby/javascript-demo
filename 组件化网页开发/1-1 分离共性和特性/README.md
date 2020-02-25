# jquery-webapp-component

> 慕淘网之站点导航实现--分步代码

#### Build Setup

``` bash
# install dependencies
npm install -g http-live-server

# serve with hot reload at localhost:8080
live-server
```

#### 1. 初始化项目
##### <font style="font-size:16px;">1.1 使用CDN加载jQuery的优点:</font>
* 减轻服务器压力
* 速度块
* 可以缓存
##### <font style="font-size:16px;">1.2 使用CDN加载jQuery的缺点:</font>
* 稳定性, 依赖了即手牵制
##### <font style="font-size:16px;">1.3 使用jQuery</font>
```javascript
    // window.jQuery || document.write('<script src="js/jquery.js"><' + '/script>')
    window.jQuery || document.write('<script src="js/jquery.js"><\/script>');
```
##### <font style="font-size:16px;">1.4 先初略看一下项目PSD</font>
##### <font style="font-size:16px;">1.5 项目结构</font>

>
>  css 
>
>  > base.css   (不是基于项目的, 例如reset功能)
>  > common.css (基于项目的公共的样式, 模块, 组件样式)
>  > index.css  (页面专属样式, 首页样式) 还可以有分类页,product.css等等
>
>  img
>
>  js 
>  > index.js
>  > jquery.js
>
> index.html

##### <font style="font-size:16px;">1.6 HTML结构分析 1</font>
```html
<div class="nav-site">
  <div class="container">

    <ul class="fl">左边列表</ul>
    
    <ul class="fr">右边列表</ul>
  </div>
</div>


```

##### <font style="font-size:16px;">1.7 HTML结构分析 2</font>
```html
<div class="nav-site">
  <div class="container">

    <ul class="fl">
      <li><a href="">Content AAA</a></li>
      <li><a href="">Content BBB</a></li>
      <li><a href="">Content CCC</a></li>
    </ul>
    
    <ul class="fr">
      <li>nav AAA</li>
      <li>nav BBB</li>
      <li>nav CCC</li>
    </ul>
  </div>
</div>


```
##### <font style="font-size:16px;">1.8 HTML结构分析 3</font>
```html
<div class="nav-site">
  <div class="container">

    <ul class="fl">
      <li><a href="">Content AAA</a></li>
      <li><a href="">Content BBB</a></li>
      <li><a href="">Content CCC</a></li>
    </ul>
    
    <ul class="fr">
      <li>
        <a href="">nav AAA</a>
      </li>
      <li>
        <a href="">nav BBB</a>
      </li>
      <li>
        <a href="">nav CCC</a>
      </li>
    </ul>
  </div>
</div>


```
##### <font style="font-size:16px;">1.9 根据PSD找出公共样式</font>
* 比如color一样的<a>添加相同的class
* 通过独有的class(nav-site-signupu) 加 公共样式的class(link)组合

##### <font style="font-size:16px;">2.0 抽离公共样式模块化</font>
* container, link, 等项目的公共样式抽离到common.css

##### <font style="font-size:16px;">2.1 页面专属样式 - 首页</font>
* nav-site, container, nav-site-login, 等等写道index.css
* 下拉样式先不抽离, 先写index.css中, js还没开始写, dropdown还没抽离成一个组件
* dropdown
* dropdown-togle
* dropdown-arrow
* dropdown-left
* dropdown-right .. 等

##### <font style="font-size:16px;">2.2 写JS下拉功能逻辑</font>
* 从事件方法更改对应的样式, JS中修改样式会可能引发重绘和回流
* 重绘(比如js改变div背景色, 本身的属性) \
* 回流(回流一定重绘, 不仅改变自己属性, 还改变的div的大小位置, 得重新计算旁边div位置)

##### <font style="font-size:16px;">2.3 封装成class,写入index.css中</font>
* dropdown-active越少越好啦
* 因为样式只要一个class就能这样写: .dropdown-active dropdown-toggle {} ..
* 把JS操作的样式,改成切换对应样式的class(dropdown-active), 减少重绘和回流
*
* 拉菜单分步已完成, 准备抽离dropdown组件吧
#### 2.下拉菜单组件化
> 页面上有很多地方使用了下拉菜单,<br>
> 结构: 都是一个切换按钮, 一个箭头, 一个下拉层<br>
> 行为: 都是鼠标hover显示下拉层, 离开隐藏下拉层
> 只是 <span style="color:red;">下拉方向</span> 和 <span style="color:red;">样式</span> 不一样而已<br>
* 下拉菜单组件: 简单理解为由一定的 html结构, css样式, js行为, 3者组合在一起; 组成一个具有特定功能, 相对独立的个体。
##### <font style="font-size:16px;">2.1 提取下拉菜单组件 - html 共性</font>
###### 1. 提取HTML结构
> 复制一个index.html中的li下拉菜单
```html
<li class="dropdown fl" >
  <a href="###" target="_blank" class="dropdown-toggle link ">我的慕淘<i class="dropdown-arrow"></i></a>
  <ul class="dropdown-layer dropdown-left">
    <li><a href="###" target="_blank" class="dropdown-item">已买到的宝贝</a></li>
    <li><a href="###" target="_blank" class="dropdown-item">我的足迹</a></li>
  </ul>
</li>
```
###### 2. 提取HTML结构
> li改成div, 别人用的时候不一定是ul中使用,  fl很显然没用了
```html
<div class="dropdown" >
  <a href="###" target="_blank" class="dropdown-toggle link ">我的慕淘<i class="dropdown-arrow"></i></a>
  <ul class="dropdown-layer dropdown-left">
    <li><a href="###" target="_blank" class="dropdown-item">已买到的宝贝</a></li>
    <li><a href="###" target="_blank" class="dropdown-item">我的足迹</a></li>
  </ul>
</div>
```
###### 3. 提取HTML结构
> div内部由切换按钮和下拉层组成, 切换按钮不一定是一个<a>,很显然: href, link没用了
```html
<div class="dropdown" >
  <div href="###"  class="dropdown-toggle">我的慕淘<i class="dropdown-arrow"></i></div>
  <ul class="dropdown-layer dropdown-left">
    <li><a href="###" target="_blank" class="dropdown-item">已买到的宝贝</a></li>
    <li><a href="###" target="_blank" class="dropdown-item">我的足迹</a></li>
  </ul>
</div>
```
###### 4. 提取HTML结构
> 下拉层组成也不能用ul了, 干掉里面内容, 不同下拉菜单的下拉层是不一样的
>
> 这就是一个基本的下拉组件的结构了
```html
<div class="dropdown" >
  <div href="###"  class="dropdown-toggle">我的慕淘<i class="dropdown-arrow"></i></div>
  <div class="dropdown-layer dropdown-left">

  </div>
</div>
```
##### <font style="font-size:16px;">2.2 提取下拉菜单组件 - css 共性</font>
###### 1.提取样式
> 把index.css 中的dropdown相关样式放入dropdown.html中的<style>
```css
/*下拉菜单样式dropdown*/
.dropdown{
    position: relative;
}
.dropdown-toggle{
  position: relative;
  z-index: 2;
  display:block;
  height: 100%;
  padding: 0 16px 0 12px;
  border-left: 1px solid #f3f5f7;
  border-right: 1px solid #f3f5f7;

}
.dropdown-arrow{
  display: inline-block;
  width:8px;
  height:6px;
  background: url(../img/dropdown-arrow.png) no-repeat;
  margin-left:8px;
  vertical-align: middle;

}
.dropdown-layer{
    display:none;
    position: absolute;
    top:43px;
    background-color:#fff;
    z-index: 1;
    border: 1px solid #cdd0d4;
}
.dropdown-left{
  left:0;
  right:auto;

}
.dropdown-right{
  right:0;
  left:auto;

}
.dropdown-item{
  display:block;
  height:30px;
  line-height:30px;
  padding:0 12px;
  color:#4d555d;
  white-space: nowrap;

}
.dropdown-item:hover{
  background-color: #f3f5f7;
}

.dropdown-active .dropdown-toggle,
.dropdown:hover .dropdown-toggle{
  background-color:#fff;
    border-color:#cdd0d4;
}
.dropdown-active .dropdown-arrow,
.dropdown:hover .dropdown-arrow{
  background-image:url(../img/dropdown-arrow-active.png);
}

.dropdown-active .dropdown-layer,
.dropdown:hover .dropdown-layer{
  display:block;
}
```
###### 2.提取样式
> 保留公共样式, 提出独有样式
```css

/*下拉菜单样式dropdown*/
/* 容器 保留 */ 
.dropdown{
    position: relative;
}
/*切换按钮*/
.dropdown-toggle{
  position: relative;
  z-index: 2;
  display:block;  /* 去除, 这个是针对<a>的 使用的地方不一定有*/ 
  height: 100%;   /* 去除, 别人的不一定是100%高度 使用的地方不一定有*/ 
  padding: 0 16px 0 12px;           /* 去除独有 使用的地方不一定有*/ 
  border-left: 1px solid #f3f5f7;   /* 去除独有 使用的地方不一定有*/ 
  border-right: 1px solid #f3f5f7;  /* 去除独有 使用的地方不一定有*/ 

}
/*下拉箭头*/
.dropdown-arrow{
  display: inline-block;
  width:8px;        /* 去除独有 大小不一定*/ 
  height:6px;       /* 去除独有 大小不一定*/ 
  background: url(../img/dropdown-arrow.png) no-repeat; /* 去除独有 image不一定; 保留background-rpeat: no-repeat*/ 
  margin-left:8px;  /* 去除独有 margin不一定*/ 
  vertical-align: middle;

}
/*下拉层*/
.dropdown-layer{
    display:none;
    position: absolute;
    top:43px;                 /*去除独有 距离顶部高度是不一定*/ 
    background-color:#fff;    /*去除独有 背景色不一定*/ 
    z-index: 1;               
    border: 1px solid #cdd0d4;/*去除独有 边框色不一定*/ 
}
/*保留*/
.dropdown-left{
  left:0;
  right:auto;

}
/*保留 最后一个下拉边框和容器边框对齐*/
.dropdown-right{
  right:0;
  left:auto;

}
/*去除 不同使用下拉组件的地方, 下拉层是不一样的*/
.dropdown-item{
  display:block;
  height:30px;
  line-height:30px;
  padding:0 12px;
  color:#4d555d;
  white-space: nowrap;

}
/*去除 不同使用下拉组件的地方, 下拉层是不一样的*/
.dropdown-item:hover{
  background-color: #f3f5f7;
}

/*去除 不同使用下拉组件的地方, 下拉层是不一样的*/
.dropdown-active .dropdown-toggle,
.dropdown:hover .dropdown-toggle{
  background-color:#fff;
    border-color:#cdd0d4;
}
/*去除 不同使用下拉组件的地方, 下拉层是不一样的*/
.dropdown-active .dropdown-arrow,
.dropdown:hover .dropdown-arrow{
  background-image:url(../img/dropdown-arrow-active.png);
}

/*去除 不同使用下拉组件的地方, 下拉层是不一样的*/
.dropdown-active .dropdown-layer,
.dropdown:hover .dropdown-layer{
  display:block;
}
```
###### 3.提取样式
> 这就是我们下拉菜单组件的一个最基本的样式
```css
/*下拉菜单样式dropdown*/
/* 容器 保留 */ 
.dropdown{
    position: relative;
}
/*切换按钮*/
.dropdown-toggle{
  position: relative;
  z-index: 2;
}
/*下拉箭头*/
.dropdown-arrow{
  display: inline-block;
  background-rpeat: no-repeat;
  vertical-align: middle;

}
/*下拉层*/
.dropdown-layer{
    display:none;
    position: absolute;
    z-index: 1;               
}
/*保留*/
.dropdown-left{
  left:0;
  right:auto;

}
/*保留 最后一个下拉边框和容器边框对齐*/
.dropdown-right{
  right:0;
  left:auto;

}

```

##### <font style="font-size:16px;">2.3 提取下拉菜单组件 - css 特性</font>
> 不同下拉菜单组件, 他们各自的结构和样式, 
> 比如说头部的站点导航, 它自己独有的样式

###### 1.提取独有的样式
```css
/*下拉菜单样式dropdown*/
/* 容器 保留 */ 
.dropdown{
    position: relative;
}
/*切换按钮*/
.dropdown-toggle{
  position: relative;
  z-index: 2;
}
/*下拉箭头*/
.dropdown-arrow{
  display: inline-block;
  background-rpeat: no-repeat;
  vertical-align: middle;

}
/*下拉层*/
.dropdown-layer{
    display:none;
    position: absolute;
    z-index: 1;               
}
/*保留*/
.dropdown-left{
  left:0;
  right:auto;

}
/*保留 最后一个下拉边框和容器边框对齐*/
.dropdown-right{
  right:0;
  left:auto;

}

/*添加 站点导航 独有的样式*/
.nav-site .dropdown {
/*这样写看起来是可以,  但是不方便复用*/
/*如果在复制一份, 其他地方也有一个这样的dropdown组件, 要使样式生效就得 套一个.nav-site父元素容器*/
}

```
###### 2.提取独有的样式
> 不同下拉菜单 html添加不同命名的class 编写特性样式
> 添加 menu \<div class="menu dropdown" \>
> 如果是购物车的样式呢 cat \<div class="cat dropdown" \>
```html
<div class="menu dropdown" >
  <div href="###"  class="dropdown-toggle">我的慕淘<i class="dropdown-arrow"></i></div>
  <div class="dropdown-layer dropdown-left">

  </div>
</div>
```
```css
/*下拉菜单样式dropdown*/
/* 容器 保留 */ 
.dropdown{
    position: relative;
}
/*切换按钮*/
.dropdown-toggle{
  position: relative;
  z-index: 2;
}
/*下拉箭头*/
.dropdown-arrow{
  display: inline-block;
  background-rpeat: no-repeat;
  vertical-align: middle;

}
/*下拉层*/
.dropdown-layer{
    display:none;
    position: absolute;
    z-index: 1;               
}
/*保留*/
.dropdown-left{
  left:0;
  right:auto;

}
/*保留 最后一个下拉边框和容器边框对齐*/
.dropdown-right{
  right:0;
  left:auto;

}

/*添加 站点导航 独有的样式*/
.nav-site .dropdown {
/*这样写看起来是可以,  但是不方便复用*/
/*如果在复制一份, 其他地方也有一个这样的dropdown组件, 要使样式生效就得 套一个.nav-site父元素容器*/
}

/* 独有的特性 */
/*.menu .dropdown  其实就是之前去除的特性样式, 在前面加上添加的 class menu*/
  /*.menu .dropdown*/

.menu .dropdown-toggle {
  display: block;
  height: 100%;
  padding: 0 16px 0 12px;
  border-left: 1px solid #f3f5f7;
  border-right: 1px solid #f3f5f7;
}

.menu .dropdown-arrow {
  width: 8px;
  height: 6px;
  background-image: url(../img/dropdown-arrow.png);
  margin-left: 8px;
}

.menu .dropdown-layer {
  top: 100%;
  background-color: #fff;
  border: 1px solid #cdd0d4;
}

.menu-item {
  display: block;
  height: 30px;
  line-height: 30px;
  padding: 0 12px;
  color: #4d555d;
  white-space: nowrap;
}

.menu-item:hover {
  background-color: #f3f5f7;
}

/*.menu.dropdown-active  JS的添加的hove功能,  */
/*.menu.dropdown:hover .dropdown-arrow  CSS自带的hove功能,  */
.menu.dropdown-active .dropdown-toggle,
.menu.dropdown:hover .dropdown-toggle {
    background-color: #fff;
    border-color: #cdd0d4;
}

/*.menu.dropdown-active  JS的添加的hove功能,  */
/*.menu.dropdown:hover .dropdown-arrow  CSS自带的hove功能,  */
.menu.dropdown-active .dropdown-arrow,
.menu.dropdown:hover .dropdown-arrow{
    background-image: url(../img/dropdown-arrow-active.png);
}
/*.menu.dropdown-active  JS的添加的hove功能,  */
/*.menu.dropdown:hover .dropdown-arrow  CSS自带的hove功能,  */
.menu.dropdown-active .dropdown-layer,
.menu.dropdown:hover .dropdown-layer {
    display: block;
}
```
###### 3.兼容IE6
* .menu.dropdown-active 这种写法不兼容IE6 ?
* 干掉menu  .dropdown-active就会影响所有使用组件的地方(比如cat) 不能单独用.dropdown-active
* 添加 menu-active 如果是cat就添加 cat-active
* 如果是购物车的样式呢 cat \<div class="cat dropdown" \>
* \<div class="menu dropdown 程序识别标识添加:menu-active" 标识:data-active="menu" \>  
* 样式就使用: .menu-active


#### 第3步
#### 第4步
