# 13.4 文本型parser-level注释块: 删除代码
和prototype-only注释块类似，Thymeleaf的三种文本模板模式（`TEXT`,`JAVASCRIPT`,`CSS`）都支持将`/*[- */`和`/* -]*/`之间的代码自动删除：
```
var x = 23;

/*[- */

var msg  = "This is shown only when executed statically!";

/* -]*/

var f = function() {
...
```
或是在文本模式中这样：
```
...
/*[- Note the user is obtained from the session, which must exist -]*/
Welcome [(${session.user.name})]!
...
```
