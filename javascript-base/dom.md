# DOM（Document Object Model）

> DOM - 文档对象模型。用来呈现以及与任意 HTML 或 XML文档交互的API。DOM 是载入到浏览器中的文档模型，以节点树的形式来表现文档，每个节点代表文档的构成部分（例如:页面元素、字符串或注释等等）。

> DOM 是万维网上使用最为广泛的API之一，因为它允许运行在浏览器中的代码访问文件中的节点并与之交互。节点可以被创建，移动或修改。事件监听器可以被添加到节点上并在给定事件发生时触发。

> DOM 并不是天生就被规范好了的，它是浏览器开始实现JavaScript时才出现的。这个传统的 DOM 有时会被称为 DOM 0。

### DOM树结构图：
![DOM树结构图](./imgs/dom.gif 'DOM树结构图')

### 节点类型

节点类型|属性nodeType值
:---:|:---:
元素节点|1
属性节点|2
文本节点|3
注释节点（comment）|8
document|9
documentFragment|11

### DOM方法
1. 获取节点
```
document.getElementById(id)  // 通过id号来获取元素，返回一个元素对象。ID不能重名，如果ID重复，只能取到第一个。
document.getElementsByName(name)  // 通过name属性获取id号，返回NodeList。
document.getElementsByClassName(className)  // 通过class来获取元素，返回NodeList（ie8以上才有）
document.getElementsByTagName(tagName)   // 通过标签名获取元素，返回NodeList
document.querySelector(str)  // 返回文档中匹配指定 CSS 选择器的第一个元素
document.querySelectorAll(str)  // 返回的对象是NodeList
element.getElementsByClassName(className)  // 获取某个DOM元素下，class为className的元素，返回NodeList
```
2. 获取/设置元素的属性值
```
element.getAttribute(attributeName)  // 返回对应属性的属性值
element.setAttribute(attributeName, attributeValue)  // 传入属性名及设置的值
```
3. 创建节点
```
document.createElement("h1")  // 创建一个html元素，这里以创建h1元素为例
document.createTextNode(str)  // 创建一个文本节点
document.createAttribute("class")  // 创建一个属性节点，这里以创建class属性为例
document.createDocumentFragment()  // 创建文档片断
```
4. 添加节点
```
element.appendChild(Node)  // 往element内部最后面添加一个节点，参数是节点类型，返回添加的节点
elelment.insertBefore(newNode, Node)  // 在element中Node前面插入newNode，返回插入的节点
```
5. 删除节点
```
parent.removeChild(child)  // 删除父节点下的子节点，删除成功返回该被删除的节点，否则返回null
child.parentNode.removeChild(child)  // 我们常用这种方式删除一个节点，这样就不用去引用一个父节点
```
5. 克隆节点
```
element.cloneNode(deep)  // 返回拷贝的节点。deep可选，默认false。deep为true，它还将递归复制当前节点的所有子孙节点。否则，它只复制当前节点。
```

### DOM属性

属性|说明
:---:|:---:
nodeName|节点名称。元素节点返回标签名，属性节点返回属性名，文本节点返回#text，文档节点返回#document。只读。注意大小写。
nodeType|节点的类型。返回值：元素节点1，属性节点2，文本节点3。见上图。
nodeValue|节点的值，返回一个字符串，指示这个节点的值。元素节点返回 null，属性节点返回属性值，文本节点返回文本。nodeValue 可读可写，只是对元素节点不能写。一般只用于设置文本节点的值。
chlidren|返回当前元素所有子元素节点对象，只返回HTML节点
childNodes|返回子节点数组。文本和属性节点的 childNodes 永远是 null。可以用 hasChildNodes 来判断是否有子节点。只读。
firstChild|返回第一个子节点。文本和属性节点没有子节点，会返回一个空数组。对于元素节点，若是没有子节点会返回 null。有一个等价式：firstChild = childNodes[0]。
lastChild|返回最后一个子节点。返回值同 firstChild。有一个等价式：lastChide = childNodes[childNodes.length - 1]。
nextSibling|返回节点的下一个兄弟节点。如果没有下一个兄弟节点的话，返回 null。只读。
previousSibling|返回节点的上一个兄弟节点。同上。
parentNode|返回节点的父节点。document.parentNode 返回 null，其他的情况下都将返回一个元素节点，因为只有元素节点拥有子节点，除了 document 外任何节点都拥有父节点。只读。
innerHTML|返回元素的所有文本，包括html代码
attributes|获取当前节点的所有属性节点。 返回数组格式
innerText|返回当前元素的自身及子代所有文本值，只是文本内容，不包括html代码
style|获取/设置元素的样式

### 元素宽/高的获取
```
element.style.width/height  // 元素的宽/高（包括元素宽/高，不包括内边距、边框和外边距）
element.currentStyle.width/height  // 元素渲染后的宽/高，仅支持IE
window.getComputedStyle(element).width/height  // 元素渲染后的宽/高，不支持低版本浏览器
element.offsetWidth/offsetHeight  // 盒模型宽/高
element.clientWidth/clientHeight  // 元素宽/高 + 内边距，不包括边框
element.scrollWidth/scrollHeigh  // 元素的宽/高（包括元素宽/高、内边距和溢出尺寸，不包括边框和外边距），无溢出的情况，与clientWidth/clientHeight相同

element.offsetTop/offsetLeft  // 返回元素的上外缘距离最近采用定位父元素内壁的距离，如果父元素中没有采用定位的，则是获取上外边缘距离文档内壁的距离。所谓的定位就是position属性值为relative、absolute或者fixed。返回值是一个整数，单位是像素。此属性是只读的。

element.scrollLeft/scrollTop  // 此属性可以获取或者设置对象的最顶部到对象在当前窗口显示的范围内的顶边的距离，也就是元素滚动条被向下拉动的距离。返回值是一个整数，单位是像素。此属性是可读写的。
// document.body.scrollTop || document.documentElement.scrollTop

**当鼠标事件发生时（不管是onclick，还是omousemove，onmouseover等）**
clientX  // 鼠标相对于浏览器（这里说的是浏览器的有效区域）左上角x轴的坐标。不随滚动条滚动而改变。
clientY  // 鼠标相对于浏览器（这里说的是浏览器的有效区域）左上角y轴的坐标。不随滚动条滚动而改变。

pageX  // 鼠标相对于浏览器（这里说的是浏览器的有效区域）左上角x轴的坐标。 随滚动条滚动而改变。
pageY  // 鼠标相对于浏览器（这里说的是浏览器的有效区域）左上角y轴的坐标。随滚动条滚动而改变。

screenX  // 鼠标相对于显示器屏幕左上角x轴的坐标。
screenY  // 鼠标相对于显示器屏幕左上角y轴的坐标。

offsetX  // 鼠标相对于事件源左上角X轴的坐标。
offsetY  // 鼠标相对于事件源左上角Y轴的坐标。
```
![event-target](./imgs/event-target.jpg 'event-target')
