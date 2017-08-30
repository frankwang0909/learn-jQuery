# 介绍事件（Events）

## 1.序言

Web pages are all about interaction. Users perform a countless number of actions such as moving their mice over the page, clicking on elements, and typing in textboxes — all of these are examples of events. In addition to these user events, there are a slew of others that occur, like when the page is loaded, when video begins playing or is paused, etc. Whenever something interesting occurs on the page, an event is fired, meaning that the browser basically announces that something has happened. It's this announcement that allows developers to "listen" for events and react to them appropriately.

网页是关于交互的。用户执行大量的行为，比如在页面上移动鼠标光标，点击元素，在文本框输入内容。所有这些都是事件的例子。除了这些用户事件，还有大量的事件发生，比如当页面加载完成，当视频开始播放或者暂停等。无论在页面发生了什么有趣的事情，事件都会被触发，即浏览器会宣布某事发生了。这个宣告允许开发者监听事件，并作出相应的响应。

## 2.什么是 DOM 事件?

As mentioned, there are a myriad of event types, but perhaps the ones that are easiest to understand are user events, like when someone clicks on an element or types into a form. These types of events occur on an element, meaning that when a user clicks on a button for example, the button has had an event occur on it. While user interactions aren't the only types of DOM events, they're certainly the easiest to understand when starting out. Mozilla Developer Network has a good reference of available DOM events.

如上所述，事件有许多的类型，但是最容易理解的也许是用户事件，比如当某人点击了一个元素或者在表单中输入。这些类型的事件发生在元素上，即当用户点击了一个按钮，这个按钮上会发生事件。虽然用户交互不是唯一的 DOM 事件类型，但它们显然是在开始时最容易理解的。 Mozilla Developer Network 有一份关于[可用的 DOM 事件](https://developer.mozilla.org/en-US/docs/Web/Events)的参考手册。

## 3.监听事件的方法

There are many ways to listen for events. Actions are constantly occurring on a webpage, but the developer is only notified about them if they're listening for them. Listening for an event basically means you're waiting for the browser to tell you that a specific event has occurred and then you'll specify how the page should react.

有许多方法可以监听事件。行为（事件）始终发生在网页，但只有监听了事件，开发者才能收到关于它们的通知。监听事件意味着等待浏览器告知指定的事件发生了，然后规定网页如何作出响应。

To specify to the browser what to do when an event occurs, you provide a function, also known as an event handler. This function is executed whenever the event occurs (or until the event is unbound).

我们提供一个函数，通常也成为事件处理（event handler)，指定事件发生时浏览器要做什么。每次事件发生时，这个函数都会被执行，直到该事件被取消。

For instance, to alert a message whenever a user clicks on a button, you might write something like this:

例如，当用户点击一个按钮，弹出一条消息，我们可以写成这样：

```html
<button onclick="alert('Hello')">Say hello</button>
```

The event we want to listen to is specified by the button's onclick attribute, and the event handler is the alert function which alerts "Hello" to the user. While this works, it's an abysmal way to achieve this functionality for a couple of reasons:

- 1.First, we're coupling our view code (HTML) with our interaction code (JS). That means that whenever we need to update functionality, we'd have to edit our HTML which is just a bad practice and a maintenance nightmare.
- 2.Second, it's not scalable. If you had to attach this functionality onto numerous buttons, you'd not only bloat the page with a bunch of repetitious code, but you would again destroy maintainability.

我们想要监听的事件由这个按钮的`onclick` 属性指定， 事件处理函数即这个`alert` 函数会弹出 “Hello” 给用户。虽然这种写法有用，但这项功能的这种实现方式是极糟糕的，原因如下：

- 1. 首先，我们把视图代码（HTML）和行为代码（JS） 混在一起了。这意味着每当我们需要更新功能，我们不得不编辑 HTML 代码，这是一个很糟的实现，很难维护。
- 2. 其次，它不可拓展。如果我们想把这个功能绑定到许多按钮上，不仅需要写一大堆的重复代码使得页面变得臃肿，而且还破坏了可维护性。

