# 介绍事件（Events）

## 1.序言

网页是关于交互的。用户执行大量的行为，比如在页面上移动鼠标光标，点击元素，在文本框输入内容。所有这些都是事件的例子。除了这些用户事件，还有大量的事件发生，比如当页面加载完成，当视频开始播放或者暂停等。无论在页面发生了什么有趣的事情，事件都会被触发，即浏览器会宣布某事发生了。这个宣告允许开发者监听事件，并作出相应的响应。

## 2.什么是 DOM 事件?

如上所述，事件有许多的类型，但是最容易理解的也许是用户事件，比如当某人点击了一个元素或者在表单中输入。这些类型的事件发生在元素上，即当用户点击了一个按钮，这个按钮上会发生事件。虽然用户交互不是唯一的 DOM 事件类型，但它们显然是在开始时最容易理解的。 Mozilla Developer Network 有一份关于[可用的 DOM 事件](https://developer.mozilla.org/en-US/docs/Web/Events)的参考手册。

## 3.监听事件的方法

有许多方法可以监听事件。行为（事件）始终发生在网页，但只有监听了事件，开发者才能收到关于它们的通知。监听事件意味着等待浏览器告知指定的事件发生了，然后规定网页如何作出响应。

我们提供一个函数，通常也成为事件处理（event handler)，指定事件发生时浏览器要做什么。每次事件发生时，这个函数都会被执行，直到该事件被取消。

例如，当用户点击一个按钮，弹出一条消息，我们可以写成这样：

```html
<button onclick="alert('Hello')">Say hello</button>
```

我们想要监听的事件由这个按钮的`onclick` 属性指定， 事件处理函数即这个`alert` 函数会弹出 “Hello” 给用户。虽然这种写法有用，但这项功能的这种实现方式是极糟糕的，原因如下：

- 1. 首先，我们把视图代码（HTML）和行为代码（JS） 混在一起了。这意味着每当我们需要更新功能，我们不得不编辑 HTML 代码，这是一个很糟的实现，很难维护。
- 2. 其次，它不可拓展。如果我们想把这个功能绑定到许多按钮上，不仅需要写一大堆的重复代码使得页面变得臃肿，而且还破坏了可维护性。

像这样使用*行内的事件处理*被认为是*obstrusive JavaScript*。它的对立面，*unobsrusive JavaScript* 是事件处理中更常用的方式。*unobstrusive JavaScript* 的概念指的是 HTML 和 JS 代码分离，因而更易于维护。 关注点分离是很重要的，因为它使得相似的代码片段（即 HTML, JS, CSS）放在一起，不同的代码分离开，有利于修改和优化。另外，*unobsrusive JavaScript*  强调尽可能少地往页面内添加代码。 如果用户的浏览器不支持 JavaScript, 那么JavaScript 代码就不应该混杂在页面的标记语言（HTML）中。同时，为了防止命名冲突，JS 代码应该为不同的功能代码或类库代码使用命名空间。jQuery 是这方面的一个好例子。在 `jQuery` 对象/构造函数 （以及别名`$` ）中只使用一个全局变量，jQuery 所有的功能都打包在这一个对象中。

为了低调地完成这个任务，让我们稍微修改一下 HTML 代码，删除`onclick` 属性，替换为 `id` 属性。稍后我们会在一个脚本文件中用这个`id` 与按钮关联。

```html
<button id="helloBtn">Say hello</button>
```

如果想在用户点击这个按钮后收到通知，我们应该在一个单独的脚本文件中这样写：

```javascript
// Event binding using addEventListener
var helloBtn = document.getElementById( "helloBtn" );

helloBtn.addEventListener( "click", function( event ) {
	alert( "Hello." );
}, false );
```

这里我们通过调用`getElementById`并把它的返回值赋值给一个变量，从而保存了这个按钮元素的引用。然后，我们调用`addEventListener`，并提供了一个当这个事件发生时将会执行的事件处理函数。虽然，在现在浏览器中这段代码能很好地执行，但它不兼容于 IE 9 之前的 IE 浏览器。这是因为微软选择使用不同的方法，即`attachEvent`, 以区别于 W3C 的标准 `addEventListener`。 直到 IE 9 的发布，才设法解决了这个问题。因此，使用 jQuery 是有帮助的。 因为它处理了浏览器间的不一致，允许开发者使用一个简单的 API 处理这些任务，如下所示：

