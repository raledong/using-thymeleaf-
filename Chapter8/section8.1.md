# 8.1 引入模板片段
### 定义和引入片段 ###
---------------------------------------
我们常希望从别的模板中引入片段至当前模板中，比如引入页脚，抬头和菜单。

为了实现这个目的，Thymeleaf需要我们用`th:fragment`将这些部分定义为 *片段fragments*。

假设我们想要将一个标准的版权页脚加到杂货店的页面上，于是我们创建了`/WEB-INF/templates/footer.html`页面来包含这段代码：
```
<!DOCTYPE html>

<html xmlns:th="http://www.thymeleaf.org">

  <body>

    <div th:fragment="copy">
      &copy; 2011 The Good Thymes Virtual Grocery
    </div>

  </body>

</html>
```
上面这段代码定义了一个叫做`copy`的片段，我们可以在主页上使用`th:insert`或`th:replace`属性引入这段代码。
```
<body>

  ...

  <div th:insert="~{footer :: copy}"></div>

</body>
```
注意`th:insert`的值为片段表达式（`~{...}`），这个表达式会生成对应的片段。上例中的片段表达式不是很复杂，因此（`~{,}`）符号是完全可选的，所以上面的代码等价于：
```
<body>

  ...

  <div th:insert="footer :: copy"></div>

</body>
```

### 片段规范语法 ###
---------------------------------------
片段表达式的语法很直观。它有三种形式：
* `"~{templatename::selector}"`：从名为`templatename`的模板中用指定的markup选择器选择`selector`选择代码片段。注意`selector`可以是一个单纯的片段名，所以你可以像上面的`~{footer :: copy}`一样用`~{templatename : fragmentname}`选择片段。
>markup选择器语法由底层的 *AttoParser parsing library* 定义，它类似于XPath表达式或CSS选择器，更多信息参考 **附录C**。

* `"~{templatename}"`：包含了名为`templatename`模板的全部内容。
>注意你用在`th:insert/th:replace`属性中的模板名必须能够被当前模板引擎使用的模板解析器识别。

* `"~{::selector}"`或`"~{this::selector}"`从当前模板中找到匹配选择器`selector`的片段插入。如果当前模板上没有找到匹配表达式的片段，模板调用的堆栈将被遍历，直到找到这个`selector`匹配的片段。

上面例子中的`templatename`和`selector`的值都可以是表达式（甚至是选择表达式）：
```
<div th:insert="footer :: (${user.isAdmin}? #{footer.admin} : #{footer.normaluser})"></div>
```
再次注意`~{...}`标识在`th:insert/th:replace`中是可选的。

片段中能包含任何`th:*`属性。这些属性在片段被插入目标模板中（使用`th:insert/th:replace`属性的模板）时会立刻计算，而且它可以使用目标模板中定义的任何上下文变量。
> 用这种方式引用片段的最大的好处是片段所在的页面是完整甚至是结构合法的，它可以被浏览器正常展示。与此同时它还可以通过Thymeleaf被别的模板引用。

### 不通过`th:fragment`引用片段 ###
---------------------------------------
感谢Markup选择器，我们可以不用任何`th:fragment`属性来引用片段。它甚至可以是来自一个没有使用Thymeleaf的应用的一段markup代码：
```
...
<div id="copy-section">
  &copy; 2011 The Good Thymes Virtual Grocery
</div>
...
```
我们可以使用`id`属性来引用这段代码，其方式类似于CSS选择器：
```
<body>

  ...

  <div th:insert="~{footer :: #copy-section}"></div>

</body>
```

### `th:insert`和`th:replace`（以及`th:include`）之间的区别 ###
`th:insert`和`th:replace`（和`th:include`，该属性自3.0版本开始不推荐使用）之间的区别？
* `th:insert`是最简单的：将片段插入宿主标签下
* `th:replace`将宿主标签替换为片段
* `th:include`类似于`th:insert`,但是它只是插入该片段的子元素。

所以下面这段HTML代码：
```
<footer th:fragment="copy">
  &copy; 2011 The Good Thymes Virtual Grocery
</footer>
```
使用宿主`div`标签引用了三次，如下：
```
<body>

  ...

  <div th:insert="footer :: copy"></div>

  <div th:replace="footer :: copy"></div>

  <div th:include="footer :: copy"></div>

</body>
```
生成的结果为：
```
<body>

  ...

  <div>
    <footer>
      &copy; 2011 The Good Thymes Virtual Grocery
    </footer>
  </div>

  <footer>
    &copy; 2011 The Good Thymes Virtual Grocery
  </footer>

  <div>
    &copy; 2011 The Good Thymes Virtual Grocery
  </div>

</body>
```
