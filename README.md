# Note-JavaScript12
DOM, CSS的一些操作，事件
### DOM
#### 操作CSS
##### 获取当前有效样式
>通过 element.style 属性，我们只能获取内联样式内容，并不能获取 CSS 样式内容。

- 那我们就有`getComputedStyle`和`currentStyle`方法
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>获取当前有效样式</title>
    <style>
        .pClass {
            color: lightcoral;
        }
    </style>
</head>
<body>
<p id="p1" class="pClass" style="color: lightseagreen;">这是一个段落内容.</p>
<script>
    /*
        获取当前元素有效的样式
        window.getComputedStyle(element, [pseudoElt])
        * 参数
          * element - 表示获取当前有效样式的标签
          * pseudoElt - 可选，用于匹配伪元素
            * 如果当前操作的是普通元素，必须为 null
        * 返回值 - 返回指定标签的当前有效样式
          * 还是 CSSStyleDeclaration 对象
        window.getComputedStyle(element, null) - 一般不会针对伪元素
        * 注意 - 具有浏览器兼容问题(IE 8及之前版本不支持)
      */
    var p1 = document.getElementById('p1');
    /*var result = window.getComputedStyle(p1, null);
    console.log(result.color);*/

    console.log(p1.currentStyle.color);

    // 浏览器解决方案
    function getStyle(elem){
        var result;
        // 优先判断使用频率高的
        if (window.getComputedStyle){
            result = window.getComputedStyle(elem, null);
        } else {
            result = elem.currentStyle;
        }
        return result;
    }
</script>
</body>
</html>
```
##### 获取可见宽度和高度  
- 获取 HTML 页面标签的可见宽度和高度的属性
    - 可见宽度: clientWidth
    - 可见高度: clientHeight  
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>获取可见宽度和高度</title>
    <style>
        .dClass {
            box-sizing: border-box;
            width: 200px;
            height: 200px;
            border: 50px solid lightslategray;
            background-color: lightseagreen;
            padding: 50px;
            margin: 50px;
        }
    </style>
</head>
<body>
<div id="d1" class="dClass"></div>
<script>
    var d1 = document.getElementById('d1');
    /*
        获取可见宽度和高度 - 不能设置(一般多种样式组合的都这样)
        * content-box
          * clientWidth = width + padding-left + padding-right
          * clientHeight = height + padding-top + padding-bottom
        * border-box
          * clientWidth = width - border-left - border-right
          * clientHeight = height - border-top - border-bottom
     */
    console.log(d1.clientWidth,d1.clientHeight);
</script>
</body>
</html>
```
##### 获取实际宽度和高度
- 获取 HTML 页面标签的实际宽度和高度的属性
    - 宽度: offsetWidth
    - 高度: offsetHeight
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>获取实际宽度和高度</title>
    <style>
        .dClass {
            box-sizing: border-box;
            width: 200px;
            height: 200px;
            border: 50px solid lightslategray;
            background-color: lightseagreen;
            padding: 50px;
            margin: 50px;
        }
    </style>
</head>
<body>
<div id="d1" class="dClass"></div>
<script>
    var d1 = document.getElementById('d1');
    /*
        获取实际宽度和高度
        * content-box
          * offsetWidth = width + padding + border
          * offsetHeight = height + padding + border
        * border-box
          * offsetWidth = width
          * offsetHeight = height
     */
    console.log(d1.offsetWidth,d1.offsetHeight);