Utilizing inline event handlers like this can be considered *obtrusive JavaScript,* but its opposite, *unobtrusive JavaScript* is a much more common way of discussing the topic. The notion of *unobtrusive JavaScript* is that your HTML and JS are kept separate and are therefore more maintainable. Separation of concerns is important because it keeps like pieces of code together (i.e. HTML, JS, CSS) and unlike pieces of code apart, facilitating changes, enhancements, etc. Furthermore, unobtrusive JavaScript stresses the importance of adding the least amount of cruft to a page as possible. If a user's browser doesn't support JavaScript, then it shouldn't be intertwined into the markup of the page. Also, to prevent naming collisions, JS code should utilize a single namespace for different pieces of functionality or libraries. jQuery is a good example of this, in that the `jQuery` object/constructor (and also the `$` alias to `jQuery`) only utilizes a single global variable, and all of jQuery's functionality is packaged into that one object.

像这样使用*行内的事件处理*被认为是*obstrusive JavaScript*。它的对立面，*unobsrusive JavaScript* 是事件处理中更常用的方式。*unobstrusive JavaScript* 的概念指的是 HTML 和 JS 代码分离，因而更易于维护。 关注点分离是很重要的，因为它使得相似的代码片段（即 HTML, JS, CSS）放在一起，不同的代码分离开，有利于修改和优化。另外，*unobsrusive JavaScript*  强调尽可能少地往页面内添加代码。 如果用户的浏览器不支持 JavaScript, 那么JavaScript 代码就不应该混杂在页面的标记语言（HTML）中。同时，为了防止命名冲突，JS 代码应该为不同的功能代码或类库代码使用命名空间。jQuery 是这方面的一个好例子。在 `jQuery` 对象/构造函数 （以及别名`$` ）中只使用一个全局变量，jQuery 所有的功能都打包在这一个对象中。

To accomplish the desired task unobtrusively, let's change our HTML a little bit by removing the `onclick` attribute and replacing it with an `id`, which we'll utilize to "hook onto" the button from within a script file.

为了低调地完成这个任务，让我们稍微修改一下 HTML 代码，删除`onclick` 属性，替换为 `id` 属性。稍后我们会在一个脚本文件中用这个`id` 与按钮关联。

```html
<button id="helloBtn">Say hello</button>
```

If we wanted to be informed when a user clicks on that button unobtrusively, we might do something like the following in a separate script file:

如果想在用户点击这个按钮后收到通知，我们应该在一个单独的脚本文件中这样写：

```javascript
// Event binding using addEventListener
var helloBtn = document.getElementById( "helloBtn" );

helloBtn.addEventListener( "click", function( event ) {
	alert( "Hello." );
}, false );
```


Here we're saving a reference to the button element by calling `getElementById` and assigning its return value to a variable. We then call `addEventListener` and provide an event handler function that will be called whenever that event occurs. While there's nothing wrong with this code as it will work fine in modern browsers, it won't fare well in versions of IE prior to IE9. This is because Microsoft chose to implement a different method, `attachEvent`, as opposed to the W3C standard `addEventListener`, and didn't get around to changing it until IE9 was released. For this reason, it's beneficial to utilize jQuery because it abstracts away browser inconsistencies, allowing developers to use a single API for these types of tasks, as seen below.

这里我们通过调用`getElementById`并把它的返回值赋值给一个变量，从而保存了这个按钮元素的引用。然后，我们调用`addEventListener`，并提供了一个当这个事件发生时将会执行的事件处理函数。虽然，在现在浏览器中这段代码能很好地执行，但它不兼容于 IE 9 之前的 IE 浏览器。这是因为微软选择使用不同的方法，即`attachEvent`, 以区别于 W3C 的标准 `addEventListener`。 直到 IE 9 的发布，才设法解决了这个问题。因此，使用 jQuery 是有帮助的。 因为它处理了浏览器间的不一致，允许开发者使用一个简单的 API 处理这些任务，如下所示：

```javascript
// Event binding using a convenience method
$( "#helloBtn" ).click(function( event ) {
    alert( "Hello." );
});
```

The `$( "#helloBtn" )` code selects the button element using the `$` (a.k.a. `jQuery`) function and returns a jQuery object. The jQuery object has a bunch of methods (functions) available to it, one of them named `click`, which resides in the jQuery object's prototype. We call the `click` method on the jQuery object and pass along an anonymous function event handler that's going to be executed when a user clicks the button, alerting "Hello." to the user.

 `$( "#helloBtn" )` 使用 `$` (即 `jQuery`) 函数来选择按钮元素，返回一个 jQuery 对象。这个 jQuery 对象有大量可用的方法（函数），其中一个叫做`click`的方法，在 jQuery 对象的原型（prototype）上。我们在 jQuery 对象上调用这个 `click` 方法，并且传递了一个匿名的事件处理函数。当用户点击按钮时，将会执行这个匿名的事件处理函数，弹出 "Hello" 给用户。

