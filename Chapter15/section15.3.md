# 15.3 转换服务
*转换服务* 允许我们通过双大括号语法(`${{...}}`)对数据执行转换和格式化操作.这实际上是标准方言的一个功能，而不属于Thymeleaf模板引擎。

因此，配置它的方法是在模板引擎中的`StandardDialect`实例中设置我们自定义的`IStandardConversionService`的实现。如下：
```
IStandardConversionService customConversionService = ...

StandardDialect dialect = new StandardDialect();
dialect.setConversionService(customConversionService);

templateEngine.setDialect(dialect);
```
> 注意thymeleaf-spring3和thymeleaf-spring4都包含`SpringStandardDialect`,这个方言已经有了一个预定义的`IStandardConversionService`实现，这个实现将Spring自己的转换服务与Thymeleaf集成。
