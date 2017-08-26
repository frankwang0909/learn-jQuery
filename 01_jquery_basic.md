# jQuery 基础知识

## 1.jQuery 优势：

1.1 简洁的API：使得操作 DOM 和 Ajax 非常简单明了。

1.2 统一了不同浏览器的 API 接口，使得开发者不用担心代码在不同浏览器之间的兼容性。


## 2.jQuery 版本：

2.1 需要支持IE 6/7/8，就使用jQuery 1.x版;否则使用最新版。

2.2 jQuery 2.x版 可使用于 Node 等非浏览器环境中。


## 3.jQuery 基础

### 3.1. jQuery 对象：

一个全局对象，可以简写为美元符号`$`。jQuery的全部方法，都定义在这个对象上面。

### 3.2. jQuery构造函数

jQuery对象本质上是一个`构造函数`，主要作用是返回`jQuery对象的实例`。

```javascript
var listItems = $('li');
```
上面代码表面上是选中li元素，实际上是返回对应于li元素的`jQuery实例`。因为只有这样，才能在DOM对象上使用jQuery提供的各种方法。

jQuery构造函数可以多种参数，返回不同的值。

3.2.1 `CSS选择器`作为参数：最常用的jQuery构造函数的参数

```javascript
$("*")
$("#lastname")
$(".intro")
$("h1,div,p")
$("p:last")
$("tr:even")
$("p:first-child")
$("p:nth-of-type(2)")
$("div + p")
$("div:has(p)")
$(":empty")
$("[title^='Tom']")
```

3.2.2 `DOM对象`作为参数: `DOM对象`会被转为`jQuery对象的实例`。

```javascript
$(document.body) instanceof jQuery
// true
```

3.2.3 `HTML字符串`作为参数:  返回jQuery实例, 它对应的DOM结构不属于当前文档。

```javascript
$('<input class="form-control" type="hidden" name="foo" value="bar" />');
```

也可以写成如下代码：

```javascript
$('<input/>', {
  'class': 'form-control',
  type: 'hidden',
  name: 'foo',
  value: 'bar'
});
```

备注：由于`class`是JavaScript的保留字，所以只能放在`引号`中。

3.2.4 第二个参数：

默认情况下，jQuery将文档的`根元素（html）`作为寻找匹配对象的起点。如果要指定某个网页元素（比如某个div元素）作为寻找的起点，可以将它放在jQuery函数的第二个参数。

```javascript
$('li', someElement);	
```

上面代码表示，只寻找属于someElement对象下属的li元素。someElement可以是jQuery对象的实例，也可以是DOM对象。

### 3.3 jQuery构造函数返回的结果集

jQuery的核心思想是“先选中某些网页元素，然后对其进行某种处理”（find something, do something），也就是说，先选择后处理，这是jQuery的基本操作模式。

3.3.1 length 属性：

jQuery对象返回的结果集是一个类似数组的对象，包含了所有被选中的网页元素。

查看该对象的length属性，可以知道到底选中了多少个结果。

```javascript
if ( $('li').length === 0 ) {
	console.log('不含li元素');
}
```

因为不管有没有选中，jQuery构造函数总是返回`一个实例对象`，而对象的布尔值永远是`true`。使用`length属性`才是判断有没有选中的正确方法。
​	
```javascript
if ($('div.foo').length > 0) { ... }
```

或者：

```javascript
if ($('div.foo').length) { ... }
```

3.3.2 下标运算符：将`jQuery实例`转成`DOM对象`。

jQuery选择器返回的是一个`类似数组的对象`。

**注意**：使用`下标运算符`取出的`单个对象`，并不是jQuery对象的实例，而是`一个DOM对象`, 即`Element节点`的实例。 `下标运算符` 主要用途是将`jQuery实例`转成`DOM对象`。

```javascript
$('li')[0] instanceof jQuery // false
$('li')[0] instanceof Element // true
```

如果希望获取结果集里的单个jQuery对象的实例，应该使用jQuery 提供的`.eq()` 方法。

```javascript
$('li').eq(0) instanceof jQuery;    // true
$('li').eq(0) instanceof Element;   // false
```

3.3.3 get() 方法：下标运算符的另一种写法。

```javascript
$('li').get(0) instanceof Element;  // true
$('li').get(0) instanceof jQuery;   // false
$('li').get(0) === $('li')[0];      // true
```

3.3.4 eq() 方法：

在结果集取出一个jQuery对象的实例，不需要取出DOM对象，则使用eq方法，它的参数是实例在结果集中的位置（从0开始）。

eq()方法返回的是`jQuery的实例`，所以可以在返回结果上使用`jQuery实例对象的方法`(链式调用)。

```javascript
$('li').eq(0).text();	
```

3.3.5 each() 和 map() 方法：遍历结果集，对每一个成员进行某种操作。

1） each()接受`一个函数`作为参数，依次处理集合中的每一个元素。

each方法参数的函数，本身有两个参数，第一个是当前元素在集合中的位置，第二个是当前元素对应的DOM对象。


```javascript
$('li').each(function( index, element) {
  $(element).prepend( '<em>' + index + ': </em>' );
});
```


2） map() 用法与each方法完全一样，区别在于**each()没有返回值**，只是对每一个元素执行某种操作，而map方法**返回一个新的jQuery对象**。

```javascript
$("input").map(function (index, element){
    return $(this).val();
})
.get()
.join(", ");
```

3.3.6 内置循环：

jQuery默认对当前结果集进行循环处理，所以如果直接使用jQuery内置的某种方法，each和map方法是不必要的。

$(".class").addClass("highlight");
上面代码会执行一个内部循环，对每一个选中的元素进行addClass操作。

由于内置循环的存在，从性能考虑，应该尽量减少不必要的操作步骤。

```javascript
$(".class").css("color", "green").css("font-size", "16px");
```

应该写成

```javascript
$(".class").css({ 
  "color": "green",
  "font-size": "16px"
});
```

### 3.4 链式操作：

jQuery的大部分方法返回的都是`jQuery对象`，因此可以`链式操作`。也就是说，后一个方法可以紧跟着写在前一个方法后面。

```javascript
$('li').click(function (){
    $(this).addClass('clicked');
})
.find('span')
.attr( 'title', 'Hover over me' );
```

### 3.5 $(document).ready():

`$(document).ready()`接受一个`函数`作为参数，将该参数作为`document对象`的`DOMContentLoaded 事件`的回调函数。当页面解析完成（即下载完 HTML 标签）以后，在所有外部资源（图片、脚本等）完成加载之前，该函数就会立刻运行。

通常简写成：

```javascript
$(function() {
  // Handler for .ready() called.
});
```

与 `window 对象` 的 `onload 事件`的区别在于, `window.onload() ` 需要等所有资源（样式、图片、脚本等）加载完成，才会执行。