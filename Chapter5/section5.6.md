# 5.6 设置任何属性的值（默认属性处理器）
Thymeleaf提供给了一个默认属性处理器，它可以设置任何属性的值，哪怕标准方言中没有为特别的`th:*`属性定义处理器。

所以如下代码：
```
<span th:whatever="${user.name}">...</span>
```
等价于：
```
<span whatever="John Apricot">...</span>
```
