BOM（浏览器对象模型）：包含浏览器本身的一些信息的设置和获取，eg：浏览器高度、宽度，浏览器跳转地址
- navigator
- screen
- location
- history

Q：判断是否是Chrome浏览器（根据浏览器特性【UA】识别客户端）
```javascript
var ua = (window).navigator.userAgent
var isChrome = ua.indexOf('Chrome')
console.log(isChrome)
```

Q：获取屏幕的宽度和高度
```javascript
console.log((window).screen.width)
console.log((window).screen.height)
```

Q：获取网址、协议、path、参数、hash等
```javascript
// 例如当前网址：https://juejin.cn/timeline/frontend?a=10&b=10#some
console.log((window).location.href) // https://juejin.cn/timeline/frontend?a=10&b=10#some
console.log((window).location.protocol) // https:
console.log((window).location.pathname) // /timeline/fronted
console.log((window).location.search) // ?a=10&b=10
console.log((window).location.hash) // #some
```

Q；如何调用浏览器前进、刷新、后退功能
```javascript
    (window).history.forward() // 前进，
    (window).history.go() // 刷新当前页,等价于history.go(0)
    (window).history.back() // 后退，
```

- history.forward：在会话历史中向前移动一页
- history.go：从会话历史记录中加载特定页面，可以使用它在历史记录中前后移动
- history.back：在会话历史记录中向后移动一页。如果没有上一页，则此方法调用不执行任何操作。