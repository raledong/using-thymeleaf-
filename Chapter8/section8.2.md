# 8.2 可参数化的片段变量
为了使模板片段的调用更像是方法调用，使用`th:fragment`定义的片段能够指定一组参数：
```
<div th:fragment="frag (onevar,twovar)">
    <p th:text="${onevar} + ' - ' + ${twovar}">...</p>
</div>
```
用`th:insert`或`th:replace`调用这个片段时：
```
<div th:replace="::frag (${value1},${value2})">...</div>
<div th:replace="::frag (onevar=${value1},twovar=${value2})">...</div>
```
需要注意的是，使用第二种方法调用片段时参数的顺序不重要。
```
<div th:replace="::frag (twovar=${value2},onevar=${value1})">...</div>
```

### 无片段参数声明的片段局部变量###
---------------------------------------
即便片段的声明处没有定义参数：
```
<div th:fragment="frag">
    ...
</div>
```
我们可以使用上面指定的第二种语法调用它（并且只能使用第二种语法）：
```
<div th:replace="::frag (onevar=${value1},twovar=${value2})">
```
这等价于同时使用`th:replace`和`th:with`：
```
<div th:replace="::frag" th:with="onevar=${value1},twovar=${value2}">
```
注意，片段的局部变量（无论是有参数声明还是无参数声明），不会在执行前清空当前的上下文。片段还是能够和原来一样，使用调用该片段的模板中的上下文变量。

### 用`th:assert`实现模板内断言 ###
---------------------------------------
`th:assert`属性可以指定一组用逗号分隔的表达式，这些表达式都会被计算并且应该全部为`true`，如果有任何一个表达式的值为`false`，则会抛出异常。
```
<div th:assert="${onevar},(${twovar} != 43)">...</div>
```
这对于验证片段声明中的参数非常方便：
```
<header th:fragment="contentheader(title)" th:assert="${!#strings.isEmpty(title)}">...</header>
```
