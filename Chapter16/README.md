# 16.模板缓存
Thymeleaf基于一组解析器 - markup解析器和文本解析器 - 按事件顺序解析模板（开标签，文本，闭标签，注释等），以及一系列处理器 -每个处理器对应一个功能- 来修改模板解析事件的顺序，从而和原始模板与数据一起，构建出我们所期待的答案。

它还默认包括了一个存储解析后模板的缓存，即在处理模板文件之前读取的事件序列。在构建web应用时这个功能非常有用，基于下面几个原则：
* 输入/输出是任何应用最慢的部分。相比而言内存处理非常快。
* 拷贝一个存在于内存中的事件序列通常比读取一个模板文件，解析它并创建一个新的事件序列要快得多。
* Web应用通常只有几十个模板
* 模板文件一般为小至中等大小，且在应用运行时不会被更改

这一切产生了一种想法，即可以将web应用中最经常用到的模板缓存起来，从而无需浪费大量的内存。而且它还可以节省很多读写小型且基本不会发生改变的文件的时间。

那么我们如何管理缓存？首先，我们之前已经了解了在模板解析器中开启和关闭它，以及定义它作用于特定的模板上：
```
// 默认为true
templateResolver.setCacheable(false);
templateResolver.getCacheablePatternSpec().addPattern("/users/*");
```
而且我们还可以通过创建 *Cache Manager* 对象修改它的配置，它是默认的`StandardCacheManager`的一个实例：
```
// 默认为200
StandardCacheManager cacheManager = new StandardCacheManager();
cacheManager.setTemplateCacheMaxSize(100);
...
templateEngine.setCacheManager(cacheManager);
```
参考javadoc API中的`org.thymeleaf.cache.StandardCacheManager`了解配置缓存的更多信息。

缓存中的条例可以手动删除：
```
// 彻底清空缓存
templateEngine.clearTemplateCache();

// 清空特定模板的缓存
templateEngine.clearTemplateCacheFor("/users/userList");
```
