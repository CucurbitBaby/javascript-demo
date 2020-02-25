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
#### 2. 下拉菜单组件化
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

#### 3. 让下拉菜单组件开始工作
> 目前需要使用下拉插件组件的地方都需要添加class才行
> 封装成函数更便捷,将想用的DOM传进来
##### <font style="font-size:16px;">3.1 封装成函数</font>
```js
    // -----简单写法-----
    $('.dropdown').hover(function() {
      var $this=$(this); // 缓存this，以避免重复加载
        $this.addClass($this.data('active')+'-active');
    }, function() {
      var $this=$(this);
        $this.removeClass($this.data('active')+'-active');
    });



    //------封装代码方式--------
       function dropdown(elem) {
        var $elem = $(elem),
            activeClass = $elem.data('active') + '-active';
        $elem.hover(function() {
            $elem.addClass(activeClass);
        }, function() {
            $elem.removeClass(activeClass);
        });

     }
    // 单个下拉菜单 dropdown($('.dropdown')[0])

    //多个下拉菜单 
    $('.dropdown').each(function(){
      dropdown($(this));
    });

```

##### <font style="font-size:16px;">3.2 插件方式</font>
```js
    function dropdown(elem) {
        var $elem = $(elem),
            activeClass = $elem.data('active') + '-active';
        $elem.hover(function() {
            $elem.addClass(activeClass);
        }, function() {
            $elem.removeClass(activeClass);
        });

     }

    // 插件的使用方法
    $.fn.extend({
      dropdown:function(){
            // return this;    // 就是下面的$('.dropdown')  可能是个数组哦
        return this.each(function(){
          dropdown(this);

        });

      }
    });

    $('.dropdown').dropdown(); // 插件为了更好的调用
```

##### <font style="font-size:16px;">3.3 封装成模块</font>
* dropdown.js
```js
(function($){
    'use strict';

    function dropdown(elem) {
        var $elem = $(elem),
            activeClass = $elem.data('active') + '-active';
        $elem.hover(function() {
            $elem.addClass(activeClass);
        }, function() {
            $elem.removeClass(activeClass);
        });

     };

    $.fn.extend({
        dropdown:function(){
            return this.each(function(){
                dropdown(this);

            });

        }
    });

    
})(jQuery);
```
##### <font style="font-size:16px;">3.4 改写站点用的dropdown组件用法</font>
* 改写DOM的class为menu
```html
<li class="menu dropdown fl"  data-active="menu">
    <a href="###" target="_blank" class="dropdown-toggle link ">我的慕淘<i class="dropdown-arrow"></i></a>
    <ul class="dropdown-layer dropdown-left">
        <li><a href="###" target="_blank" class="menu-item">已买到的宝贝</a></li>
        <li><a href="###" target="_blank" class="menu-item">我的足迹</a></li>
    </ul>
</li>
```
* 将dropdown.html的<style\>全部copy到common.css中


