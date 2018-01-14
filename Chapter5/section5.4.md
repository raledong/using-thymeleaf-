# 5.4 首尾添加
Thymeleaf还提供了`th:attrappend`和`th:attrprepend`属性，用来将表达式的结果作为前缀或后缀添加到已有的属性值上。

例如，你也许希望将一个想要存储到上下文变量的CSS类名添加到（是添加而不是设置）到某个按钮上，当用户进行某些操作时，就将这个内容添加到class上：
```
<input type="button" value="Do it!" class="btn" th:attrappend="class=${' ' + cssStyle}" />
```

当你处理这个模板时，如果`cssStyle`变量的值为`"warning"`，则你会得到如下结果：

```
<input type="button" value="Do it!" class="btn warning" />
```

在标准方言中海油两个特定的连接属性内容的属性：`th:classappend`和`th:styleappend`属性，他们分别用来添加一个CSS类或是一段 *style* 代码至一个元素，而不是覆盖已有的内容：
```
<tr th:each="prod : ${prods}" class="row" th:classappend="${prodStat.odd}? 'odd'">
```
(现在无需了解`th:each`属性，它是一个 *迭代* 属性，稍后我们会讨论。)
