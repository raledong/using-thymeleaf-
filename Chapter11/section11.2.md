# 11.2 parser-level 注释块
parser-level注释块是指Thymeleaf解析时直接从模板中删除的代码。如下：
```
<!--/* This code will be removed at Thymeleaf parsing time! */-->
```
Thymeleaf会删除`<!--/*`和`*/-->`之间的全部内容，所以这些注释块在模板被静态打开时也能够用于展示，并且知道Thymeleaf在处理它时将其删除：
```
<!--/*-->
  <div>
     you can see me only before Thymeleaf processes me!
  </div>
<!--*/-->
```
当模板表格中有很多`<tr>`标签时，这会非常方便，比如：
```
<table>
   <tr th:each="x : ${xs}">
     ...
   </tr>
   <!--/*-->
   <tr>
     ...
   </tr>
   <tr>
     ...
   </tr>
   <!--*/-->
</table>
```
