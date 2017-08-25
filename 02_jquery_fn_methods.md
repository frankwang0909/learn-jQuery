
# 常用的 jQuery 实例对象的方法：

## 1.结果集的过滤方法

选择器选出一组符合条件的网页元素以后，jQuery提供了许多方法，可以过滤结果集，返回更准确的目标。

### 1.1 first() 和 last():

first()返回结果集的第一个成员，last()返回结果集的最后一个成员。

```javascript
$("li").first();
$("li").last();
```

### 1.2 next() 和 prev():

next() 返回紧邻的下一个同级元素，prev() 返回紧邻的上一个同级元素。

```javascript
$("li").first().next();
$("li").last().prev();
```

如果 next() 和 prev() 带有参数，表示选择符合该参数的同级元素。

```javascript
$("li").first().next('.item');
$("li").last().prev('.item');
```

### 1.3 parent(), parents(), children():

parent()返回当前元素的`父元素`。

```javascript
$("p").parent();
$("p").parent(".selected");
```

parents()返回当前元素的`所有上级元素`（直到html元素）。

```javascript
$("p").parents();
$("p").parents("div");
```

children()返回选中元素的`所有子元素`。

```javascript
$("div").children();
$("div").children(".selected");
```

下面的写法结果相同，但是效率较低:
```javascript
$('div > *');
$('div > .selected');
```

### 1.4 siblings(),  nextAll(),  prevAll():

siblings()返回当前元素的`所有同级元素`。

```javascript
$('li').first().siblings();
$('li').first().siblings('.item');
```

nextAll()返回当前元素`其后的所有同级元素`。

```javascript
$('li').first().nextAll();
```
prevAll()返回当前元素`前面的所有同级元素`。

```javascript
$('li').last().prevAll();
```

### 1.5 closest()，find():

closest()返回**当前元素及其所有上级元素**之中，`第一个符合条件`的元素。
```javascript
$('li').closest('div')；
```

find()返回当前元素的`所有符合条件的下级元素`。
```javascript
$('div').find('li')；
```
上面代码中的find()，选中所有div元素下面的li元素，等同于$(‘li’, ‘div’)。由于这样写缩小了搜索范围，所以要优于$(‘div li’)的写法。

### 1.6 filter()，not()，has():

filter()用于过滤结果集，它可以接受多种类型的参数，`只返回与参数一致`的结果。
```javascript
// 返回符合CSS选择器的结果
$('li').filter('.item')

// 返回函数返回值为true的结果
$("li").filter(function(index) {
    return index % 2 === 1;
})

// 返回符合特定DOM对象的结果
$("li").filter(document.getElementById("unique"))

// 返回符合特定jQuery实例的结果
$("li").filter($("#unique"))

```

not()的用法与filter()完全一致，但是返回相反的结果，即**过滤掉匹配项** 。
```javascript
$('li').not('.item');
```

has() 与 filter() 作用相同，但是只过滤出**子元素中**符合条件的元素。
```javascript
$("li").has("ul")
```
上面代码返回具有`ul子元素`的li元素。

### 1.7 add()，addBack()，end(): （不常用）

add()用于为结果集添加元素。
```javascript
$('h2').add('h3');
```

addBack()将当前元素加回原始的结果集。
```javascript
$('li').parent().addBack();
```

end()用于返回原始的结果集。
```javascript
$('li').first().end();
```


## 2.DOM 相关的方法：

jQuery的许多方法都是`取值器（getter）`与`赋值器（setter）`的合一，即取值和赋值都是同一个方法，不使用参数的时候为取值器，使用参数的时候为赋值器。

### 2.1 html()和text():

html()返回该元素`包含的HTML代码`; text()返回该元素`包含的文本`。

假设有网页仅还有一个p元素：
```html
<p><em>Hello World!</em></p>
```

1）不使用参数，作为取值器使用：获取选中元素包含的 HTML 代码或者文本内容。
```javascript
$('p').html()
// <em>Hello World!</em> 

$('p').text()
// Hello World! 
```

2）使用参数，作为赋值器使用：设置获取选中元素的 HTML 代码或者文本内容。
```javascript
$('p').html('<strong>Hello World!</strong>')
// 网页代码变为<p><strong>Hello World!</strong></p> 

$('p').text('Hello World!')
// 网页代码变为<p>Hello World!</p> 
```

### 2.2 val(): 用于input，select, textarea等表单元素。

1）val() 不含参数时，作为取值器，返回结果集`第一个元素的值`。
```javascript
$('input[type="text"]').val()
```

2）包含参数时，作为赋值器，设置当前结果集所有元素的值
```javascript
$('input[type="text"]').val('new value')
```

### 2.3 css(): 用于改变CSS设置或者获取CSS属性的值。

1）作为赋值器使用，改变CSS设置。
```javascript
$('h1').css('padding-left', '20px')
// 或者
$('h1').css({
  'padding-left': '20px',
  'fontSize':'25px'
});
```