There are a number of ways that events can be listened for using jQuery:

使用 jQuery 有许多种方式可以监听事件：

```javascript
// The many ways to bind events with jQuery
// jQuery 有许多方式可以绑定事件

// Attach an event handler directly to the button using jQuery's shorthand `click` method.
// 例1.使用 jQuery 的快捷方法`click`，直接绑定事件到按钮上。
$( "#helloBtn" ).click(function( event ) {
    alert( "Hello." );
});
// Attach an event handler directly to the button using jQuery's `bind` method, passing it an event string of `click`
// 例2.使用 jQuery 的 `bind` 的方法，传递`click` 事件字符串作为参数，直接绑定事件到按钮上。
$( "#helloBtn" ).bind( "click", function( event ) {
    alert( "Hello." );
});

// As of jQuery 1.7, attach an event handler directly to the button using jQuery's `on` method.
// 例3. 从 jQuery 1.7 开始， 使用 jQuery 的 `on` 的方法，直接绑定事件到按钮上。
$( "#helloBtn" ).on( "click", function( event ) {
    alert( "Hello." );
});
// As of jQuery 1.7, attach an event handler to the `body` element that is listening for clicks, and will respond whenever *any* button is clicked on the page.
// 例4. 从 jQuery 1.7 开始，将绑定事件到监听`click`事件的`body` 元素上，在这个页面内的任何按钮点击，都会做出响应。
$( "body" ).on({
    click: function( event ) {
        alert( "Hello." );
    }
}, "button" );

// An alternative to the previous example, using slightly different syntax.
// 例5. 上一个例子的另一种写法，使用了稍微不同的语法。
$( "body" ).on( "click", "button", function( event ) {
    alert( "Hello." );
});
```

As of jQuery 1.7, all events are bound via the `on` method, whether you call it directly or whether you use an alias/shortcut method such as `bind` or `click`, which are mapped to the `on` method internally. With this in mind, it's beneficial to use the `on` method because the others are all just syntactic sugar, and utilizing the `on` method is going to result in faster and more consistent code.

从 jQuery 1.7 版开始，无论直接调用 `on`还是使用 `bind` 或 `click`等别名/快捷方法（这些方法在内部映射到`on`方法），所有事件都是通过`on`方法绑定的。记住这点，使用`on`方法很有帮助，因为所有其他方法都只是语法糖，使`on`方法代码执行更快，且代码前后更一致。

Let's look at the `on` examples from above and discuss their differences. In the first example, a string of `click` is passed as the first argument to the `on` method, and an anonymous function is passed as the second. This looks a lot like the `bind` method before it. Here, we're attaching an event handler directly to `#helloBtn`. If there were any other buttons on the page, they wouldn't alert "Hello" when clicked because the event is only attached to `#helloBtn`.

让我们来看看上面的 `on` 方法的例子，讨论它们的区别。在第一个例子中，`click` 字符串作为第一个参数传入 `on` 方法，一个匿名函数作为第二个参数。这有点儿类似它前一个例子中的 `bind` 方法。这里我们直接将事件处理函数绑定到 `#helloBtn`。 如果页面内有其他的按钮，它们被点击时不会弹出 "Hello"，因为这个事件只绑定到了 `#helloBtn` 。

In the second `on` example, we're passing an object (denoted by the curly braces `{}`), which has a property of `click` whose value is an anonymous function. The second argument to the `on` method is a jQuery selector string of `button`. While examples 1–3 are functionally equivalent, example 4 is different in that the `body` element is listening for click events that occur on *any* button element, not just `#helloBtn`. The final example above is exactly the same as the one preceding it, but instead of passing an object, we pass an event string, a selector string, and the callback. Both of these are examples of event delegation, a process by which an element higher in the DOM tree listens for events occurring on its children.

