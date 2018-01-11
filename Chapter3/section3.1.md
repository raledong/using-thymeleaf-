# 3.1 一个支持多语言的欢迎界面
我们的第一个任务是为杂货店网站创建一个主页。

这个页面的第一个版本非常简单：只需要一个标题和一个欢迎语。下面是我们的`/WEB-INF/templates/home.html`文件：
```
<!DOCTYPE html>

<html xmlns:th="http://www.thymeleaf.org">

  <head>
    <title>Good Thymes Virtual Grocery</title>
    <meta http-equiv="Content-Type" content="text/html; charset=UTF-8" />
    <link rel="stylesheet" type="text/css" media="all"
          href="../../css/gtvg.css" th:href="@{/css/gtvg.css}" />
  </head>

  <body>

    <p th:text="#{home.welcome}">Welcome to our grocery store!</p>

  </body>

</html>
```
你首先会发现这是HTML5文件，可以被任何浏览器直接渲染因为它不包含任何的非HTML标签(浏览器会忽视所有它们不理解的属性，比如`th:text`)

但是你还会发现这个模板不是一个标准的HTML5文件，因为我们使用的形如`th:*`格式的属性是不标准的，这些属性不属于HTML5标准。事实上，我们甚至在`<html>`标签中添加了一个`xmlns:th`属性，这么做非常的不HTML5化。
```
<html xmlns:th="http://www.thymeleaf.org">
```
...添加了这个属性以后其实对模板处理不会带来任何影响，但是可以阻止我们的IDE因为`th:*`属性缺少命名空间的声明而一直报错。

所以如果我们希望这个模板是满足HTML5标准的话该怎么办呢？很简单：转变为Thymeleaf的数据属性语法，在属性上使用`data-`前缀，使用`-`而不是`:`做分隔符。
```
<!DOCTYPE html>

<html>

  <head>
    <title>Good Thymes Virtual Grocery</title>
    <meta http-equiv="Content-Type" content="text/html; charset=UTF-8" />
    <link rel="stylesheet" type="text/css" media="all"
          href="../../css/gtvg.css" data-th-href="@{/css/gtvg.css}" />
  </head>

  <body>

    <p data-th-text="#{home.welcome}">Welcome to our grocery store!</p>

  </body>

</html>
```
使用`data-`作为前缀的自定义属性是符合HTML5标准。因此，使用如上所示的代码，我们的模板就是一个合法的*HTML5*文档。
>这两种方法完全是等价且可以互换的，但是为了使样例代码看上去简洁和紧凑，这个教程将会使用命名空间标注法(`th:*`)。不仅如此，`th:*`更加普遍并且再其它Thymeleaf模板模式中也通用(XML.TEXT...)，但是`data-`标注法只能用于HTML模式。

###使用th:text和外部的文本###
---------------------------------------
外部文本是指从模板文件中提取一段模板代码并保存到一个单独的文件中(通常是`.properties`文件)。通过这种方式，这些文本可以很方便的被替换为等价的别国语言的文本。(这个过程称为国际化或者简称为`i18n`)。外部的文本片段通常被称为`消息messages`。

消息通常都有一个`键key`来标记他们，thymeleaf允许你使用`#{}`语法来指定文本应该对应的特定消息。
```
<p th:text="#{home.welcome}">Welcome to our grocery store!</p>
```
在这里我们看到的其实是Thymeleaf标准方言的两个功能。
* `th:text`属性计算了属性中的值并且将宿主标签中的内容改变为这个值。在这段代码里就是替换掉了代码中的*“Welcome to our grocery store!”* 。
* 在 *标准语法表达式* 中定义的`#{home.welcome}`表达式说明了`th:text`中的内容应该是键值为`home.welcome`的消息，这个消息会根据网站的访问地区调用相应语言的版本。
