# 5.7 支持对HTML5友好的属性和元素名称
也可以用一种完全不同的语法使处理器处理模板，这个语法对HTML5更友好。

```
<table>
    <tr data-th-each="user : ${users}">
        <td data-th-text="${user.login}">...</td>
        <td data-th-text="${user.name}">...</td>
    </tr>
</table>
```
`data-{prefix}-{name}`语法是HTML5中自定义属性的标准方法，它无需开发者使用任何命名空间名称如`th:*`。Thymeleaf使所有的方言都自动支持这种语法（不止是标准方言）。

还有一种语法可以用来自定义标签：`{prefix}-{name}`，它遵循了W3C自定义元素的标准（它是W3C Web Components规范的一部分）。它可以用于如`th:block`元素（或`th-block`），稍后会继续对其进行说明。

**注意**: 这个语法是对命名空间法`th:*`的一个补充而不是替换。我们并不准备废弃命名空间的语法。
