# 4.13 不执行操作标识(The No-Operation token)
*不执行操作标识* 使用下划线（`_`）作为标识。

这个标识背后的想法是让表达式的结果不执行任何操作，比如，执行正常操作，就当被处理的属性（如`th:text`）不存在一样。

这允许开发者使用原型中的内容作为默认值，比如，原来需要写成如下形式：
```
<span th:text="${user.name} ?: 'no user authenticated'">...</span>
```
...其实我们可以直接在原型中使用 *‘no user authenticated’*, 使得代码从设计的角度上看起来更简介：
```
<span th:text="${user.name} ?: _">no user authenticated</span>
```
