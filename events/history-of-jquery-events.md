# jQuery Events 历史

贯穿整个 jQuery 的发展史，事件绑定的方式 因为许多原因（从性能表现到语法语义等）而发生过改变。jQuery 1.7 版开始，`.on()` 方法作为直接绑定事件以及创建委托事件的方式。这篇文字旨在探讨事件委托(event delegation)从 jQuery 1.0版本到现在的历史以及每个版本是如何使用事件委托的。

以下 HTML 代码作为示例，当它被点击时，我们想要在控制台中打印出每一个有`<li>` 。

```html
<div id="container">
    <ul id="list">
        <li>Item #1</li>
        <li>Item #2</li>
        <li>Item #3</li>
        <li>...</li>
        <li>Item #100</li>
    </ul>
</div>
```

### 1.  [.bind()](http://api.jquery.com/bind/) (已弃用)

jQuery 1.0 版开始引入。

可以使用` .bind()` 为每个元素绑定处理函数。

```javascript
$( "#list li" ).bind( "click", function( event ) {
    var elem = $( event.target );
    console.log( elem.text() );
});
```

在 [事件委托](http://learn.jquery.com/event/event-delegation/) 中已经讨论过，这种方式不是最优的。

### 2.  liveQuery

liveQuery 是一个流行的 jQuery 插件。它允许创建事件，这些事件可为现有或者将来的元素触发。这个插件 没有使用事件委托，而是使用昂贵的CPU 进程，每20秒去 调查 DOM 的改变，然后触发相应的事件。

### 3.  [.bind()](http://api.jquery.com/bind/) 委托 (已弃用)

jQuery 1.0 版开始引入。

通常，我们不会把`bind()` 与 事件委托联系在一起。然而，在 1.3版本之前，它是我们能够使用事件委托的唯一方式。

```javascript
$( "#list" ).bind( "click", function( event ) {
    var elem = $( event.target );
    if ( elem.is( "li" ) ) {
        console.log( elem.text() );
    }
});
```

我们可以利用 `事件冒泡`，将 `click` 事件绑定到父元素`<ul>` 上。 当`<li>` 被点击时，事件冒泡到父元素`<ul>` 上。父元素将会执行事件处理函数。我们的事件处理函数会检查`event.target` （触发事件的元素）是否与选择器匹配。

### 4.  [.live()](http://api.jquery.com/live/) (已弃用)

jQuery 1.3 版开始引入。

默认情况下，所有的 `.live()` 事件处理函数都会被绑定到 文档的根元素上。

```javascript
$( "#list li" ).live( "click", function( event ) {
    var elem = $( this );
    console.log( elem.text() );
});
```

当我们使用`live()` 时，事件被绑定到`$(document)` 上。当`<li>`被点击时，发生事件冒泡，在以下的每个元素上都会触发 *click* 事件：

- `<ul>`
- `<div>`
- `<body>`
- `<html>`
- *document* root

最后接收到 *click* 事件的是 `document` ，即`.live()` 事件所绑定的地方。`live()` 会检查 选择器 `#list li` 是否是触发事件的元素，如果是，则执行事件处理函数。

### 5. [.live()](http://api.jquery.com/live/) w/ context (已弃用)

jQuery 1.4 版开始引入。

从1.0版本开始，就已经支持将*上下文* 作为第二个可选的参数传入`$()` 方法。然而直到1.4 版本，才支持在`$.live()` 中使用*上下文*。 

如果我们使用之前的 `.live()` 的例子，提供默认的*上下文*， 如下面代码所示：

```javascript
$( "#list li", document ).live( "click", function( event ) {
    var elem = $( this );
    console.log( elem.text() );
});
```

由于使用`.live()` 方法时可以重写 *上下文*， 我们可以指定一个在 DOM 树中更加靠近这个元素的*上下文* .

```javascript
$( "li", "#list" ).live( "click", function( event ) {
    var elem = $( this );
    console.log( elem.text() );
});
```

在这个例子中，当一个`<li>` 被点击后，事件仍然会像之前一样沿着 DOM 树 一直向上冒泡。然而，我们的事件处理函数现在绑定在父元素`<ul>` 标签上，所以，不需要等到事件冒泡到 文档根元素。

### 6.  [.delegate()](http://api.jquery.com/delegate/)  (已弃用)

jQuery 1.4.2 版开始引入。

`.delegate()` 方法有一些明显的不同，主要体现在1）绑定委托的事件处理函数的`上下文`，2）当事件冒泡到委托的元素时，去匹配的`选择器`。

```javascript
$( "#list" ).delegate( "li", "click", function( event ) {
    var elem = $( this );
    console.log( elem.text() );
});
```

### 7.  [.on()](http://api.jquery.com/on/)

jQuery 1.7 版开始引入。

`on()` 方法给了我们一种语义上的方法，可以直接绑定事件以及委托事件。它提供一个唯一的 用于创建事件的 API，使得那些弃用的`.bind()`， `.live()` 和`.delegate()` 等方法不再需要。

### 小结 

以上所有的*事件委托*方式在它们发布之时，都曾是革命性且最好的实现方式。根据你所使用的 jQuery 版本，使用合适的*事件委托*方式。

本文翻译自 [http://learn.jquery.com](http://learn.jquery.com/events/history-of-events/)