2）作为取值器使用，获取CSS属性的值：
```javascript
$('h1').css('fontSize'); //25px
```


### 2.4 hasClass(), addClass()，removeClass()，toggleClass():

hasClass()用于判断是否有某个css类，返回一个布尔值。
```javascript
$('li').addClass('special')； // false
```

addClass()用于添加一个css类，removeClass()用于移除一个css类，toggleClass()用于折叠一个css类（如果无就添加，如果有就移除）。
```javascript
$('li').addClass('special');
$('li').removeClass('special');
$('li').toggleClass('special');
```

### 2.5 prop()，attr(): 易混淆的两种属性!

attr() 和 prop() 针对的是不同的属性。在英语中，attr是attribute的缩写，prop是property的缩写，中文翻译都是“属性”。

1)一种是`网页HTML元素`的属性，比如a元素的`href属性`、img元素的`src属性`。这要使用`attr()`读写。
```javascript
// 读取属性值
$('a').attr('href');

//写入属性值
$('a').attr('herf', 'http://www.jquery.com');
```

下面是通过设置a元素的target属性，使得网页上的外部链接在新窗口打开的例子。
```javascript
$('a[href^="http"]').attr('target', '_blank');
$('a[href^="//"]').attr('target', '_blank');
$('a[href^="' + window.location.origin + '"]').attr('target', '_self');
```

2)另一种是`DOM元素`的属性，比如`tagName、nodeName、nodeType`等等。这要使用`prop()`读写。
```javascript
// 读取属性值
$('textarea').prop(name)

// 写入属性值
$('textarea').prop(name, val)
```

有时，attr() 和 prop() 对同一个属性会读到不一样的值。比如，网页上有一个单选框。
```html
<input type="checkbox" checked="checked" />
```

对于checked属性，attr() 读到的是checked，prop() 读到的是true。
```javascript
$(input[type=checkbox]).attr("checked") // "checked"
$(input[type=checkbox]).prop("checked") // true
```

可以看到，attr() 读取的是`网页HTML元素`上该属性的值，而 prop() 读取的是`DOM元素`的该属性的值。根据规范，element.checked应该返回一个`布尔值`。所以，判断单选框是否选中，要使用 prop() 。事实上，不管这个单选框是否选中，attr(“checked”)的返回值都是checked。
```javascript
if ($(elem).prop("checked")) { /*... */ };
// 下面两种方法亦可
if ( elem.checked ) { /*...*/ };
if ( $(elem).is(":checked") ) { /*...*/ };
```

### 2.6 removeProp()，removeAttr():

removeProp()移除某个`DOM属性`，removeAttr()移除某个`HTML属性`。
```javascript
$("a").removeProp('title');
$('a').removeAttr("title");
```

### 2.7 data(): 在一个DOM对象上储存数据。
```javascript
// 储存数据
$("div").data("foo", 52);

// 读取数据
$("div").data("foo"); // 52
```

## 3.添加、复制和移动网页元素的方法

jQuery方法提供一系列方法，可以改变元素在文档中的位置。

### 3.1 尾部追加元素：append()，appendTo()

append() 将`参数中的元素`插入当前元素的尾部。
```javascript
$("div").append("<p>World</p>");
// <div>Hello </div>
// 变为
// <div>Hello <p>World</p></div>
```

appendTo() 将 `当前元素` 插入参数中的元素尾部。
```javascript
$("<p>World</p>").appendTo("div");
```

### 3.2 prepend()，prependTo()

prepend() 将`参数中的元素`，变为到当前元素的`第一个子元素`。
```javascript
$("p").prepend("Hello");
// <p>World</p>
// 变为
// <p>Hello World</p>
```

prependTo() 将`当前元素`变为到参数中的元素的第一个子元素。
```javascript
$("<p></p>").prependTo("div");
```

### 3.3 after()，insertAfter():

after() 将`参数中的元素`插在`当前元素`后面，作为相邻元素。
```javascript
$("div").after("<p>Yes, I'm fine.</p>");
// <div>Are you OK?</div>
// 变为
// <div>Are you OK?</div>
// <p>Yes, I'm fine.</p>
```

insertAfter() 将`当前元素`插在`参数中的元素`后面。

```javascript
$("<p>Yes, I'm fine.</p>").insertAfter("div");
// <div>Are you OK?</div>
// 变为
// <div>Are you OK?</div>
// <p>Yes, I'm fine.</p>
```

### 3.4 before()，insertBefore()

before() 将`参数中的元素`插在当前元素的前面。
```javascript
$("div").before("<p>Are you OK?</p>");

// <div>Yes, I'm fine.</div>
// 变为
// <p>Are you OK?</p>
// <div>Yes, I'm fine.</div>
```
insertBefore() 将`当前元素`插在参数中的元素的前面。
```javascript
$("<p>Are you OK?</p>").insertBefore("div");
```