#### 4. 下拉箭头的实现
##### <font style="font-size:16px;">4.1 各种方式实现</font>
> 图片缺点:
> >至少一次http请求
> > 不方便修改(颜色大小什么的)和维护
> 
> base64   https://tool.css-js.com/base64.html
> base64优点: 减少http请求
> 缺点: 
> > 1. IE6 7不支持
> > 2. 编码后比原图大
> > 3. 手动修改麻烦
> > 4. 不能缓存(除非随着HTML缓存整个页面)
> 
> CSS实现: 
> > 添加类名 我的慕淘<i class="dropdown-arrow icon-triangle-down"\><\/i> 
> > 默认样式 .icon-triangle-down {向上样式}
> > 鼠标移动 .menu-active .icon-triangle-down {向下样式}
> > border-right-color: transparent; IE6 不兼容(body背景色可以看出)
> > 解决方式就是_border-right-color: transparent; IE6下设置body一样背景色
> 
##### <font style="font-size:16px;">4.2 图标字体</font>
> 矢量图优点: 不失真; 减少http请求; 兼容性好
> 矢量图缺点:
> > 基本用作小图标
> > 无法100%还原设计稿
> > 跟设计沟通开始就让她去图标字体库中去选
> 图标库: icomoon.io  iconfont.cn
* 1. 将字体文件放到项目中
* 2. iconfot.css复制古来之后更改字体路径
* 3. 使用自定义class都行: .icon {font-famil: "copycss中定义的"; .. }
```css
  @font-face {
    font-family: "iconfont";
    src: url('font/iconfont.eot?t=1477124206');
    /* IE9*/
    src: url('font/iconfont.eot?t=1477124206#iefix') format('embedded-opentype'),
      /* IE6-IE8 */
      url('font/iconfont.woff?t=1477124206') format('woff'),
      /* chrome, firefox */
      url('font/iconfont.ttf?t=1477124206') format('truetype'),
      /* chrome, firefox, opera, Safari, Android, iOS 4.2+*/
      url('font/iconfont.svg?t=1477124206#iconfont') format('svg');
    /* iOS 4.1- */
  }

  /* 父类指向 @font-face 定义的font-family*/
  .icon {
    font-family: "iconfont" !important;
    font-size: 14px;
    font-style: normal; /*斜体扶正 */
    -webkit-font-smoothing: antialiased;      /* 抗 */
    -webkit-text-stroke-width: 0.2px;         /* 锯 */
    -moz-osx-font-smoothing: grayscale;       /* 齿 */
  }
  
  /* ... 想使用什么自填添加什么class */
  
  .icon-xiala:before { content: "\e609"; }
  
  /* ... */
```
* 使用直接在DOM中添加class: icon icon-xiala
```html
我的慕淘<i class="dropdown-arrow icon icon-xiala"></i> 
```
* IE6 不兼容Unicode编码
```html
我的慕淘<i class="dropdown-arrow icon">&#xe609;</i>
<!-- 这样做hover还得更改i里面的Unicode编码 -->
<!-- no no no 用css3旋转 -->
```
##### <font style="font-size:16px;">4.3 下拉图标旋转</font>
```css
  .icon {
    font-family: "iconfont" !important;
    font-size: 14px;
    font-style: normal;
    -webkit-font-smoothing: antialiased;
    -webkit-text-stroke-width: 0.2px;
    -moz-osx-font-smoothing: grayscale;
  }

  /*单独菜单旋转*/
  /*  .menu-active .dropdown-arrow {  
  -o-transform: rotate(180deg);
  -ms-transform: rotate(180deg);
  -moz-transform: rotate(180deg);
  -webkit-transform: rotate(180deg);
  transform: rotate(180deg);
  }*/

  /*多菜单旋转*/
  [class*="-active"] .dropdown-arrow {   /* 包含选择器 */
    -o-transform: rotate(180deg);
    -ms-transform: rotate(180deg);
    -moz-transform: rotate(180deg);
    -webkit-transform: rotate(180deg);
    transform: rotate(180deg);
/*    -o-transition: all 0.5s;
    -ms-transition: all 0.5s;
    -moz-transition: all 0.5s;
    -webkit-transition: all 0.5s;
    transition: all 0.5s;*/
  }
  
  /* 需要自取:  class要添加到DOM中 */
  .transition {
    -o-transition: all 0.5s;
    -ms-transition: all 0.5s;
    -moz-transition: all 0.5s;
    -webkit-transition: all 0.5s;
    transition: all 0.5s;
  }
```
#### <font style="font-size:16px;">5. 显示隐藏模块</font>
##### <font style="font-size:16px;">5.1 下拉层显示隐藏方式</font>
> 其他组件也用到的哦, 把显示隐藏封装成模块
> 解耦代码: / 组件化网页开发 / 2-2 静静的显示和隐藏(1) / test / showhide.html
> 通过回调解耦比较常用, 但不是和多人协作
```js

  // 正常显示和隐藏
  var silent = {
    show: function() { 
    },
    hide: function() {
    }
  };

  // 带效果的显示和隐藏，css3实现方法
  var css3 = {
    fade: {               // 淡入淡出
      show: function() {

      },
      hide: function() {

      }
    },
    slideUpDown: {        // 上下滚动
      show: function() {

      },
      hide: function() {

      }
    },
    slideLeftRight: {     // 左右滚动
      show: function() {

      },
      hide: function() {

      }
    },
    fadeslideUpDown: {    // 淡入淡出上下滚动
      show: function() {

      },
      hide: function() {

      }
    },

    fadeslideLeftRight: { // 淡入淡出左右滚动
      show: function() {

      },
      hide: function() {

      }
    }
  };

  // 带效果的显示和隐藏，js实现方法
  var js = {
    fade: {               // 淡入淡出
      show: function() {

      },
      hide: function() {

      }
    },
    slideUpDown: {        // 上下滚动
      show: function() {

      },
      hide: function() {

      }
    },
    slideLeftRight: {     // 左右滚动
      show: function() {

      },
      hide: function() {

      }
    },
    fadeslideUpDown: {    // 淡入淡出上下滚动
      show: function() {

      },
      hide: function() {

      }
    },
    fadeslideLeftRight: { // 淡入淡出左右滚动
      show: function() {

      },
      hide: function() {

      }
    }
  };
```
##### <font style="font-size:16px;">5.2 发布订阅的方式解耦</font>
> 发布消息: 触发一个事件
> 订阅消息: 绑定一个事件

