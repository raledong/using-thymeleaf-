# 11.1 标准HTML/XML注释
标准HTML/XML备注`<!-- ... -->`在Thymeleaf模板中均可使用。在备注中的任何内容都不会被Thymeleaf处理，而是直接逐字拷贝到结果中：
```
<!-- User info follows -->
<div th:text="${...}">
  ...
</div>
```
