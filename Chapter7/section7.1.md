# 7.1 "if"和"unless"
有时候你需要模板中的一段代码只有在满足一定条件后才会渲染。

例如，假设我们想要在产品列表中用一列来展示该产品的评论数量，如果有评论，还要展示一个前往该商品评论详情界面的链接。

为了实现这个功能呢，我们可以使用`th:if`属性：
```
<table>
  <tr>
    <th>NAME</th>
    <th>PRICE</th>
    <th>IN STOCK</th>
    <th>COMMENTS</th>
  </tr>
  <tr th:each="prod : ${prods}" th:class="${prodStat.odd}? 'odd'">
    <td th:text="${prod.name}">Onions</td>
    <td th:text="${prod.price}">2.41</td>
    <td th:text="${prod.inStock}? #{true} : #{false}">yes</td>
    <td>
      <span th:text="${#lists.size(prod.comments)}">2</span> comment/s
      <a href="comments.html"
         th:href="@{/product/comments(prodId=${prod.id})}"
         th:if="${not #lists.isEmpty(prod.comments)}">view</a>
    </td>
  </tr>
</table>
```
这里信息量很大，所以让我们重点看一行代码：
```
<a href="comments.html"
   th:href="@{/product/comments(prodId=${prod.id})}"
   th:if="${not #lists.isEmpty(prod.comments)}">view</a>
```
当这个商品有评论时，它会创建一个指向评论页面的链接（URL为`/product/comments`），参数为`prodId`，用于设置商品的id。

让我们看一下产生的结果：
```
<table>
  <tr>
    <th>NAME</th>
    <th>PRICE</th>
    <th>IN STOCK</th>
    <th>COMMENTS</th>
  </tr>
  <tr>
    <td>Fresh Sweet Basil</td>
    <td>4.99</td>
    <td>yes</td>
    <td>
      <span>0</span> comment/s
    </td>
  </tr>
  <tr class="odd">
    <td>Italian Tomato</td>
    <td>1.25</td>
    <td>no</td>
    <td>
      <span>2</span> comment/s
      <a href="/gtvg/product/comments?prodId=2">view</a>
    </td>
  </tr>
  <tr>
    <td>Yellow Bell Pepper</td>
    <td>2.50</td>
    <td>yes</td>
    <td>
      <span>0</span> comment/s
    </td>
  </tr>
  <tr class="odd">
    <td>Old Cheddar</td>
    <td>18.75</td>
    <td>yes</td>
    <td>
      <span>1</span> comment/s
      <a href="/gtvg/product/comments?prodId=4">view</a>
    </td>
  </tr>
</table>
```
太棒了！这正是我们想要的页面。

要注意`th:if`属性不只会计算布尔情况。它还能计算其它类型的表达式，只要这些表达式满足以下情况，则为`true`，否则为`false`:
* 如果值非null：
  * 如果是布尔值且为`true`
  * 如果是一个数字且不为0
  * 如果是一个字符且不为0
  * 如果是一个字符串且值不为“false”，”off”和“no”
  * 当值不是一个布尔值或数字或字符或字符串
* （如果值为null，则`th:if`的结果为false）

除此以外，`th:if`还有一个逆属性`th:unless`，我们可以用在之前的例子里，免于在OGNL表达式中使用`not`：
```
<a href="comments.html"
   th:href="@{/comments(prodId=${prod.id})}"
   th:unless="${#lists.isEmpty(prod.comments)}">view</a>
```
