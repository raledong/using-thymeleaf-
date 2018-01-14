# 5.5 拥有固定值的布尔属性
HTML中存在布尔属性这个概念，它是指那些没有值的属性。当这个属性出现时，则意味着其值为`true`。在XHTML中，这些属性只有一个值就是其本身。

例如`checked`属性：
```
<input type="checkbox" name="option2" checked /> <!-- HTML -->
<input type="checkbox" name="option1" checked="checked" /> <!-- XHTML -->
```
标准方言包括一些属性，这些属性在计算完条件表达式后将布尔属性设置为对应的值。如果条件表达式为`true`，则设置为其固定的值，否则不会设置任何内容。

```
<input type="checkbox" name="active" th:checked="${user.active}" />
```
标准方言中定义的能够设置有固定值的布尔属性值可以查看[官方文档](http://www.thymeleaf.org/doc/tutorials/3.0/usingthymeleaf.html#fixed-value-boolean-attributes)