</script>
</body>
</html>
```
##### 获取定位父元素
> 需要说明的是: 除了 offsetParent 属性可以获取指定标签的定位父元素，可以有 offsetLeft 和 offsetTop 属性来获取相对于其定位父元素的水平偏移量和垂直偏移量。

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>获取定位父元素</title>
    <style>
        #d1 {
            width: 300px;
            height: 300px;
            padding: 50px;
            background-color: lightslategray;

            position: relative;
        }
        #d2 {
            width: 200px;
            height: 200px;
            padding: 50px;
            background-color: lightseagreen;

            position: relative;
        }
        #d3 {
            width: 100px;
            height: 100px;
            padding: 50px;
            background-color: lightcoral;
        }
    </style>
</head>
<body>
<div id="d1">
    <div id="d2">
        <div id="d3"></div>
    </div>
</div>
<script>
    var d3 = document.getElementById('d3');
    // 获取当前节点的父节点
    console.log(d3.parentNode);
    /*
        获取定位父元素
        * 作用 - 获取当前元素的祖先元素中第一个开启定位的元素
        * 情况
          * 如果祖先元素中没有一个元素开启定位 -> <body>元素
          * 如果祖先元素中只有一个元素开启定位 -> 开启定位的元素
          * 如果祖先元素中有多个开启了定位 -> 距离当前元素最近的开启定位的元素
     */
    console.log(d3.offsetParent);
</script>
</body>
</html>
```
##### 获取滚动宽度和高度   
- 有关滚动的相关属性如下:
    - scrollWidth: 获取指定标签滚动区的宽度。
    - scrollHeight: 获取指定标签滚动区的高度。
    - scrollLeft: 获取水平滚动条滚动的距离。
    - scrollTop: 获取垂直滚动条滚动的距离。
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>获取滚动宽度和高度</title>
    <style>
        body {
            margin: 0px;
            /*height: 2000px;*/
        }
        #show {
            width: 80%;
            height: 500px;
            border: 10px solid lightslategray;
            margin: 0 auto;
            overflow: auto;
        }
        #content {
            width: 100%;
            height: 2000px;
            background-color: lightcoral;
        }
    </style>
</head>
<body>
<div id="show">
    <div id="content"></div>
</div>
<script>
    /*
        * 当前页面
          * window对象 -> 表示当前浏览器窗口
          * document对象 -> 表示当前HTML文档
            * IE 8及之前版本 -> 不支持
          * <body>元素 -> 表示显示在浏览器的内容
            * IE浏览器 -> 不支持
        * 滚动事件 - 1.页面的滚动条；2.鼠标滚轮
          * scroll事件 - 表示滚动事件
          * mousewheel事件 - 表示鼠标滚轮事件
     */
    /*var body = document.body;
    window.onscroll = function(){
        console.log(body.scrollHeight,body.scrollTop);
    }*/

    var show = document.getElementById('show');
    show.onscroll = function(){
        /*
            * scrollHeight - 表示当前元素可滚动的高度
            * scrollTop - 表示当前元素距离顶部的距离
         */
        console.log(show.scrollHeight,show.scrollTop);
    }

</script>
</body>
</html>
```
![关于在滚动事件中内容高度和容器高度的关系示意图](http://a3.qpic.cn/psb?/V118JuTr0BKcy7/A52p7xyaQTPItEBrl3pnnZiBsxePIcF4SjiN8NWaACM!/b/dPIAAAAAAAAA&bo=GwWAAgAAAAADB74!&rf=viewer_4)  
#### 事件  
> 所谓事件，就是 HTML 页面或者浏览器窗口发生的一些交互瞬间。比如用户点击了 HTML 页面中的一个按钮，HTML 页面为用户提示一些内容，这个效果就是通过事件来完成的。  
##### 事件监听 
- 为 HTML 页面指定标签绑定指定事件，可以通过以下三种方式实现:
   - HTML 标签的事件属性: 这种方式 HTML 代码和 JavaScript 逻辑不能很好地分离，所以不建议使用。
   - DOM 标准的事件: 这种方式只能为指定的一个标签绑定一个事件，并且只能具有一个事件处理函数。
   - 事件监听器: 这种方式是目前最受欢迎的一种方式。**但 IE 8 及之前的版本不支持！**    
   
1. 注册事件 
```html  
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>注册事件</title>
</head>
<body>
<!--
    元素的属性分类:
    1. 标准(通用)属性 - 几乎所有元素都支持
       id name class style
    2. 私有属性 - 具体元素具有独有的属性
    3. 事件属性 - 为当前元素注册(绑定)事件
       * HTML与JavaScript没有有效地分离 - 耦合度较高
    一、HTML的事件属性
