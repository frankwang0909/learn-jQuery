# 触发事件处理函数

jQuery 提供了一个`.trigger()` 方法，可以在没有用户交互时，触发绑定到元素上的 事件处理函数（event handlers） 的方法。

## 1.哪些事件处理函数可以被`.trigger()`触发?

jQuery 的事件处理系统是建立在 原生浏览器事件之上的。使用  `.on( "click", function() {...} )` 增加的事件处理函数，可以通过 jQuery 的` .trigger( "click " )` 触发，是因为jQuery 在这个事件处理函数被添加时，存储了这个函数的引用。

除此之外，它将在 （HTML标签的）`onclick `属性上内触发这段 JavaScript 代码（即这个事件处理函数）。

·`.trigger()` 方法不能被用于模拟诸如点击`文件上传控件`或`锚链接（即a标签）` 等原生的浏览器事件。这是因为通过 jQuery 事件系统绑定的事件中，没有与这些事件类似的事件。

```html
<a href="http://learn.jquery.com">Learn jQuery</a>
```

```javascript
// This will not change the current page
$( "a" ).trigger( "click" );
```

## 2.如果不能使用`.trigger()`，如何模拟原生的浏览器事件？

In order to trigger a native browser event, you have to use [document.createEventObject](http://msdn.microsoft.com/en-us/library/ie/ms536390%28v=vs.85%29.aspx) for < IE9 and [document.createEvent](https://developer.mozilla.org/en-US/docs/Web/API/Document/createEvent)for all other browsers. Using these two APIs, you can programmatically create an event that behaves exactly as if someone has actually clicked on a file input box. The default action will happen, and the browse file dialog will display.

为了触发原生浏览器事件，针对IE9，必须使用 [document.createEventObject](http://msdn.microsoft.com/en-us/library/ie/ms536390%28v=vs.85%29.aspx)  ， 其他浏览器使用  [document.createEvent](https://developer.mozilla.org/en-US/docs/Web/API/Document/createEvent) 。通过这两个 API, 我们可以创建一个事件，它看起来就像是有人真地点击了一个文件上传控件。

The jQuery UI Team created [jquery.simulate.js](https://github.com/jquery/jquery-simulate/) in order to simplify triggering a native browser event for use in their automated testing. Its usage is modeled after jQuery's trigger.

  ` jQuery UI` 团队为了简化在他们自动化测试中使用的原生浏览器事件的触发， 创建了 [jquery.simulate.js](https://github.com/jquery/jquery-simulate/) 。 它的用法模仿了jQuery的`trigger()`。

```javascript
// Triggering a native browser event using the simulate plugin
$( "a" ).simulate( "click" );
```

上述代码，不仅会触发 jQuery 事件处理函数，还会跟随a链接跳转页面。


## 3.`.trigger()` vs `.triggerHandler()`

`.trigger()` 和 `.triggerHandler()` 有以下四个不同：

1. `.triggerHandler()` 只触发 jQuery 对象第一个成员上的事件。
2. `.triggerHandler()` 不能链式调用。它返回的不是 jQuery 对象，而是最后一个处理函数返回的值。
3. `.triggerHandler()` will not cause the default behavior of the event (such as a form submission).
4. 被 `.triggerHandler()` 触发的事件 不会再 DOM 树 中冒泡，只有单个元素上的处理函数会被触发。

更多信息，请查阅  [triggerHandler documentation](http://api.jquery.com/triggerHandler/) 。


## 4.不要使用 `.trigger()` 直接去执行具体的函数

虽然这个 `.trigger()` 方法有用，但它不应该去直接调用一个处理点击事件的函数。相反，我们应该把想要调用的函数存在一个变量中，在绑定事件时把这个变量名作为参数传进去。之后，我们可以在任何时候调用这个函数，而不需要使用 `.trigger()`.

```javascript
// Triggering an event handler the right way
var foo = function( event ) {
    if ( event ) {
        console.log( event );
    } else {
        console.log( "this didn't come from an event!" );
    }
};
 
$( "p" ).on( "click", foo );
 
foo(); // instead of $( "p" ).trigger( "click" )
```
使用 [jQuery 插件](https://gist.github.com/661855) 的[发布-订阅 模式](http://en.wikipedia.org/wiki/Publish%E2%80%93subscribe_pattern) 可以构建更加复杂的触发事件的架构。运用这项技术，`trigger()` 可以用于 通知其他部分的代码 应用的某个事件发生了。


[本文翻译自 http://learn.jquery.com](http://learn.jquery.com/events/triggering-event-handlers/)

