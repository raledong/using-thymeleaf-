# 4.4 URLs
URL在web应用的模板中是一类公民，因为它们实在是太重要了。Thymeleaf标准方言为其提供了一个特殊的语法，`@`和`@{...}`

有以下几种类型的URL：
* 绝对路径 Absolute URLs: `http://www.thymeleaf.org`
* 相对路径 Relative URLs:
  * 相对页面的 Page-relative: `user/login.html`
  * 相对上下文的 Context-relative: `/itemdetails?id=3` (服务器中的上下文名称会被自动添至开头)
  * 相对服务器的 Server-relative: `~/billing/processInvoice` (允许调用同一个服务器上另一个上下文（= 应用）中的URL）
  * 相对协议的 Protocol-relative: `//code.jquery.com/jquery-2.0.3.min.js`

这些表达式的实时处理和输出转化后的URL通过实现`org.thymeleaf.linkbuilder.ILinkBuilder`接口来完成。这个接口需要被注册至使用的`ITemplateEngine`对象。

存在这个接口的一个默认实现类`org.thymeleaf.linkbuilder.StandardLinkBuilder`，这已经满足大多数离线场景(非web应用)和基于Servlet API的web场景。其它场景（如集成不是基于Servlet API的web框架）可能需要特殊的link builder接口的实现。

让我们使用这种新语法。先看一下`th:href`属性：
```
<!-- 会生成 'http://localhost:8080/gtvg/order/details?orderId=3' (在重写之后) -->
<a href="details.html"
   th:href="@{http://localhost:8080/gtvg/order/details(orderId=${o.id})}">view</a>

<!-- 会产生 '/gtvg/order/details?orderId=3' (在重写之后) -->
<a href="details.html" th:href="@{/order/details(orderId=${o.id})}">view</a>

<!-- 会产生 '/gtvg/order/3/details' (在重写之后) -->
<a href="details.html" th:href="@{/order/{orderId}/details(orderId=${o.id})}">view</a>
```
这里需要注意几件事情：
* `th:href`是一个修饰型属性：一旦被处理，他就能计算URL的值并且将其赋予`<a>`标签的`href`属性。
* 我们允许传递给URL的参数中使用表达式（如之前看到的`orderId=${o.id}`）。URL参数的编码也会按需自动执行。
* 如果需要传递多个参数，这些参数之间需要用逗号隔开：`@{/order/process(execId=${execId},execType='FAST')}`
* 路径中包含参数变量的形式也是允许的：`@{/order/{orderId}/details(orderId=${orderId})}`
* 以`/`作为开头的路径（如`/order/details`）会被自动添加上应用上下文名称作为前缀。
* 如果cookies被禁用了，那么`";jsessionid=..."`后缀会被添加至相对路径，这样session就可以被保存。这个叫做 *URL重写（URL Rewriting）*。Thymeleaf允许你使用Servlet API中的`response.encodeURL(...)`方法，为每一个URL加入自定义的重写过滤器
* `th:href`属性允许我们在模板中额外设置（可选）一个静态的`href`属性。这样的话，这个用于原型设计的静态模板中的链接在浏览器中还是可以跳转的。

与消息语法（`#{...}`）一样，URL本身也可以是另一个表达式的计算结果：
```
<a th:href="@{${url}(orderId=${o.id})}">view</a>
<a th:href="@{'/details/'+${user.login}(orderId=${o.id})}">view</a>
```
