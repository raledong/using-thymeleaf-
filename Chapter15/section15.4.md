# 15.4 日志
Thymeleaf很重视日志，并且试图通过日志接口提供尽可能多的有用信息。

使用的日志库为`slf4j`，它实际上充当了一座桥梁，用来连接任何我们希望在应用中使用的日志实现（比如，`log4j`）。

Thymeleaf类能够记录`TRACE`，`DEBUG`和`INFO`级别的信息，这取决于我们希望获得哪种级别的细节。而且除了一般日志，它还用了和TemplateEngine类相关联的三个特殊的日志记录器。我们可以根据不同的目的分别进行配置：
* `org.thymeleaf.TemplateEngine.CONFIG` 会输出初始化期间配置库的详细信息。
* `org.thymeleaf.TemplateEngine.TIMER` 会输出处理每个模板所用的时间（useful for benchmarking!）
* `org.thymeleaf.TemplateEngine.cache` 是输出和缓存相关的特定信息的一组日志记录器的前缀。尽管用户配置的缓存记录器的名称可以改变，它们的默认值为：
  * `org.thymeleaf.TemplateEngine.cache.TEMPLATE_CACHE`
  * `org.thymeleaf.TemplateEngine.cache.EXPRESSION_CACHE`

使用`log4j`作为日志系统的配置范例如下：
```
log4j.logger.org.thymeleaf=DEBUG
log4j.logger.org.thymeleaf.TemplateEngine.CONFIG=TRACE
log4j.logger.org.thymeleaf.TemplateEngine.TIMER=TRACE
log4j.logger.org.thymeleaf.TemplateEngine.cache.TEMPLATE_CACHE=TRACE
```
