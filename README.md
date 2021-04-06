## 拖拽html
1、使用拖拽事件的时候，报错‘Cannot set property ‘ondragstart’ of null’：

原因：JS的引进放在了head标签里面，浏览器遇到`<script>`标签的时候会立即执行脚本，这个时候DOM树还没有构建完成，访问不到节点

解决：

把JS的引进放在`body`标签底部
使用’defer’异步加载js文件，不会影响DOM渲染，js脚本会在DOM渲染完毕后，DOMContentLoaded事件调用前执行
defer和async：
async也是异步加载js文件，区别在于async还是异步执行js文件，先加载完的脚本先执行。(适用于不关心DOM元素的脚本)

## 拖拽事件
拖动目标：
ondragstart：开始拖动
ondrag：正在拖动
ondragend：完成拖动

释放目标：
ondragenter：拖动对象进入容器范围
ondragover：拖动对象在另一容器范围内拖动
ondragleave：拖动对象离开容器范围
ondrop：拖动过程中释放鼠标

## 插入元素
DOM的insertBefore()方法：把一个新元素插到一个现有元素的前面

parentElement.insertBefore(newElement,targetElement)

实现在一个现有元素后面插入新元素：
nextSibling —> 下一个兄弟元素，若没有则为null
把新元素插到目标元素的下一个兄弟元素前面，如果下一个兄弟元素为null将被插到子节点的末尾

parentElement.insertBefore(newElement,targetElement.nextSibling)

## css3动画
1、获取元素位置
getBoundingClientRect()用于获取页面中某元素的左右上下分别相对浏览器视窗的位置

2、node.nodeType
判断节点类型。如果节点是元素节点返回1，如果是属性节点返回2

3、transform:translate3d(0,0,0)
触发加速，使GPU适应CSS过渡，更加流畅

4、target.offsetWidth
通过获取元素的offsetWidth或者offsetHeight触发重绘。

1、判断元素是否在动画中，防止重复动画
在拖动的时候，dragover是会不停触发的，导致过多的加载动画，所以需要判断元素是否已经添加动画，可以设置一个定时器，在事件到了之后将transition和transform清空

`````js
if (target.animated) {
	return
}

target.animated = setTimeout(function() {
	_css(target, 'transition', '');
	_css(target, 'transform', '');
	target.animated = false;
}, ms);
`````

