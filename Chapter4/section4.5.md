# 4.5 片段
片段表达式是一种简单的方法来表示markup片段，从而将这些片段移动到各个模板中。这允许我们复制片段，将片段作为参数传递给别的模板等。

片段最常用于片段插入，可以使用`th:insert`或`th:replace`（在后面的章节中会讲解更多）：
```
<div th:insert="~{commons :: main}">...</div>
```
它们也和别的变量一样，可以在任何地方使用：
```
<div th:with="frag=~{footer :: #main/text()}">
  <p th:insert="${frag}">
</div>`
```
在后面的教程中，会有一整个章节介绍**模板布局Template Layout**，包括深入讲解片段表达式。
