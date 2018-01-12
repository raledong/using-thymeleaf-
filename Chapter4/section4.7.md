# 4.7 文本连接
无论是常量或是变量的计算结果或是消息表达式的计算结果，文本都能直接使用`+`操作符进行连接。
```
<span th:text="'The name of the user is ' + ${user.name}">
```
