# Event Helpers

jQuery 提供了一些事件相关简便方法可以帮我们节省时间，比如`hover()` 函数。

### `.hover()`

`.hover()` 方法可以传递一个或者两个函数作为`mouseenter` 和 `mouseleave` 事件发生时执行的回调函数。如果传入一个函数作为参数，那么两个事件都执行这个函数。如果传入两个函数作为参数，这`mouseenter`事件执行第一个函数，`mouseleave` 事件执行第二个函数。

**注意:**  jQuery 1.4版 之前 `.hover()` 方法需要传入两个函数作为参数。

```javascript
// The hover helper function
$( "#menu li" ).hover(function() {
    $( this ).toggleClass( "hover" );
});
```

更多的简便方法，请查阅 [API site for Events](https://api.jquery.com/category/events/).