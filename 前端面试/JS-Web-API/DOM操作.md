Q：DOM和HTML的区别和联系
- XML：一种可扩展（可以描述任何结构化身为数据）的标记语言。是一颗树
```xml
<?xml version="1.0" encoding="UTF-8"?>
<note>
  <to>Tove</to>
  <from>Jani</from>
  <heading>Reminder</heading>
  <body>Don't forget me this weekend!</body>
  <other>
    <a></a>
    <b></b>
  </other>
</note>
```
- HTML：一个有既定标签标准的XML格式，标签的名字、层级关系和属性，都被标准化（否则浏览器无法解析，且大多数浏览器都通用）。也是一颗树
```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title>Document</title>
</head>
<body>
    <div>
        <p>this is p</p>
    </div>
</body>
</html>
```
页面请求流程：
1，开发人员开发的HTML代码保存到一个文档（例如以`.html`或`.htm`结尾的文件）中，文件存储在服务器上；
2. 用户通过浏览器访问特定页面时向服务器发送请求，服务器响应请求返回这个文档，文档内容就是HTML格式的代码；
3. 浏览器将这个文档按照标准渲染成一个页面(DOM)，DOM可被浏览器、JS理解（允许JS修改页面内容） 

因此：
- DOM：JS能识别的HTML结构，一个普通的JS对象或者数组，也是一颗树

Q：如何获取DOM节点
```javascript
// 通过id获取
var div = document.getElementById('div1') // 元素

// 通过标签名称获取
var divList = document.getElementsByTagName('div') // 集合
console.log(divList.length)
console.log(divList[0])

// 通过class获取
var containerList = document.getElementsByClassName('container') // 集合

// 通过css选择器
var pList = document.querySelectorAll('p') // 集合
```

Q：property和attribute的区别是什么？
- property：property的获取和修改，是直接改变JS对象
- attribute：attribute对HTML属性的get和set，是直接改变HTML属性，和DOM节点的JS范畴的property没有关系,get和set attribute时，还会触发DOM的查询或者重绘、重排，频繁操作会影响页面性能。
```javascript
var pList = document.querySelectorAll('p')
var p = pList[0]
console.log(p.style.width) // 获取样式
p.style.width = '100px' // 修改样式
console.log(p.className) // 获取class
p.className = 'p1' // 修改class

// 获取nodeName和nodeType
console.log(p.nodeName)
console.log(p.nodeType)
```
```javascript
var pList = document.querySelectorAll('p')
var p = pList[0]
p.getAttribute('data-name')
p.setAttribute('data-name', 'juejin')
p.getAttribute('style')
p.setAttribute('style', 'font-size:30px')
```

Q：DOM操作的基本API有哪些？
- 新增节点
```javascript
var div1 = document.getElementById('div1')

// 添加新节点
var p1 = document.createElement('p')
p1.innerHTML = 'this is p1'
div1.appendChild(p1) // 添加新创建的元素

// 移动已有节点。注意，这里是“移动”，并不是拷贝
var p2 = document.getElementById('p2')
div1.appendChild(p2)
```
- 获取父元素
```javascript
var div1 = document.getElementById('div1')
var parent = div1.parentElement
```
- 获取子元素
```javascript
var div1 = document.getElementById('div1')
var child = div1.childNodes // 集合
```
- 删除节点
```javascript
var div1 = document.getElementById('div1')
var child = div1.childNodes
div1.removeChild(child[0])
```
- 获取前一个节点
- 获取后一个节点