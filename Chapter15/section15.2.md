# 15.2 消息解析器
我们没有显式的为杂货店应用定义一个消息解析器的实现。正如前面所说，这意味着当前使用的实现为`org.thymeleaf.messageresolver.StandardMessageResolver`对象。

`StandardMessageResolver`是`IMessageResolver`接口的标准实现，但是我们也可以按需自定义，从而适应应用的特殊需求。

> Thymeleaf + Spring集成包默认提供了`IMessageResolver`的实现，它通过使用Spring应用上下文中声明的`MessageSource`bean，以标准的Spring方式获取外部消息。

### 标准消息解析器 ###
---------------------------------------
`StandardMessageResolver`是如何找到某个模板中请求的消息的？

如果模板的名称为`home`，位于`/WEB-INF/templates/home.html`，且语言环境为`gl_ES`，那么解析器以如下顺序在下列文件中查找消息：
* `/WEB-INF/templates/home_gl_ES.properties`
* `/WEB-INF/templates/home_gl.properties`
* `/WEB-INF/templates/home.properties`
可以参考JavaDoc文件中的`StandardMessageResolver`类了解完整的消息解析机制的运作方式。

### 配置消息解析器 ###
---------------------------------------
如果我们想要添加一个（或多个）消息解析器至模板引擎呢？很简单：
```
// 只设置一个
templateEngine.setMessageResolver(messageResolver);

// 设置多个
templateEngine.addMessageResolver(messageResolver);
```
为什么我们需要多个消息解析器？和多个模板解析器的理由是一样的：多个消息解析器按顺序处理，如果第一个不能解析某条消息，那么就会询问第二个，第三个等。