-->
<button onclick="myClick()" id="btn1">按钮</button>
<button id="btn2">按钮</button>
<button id="btn3">按钮</button>
<script>
    // 为button绑定onclick事件
    var btn2 = document.getElementById('btn2');
    /*
        二、DOM的事件属性 -> 是DOM中哪个对象的属性?
     */
    btn2.onclick = myClick;

    function myClick(){
        console.log('xxx');
    }

    /*
        三、添加事件的监听器
        addEventListener(type, listener[, options])
        * 参数
          * type - 表示当前注册监听的事件类型(名称)
            * 事件类型(名称)没有前缀on的
          * listener - 表示当前注册监听事件的回调函数(事件的处理函数)
          * options - 可选，表示是冒泡还是捕获阶段
            * 一般默认就行，省略
     */
    var btn3 = document.getElementById('btn3');
    btn3.addEventListener('click',function(){
        console.log('xxx');
    });
</script>
</body>
</html>
```
2. 注册事件的对比   
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>注册事件的对比</title>
</head>
<body>
<button id="btn">按钮</button>
<script>
    var btn = document.getElementById('btn');
    // DOM的事件属性
    /*btn.onclick = function(){
        console.log('xxx');
    }
    btn.onclick = function(){
        console.log('yyy');
    }*/
    // 事件的监听器
    btn.addEventListener('click',function(){
        console.log('xxx');
    });
    btn.addEventListener('click',function(){
        console.log('yyy');
    });

</script>
</body>
</html>
```
![DOM的事件属性与事件监听器的区别](http://a3.qpic.cn/psb?/V118JuTr0BKcy7/w1M006*2ph6*Ea0d999SHhZqndYmr1ePiQM5sGJwGeQ!/m/dPIAAAAAAAAA&bo=GwWAAgAAAAADB74!&rf=photolist)   

3. 监听器的问题   
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>监听器的问题</title>
</head>
<body>
<button id="btn">按钮</button>
<script>
    var btn = document.getElementById('btn');
    /*
        addEventListener()方法
        * 具有浏览器兼容问题 -> IE 8及之前版本不支持
          * attachEvent(eventName, functionName)方法
            * eventName - 表示事件名称(类型)
              * 注意 - 该事件名称(类型)具有前缀"on"
            * functionName - 表示事件的处理函数
     */
    /*btn.addEventListener('click',function(){
        console.log('xxx');
    });*/

    /*btn.attachEvent('on'+'click',function(){
        console.log('xxx');
    });*/

    bind(btn,'click',function(){
        console.log('xxx');
    });

    // 注册事件的浏览器兼容解决方案
    function bind(elem, eventName, functionName){
        if (elem.addEventListener){
            elem.addEventListener(eventName,functionName);
        } else {
            elem.attachEvent('on' + eventName,functionName);
        }
    }

</script>
</body>
</html>
```
4. 注册事件的this   
> 在事件的处理函数中，可以通过 this 关键字来指代绑定该事件的标签。   

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>注册事件的this</title>
    <style>
        ul {
            height: 200px;
            background-color: lightslategray;
        }
        li {
            height: 100px;
            background-color: lightseagreen;
        }
        a {
            background-color: lightcoral;
        }
    </style>
</head>
<body>
<button id="btn">按钮</button>

<ul id="parent">
    <li><a href="#">链接</a></li>
</ul>

<script>
    var btn = document.getElementById('btn');
    /*
        注册事件的处理函数中 - 存在this
        * this的含义不会根据注册事件方式的不同而变化
        * this的含义 -> 代表注册(绑定)事件的目标元素
     */

    /*btn.onclick = function(){
        console.log(this);
    }*/

    /*btn.addEventListener('click',function(){
        console.log(this);
    });*/

    var parent = document.getElementById('parent');

    /*parent.addEventListener('click',function(){
        console.log(this);
    })*/

    /*
        attachEvent()方法中的this
        * 含义 -> 代表的是window对象
     */
    /*parent.attachEvent('onclick',function(){
        console.log(this);
    });*/

    bind(parent,'click',function(){
        /*
            浏览器的兼容问题
            1.其他浏览器 -> 绑定当前事件的目标元素
            2.IE 8浏览器 -> window 对象
         */
        console.log(this);
    });

    // 注册事件的浏览器兼容解决方案
    function bind(elem, eventName, functionName){
        if (elem.addEventListener){
            elem.addEventListener(eventName,functionName);
        } else {
            elem.attachEvent('on' + eventName,function(){
                // 将目标元素传递给底层代码，替换this的指向问题
                functionName.call(elem);
            });
        }
    }

</script>
</body>
</html>
```
##### 事件的对象  
> HTML 页面的标签绑定事件的处理函数中，提供了一个事件对象（event）。这个事件对象会返回关于该事件的信息，以及该事件绑定在哪个元素中。事件对象是以事件的处理函数中的参数形式出现，并不需要我们自己创建，直接使用即可。  

1. 事件对象  
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>事件对象</title>
</head>
<body>
<button id="btn">按钮</button>
<script>
    var btn = document.getElementById('btn');
    /*
        事件对象Event
        * 浏览器的兼容问题
          * 其他浏览器 -> 作为事件的处理函数中的形参出现
          * IE 8之间版本浏览器 -> 作为 window 对象的属性出现
     */
    /*btn.addEventListener('click',function(event){
        console.log(event);
    });*/
    /*btn.attachEvent('onclick',function(){
        console.log(window.event);
    });*/

    bind(btn,'click',function(event){

        // 提供event对象的浏览器兼容解决方案
        /*var e;
        if (event){
            e = event;
        } else {
            e = window.event;
        }*/
        // 改写为运算符
        var e = event || window.event;
        console.log(e);
    });

    // 注册事件的浏览器兼容解决方案
    function bind(elem, eventName, functionName){
        if (elem.addEventListener){
            elem.addEventListener(eventName,functionName);
        } else {
            elem.attachEvent('on' + eventName,function(){
                // 将目标元素传递给底层代码，替换this的指向问题
                functionName.call(elem);
            });
        }
    }
</script>
</body>
</html>
```
2. 绑定元素的目标元素   
>Event 事件对象提供了 target 属性用于获取触发事件的目标元素（标签）。  

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>绑定元素的目标元素</title>
    <style>
        ul {
            height: 150px;
            padding: 50px 0;
            background-color: lightslategray;
        }
        li {
            height: 50px;
            padding: 50px 0;
            background-color: lightseagreen;
        }
        a {
            background-color: lightcoral;
        }
    </style>
</head>
<body>
<ul id="parent">
    <li><a href="#">链接</a></li>
</ul>
<script>
    var parent = document.getElementById('parent');

    bind(parent,'click',function(event){
        var e = event || window.event;
        /*
            事件对象提供的获取目标元素的属性
            * target属性 -> 获取触发事件的目标元素
              * 浏览器的兼容问题 - IE 8及之前版本不支持
              * IE 8及之前版本提供 srcElement 属性
            * this -> 获取绑定事件的目标元素
         */
//        console.log(e.target);
//        console.log(e.srcElement);
        var target = e.target || e.srcElement;
        console.log(e);
    });

    // 注册事件的浏览器兼容解决方案
    function bind(elem, eventName, functionName){
        if (elem.addEventListener){
            elem.addEventListener(eventName,functionName);
        } else {
            elem.attachEvent('on' + eventName,function(){
                // 将目标元素传递给底层代码，替换this的指向问题
                functionName.call(elem);
            });
        }
    }

</script>
</body>
</html>
```
3. 阻止默认行为   
> HTML 页面的一些标签具有默认行为。所谓默认行为，就是不用编写 JavaScript 代码就可以实现的动态效果。   
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>阻止默认行为</title>
    <style>
        a {
            display: inline-block;
            width: 35px;
            height: 25px;
            border: 1px solid lightslategray;
            padding: 10px 20px;
            text-decoration: none;
        }
    </style>
</head>
<body>
<!-- 作用 - 跳转 -->
<a id="aElem" href="#">链接</a>
<script>
    var aElem = document.getElementById('aElem');
    /*aElem.addEventListener('click',function(event){
        event.preventDefault();
    });*/
    /*aElem.attachEvent('onclick',function(){
        var event = window.event;
        event.returnValue = false;
    });*/

    /*bind(aElem,'click',function(event){
        var e = event || window.event;
        // 阻止默认行为的浏览器兼容解决方案
        if (event.preventDefault){
            event.preventDefault();
        } else {
            event.returnValue = false;
        }
    });*/

    aElem.onclick = function(event){
        /*
            这种方式阻止默认行为
            * 只适用于DOM的事件属性方式
            * 没有浏览器的兼容问题
            * 这种方式必须编写在函数体的最后面
          */
        return false;
    }

    // 注册事件的浏览器兼容解决方案
    function bind(elem, eventName, functionName){
        if (elem.addEventListener){
            elem.addEventListener(eventName,functionName);
        } else {
            elem.attachEvent('on' + eventName,function(){
                // 将目标元素传递给底层代码，替换this的指向问题
                functionName.call(elem);
            });
        }
    }
</script>
</body>
</html>
```
4. 获取鼠标按键   
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>获取鼠标按键</title>
    <style>
        body {
            margin: 0px;
            height: 1000px;
        }
        #menu {
            width: 50px;
            height: 100px;
            background-color: lightcoral;
            display: none;
            position: absolute;
            /*left: ;
            top: ;*/
        }
    </style>
</head>
<body>
<div id="menu"></div>
<script>
    var body = document.body;
    // click事件 - 鼠标左键单击
    body.onmousedown = function(event){
        /*
            事件对象的属性
            * button属性 - 获取鼠标按键
              * 0 - 鼠标左键
              * 1 - 鼠标滚轮
              * 2 - 鼠标右键
            * buttons属性 - 获取鼠标按键
              * 1 - 鼠标左键
              * 2 - 鼠标右键
              * 4 - 鼠标滚轮
         */
        console.log(event);

        // 提供自定义的右键菜单
        var menu = document.getElementById('menu');
        if (event.button === 2){
            // 提供自定义菜单
            menu.style.display = 'block';
            menu.style.left = event.clientX + 'px';
            menu.style.top = event.clientY + 'px';
        } else if (event.button === 0){
            menu.style.display = 'none';
        }
    }

    // 阻止默认的右键菜单
    body.oncontextmenu = function(event){
        event.preventDefault();
    }

</script>
</body>
</html>
```
5. 获取鼠标坐标值   
- 当 HTML 页面中标签绑定的事件被触发时，我们还可以通过 Event 事件对象获取鼠标当前的坐标值。
   - pageX 和 pageY: 表示鼠标在整个页面中的位置。如果页面过大（存在滚动条），部分页面可能存在可视区域之外。
   - clientX 和 clientY: 表示鼠标在整个可视区域中的位置。
   - screenX 和 screenY: 表示鼠标在整个屏幕中的位置。从屏幕（不是浏览器）的左上角开始计算。
   - offsetX 和 offsetY: 表示鼠标相对于定位父元素的位置。
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>获取鼠标坐标值</title>
    <style>
        body {
            margin: 0px;
            height: 1000px;
        }
    </style>
</head>
<body>
<script>
    var body = document.body;
    body.onclick = function(event){
        console.log(event.clientY,event.pageY,event.screenY);
    }

</script>
</body>
</html>
```
![获取鼠标的坐标值](http://a2.qpic.cn/psb?/V118JuTr0BKcy7/XhLmNEf1UE9t3Pekcu0dnfUbCNXlJfsP0vvohLGBclY!/m/dGkBAAAAAAAA&bo=GwWAAgAAAAADB74!&rf=photolist)   