### 3.5 clone(): 克隆当前元素。
```javascript
var clones = $('li').clone();
```
注意：HTML的id属性是唯一的，不能重复出现。因此对于那些有id属性的节点，clone方法会连id属性一起克隆。所以，要把克隆的节点插入文档的时候，务必要修改或移除id属性。
```javascript
var clones = $('li').clone().prop('id', newIdName);
```

### 3.6 remove()，detach()，replaceWith()

remove()移除并`返回一个元素`，`取消`该元素上所有事件的绑定。
```javascript
$('p').remove();
```

detach()也是移除并返回一个元素，但是`不取消`该元素上所有事件的绑定。
```javascript
$('p').detach();
```

replaceWith()用参数中的元素，`替换并返回当前元素`，`取消`当前元素的所有事件的绑定。
```javascript
$('p').replaceWith('<div></div>')
```

### 3.7 wrap()，wrapAll()，unwrap()，wrapInner()

wrap() 将`参数中的元素`变成`当前元素的父元素`。
```javascript
$("p").wrap("<div></div>")

// <p>Hello</p>
// 变为
// <div><p>Hello</p></div>
```

wrap() 的参数还可以是一个`函数`。
```javascript
$("p").wrap(function() {
  return "<div></div>";
});
```
上面代码返回与前一个例子一样的结果。

wrapAll() 为`结果集的所有元素`，添加一个`共同的父元素`。
```javascript
$("p").wrapAll("<div></div>")

// <p></p><p></p>
// 变为
// <div><p></p><p></p></div>
```

unwrap() 移除`当前元素的父元素`。
```javascript
$("p").unwrap()

// <div><p></p></div>
// 变为
// <p></p>
```

wrapInner() 为当前元素的`所有子元素`，添加一个父元素。
```javascript
$("p").wrapInner('<strong></strong>')
// <p>Hello</p>
// 变为
// <p><strong>Hello</strong></p>
```

## 4.动画效果：

使用jQuery提供的方法，可以很容易地显示网页动画效果。但是不如CSS3 动画强大和节省资源，所以应该优先考虑使用CSS3 动画。

### 4.1动画效果的简便方法

    show()：显示当前元素。
    hide()：隐藏当前元素。
    toggle()：显示或隐藏当前元素。
    fadeIn()：将当前元素的不透明度（opacity）逐步提升到100%。
    fadeOut()：将当前元素的不透明度逐步降为0%。
    fadeToggle()：以逐渐透明或逐渐不透明的方式，折叠显示当前元素。
    slideDown()：以从上向下滑入的方式显示当前元素。
    slideUp()：以从下向上滑出的方式隐藏当前元素。
    slideToggle()：以垂直滑入或滑出的方式，折叠显示当前元素。

上面这些方法可以不带参数调用，也可以接受毫秒或预定义的关键字作为参数。
```javascript
$('.hidden').show();
$('.hidden').show(300);
$('.hidden').show('slow');
```

这些方法还可以接受一个`函数`作为第二个参数，表示动画结束后的`回调函数`。
```javascript
$('p').fadeOut(300, function() {
  $(this).remove();
});
```

### 4.2 animate():

上面这些动画效果方法，实际上都是animate方法的简便写法。

animate()接受两个参数，第一个参数是`一个对象`，表示动画结束时相关CSS属性的值，第二个参数是`动画持续的毫秒数`。需要注意的是，第一个参数对象的成员名称，必须与CSS属性名称一致，如果CSS属性名称带有连字号，则需要用“骆驼拼写法”改写。
```javascript
$('html, body').animate({
    scrollTop: 0
}, 500);
```

animate() 还可以接受第三个参数，表示动画结束时的`回调函数`。
```javascript
$('div').animate({
    left: '+=50', // 增加50
    opacity: 0.25,
    fontSize: '12px'
  },
  1000, // 持续时间
  function() { // 回调函数
     console.log('done!');
  }
);
```

### 4.3 stop()，delay():

stop()表示`立即停止`执行当前的动画。
```javascript
$("#stop").click(function() {
  $(".block").stop();
});
```
上面代码表示，点击按钮后，block元素的动画效果停止。

delay()接受一个时间参数，表示`暂停`多少毫秒后继续执行。
```javascript
$("#foo").slideUp(300)
        .delay(500)
        .fadeIn(400);
```
上面代码表示，slideUp动画之后，暂停500毫秒，然后继续执行fadeIn动画。

## 5. 处理表单元素的方法：

### 5.1 serialize(): 序列化
将`表单元素的值`，转为url使用的`查询字符串`。
```javascript
$( "form" ).on( "submit", function( event ) {
  event.preventDefault();
  console.log( $( this ).serialize() );
});
// single=Single&multiple=Multiple&check=check2&radio=radio1
```

### 5.2 serializeArray():
将`表单元素的值`转为`数组`。
```javascript
$("form").submit(function (event){
  console.log($(this).serializeArray());
  event.preventDefault();
});
//  [
//      {name : 'field1', value : 123},
//      {name : 'field2', value : 'hello world'}
//  ]
```