```js
  // 正常显示和隐藏
  var silent = {
    // 第三种方式，发布订阅，多人协作
    show: function($elem) {
      // 触发时在 $elem 上触发
      $elem.trigger('show');
      $elem.show();
      // 绑定也在 $elem 绑定
      $elem.trigger('shown');
    },

    // 发布订阅，多人协作
    hide: function($elem) {
      $elem.trigger('hide');
      $elem.hide();
      $elem.trigger('hidden');
    }
  };

  //第三种调用
  var $box = $('#box');
  // 在显示按钮被点击的时候, 触发一个$box的show事件
  $('#btn-show').on('click', function() {

    silent.show($box);

  });

  //小A 订阅
  $box.on('show shown', function(e) {
    if (e.type === 'show') {
      $box.html('<p>我要显示了</p>');
    } else if (e.type === 'shown') {
      setTimeout(function() {
        $box.html($box.html() + '<p>我已经显示了</p>'); //显示后输出相应内容 
      }, 1000);
    }
  });

  //小C     需要在我show的时候新增功能, 只需要监听事件就行了
   $box.on('show shown',function(e){
     if(e.type==='show'){
         $box.css('background-color','yellow');
     } else if(e.type==='shown'){
         setTimeout(function(){
             $box.css('background-color','red');//显示后输出相应内容 
         },1000);
     }
  });

  // 新增者(观察者)只需要订阅发布者的消息类型就行了
  // 消息传递通过事件
 
  // bug,  显示状态点击show还是会触发show事件show shown事件; 加入状态即可解决
```