第二个`on` 的例子（例4），我们传入了一个由花括号`{}` 标记的对象。这个对象有一个`click` 属性，它的值是一个匿名函数。这个`on` 方法的第二个参数是一个 jQuery 选择器字符串`button`. 例子1-3 功能是一样的，例4 则有所不同，因为`body` 元素监听*任何*按钮上发生的点击事件，而不仅仅是`#helloBtn`按钮。 最后一个例子（例5）与前一个例子（功能）完全一样，但我们传了一个事件字符串（事件名称），一个选择器字符串和一个回调函数，而不是传递一个对象。这最后两个都是事件委托的例子。事件委托是一种让DOM 树种更上层的元素监听发生在它的子孙元素上的事件的机制。

## 4.事件委托（Event delegation）

Event delegation works because of the notion of *event bubbling*. For most events, whenever something occurs on a page (like an element is clicked), the event travels from the element it occurred on, up to its parent, then up to the parent's parent, and so on, until it reaches the root element, a.k.a. the `window`. So in our table example, whenever a `td` is clicked, its parent `tr` would also be notified of the click, the parent `table` would be notified, the `body` would be notified, and ultimately the `window` would be notified as well. While event bubbling and delegation work well, the delegating element (in our example, the `table`) should always be as close to the delegatees as possible so the event doesn't have to travel way up the DOM tree before its handler function is called.

事件委托奏效是因为*事件冒泡*的概念。大部分事件而言，无论何时事件发生在页面上（比如点击了一个元素），该事件将从发生事件的元素往上传递到它的父元素，然后是它的父元素的父元素，直到根元素，即`window`。在我们表格例子中，无论何时点击了一个`td` 元素，它的父元素`tr`也将会获知这个点击，祖先元素`table` 、`body`、`window`等都会获知这点击事件。虽然事件冒泡和事件委托奏效，但委托元素（在我们的例子中指的是`table`）应该尽可能地靠近被委托人（这里指的是`td`），这样事件不用在 DOM 树中冒泡太远就可以调用它的处理函数。

The two main pros of event delegation over binding directly to an element (or set of elements) are performance and the aforementioned event bubbling. Imagine having a large table of 1,000 cells and binding to an event for each cell. That's 1,000 separate event handlers that the browser has to attach, even if they're all mapped to the same function. Instead of binding to each individual cell though, we could instead use delegation to listen for events that occur on the parent table and react accordingly. One event would be bound instead of 1,000, resulting in way better performance and memory management.

与直接绑定事件到一个元素（或一组元素）上相比，事件委托有两个主要有优点：性能表现和前面提到的事件冒泡。想象一下，一个有1000多个单元格的庞大的表格，绑定事件到每一个格子。即便它们都映射到同一个函数上，浏览器都得绑定1000个单独的事件处理函数。不绑定事件到每一个单独的单元格，我们可以使用委托机制，监听发生在父元素 table 上的事件，并作出相应的响应。

The event bubbling that occurs affords us the ability to add cells via Ajax for example, without having to bind events directly to those cells since the parent table is listening for clicks and is therefore notified of clicks on its children. If we weren't using delegation, we'd have to constantly bind events for every cell that's added which is not only a performance issue, but could also become a maintenance nightmare.

发生的事件冒泡为我们提供了通过 Ajax 新增单元格而无需直接绑定事件到这些单元格的能力。因为父元素 table 正在监听点击事件，能够获知到子孙元素的点击事件。如果我们没有使用事件委托，我们不得不持续地为每一个新增的单元格绑定事件，这将不仅仅是性能问题，同时也难以维护。

## 5. event 对象

In all of the previous examples, we've been using anonymous functions and specifying an `event` argument within that function. Let's change it up a little bit.

在前面所有的例子中，我们使用匿名函数，并且指定了一个参数`event`。让我们稍微修改一下。

```javascript
// Binding a named function
// 绑定命名的函数
function sayHello( event ) {
    alert( "Hello." );
}
$( "#helloBtn" ).on( "click", sayHello );
```

