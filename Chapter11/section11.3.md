# 11.3 prototype-only 注释块
Thymeleaf允许定义特殊的注释块，当模板被静态打开时（比如，打开为原型），这段代码会被标记为注释，但是当Thymeleaf运行模板时将其当做正常的markup代码。

```
<span>hello!</span>
<!--/*/
  <div th:text="${...}">
    ...
  </div>
/*/-->
<span>goodbye!</span>
```
Thymeleaf的分析系统会删除`<!--/*/`和`/*/-->`标记而不改变其内容，使其成为非注释内容。所以运行这个模板时，Thymeleaf实际上看到的内容如下：
```
<span>hello!</span>

  <div th:text="${...}">
    ...
  </div>

<span>goodbye!</span>
```
与parser-level注释块一样，这个功能和方言无关。
