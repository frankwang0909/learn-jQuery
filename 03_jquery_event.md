# jQuery 的事件处理

## 1.事件绑定的简便方法：

jQuery提供一系列方法，允许直接为常见事件绑定回调函数。

比如，click()可以为一个元素绑定click事件的回调函数。
```javascript
$('input').click(function (e){
  console.log($(this).val());
});
```

这样绑定事件的简便方法有如下一些：

    1.click();
    2.keydown(), keypress(), keyup();
    3.mouseover(),mouseout();mouseenter(),mouseleave();hover();
    4.scroll();
    5.focus(),blur();
    6.resize();
    
hover()需要特别说明。它接受`两个回调函数`作为参数，分别代表`mouseenter`和`mouseleave`事件的回调函数。

```javascript
$('input').hover(handlerIn, handlerOut);
// 等同于
$('input').mouseenter(handlerIn).mouseleave(handlerOut);
```

## 2. on():

jQuery事件绑定的统一接口。事件绑定的那些简便方法，其实都是on()的简写形式。

### 2.1 on()接受`两个参数`，第一个是`事件名称`，第二个是`回调函数`。
```javascript
$('li').on('click', function (e){
  console.log($(this).text());
});
```

在回调函数内部，`this关键字`指的是发生`该事件的DOM对象`。为了使用jQuery提供的方法，必须将`DOM对象`转为`jQuery对象`，因此写成`$(this)`。

### 2.2 一次为多个事件指定同样的回调函数。

为文本框的focus和blur事件绑定同一个回调函数:
```javascript
$('input[type="text"]').on('focus blur', function (){
  console.log('focus or blur');
});
```

### 2.3 为当前元素的某一个子元素，添加回调函数。

此时，on() 接受`三个`参数，`子元素选择器`作为第二个参数，夹在`事件名称`和`回调函数`之间。
```javascript
$('ul').on('click', 'li', function (e){
  console.log(this);
});
```

优点：

1）click事件在ul元素上触发回调函数，会检查event对象的target属性是否为li子元素，如果为true，再调用回调函数。这样就比为ul元素下的所有li元素一一绑定回调函数，节省了内存空间。

2）这种绑定的回调函数，对于在绑定后生成的li元素依然有效。


## 3. off():off方法

off() 用于移除事件的回调函数。
```javascript
$('li').off('click')
```

## 4. trigger():触发回调函数

它的参数就是`事件名称`。
```javascript
$('li').trigger('click');
```
上面代码触发li元素的click事件回调函数。与那些简便方法一样，trigger方法只触发回调函数，而不会引发浏览器的默认行为。

