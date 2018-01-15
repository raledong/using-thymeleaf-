# 12.4 内联CSS
Thymeleaf还允许在CSS的`<style>`标签中使用内联，比如：
```
<style th:inline="css">
  ...
</style>
```
比如，假设我们有两个设置为不同`String`值的变量：
```
classname = 'main elems'
align = 'center
```
我们可以这么使用它：
```
<style th:inline="css">
    .[[${classname}]] {
      text-align: [[${align}]];
    }
</style>
```
结果为：
```
<style th:inline="css">
    .main\ elems {
      text-align: center;
    }
</style>
```
注意内联CSS也有智能功能，和JavaScript一样。特别的，通过转义表达式如`[[${classname}]]`输出的表达式会被转义为CSS标识。所以在上面的代码段中我们的`classname = 'main elems'`变成了`main\ elems`。

### 高级功能：CSS自然模板等 ###
---------------------------------------
和之前说明的JavaScript自然模板的方法相同，内联CSS也支持`<style>`标签同时静态和动态执行，比如，作为 *CSS自然模板*，通过在注释中包装内联表达式：
```
<style th:inline="css">
    .main\ elems {
      text-align: /*[[${align}]]*/ left;
    }
</style>
```
