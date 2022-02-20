# XMLHttpRequest

Q：手写XMLHttpRequest、不借助任何库
```javascript
var xhr = new XMLHttpRequest()
xhr.onreadystatechange = function (){
    // 这里的函数异步执行，可参考之前JS基础中的异步模块
    if(xhr.readyState == 4) {
        if(xhr.status == 200) {
            alert(xhr.responseText)
        }
    }
}
xhr.open("GET", "/api", false)
xhr.send(null)
```

# 状态码说明

1. xhr.readyState(浏览器判断请求过程中的各个阶段阶段) 
- 0-代理被创建，但尚未调用open()方法。
- 1-open()方法已经被调用。 
- 2-send()方法已经被调用，并且头部和状态已经可获得 
- 3-下载中，responseText属性已经包含部分数据 
- 4-下载操作已经完成

2. xhr.status(HTTP协议规定的不同结果的返回状态说明)【Q：HTTP协议中，response的状态码，常见的有哪些】
- 200-正常
- 3xx
  - 301-永久重定向，如http://xxx.com这个GET请求（最后没有/），就会被301到http://xxx.com/（最后是/）
  - 302-临时重定向。临时的，不是永久的
  - 304-资源找到但是不符合请求条件，不会返回任何主体，如发送GET请求时，head中有If-Modified-Since:xxx（要求返回更新时间是xxx时间之后的资源），如果此时服务器端资源未更新，则会返回304，即不符合要求
- 404-找不到资源
- 5xx-服务器端出错了
  - 500-
  - 502-Bad Gateway
  - 504-Bad Gateway timeout网关超时

# Fetch API(新规范，支持Promise回调，各浏览器不一定兼容)

API可用性：[点击这里查看_caniuse](https://caniuse.com/)

```javascript
fetch('some/api/data.json', {
    method: 'POST', //请求类型 GET、POST、...
    headers: {}, //请求的头信息，形式为Headers对象或ByteString
    body: {}, // 请求发送的数据blob、bufferSource、FormData、URLSearchParams(get或head方法中不能包含body)
    mode: '', // 请求的模式，是否跨域等，如cors、no-cors或same-origin
    credentials: '', // cookie的跨域策略，如omit、same-orgin或者include
    cache: '', // 请求的cache模式：default、no-store、reload、no-cache、force-cache或only-if-cached
}).then(function (response) { ... });
```

Q：如何实现跨域？

1. 浏览器中有`同源策略`，即一个域下的页面中，无法通过Ajax获取到其他域的接口。例如有一个接口http://m.juejin.com/course/ajaxcourserecom?cid=459，你自己的一个页面http://www.yourname.com/page1.html中的 Ajax 无法获取这个接口。这正是命中了“同源策略”。如果浏览器哪些地方忽略了同源策略，那就是浏览器的安全漏洞，需要紧急修复。
2. url中不算跨域的地方：
- 协议
- 域名
- 端口
3. HTML中能逃避同源策略的标签
- script(<script src='xxx'></script>)-网络第三方CDN库、比如elementUI、Vue等
- img(<img src='xxx' />)-打点统计
- link(<link href='xxx'>)-网络第三方CDN样式、比如bootstrap
4. JSONP
```html
<script>
  window.callback = function (data){
      // 这是我们跨域得到的信息
      console.log(data)
  }
  callback({...}) //
</script>
```
5. 服务器端设置http header
```javascript
response.setHeader('Access-Control-Allow-Origin', 'http://m.juejin.com/') //第二个参数填写允许跨域的域名称，不建议直接写“*”
response.setHeader('Access-Control-Allow-Headers','X-Requested-With')
response.setHeader('Access-Control-Allow-Methods','PUT,POST,GET,DELETE,OPTIONS')

// 接受跨域的cookie
response.setHeader('Access-Control-Allow-Credentials', 'true')
```