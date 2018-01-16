# 13.3 文本型prototype-only注释快: 添加代码
`JAVASCRIPT`和`CSS`模板模式（不支持`文本`模式）允许将代码插入特殊的注释语法中`/*[+...+]*/`，Thymeleaf在处理模板时会自动将这段代码取消注释：
```
var x = 23;

/*[+

var msg  = "This is a working application";

+]*/

var f = function() {
    ...
```
执行结果为：
```
var x = 23;

var msg  = "This is a working application";

var f = function() {
...
```
你可以在这些注释中引入表达式，它们在运行时会被计算：
```
var x = 23;

/*[+

var msg  = "Hello, " + [[${session.user.name}]];

+]*/

var f = function() {
...
```
