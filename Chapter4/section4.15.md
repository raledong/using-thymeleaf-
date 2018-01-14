# 4.15 数据转换/格式化
Thymeleaf为变量表达式(`${...}`)和选择表达式（`*{...}`）定义了一个双括号的语法，它允许我们通过配置的转化服务对数据进行转化。

形式基本如下：
```
<td th:text="${{user.lastAccessDate}}">...</td>
```
注意这里的双括号`${{...}}`，这里实际上将`user.lastAccessDate`的值在渲染至模板上之前传递给 *转化服务* 并让其执行 **格式化服务**（将其转化为`String`）。

假设`user.lastAccessDate`是`java.util.Calendar`类，如果现有一个注册了的转化服务（`即IStandardConversionService的实现类`）并且这个服务中包含`Calendar -> String`的转化功能，那么这个服务就会启动。

`IStandardConversionService`接口的默认实现（即`StandardConversionService`类）仅仅是在所有对象上调用`.toString()`方法将对象转化为`String`。想要了解更多关于如何注册一个自定义的转化服务，可以去查看[关于配置的更多内容](http://www.thymeleaf.org/doc/tutorials/3.0/usingthymeleaf.html#more-on-configuration)章节。