##### <font style="font-size:16px;">5.3 添加状态及初始状态</font>
```js
 // 放入showHide.js 模块
 // 正常显示和隐藏
var silent = {
    // 初始状态时show, 点击show还能执行一次(因为首次执行时还没有状态), 初始化即可
    // 初始化显示和隐藏的状态
    init:function ($elem) {
        // 隐藏就把状态设置为hidden
        // 显示就把状态设置为shown
        if($elem.is(':hidden')){
            $elem.data('status','hidden');
        }else{
            $elem.data('status','shown');

        }
    },
    show: function($elem) {
      // 判断状态，解决重复触发事件  
      if($elem.data('status')==='show') return; 
      // 默认如果是显示的init的时候状态就是shown了, 首次点击show也没效果, 只能点击hide
      if($elem.data('status')==='shown') return; 
        //给元素添加状态值
        $elem.data('status','show').trigger('show');            
        $elem.show();
        $elem.data('status','shown').trigger('shown');            


    },
    hide: function($elem) {
    if($elem.data('status')==='hide') return; 
    if($elem.data('status')==='hidden') return; 
        $elem.data('status','hide').trigger('hide');            
        $elem.hide();
        $elem.data('status','hidden').trigger('hidden'); 
    }
};

// showhide-2.html
 var $box = $('#box');
  // 在执行show之前因为执行一次init,就一次,
  silent.init($box);
  $('#btn-show').on('click', function() {
    silent.show($box);

  });

  $box.on('show shown hide hidden', function(e) {
    console.log(e.type);
  });

  $('#btn-hide').on('click', function() {
    silent.hide($box);
  });

```
##### <font style="font-size:16px;">5.4 CSS方式实现</font>
```css
  /*css和class添加过渡, 然后用JS显示隐藏即可*/
  .transition {
    -o-transition: all 0.5s;
    -ms-transition: all 0.5s;
    -moz-transition: all 0.5s;
    -webkit-transition: all 0.5s;
    transition: all 0.5s;
  }

  .fadeOut {
    visibility: hidden !important;
    opacity: 0 !important;
  }
```
```js

  // display属性是没有过渡效果的, 
  // 用 opactity 替代, 隐藏了是视觉上的看不见, 但还是存在的(文档流还在),还能响应事件
  // 用 opactity + vasibility:visible/hidden
  // 解决占位也可以用position: absolute;来解决, 但是不是每个DOM都需要有position: absolute;的
  // 所有还得配合display: block/none;
  
  // display: block/none;虽然没有动画
  // 但可以点击显示的时候, 可以先让元素从dispaly:noen;变到block;
  // 然后就可以 opactity + vasibility进行过渡动画了
  // 隐藏的时候先opactity + vasibility进行过渡动画, 动画完毕就display: none; 

  // $elem.show();
  // $elem.css({..}) 几乎同步执行, 所以看不到过渡动画
  // $elem.css()改成异步执行即可(就是下面的setTimeout防止提前执行)
  // 把css封装到class, add/removeClass

  // 隐藏的时候怎么知道动画执行完毕了呢?
  // CSS3动画执行完毕会有一个事件叫做 transitionend
  // 我们可以再hide()addClass隐藏之前, 绑定这个事件transitionend 
  // 因为$elem.addClass('fadeOut');过渡就会开始

  // 虽然加了
  fade: { // 淡入淡出
     show: function($elem) {
        // 元素显示之前, 发布消息 show
        $elem.trigger('show');

        // 元素显示之后, 发布消息 shown
        $elem.on('transitionend',function () {
            $elem.trigger('shown');
        });
        $elem.show();
        setTimeout(function () {
            $elem.removeClass('fadeOut');
        },20);

        
     },
     hide: function($elem) {
        $elem.on('transitionend',function () {
            $elem.hide();
        });
        $elem.addClass('fadeOut');
     }
  },

// showhide-2.html
  var $box = $('#box');
  // silent.init($box);
  $('#btn-show').on('click', function() {
    css3.fade.show($box);

  });
  $('#btn-hide').on('click', function() {
    css3.fade.hide($box);
  });

  $box.on('show shown hide hidden', function(e) {
    console.log(e.type);
  });

```

