# 6.1 迭代基础
为了在`/WEB-INF/templates/product/list.html`页面中展示商品，我们需要使用表格。每一个产品会在一行中展示（一个`<tr>`元素）。而目前我们需要创建一个模板行，这一行会明确说明我们将如何展示每个商品的内容，然后指引Thymeleaf针对每个商品重复生成这一行。

标准语法提供了一个属性让我们实现这个功能：`th:each`:

### 使用th:each ###
---------------------------------------
在产品列表界面上，我们需要一个控制器方法从服务层获得产品列表并将其添加到模板的上下文中：
```
public void process(
        final HttpServletRequest request, final HttpServletResponse response,
        final ServletContext servletContext, final ITemplateEngine templateEngine)
        throws Exception {

    ProductService productService = new ProductService();
    List<Product> allProducts = productService.findAll();

    WebContext ctx = new WebContext(request, response, servletContext, request.getLocale());
    ctx.setVariable("prods", allProducts);

    templateEngine.process("product/list", ctx, response.getWriter());

}
```
然后我们可以在模板中使用`th:each`遍历产品列表：
```
<!DOCTYPE html>

<html xmlns:th="http://www.thymeleaf.org">

  <head>
    <title>Good Thymes Virtual Grocery</title>
    <meta http-equiv="Content-Type" content="text/html; charset=UTF-8" />
    <link rel="stylesheet" type="text/css" media="all"
          href="../../../css/gtvg.css" th:href="@{/css/gtvg.css}" />
  </head>

  <body>

    <h1>Product list</h1>

    <table>
      <tr>
        <th>NAME</th>
        <th>PRICE</th>
        <th>IN STOCK</th>
      </tr>
      <tr th:each="prod : ${prods}">
        <td th:text="${prod.name}">Onions</td>
        <td th:text="${prod.price}">2.41</td>
        <td th:text="${prod.inStock}? #{true} : #{false}">yes</td>
      </tr>
    </table>

    <p>
      <a href="../home.html" th:href="@{/}">Return to home</a>
    </p>

  </body>

</html>
```

属性值`prod : ${prods}`是指“`${prods}`值中的每一个元素，重复这个模板片段，在这个片段中使用名为`prod`的当前元素”。让我们给我们看到的每个东西命名：
* 将`${prods}`称为 *迭代表达式* 或 *可迭代变量*。
* 将`prod`称为 *单个迭代变量*。

注意迭代变量`prod`的作用域为`<tr>`元素之内，这意味着它在其内部的标签`<td>`是可用的。

### 可迭代的值 ###
---------------------------------------
`java.util.List`类不是Thymeleaf中唯一能迭代的类型。基本上所有的对象都可以被`th:each`属性遍历：
* 任何实现了`java.util.Iterable`的对象
* 任何实现了`java.util.Enumeration`的对象
* 任何实现了`java.util.Iterator`的对象，它的值是iterator的返回值，而无需将所有的值缓存在内存中
* 任何实现了`java.util.Map`的对象。当遍历map时，迭代变量是`java.util.Map.Entry`类
* 任何数组
* 任何其它对象将被视为只包含对象自身的单值列表
