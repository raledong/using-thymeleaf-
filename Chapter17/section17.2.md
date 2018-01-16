# 17.2 配置解耦模板
### 开启解耦模板 ###
---------------------------------------
默认情况下所有模板的逻辑解耦并不是开启的。而且配置的模板解析器需要额外指定其解析的模板开启逻辑解耦。

除了`StringTemplateResolver`（它并不支持逻辑解耦），其它所有的`ITemplateResolver`实现都会提供一个名为`useDecoupledLogic`的标识来标记该模板解析器解析的模板可能会有全部或部分的逻辑位于另一个资源内：
```
final ServletContextTemplateResolver templateResolver =
        new ServletContextTemplateResolver(servletContext);
...
templateResolver.setUseDecoupledLogic(true);
```

### 混合使用逻辑解耦和逻辑不解耦 ###
---------------------------------------
开启模板逻辑解耦后，并非一定要使用逻辑解耦。开启后，引擎会查找包含解耦逻辑的资源，如果存在，就解析并将它和原始的模板合并。如果解耦逻辑资源不存在，也不会抛出错误。

而且，在同一个模板中我们能混合使用解耦逻辑和不解耦逻辑，比如在原始模板文件中添加一些Thymeleaf属性，并将余下的属性在另一个解耦逻辑文件中定义。这样的情况最常见于使用新的`th:ref`（在版本3.0中）属性。
