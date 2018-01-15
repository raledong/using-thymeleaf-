# 12.3 内联脚本(JavaScript inlining)
内联脚本为`HTML`模板模式下的的JavaScript的`<script>`块提供更好的集成。

和 *内联文本* 一样，这实际上等价于在`JAVASCRIPT`模板模式中对脚本内容进行处理，因此文本模板模式（下一章介绍）的所有功能都很方便使用。但是，这一章我们会重点关注如何使用它将Thymeleaf表达式的输出内容添加到JavaScript块中。

这个模式需要使用`th:inline="javascript"`显式开启：
```
<script th:inline="javascript">
    ...
    var username = [[${session.user.name}]];
    ...
</script>
```
结果为：
```
<script th:inline="javascript">
    ...
    var username = "Sebastian \"Fruity\" Applejuice";
    ...
</script>
```
在上面的代码中需要注意两件事：

*首先*， 内联脚本不仅会输出所需文本，还会使用引号将其括起来并且对其内容进行JS转义，因此表达式的结果将会被输出为 **格式正确的JS文字**。

*其次*，上面的一切会发生是因为我们使用转义的方法输出`${session.user.name}`表达式，比如使用双中括号的表达式：`[[${session.user.name}]]`。我们可以使用非转义方法取而代之：
```
<script th:inline="javascript">
    ...
    var username = [(${session.user.name})];
    ...
</script>
```
结果如下：
```
<script th:inline="javascript">
    ...
    var username = Sebastian "Fruity" Applejuice;
    ...
</script>
```
...这是格式错误的JavaScript代码。但是不转义的输出内容也可能是我们所需要的，当我们正在通过连接内联表达式构建脚本时。所以有这个工具还是很好的。

### JavaScript自然模板 ###
---------------------------------------
上文提到的内联脚本的功能远不止调用JavaScript转义和将表达式结果输出为合法的文字。

比如，我们可以将（转义的）内联表达式包在JavaScript注释中，如下：
```
<script th:inline="javascript">
    ...
    var username = /*[[${session.user.name}]]*/ "Gertrud Kiwifruit";
    ...
</script>
```
Thymeleaf会忽视一切注释之后和分号之前的内容（这里是`'Gertrud Kiwifruit'`）。所以执行后的结果就好像我们没有使用注释括起来一样：
```
<script th:inline="javascript">
    ...
    var username = "Sebastian \"Fruity\" Applejuice";
    ...
</script>
```
但是再仔细看一下原始的模板代码：
```
<script th:inline="javascript">
    ...
    var username = /*[[${session.user.name}]]*/ "Gertrud Kiwifruit";
    ...
</script>
```
注意这是 *合法的JavaScript代码*。当你静态的打开模板文件时（不通过服务器执行）它会完美运行。

所以在这里我们实现了JavaScript自然模板！

### 高级内联计算和JavaScript序列化 ###
---------------------------------------
关于内联文本需要注意一个重要的事情，表达式的计算是智能且不局限于字符串的。Thymeleaf会将下列类型的对象用JavaScript语法正确输出：
* Strings
* Numbers
* Booleans
* Arrays
* Collections
* Maps
* Beans (objects with getter and setter methods)
例如，假设我们有下列代码：
```
<script th:inline="javascript">
    ...
    var user = /*[[${session.user}]]*/ null;
    ...
</script>
```
`${session.user}`计算结果为一个`User`对象，那么Thymeleaf会正确的将其转化为Javascript语法：
```
<script th:inline="javascript">
    ...
    var user = {"age":null,"firstName":"John","lastName":"Apricot",
                "name":"John Apricot","nationality":"Antarctica"};
    ...
</script>
```
JavaScript的序列化是通过实现`org.thymeleaf.standard.serializer.IStandardJavaScriptSerializer`接口完成，可以在模板引擎使用的`StandardDialect`实例中配置。

JS序列化的默认实现是在`classpath`下寻找`Jackson库`，如果该库存在，则调用其方法实现。如果不存在，则会调用内置的序列化方法。这个方法覆盖了绝大多数场景的需求，并且提供相似的结果（但是灵活性较差）。