```javascript
// Event binding using a convenience method
$( "#helloBtn" ).click(function( event ) {
    alert( "Hello." );
});
```

 `$( "#helloBtn" )` 使用 `$` (即 `jQuery`) 函数来选择按钮元素，返回一个 jQuery 对象。这个 jQuery 对象有大量可用的方法（函数），其中一个叫做`click`的方法，在 jQuery 对象的原型（prototype）上。我们在 jQuery 对象上调用这个 `click` 方法，并且传递了一个匿名的事件处理函数。当用户点击按钮时，将会执行这个匿名的事件处理函数，弹出 "Hello" 给用户。

There are a number of ways that events can be listened for using jQuery:

使用 jQuery 有许多种方式可以监听事件：

```javascript
// jQuery 有许多方式可以绑定事件

// 例1.使用 jQuery 的快捷方法`click`，直接绑定事件到按钮上。
$( "#helloBtn" ).click(function( event ) {
    alert( "Hello." );
});

// 例2.使用 jQuery 的 `bind` 的方法，传递`click` 事件字符串作为参数，直接绑定事件到按钮上。
$( "#helloBtn" ).bind( "click", function( event ) {
    alert( "Hello." );
});

// 例3. 从 jQuery 1.7 开始， 使用 jQuery 的 `on` 的方法，直接绑定事件到按钮上。
$( "#helloBtn" ).on( "click", function( event ) {
    alert( "Hello." );
});

// 例4. 从 jQuery 1.7 开始，将绑定事件到监听`click`事件的`body` 元素上，在这个页面内的任何按钮点击，都会做出响应。
$( "body" ).on({
    click: function( event ) {
        alert( "Hello." );
    }
}, "button" );

// 例5. 上一个例子的另一种写法，使用了稍微不同的语法。
$( "body" ).on( "click", "button", function( event ) {
    alert( "Hello." );
});
```

从 jQuery 1.7 版开始，无论直接调用 `on`还是使用 `bind` 或 `click`等别名/快捷方法（这些方法在内部映射到`on`方法），所有事件都是通过`on`方法绑定的。记住这点，使用`on`方法很有帮助，因为所有其他方法都只是语法糖，使`on`方法代码执行更快，且代码前后更一致。

让我们来看看上面的 `on` 方法的例子，讨论它们的区别。在第一个例子中，`click` 字符串作为第一个参数传入 `on` 方法，一个匿名函数作为第二个参数。这有点儿类似它前一个例子中的 `bind` 方法。这里我们直接将事件处理函数绑定到 `#helloBtn`。 如果页面内有其他的按钮，它们被点击时不会弹出 "Hello"，因为这个事件只绑定到了 `#helloBtn` 。

第二个`on` 的例子（例4），我们传入了一个由花括号`{}` 标记的对象。这个对象有一个`click` 属性，它的值是一个匿名函数。这个`on` 方法的第二个参数是一个 jQuery 选择器字符串`button`. 例子1-3 功能是一样的，例4 则有所不同，因为`body` 元素监听*任何*按钮上发生的点击事件，而不仅仅是`#helloBtn`按钮。 最后一个例子（例5）与前一个例子（功能）完全一样，但我们传了一个事件字符串（事件名称），一个选择器字符串和一个回调函数，而不是传递一个对象。这最后两个都是事件委托的例子。事件委托是一种让DOM 树种更上层的元素监听发生在它的子孙元素上的事件的机制。

## 4.事件委托（Event delegation）

事件委托奏效是因为*事件冒泡*的概念。大部分事件而言，无论何时事件发生在页面上（比如点击了一个元素），该事件将从发生事件的元素往上传递到它的父元素，然后是它的父元素的父元素，直到根元素，即`window`。在我们表格例子中，无论何时点击了一个`td` 元素，它的父元素`tr`也将会获知这个点击，祖先元素`table` 、`body`、`window`等都会获知这点击事件。虽然事件冒泡和事件委托奏效，但委托元素（在我们的例子中指的是`table`）应该尽可能地靠近被委托人（这里指的是`td`），这样事件不用在 DOM 树中冒泡太远就可以调用它的处理函数。