In this slightly different example, we're defining a function called `sayHello` and then passing that function into the `on` method instead of an anonymous function. So many online examples show anonymous functions used as event handlers, but it's important to realize that you can also pass defined functions as event handlers as well. This is important if different elements or different events should perform the same functionality. This helps to keep your code [DRY](http://en.wikipedia.org/wiki/Don%27t_repeat_yourself).

在这个稍微有点不同的例子中了，我们定义了 一个`sayHello` 函数，然后把这个函数而不是一个匿名函数作为参数传给`on` 方法。网上许多例子展示了匿名函数作为事件处理函，但明白这点也很重要：你也可以传递已定义的函数作为事件处理函数。对于有不同的元素或者不同的事件执行相同的功能时，这很重要。这样可以帮助你保持你的代码 [DRY](http://en.wikipedia.org/wiki/Don%27t_repeat_yourself).

But what about that `event` argument in the `sayHello` function — what is it and why does it matter? In all DOM event callbacks, jQuery passes an *event object* argument which contains information about the event, such as precisely when and where it occurred, what type of event it was, which element the event occurred on, and a plethora of other information. Of course you don't have to call it `event`; you could call it `e` or whatever you want to, but `event` is a pretty common convention.

但`sayHello`函数的参数`event`是什么，以及它为什么重要？ 在所有的 DOM 事件回调函数中，jQuery 传入了一个 *event 对象*参数。它包含了该事件的信息，比如事件发生的时间和位置、事件类型、发生事件的元素以及其他的许多信息。当然，可以不使用`event`, 你可以用`e`或者其他任何你想使用字符串，但`event`是十分常见的习惯写法。

If the element has default functionality for a specific event (like a link opens a new page, a button in a form submits the form, etc.), that default functionality can be canceled. This is often useful for Ajax requests. When a user clicks on a button to submit a form via Ajax, we'd want to cancel the button/form's default action (to submit it to the form's `action` attribute), and we would instead do an Ajax request to accomplish the same task for a more seamless experience. To do this, we would utilize the event object and call its `.preventDefault()` method. We can also prevent the event from bubbling up the DOM tree using `.stopPropagation()` so that parent elements aren't notified of its occurrence (in the case that event delegation is being used).

如果这个元素对于某个指定的事件有默认的功能（比如链接会打开新页面，表单中的按钮会提交表单等），这些默认行为可以被取消。这通常对于 Ajax 请求非常有用。当用户点击一个按钮通过 Ajax 去提交表单时，我们希望取消按钮或表单的默认行为（通过 `form` 标签的 `action` 属性提交表单），我们希望通过 Ajax 请求来完成同样的任务以获得更加流畅的体验。为此，我们使用 event 对象，并调用它的 `preventDefault()`  方法。我们也可以使用 `stopPropagation()` 阻止事件在 DOM 树中冒泡，这样祖先元素就不会获知该事件的发生（即便使用了事件委托）。

```javascript
// Preventing a default action from occurring and stopping the event bubbling
// 阻止默认行为的发生以及事件冒泡
$( "form" ).on( "submit", function( event ) {
    // Prevent the form's default submission.
    // 阻止表单的默认提交
    event.preventDefault();

    // Prevent event from bubbling up DOM tree, prohibiting delegation
    // 阻止 事件在 DOM 树中冒泡，禁止事件委托
    event.stopPropagation();

    // Make an AJAX request to submit the form data
    // 创建 Ajax 请求来提交表单数据
});
```

When utilizing both `.preventDefault()` and `.stopPropagation()` simultaneously, you can instead `return false` to achieve both in a more concise manner, but it's advisable to only `return false` when both are actually necessary and not just for the sake of terseness. A final note on `.stopPropagation()` is that when using it in delegated events, the soonest that event bubbling can be stopped is when the event reaches the element that is delegating it.

当同时使用 `.preventDefault()` 和 `.stopPropagation()` 时，你可以代之以 `return false` ，以更加简洁的方式来实现这两个功能，但只建议真的需要同时使用这两个功能而不是仅仅为了简洁才使用`return false`。关于 `.stopPropagation()` 需要记住的最后一点：在委托的事件中使用它时，事件冒泡能够被阻止的最快时候是当事件冒泡到它所委托的元素上时。

It's also important to note that the event object contains a property called `originalEvent`, which is the event object that the browser itself created. jQuery wraps this native event object with some useful methods and properties, but in some instances, you'll need to access the original event via `event.originalEvent` for instance. This is especially useful for touch events on mobile devices and tablets.

注意 event 对象 还包含了一个叫做`originalEvent`的属性，它是浏览器自己创建的 event 对象。jQuery 用一些有用的方法和属性包裹了这个原生的 event 对象，但在某些情况下，你需要通过 `event.originalEvent` 访问原生的 event。这尤其是对移动设备和平板上的 touch 事件非常有用。

Finally, to inspect the event itself and see all of the data it contains, you should log the event in the browser's console using `console.log`. This will allow you to see all of an event's properties (including the `originalEvent`) which can be really helpful for debugging.

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