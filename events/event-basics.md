# jQuery 事件基础知识

## 1. jQuery 事件基础知识

### 1.1 创建对 DOM 元素的事件响应

jQuery 使得为页面元素创建`事件驱动式`的响应变得很简单。这些事件通常被终端用户与页面的交互所触发，比如在表单元素中输入文本或者移动鼠标光标。在某些情况下，比如页面`加载(load)`或者`卸载(unload)`事件， 浏览器本身会触发事件。

jQueyr 为大多数元素浏览器事件提供了简便的方法。这些方法（包括`.click()`, `.focus()`, `.blur()`, `.change()` 等）都是 jQuery `on()` 方法的简写形式。 当我们想将同一个处理函数绑定到多个事件上，或者当我们想要为事件处理函数提供数据时，或者当我们使用自定义事件时，或者当我们想传递一个包含多个事件和处理函数的对象时，`on()` 方法 非常有用。

```javascript
// Event setup using a convenience method
$( "p" ).click(function() {
    console.log( "You clicked a paragraph!" );
});

// Equivalent event setup using the `.on()` method
$( "p" ).on( "click", function() {
    console.log( "click" );
})
```

### 1.2 为新的页面元素添加 事件

注意 `on()` 方法只能为已经存在的元素创建事件监听。在事件监听之后创建的类似元素不会自动获得之前创建的事件行为。例如：

```javascript
$( document ).ready(function(){
    // Sets up click behavior on all button elements with the alert class
    // that exist in the DOM when the instruction was executed
    $( "button.alert" ).on( "click", function() {
        console.log( "A button with the alert class was clicked!" );
    });
    // Now create a new button element with the alert class. This button
    // was created after the click listeners were applied above, so it
    // will not have the same click behavior as its peers
    $( "<button class='alert'>Alert!</button>" ).appendTo( document.body );
});
```

请查阅关于`事件委托(event delegation)`的文章，了解如何使用 `on()` 方法可以不用重新绑定事件而为新的元素提供事件行为。

### 1.3 事件处理函数内部

每个`事件处理函数`都接受一个`事件对象(event)` 作为参数。 这个事件对象有许多属性和方法。它最常用的是使用`.preventDefault()` 方法来阻止默认行为。事件对象还有大量的其他有用的属性和方法，包括：

#### 1.3.1 pageX, pageY

事件发生时，光标相对于页面（而不是整个浏览器窗口）左上角的位置。

#### 1.3.2 type

事件的类型 (比如 "click")。

#### 1.3.3 which

被按下的按钮或键。

#### 1.3.4 data

当事件被绑定时传入的数据。例如：

```javascript
// Event setup using the `.on()` method with data

$( "input" ).on(
  	"change",
    { foo: "bar" }, // Associate data with event binding
    function( eventObject ) {
        console.log("An input value has changed! ", eventObject.data.foo);
    }
);
```

#### 1.3.5 target

触发事件的 DOM 元素。

#### 1.3.6 namespace

事件被触发时指定的命名空间。

#### 1.3.7 timeStamp

事件发生时浏览器时间与 1970年1月1日时间差的毫秒数。

#### 1.3.8 preventDefault()

阻止事件的默认行为（比如，跟随一个链接跳转）。

#### 1.3.9 stopPropagation()

阻止事件`冒泡`到其他元素。

除了`事件对象`，`事件处理函`数也能够通过关键字`this` 获取`处理函数`绑定的 DOM 元素。为了让 DOM 元素变成 jQuery 对象，从而能够使用 jQuery 的方法，我们只需 这样 `$(this)` , 通常遵循以下习惯：
```javascript
var element = $( this );
```

更完整的示例，如下所示：
```javascript
// Preventing a link from being followed
$( "a" ).click(function( eventObject ) {
    var elem = $( this );
    if ( elem.attr( "href" ).match( /evil/ ) ) {
        eventObject.preventDefault();
        elem.addClass( "evil" );
    }
});
```

### 1.4 创建多个事件的响应

我们 应用程序 的许多元素需要绑定多个事件。如果多个事件有相同的处理函数，则我们可以给`on()` 方法传入一个用`空格隔开`的事件类型的列表：

```javascript
// Multiple events, same handler
$( "input" ).on(
    "click change", // Bind handlers for multiple events
    function() {
        console.log( "An input was clicked or changed!" );
    }
);
```

当每个事件有它自己的处理函数时，我们可以给`on()` 传入包含一个或多个键值对的对象。这个对象的键（key）是事件名称， 值（value）是处理事件的函数。

```javascript
// Binding multiple events with different handlers
$( "p" ).on({
    "click": function() { console.log( "clicked!" ); },
    "mouseover": function() { console.log( "hovered!" ); }
});
```

### 1.5 为事件添加命名空间（Namespacing Events ）

在复杂的应用或者与他人分享的插件中，为事件添加命名空间是很有用的。这样我们可以有意地将我们不知道的事件隔离。

```javascript
// Namespacing events
$( "p" ).on( "click.myNamespace", function() { /* ... */ } );
$( "p" ).off( "click.myNamespace" );
$( "p" ).off( ".myNamespace" ); // Unbind all events in the namespace
```

### 1.6 取消事件监听

为了删除事件监听，我们使用`.off()` 方法，参数为事件的类型。

```javascript
// Tearing down all click handlers on a selection
$( "p" ).off( "click" );
```

如果我们给这个事件绑定了一个命名的函数，我们可以通过将命名函数作为第二个参数传入的方式，单独取消这个事件。

```javascript
// Tearing down a particular click handler, using a reference to the function
var foo = function() { console.log( "foo" ); };
var bar = function() { console.log( "bar" ); };
$( "p" ).on( "click", foo ).on( "click", bar );
$( "p" ).off( "click", bar ); // foo is still bound to the click event
```

### 1.7 创建只运行一次的事件

有时，我们需要只运行一次的特殊的处理函数。之后，不需要执行处理函数，或者执行另一个处理函数。jQuery 提供了一个`one()` 方法可以用于这种目的。

```javascript
// Switching handlers using the `.one()` method
$( "p" ).one( "click", firstClick );

function firstClick() {
    console.log( "You just clicked this for the first time!" );
    // Now set up the new handler for subsequent clicks;
    // omit this step if no further click responses are needed
    $( this ).click( function() { console.log( "You have clicked this before!" ); } );
}
```

注意：上述代码片段中， `firstClick` 函数将会在每一个段落元素被第一次点击时执行。当任意段落元素被第一次点击时，函数并不是从所有段落中移除。

`.one()` 也可以用于绑定多个事件：

```javascript
// Using .one() to bind several events
$( "input[id]" ).one( "focus mouseover keydown", firstEvent);
function firstEvent( eventObject ) {
    console.log( "A " + eventObject.type + " event occurred for the first time on the input with id " + this.id );
}
```

在这个例子中， `firstEvent`  函数会为每个事件执行一次。 这意味着，在上述代码片段中，一旦 input 元素获得焦点，事件处理函数仍然会因为第一次 keydown 事件而执行。

本文翻译自[http://learn.jquery.com/events/event-basics](http://learn.jquery.com/events/event-basics)