与直接绑定事件到一个元素（或一组元素）上相比，事件委托有两个主要有优点：性能表现和前面提到的事件冒泡。想象一下，一个有1000多个单元格的庞大的表格，绑定事件到每一个格子。即便它们都映射到同一个函数上，浏览器都得绑定1000个单独的事件处理函数。不绑定事件到每一个单独的单元格，我们可以使用委托机制，监听发生在父元素 table 上的事件，并作出相应的响应。

发生的事件冒泡为我们提供了通过 Ajax 新增单元格而无需直接绑定事件到这些单元格的能力。因为父元素 table 正在监听点击事件，能够获知到子孙元素的点击事件。如果我们没有使用事件委托，我们不得不持续地为每一个新增的单元格绑定事件，这将不仅仅是性能问题，同时也难以维护。

## 5. event 对象


在前面所有的例子中，我们使用匿名函数，并且指定了一个参数 `event`。让我们稍微修改一下。

```javascript
// 绑定命名的函数
function sayHello( event ) {
    alert( "Hello." );
}
$( "#helloBtn" ).on( "click", sayHello );
```

在这个稍微有点不同的例子中了，我们定义了 一个`sayHello` 函数，然后把这个函数而不是一个匿名函数作为参数传给`on` 方法。网上许多例子展示了匿名函数作为事件处理函，但明白这点也很重要：你也可以传递已定义的函数作为事件处理函数。 对于有不同的元素或者不同的事件执行相同的功能时，这很重要。这样可以帮助你保持你的代码 [DRY](http://en.wikipedia.org/wiki/Don%27t_repeat_yourself).

但`sayHello`函数的参数`event`是什么，以及它为什么重要？ 在所有的 DOM 事件回调函数中，jQuery 传入了一个 *event 对象*参数。它包含了该事件的信息，比如事件发生的时间和位置、事件类型、发生事件的元素以及其他的许多信息。当然，可以不使用`event`, 你可以用`e`或者其他任何你想使用字符串，但`event`是十分常见的习惯写法。

如果这个元素对于某个指定的事件有默认的功能（比如链接会打开新页面，表单中的按钮会提交表单等），这些默认行为可以被取消。这通常对于 Ajax 请求非常有用。当用户点击一个按钮通过 Ajax 去提交表单时，我们希望取消按钮或表单的默认行为（通过 `form` 标签的 `action` 属性提交表单），我们希望通过 Ajax 请求来完成同样的任务以获得更加流畅的体验。为此，我们使用 event 对象，并调用它的 `preventDefault()`  方法。我们也可以使用 `stopPropagation()` 阻止事件在 DOM 树中冒泡，这样父元素就不会获知该事件的发生（即便使用了事件委托）。

```javascript
// 阻止默认行为的发生以及事件冒泡
$( "form" ).on( "submit", function( event ) {
    // 阻止表单的默认提交
    event.preventDefault();

    // 阻止 事件在 DOM 树中冒泡，禁止事件委托
    event.stopPropagation();

    // 创建 Ajax 请求来提交表单数据
});
```

当同时使用 `.preventDefault()` 和 `.stopPropagation()` 时，你可以代之以 `return false` ，以更加简洁的方式来实现这两个功能，但只建议真的需要同时使用这两个功能而不是仅仅为了简洁才使用`return false`。关于 `.stopPropagation()` 需要记住的最后一点：在委托的事件中使用它时，事件冒泡能够被阻止的最快时候是当事件冒泡到它所委托的元素上时。

注意 event 对象 还包含了一个叫做`originalEvent`的属性，它是浏览器自己创建的 event 对象。jQuery 用一些有用的方法和属性包裹了这个原生的 event 对象，但在某些情况下，你需要通过 `event.originalEvent` 访问原生的 event。这尤其是对移动设备和平板上的 touch 事件非常有用。

最后，检查事件本身以及查看它所包含的所有数据，你应该使用 `console.log()` 在浏览器控制台打印该事件。这样可以让你看到一个事件的所有属性（包括 `originalEvent`），非常有助于调试。

```javascript
// Logging an event's information
// 打印 事件 的信息
$( "form" ).on( "submit", function( event ) {
    // Prevent the form's default submission.
    // 阻止表单的默认提交
    event.preventDefault();

    // Log the event object for inspectin'
    // 打印事件对象用于检查
    console.log( event );
    
    // Make an AJAX request to submit the form data
});
```

本文翻译自[http://learn.jquery.com/](http://learn.jquery.com/events/introduction-to-events/)