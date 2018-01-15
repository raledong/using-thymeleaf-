# 11.4 合成th:block标签
Thymeleaf的标准方言中唯一的元素处理器（不是属性）是`th:block`。

`th:block`是一个纯属性容器，它允许模板开发者指定他们所需的任何属性。Thymeleaf会执行这些属性，然后使这个块本身，而不是其内容，消失。

所以它在例如创建一个元素需要多个`<tr>`的迭代表格的时候会很有用。
```
<table>
  <th:block th:each="user : ${users}">
    <tr>
        <td th:text="${user.login}">...</td>
        <td th:text="${user.name}">...</td>
    </tr>
    <tr>
        <td colspan="2" th:text="${user.address}">...</td>
    </tr>
  </th:block>
</table>
```
当和prototype-only注释块一起使用的时候格外有用：
```
<table>
    <!--/*/ <th:block th:each="user : ${users}"> /*/-->
    <tr>
        <td th:text="${user.login}">...</td>
        <td th:text="${user.name}">...</td>
    </tr>
    <tr>
        <td colspan="2" th:text="${user.address}">...</td>
    </tr>
    <!--/*/ </th:block> /*/-->
</table>
```
注意这个方法是如何使模板成为合法的HTML文件（无需在`<table>`标签中添加非法的`<div>`标签），而且它作为静态原型在浏览器中能正常打开。