##### <font style="font-size:16px;">5.5 修改bug</font>
* 把之前的init状态方法添加进去
* 把之前执行show/hide方法的 判断状态 设置状态添加进去
* 事件重叠(绑定的事件越来越多), on换成one或者on之后立马off
* show立马hide: show hide shown hidden无序, one之前前off掉之前的事件绑定
* show立马hide: show hide hidden
* class transition init自动添加, 隐藏的时候添加fadeOut
##### <font style="font-size:16px;">5.6 提取公共代码</font>
> 去除冗余代码
> 提取两个init()公共部分, 然后通过回调函数执行不同部分 
```js
 // 正常显示和隐藏
 var silent = {
   //初始化显示和隐藏的状态
   init: function($elem) {
     if ($elem.is(':hidden')) {
       $elem.data('status', 'hidden');
     } else {
       $elem.data('status', 'shown');

     }
   },
   show: function($elem) {
     //判断状态，解决重复触发事件  
     if ($elem.data('status') === 'show') return;
     if ($elem.data('status') === 'shown') return;
     //给元素添加状态值
     $elem.data('status', 'show').trigger('show');
     $elem.show();
     $elem.data('status', 'shown').trigger('shown');


   },
   hide: function($elem) {
     if ($elem.data('status') === 'hide') return;
     if ($elem.data('status') === 'hidden') return;
     $elem.data('status', 'hide').trigger('hide');
     $elem.hide();
     $elem.data('status', 'hidden').trigger('hidden');
   }
 };
 // 带效果的显示和隐藏，css3实现方法
 var css3 = {
   fade: { // 淡入淡出
     show: function($elem) {
       $elem.trigger('show');
       $elem.on('transitionend', function() {
         $elem.trigger('shown');
       });
       $elem.show();
       setTimeout(function() {
         $elem.removeClass('fadeOut');
       }, 20);


     },
     hide: function($elem) {
       $elem.on('transitionend', function() {
         $elem.hide();
       });
       $elem.addClass('fadeOut');
     }
   },
 }


 // 提取init公共部分
 function init($elem, hiddenCallback) {

   if ($elem.is(':hidden')) {
     $elem.data('status', 'hidden');
     if (typeof hiddenCallback === 'function') hiddenCallback();

   } else {
     $elem.data('status', 'shown');
   }
 }


 // 提取show公共部分
 function show($elem, callback) {

   if ($elem.data('status') === 'show') return;
   if ($elem.data('status') === 'shown') return;
   $elem.data('status', 'show').trigger('show');
   callback();

 }

 // 提取hide公共部分
 function hide($elem, callback) {

   if ($elem.data('status') === 'hide') return;
   if ($elem.data('status') === 'hidden') return;
   $elem.data('status', 'hide').trigger('hide');
   callback();

 }

 // 使用公共函数
 // init(回调方式执行不同的代码)
 // show(回调方式执行不同的代码)
 // show(回调方式执行不同的代码)

```
##### <font style="font-size:16px;">5.6 tansition.js 兼容模块</font>
> transitionend 时间名各大浏览器不一样 / webkitTransitionEnd / oTransitionEnd
```js
/*console.log(document.body.style.transition)  空字符串就支持 undefined不支持*/
(function () {
  var transitionEndEventName = {
    transition: 'transitionend',
    MozTransition: 'transitionend',
    WebkitTransition: 'webkitTransitionEnd',
    OTransition: 'oTransitionEnd otransitionend'
  };
  var transitionEnd = '',
    isSupport = false;

  for (var name in transitionEndEventName) {
    if (document.body.style[name] !== undefined) {
      transitionEnd = transitionEndEventName[name];
      isSupport = true;
      break;
    }
  }

  // 支持 & 找到了对应的事件名称直接丢出去相应的
  window.mt = window.mt || {};
  window.mt.transition = {
    end: transitionEnd,
    isSupport: isSupport
  };
})();

// 其他模块使用 
var transition=window.mt.transition; // transition兼容解决，transition.js

```
##### <font style="font-size:16px;">5.6 CSS其他显示隐藏效果</font>
> 分析: fade和sideUpDown的共通之处
> > fade大体时通过添加移除class控制显示隐藏
> > 那sideUpDown也可以通过添加移除class添加高度
> > 注意添加CSS的优先级和周边样式
> > CSS效果的补充在init中补充
> > css3._init 内部使用的init, _init(){ init(callback) }
> > 添加例如 sideLeftRightCollapse的动画方式, 定义CSS样式之后, 添加/移除class
```css
.slideUpDownCollapse{
  height:0 !important;
  padding-top:0 !important;
  padding-bottom:0 !important;
}  

.slideLeftRightCollapse{
  width:0 !important;
  padding-left:0 !important;
  padding-right:0 !important;
} 
```
```js
slideUpDown: { // 上下滚动
   // init: function($elem) {
   //      $elem.height($elem.height());  //设置高度，解决没有slideUpDown的过程。
   //      $elem.addClass('transition');
   //      init($elem, function() {
   //          $elem.addClass('slideUpDownCollapse');

   //      });

   init: function($elem) {
       $elem.height($elem.height());
       css3._init($elem, 'slideUpDownCollapse');

   },
   show: function($elem) {
       css3._show($elem, 'slideUpDownCollapse');

   },
   hide: function($elem) {
       css3._hide($elem, 'slideUpDownCollapse');
   }
},
```
##### <font style="font-size:16px;">5.7 JS实现淡入淡出和卷下卷起效果</font>
* 淡入淡出jQuery其实已经封装好了, 直接使用
```js
    // 淡入淡出
    fade: { 
      show: function($elem) {

        $elem.fadeIn();
      },
      hide: function($elem) {

         $elem.fadeOut();
      }
    },
```
```js
  // showhide-2.html
  var $box = $('#box');

  $('#btn-show').on('click', function() {

    js.fade.show($box);
  }); 
  $('#btn-hide').on('click', function() {

    js.fade.show($box);
  });

```
* 设置状态, 增添判断
```js
    fade: { 
      show: function($elem) {

        // 使用封装的公共函数, 在回调中写自己的故事
        show($elem, function() {
          $elem.fadeIn()
        })
      },
      hide: function($elem) {
        show($elem, function() {
          $$elem.fadeOut();
        })
         
      }
    },
```
```js
    fade: { 
      show: function($elem) {

        // 使用封装的公共函数, 在回调中写自己的故事
        show($elem, function() {
          // 动画执行完毕, 发布消息shown, 还要设置一下状态
          // 什么时候执行完毕呢?  jQuery fadeIn(callback)也有自己callback
          $elem.fadeIn(function() {
            $elem.data('status', 'shown')
          })
        })
      },
      hide: function($elem) {
        show($elem, function() {
          $$elem.fadeOut(function() {
            $elem.data('status', 'hidden')
          });
        })
         
      }
    },
```
* 期望动画执行过程, 可以被打断(CSS3动画中, 使用的时off和transitionend事件)
* jQuery每次使用的fadeIn之前, stop()一下(暂停之前的动画), 如果刚点击了show按钮, 立马点击隐藏按钮, 先停止之前的动画, 那么显示回调永运就不会执行了
* 添加init设置初始化状态
* DOM添加transition就会和jQuery抢着做动画(最终还是同一个底层)
* 使用jQuery的show的时候, DOM不管有没有transition的类,都移除
* 提取js的公共代码
```js
js._init = function($elem, hiddenCallback) {
  $elem.removeClass('transition'); // js和transition动画冲突，在执行js前，将transition去掉，屏蔽风险。
  init($elem, hiddenCallback);
};

js._show = function($elem, mode) {
  show($elem, function() {
    $elem.stop()[mode](function() {
      $elem.data('status', 'shown').trigger('shown');
    });
  });
};

js._hide = function($elem, mode) {

  hide($elem, function() {
    $elem.stop()[mode](function() {
      $elem.data('status', 'hidden').trigger('hidden');
    });
  });

};

// 使用
// 带效果的显示和隐藏，js实现方法
```
##### <font style="font-size:16px;">5.8 JS实现其他效果</font>
* slideLeftRight 自定义动画
```js
 slideLeftRight: { // 左右滚动
   init: function($elem) {
     init($elem, function() {

       // 初始化的时候, 保存原始的样式
       var styles = {};
       styles['width'] = $elem.css('width');
       styles['padding-left'] = $elem.css('padding-left');
       styles['padding-right'] = $elem.css('padding-right');
       $elem.data('styles', styles);
       // 移除css动画冲突
       $elem.removeClass('transition');


       $elem.css({
         'width': 0,
         'padding-left': 0,
         'padding-right': 0
       });
     });
   },
   show: function($elem) {
     // 真正开始动画之前 show一下
     $elem.show();
     $elem.animate({
       'width': styles['width'],
       'padding-left': styles['padding-left'],
       'padding-right': styles['padding-right']
     })

   },
   hide: function($elem) {
     // 先做动画, 动画全部结束在隐藏

   }
 },

```

