# 5.3 一次设置多个属性的值
`th:alt-title`和`th:lang-xmllang`是两个比较特殊的属性，它们是用来将两个属性同时设置成同一个值的。特别的，
* `th:alt-title`会同时设置`alt`和`title`
* `th:lang-xmllang`会同时设置`lang`和`xml:lang`

对于我们的GTVG主页，我们可以将下面的内容：
```
<img src="../../images/gtvglogo.png"
     th:attr="src=@{/images/gtvglogo.png},title=#{logo},alt=#{logo}" />
```

或是下面的等价内容：
```
<img src="../../images/gtvglogo.png"
     th:src="@{/images/gtvglogo.png}" th:title="#{logo}" th:alt="#{logo}" />
```

替换成如下表示：
```
<img src="../../images/gtvglogo.png"
     th:src="@{/images/gtvglogo.png}" th:alt-title="#{logo}" />
```
