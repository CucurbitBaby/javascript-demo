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

#### 第3步
#### 第4步