* 需要判断, 就用公共函数show
```js
   show: function($elem) {
      show($elem, function(){
         // 真正开始动画之前 show一下
         $elem.show();
         $elem.sotop().animate({
           'width': styles['width'],
           'padding-left': styles['padding-left'],
           'padding-right': styles['padding-right']
         }, function() {  // 动画执行完毕, 设置状态
            $elem.data('status', 'shown').trigger('shown');
          });
      });

   },
   hide: function($elem) {
         hide($elem, function() {

             $elem.stop().animate({
                 'width': 0,
                 'padding-left': 0,
                 'padding-right': 0
             }, function() {
                 // 动画执行完毕, 隐藏元素 
                 $elem.hide();
                 $elem.data('status', 'hidden').trigger('hidden');
             });
         });
   }

```
* 其他的方式和提取公共代码大同小异
* 封装成模块
```js
// 默认配置
var defaults = {
    css3: true,
    js: true,
    animation: 'fade'
};

// 写一个私有函数showHide里面各种判断(js/css优先&浏览器支持) 和动画方式
// extend合并配置, 用户优先, 容错
// 根据mode找到对应的js css 私有对象的对应方法init

// 1. 私有函数showHide给mt.showHide
    var $box = $('#box');

    var showHide=window.mt.showHide($box,{
        css3:true,
        animation:'fade'
    });  

    // js.fadeslideLeftRight.init($box);
    $('#btn-show').on('click', function() {
        // js.fadeslideLeftRight.show($box);    // 加入都不支持这个silent.show()本来就是需要一个参数的
        // showHide.show($box);
        showHide.show();
        // $box.showHide('show');
               
    }); 

// showHide()内部需要传参2次$elem的bug, 用$.proxy()解决
mode.init($elem);
return {
    // show: mode.show,
    // hide: mode.hide
    show: $.proxy(mode.show, this, $elem),
    hide: $.proxy(mode.hide, this, $elem),
};

// 或者
// 2. 做一个jQuery插件showHide 指向 私有函数showHide
// 不用这种方式了: 
    // window.mt = window.mt || {};
    // window.mt.showHide = showHide;

$.fn.extend({
    // 这里option是实参$xxx.showHide(实参)
    showHide: function (option) {
        // 有可能是个$集合
        return this.each(function () {
            var $this = $(this),
            // 如果mode = false; 不使用用户参数了
            mode = showHide($this, typeof option === 'object' && option);


            if (typeof mode[option] === 'function') {
                mode[option]();
            }
        });
    }
});

// 上面的方式致命弱点就是: 外部每一次执行showHide() 都会调用一次;
// 很显然: 每一次调用都应该是同一个$box.showHide({ css3: true, animation: 'fade' });




// 单例模式:
$.fn.extend({
    // 这里option是实参$xxx.showHide(实参)
    showHide: function (option) {
        // 有可能是个$集合
        return this.each(function () {
            var $this = $(this),
            // var mode = null;
      
            // 干掉showHide里面的options在这里处理
            var options = $.extend({}, defaults, typeof option === 'object' && option);
            var mode =  $this.data('showHide');  // 每次调用从这里获取
    

            // $this.data('showHide', mode = showHide($this, typeof option === 'object' && option));
            $this.data('showHide', mode = showHide($this,options));

            // 如果没有, 第一次执行
            if (!mode) {
                $this.data('showHide', mode = showHide($this, options));
            }

            if (typeof mode[option] === 'function') {
                mode[option]();
            }
        });
    }
});


$.fn.extend({
    showHide: function (option) {
        // 有可能是个$集合
        return this.each(function () {
            var $this = $(this),
                options = $.extend({}, defaults, typeof option === 'object' && option),
                mode = $this.data('showHide');

            if (!mode) {
                $this.data('showHide', mode = showHide($this, options));
            }

            if (typeof mode[option] === 'function') {
                mode[option]();
            }
        });
    }
});

// 这样调用
// $box.showHide('show'); 



```
#### 第3步
#### 第4步
