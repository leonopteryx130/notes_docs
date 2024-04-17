# documentFragment对象

DocumentFragment 表示一个没有父级文件的最小文档对象。它被当做一个轻量版的 Document 使用。最大的区别是因为DocumentFragment不是真实DOM树的一部分，它的变化不会引起DOM树的重新渲染的操作(reflow) ，且不会导致性能等问题。

不过它有一种特殊的行为，该行为使得它非常有用，即当请求把一个 DocumentFragment 节点插入文档树时，插入的不是 DocumentFragment 自身，而是它的所有子孙节点。这使得 DocumentFragment 成了有用的占位符，暂时存放那些一次插入文档的节点。它还有利于实现文档的剪切、复制和粘贴操作，尤其是与 Range 接口一起使用时更是如此。 可以用 ```Document.createDocumentFragment()``` 方法创建新的空 DocumentFragment 节点。

我们可以把DocumentFragment 节点作为一个中转站，把多次的dom操作汇聚到这一次来进行，把需要添加的dom元素都先添加到documentFragment中去，减少回流次数，就可以实现性能的优化了。

### 基本用法：

假设有这样一个dom
```
<div id="test">
    <li>test1</li>
    <li>test1</li>
    <li>test1</li>
</div>
```
如果在ul上直接多次执行appendChild会造成多次回流，影响性能，所以这里用DocumentFragment更好
```
const ul = document.getElementById('test')
// 创建fragment对象
const fragment = document.createDocumentFragment()
console.log(fragment.nodeValue); //null
console.log(fragment.nodeName); //#document-fragment
console.log(fragment.nodeType); //
//  取出ul中的所有子节点并保存到fragment
let child;
while(child=ul.firstChild) {
  fragment.appendChild(child)
}
//更新fragment中的所有节点（li的内容）
Array.prototype.slice.call(fragment.childNodes).forEach(node => {
  if (node.nodeType===1) {//取得元素节点
    node.textContent = 'test2' //重新赋值为test2
  }
})
// 将fragment插入到ul
ul.appendChild(fragment)
```