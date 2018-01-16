# 13.5 自然JS和CSS模板
如上一章所见，内联JavaScript和Css提供了在JavaScript/CSS注释中引入表达式的方法，如下：
```
...
var username = /*[[${session.user.name}]]*/ "Sebastian Lychee";
...
```
...这是合法的JavaScript代码，运行结果如下：
```
...
var username = "John Apricot";
...
```
将内联表达式包在注释中的技巧可以用于全部文本模式语法中：
```
/*[# th:if="${user.admin}"]*/
    alert('Welcome admin');
 /*[/]*/
```
当模板被静态打开时，会展示代码中的警告框 - 因为这是100%合法的JavaScript -，当模板执行行时，如果用户是一个管理员，该弹窗也会出现。因此这段代码等价于：
```
[# th:if="${user.admin}"]
   alert('Welcome admin');
[/]
```
...这实际上是模板解析时初始版本转化成的代码。

注意注释中包含的元素不像内联表达式的输出那样会删除其所在的行（指从左开始至遇到第一个`;`）。只有内联表达式的输出会有这种行为。

所以Thymeleaf 3.0支持复杂的JS脚本和CSS样式表以自然模板的形式开发，且无论是作为原型还是作为运行模板都是合法的。
