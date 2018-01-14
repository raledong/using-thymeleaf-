# 4.9 算数操作符
`+`, `-`, `*`, `/`和`%`这种算数操作符也是支持的：
```
<div th:with="isEven=(${prodStat.count} % 2 == 0)">
```
注意这些操作符也可以在OGNL变量表达式中使用（如果在OGNL表达式中使用，这个表达式就会由OGNL执行，而不是Thymeleaf标准表达式引擎）：
```
<div th:with="isEven=${prodStat.count % 2 == 0}">
```
注意一些操作存在文本别名：`div` (`/`), `mod` (`%`)
