# 10.属性优先级
当你在一个标签中写了多个`th:*`属性会发生什么？比如：
```
<ul>
  <li th:each="item : ${items}" th:text="${item.description}">Item description here...</li>
</ul>
```
我们希望`th:each`属性在`th:text`属性之前执行，这样我们就能得到想要的结果。但是考虑到HTML/XML标准中没有给标签中属性的写入顺序赋予任何意义，因此需要在属性上创建一个优先级机制，确保他们会按预期运行。

因此，所有的Thymeleaf属性都被定义了一个数值优先级，从而创建了它们在标签中的执行顺序。这个顺序为：

| 顺序            | 功能     |属性   |
| ------------- |:-------------:|:-----|
| 1      | 引入片段  |`th:insert` `th:replace`|
|2       | 迭代片段  |`th:each`|
|3       | 条件评估  |`th:if` `th:unless` `th:switch` `th:case`|
|4       | 局部变量定义 |`th:object` `th:with`|
|5       | 一般属性修改 |`th:attr` `th:attrprepend` `th:attrappend`|
|6       | 特殊属性修改 |`th:value` `th:href` `th:src` ...|
|7       | 文本（标签体修改）|`th:text` `th:utext`|
|8       | 片段规格 |`th:fragment`|
|9       | 片段删除 |`th:remove`|

这个优先级机制意味着在上例中，即使倒置属性的位置，迭代片段还是会生成同样的结果（不过代码的可读性会略微下降）。
```
<ul>
  <li th:text="${item.description}" th:each="item : ${items}">Item description here...</li>
</ul>
```
