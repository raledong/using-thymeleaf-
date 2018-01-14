# 5.2 设置特定属性的值
现在你可能在想，代码写成下面这样实在是太丑了！
```
<input type="submit" value="Subscribe!" th:attr="value=#{subscribe.submit}"/>
```
...这段markup代码确实很丑，在一个属性中定义对另一个属性的赋值是一个很实用的功能，但是如果你经常需要进行赋值操作，这不是一个优雅的创建模板的方式。

Thymeleaf和你想的一样，所以`th:attr`很少在模板使用。通常情况下，你会使用别的被明确定义的进行赋值的属性，形如`th:*`（而不是像`th:attr`这样的对任何属性就行赋值的属性）。

例如，如果想要对`value`属性赋值，就用`th:value`属性。
```
<input type="submit" value="Subscribe!" th:value="#{subscribe.submit}"/>
```
这样看上去好多了！让我们对`form`标签中的`action`属性进行同样的操作：
```
<form action="subscribe.html" th:action="@{/subscribe}">
```
你还记得之前在`home.html`页面中的`th:href`属性吗？它们是完全相同类型的属性：
```
<li><a href="product/list.html" th:href="@{/product/list}">Product List</a></li>
```
还有很多像这样的属性，每个都对应一个特定的HTML5属性：详情请参考[官方文档](http://www.thymeleaf.org/doc/tutorials/3.0/usingthymeleaf.html#setting-value-to-specific-attributes)
