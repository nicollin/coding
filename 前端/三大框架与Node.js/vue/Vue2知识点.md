# 1) Vue的优点？Vue的缺点？

- 优点：
    - 渐进式
    - 组件化
    - 轻量级
    - 虚拟dom
    - 响应式
    - 单页面路由
    - 数据与视图分开
- 缺点：
    - 单页面不利于seo
    - 不支持IE8以下
    - 首屏加载时间长，(解决方案：[nextJS-React](https://www.nextjs.cn/) / [nuxtJS-Vue](https://www.nuxtjs.cn/)
      `服务器SSR渲染`)

# 2) 为什么说Vue是一个渐进式框架？

- 渐进式：想用什么就用什么，component组件、vuex全局状态都是可选项

# 3） VueReact的异同点

相同点：

1. 都使用里的虚拟DOM
2. 组件化开发
3. 都是单向数据流（数据从父组件流向子组件，不建议子组件直接修改父组件传入的数据，复杂的多组件通信应考虑使用Vuex全局状态代替）
4. 都支持服务端SSR渲染（解决方案：[nextJS-React](https://www.nextjs.cn/) / [nuxtJS-Vue](https://www.nuxtjs. cn/) ）

不同点：

1. React的JSX、Vue的template
2. 数据变化
    1. React手动（setState）
    2. Vue自动（初始化已处理为响应式：`Object.defineProperty)`，注意：未声明或者占位的普通变量`不是`响应式）
3. React数据单向绑定；Vue数据双向绑定
4. React使用Redux、Vue使用Vuex管理全局状态

# 4) MVVM是什么？和MVC有什么区别？

- MVC
    - Model（模型）：负责从数据库中取数据
    - View（视图）：负责展示数据
    - Controller（控制器）：用户可视化交互的地方，例如按钮点击事件等
    - 思想：Controller将Model的数据展示在View上
- MVVM
    - VM（View-Model）双向数据绑定：
        - 将【模型】转化为【视图】，即将后端传递的数据转化为用户看到的页面，实现方式：数据绑定
        - 将【视图】转化为【模型】，即将用户所看到的页面转化为后端的数据，实现方式：DOM事监听
    - 思想：实现了View和Model的自动同步，即当Model的属性改变时，无需再手动操作DOM元素 =>
      改变View的显示，而是单Model改变属性时，对应的View会自动同步变化（`Vue数据驱动`）
- 区别：整体看来，MVVM比MVC精简很多，不仅简化了业务与界面的依赖，还解决了数据频繁更新的问题，不需要手动操作DOM，因为在MVVM中，View不知道Model的存在，Model
  和ViewModel也观察不到View。这种低耦合模式提高了代码的可复用性

Q：Vue是不是MVVM框架

Vue是MVVM框架，但不是严格符合MVVM，因为MVVM规定Model和View不能直接通信，而Vue可以通过`ref`做到

# 5) Vue和jQuery的区别在哪？为什么放弃JQuery用Vue？

- jQuery是直接操作DOM，Vue不能直接操作DOM，Vue的数据与视图是分开的，Vue只需要操作数据即可
- 在操作DOM频繁的场景里，jQuery的操作DOM行为是频繁的，而Vue利用虚拟DOM，大大提高更新DOM时的性能
- Vue中不倡导直接操作DOM，开发者只需要把大部分精力放在数据层面上
- Vue集成的一些库，大大提高了开发效率，比如Vuex,Vue-Router等

# 6) 为什么data是个函数并且返回一个对象呢？

- data之所以是一个函数，是因为一个组件可能会多处调用，而每一次调用就会执行data函数并返回新的数据对象，这样可以有效避免多处调用之间的数据污染（使用Vuex时也应注意这一点）

# 7) Vue修饰符

- .lazy：改变输入框的值时，v-model双向绑定的值value不会直接改变，而是当光标离开输入框时，value才会改变
- .trim：类似JavaScript中的trim()方法，能够把v-model绑定的值value的首尾空格直接过滤掉
- .number：将v-model绑定的值value自动转换成数字，但类型校验依然需要
    - 先输入数字再输入字符串，只取前面数字部分
    - 先输入字符串再输入字符，number修饰符无效，输入什么就是什么
- .stop：阻止事件冒泡（DOM元素嵌套时要注意，点击时先触发子元素上的事件，再触发父元素上的事件）
- .capture：事件的默认行为是自里向外冒泡，而capture修饰符效果为自外向内触发事件捕获（点击时先触发父元素上的事件，再触发子元素上的事件）
- .self：只有点击事件绑定的DOM本身才会触发事件
- .once：事件只会执行一次
- .prevent：阻止默认事件，例如a标签的跳转
- .native：加在自定义组件的事件上，保证事件可以执行
- .left、.right、.middle：对应鼠标左右中按键触发的事件，可以为一个DOM元素绑定同时多个点击事件，且触发方式不同
- .passive：监听元素滚动事件时，会一直触发onscroll事件，对PC端无影响，但移动端会产生卡顿，使用passive修饰符相当于给onscroll事件加上lazy修饰符
- .sync：当父组件传值给子组件时，子组件可以修改父组件的值（例如弹窗显隐）
- .camel：确保绑定参数被识别为驼峰写法（例如userInfo）
- .[keyCode]：使用[keyCode]修饰符可以限制事件只能通过点击某个按键才能触发

```shell
# 部分keyCode参考

# 1. 普通键
.enter 
.tab
.delete # (捕获“删除”和“退格”键)
.space
.esc
.up
.down
.left
.right
# 2. 系统修饰键
.ctrl
.alt
.meta
.shift
```

# 8) Vue内部指令

- v-on：缩写是@，绑定事件
- v-bind：缩写是:，动态绑定各种变量
- v-model：双向绑定数据项
- v-show：根据表达式控制DOM元素显隐，表达式为假值时，只是元素不可见并非销毁
- v-if：同样根据表达式控制DOM元素显隐。当表达式为真值时，才渲染元素；为假值时销毁元素
- v-else；与v-if搭配使用
- v-else-if：与v-if、v-else搭配使用
- v-for：列表循环渲染，包括数组、对象、数字、字符串等
- v-text：遇到HTML标签不会解析，原样输出
- v-html：遇到HTML标签会先解析再渲染
- v-once：元素或组件只渲染一次
- v-pre：跳过这个元素及其子元素的编译过程，即不编译Mustache标签，显示`{{ [variable-name] }}`，跳过大量没有指令的节点会加快编译。
- v-cloak：防止页面加载时出现未渲染的Mustache语法与变量名，提升用户体验
- v-slot：缩写是#，插槽名

# 9) 组件之间的传值方式有哪些？

- 父组件传值给子组件，子组件用props接收父组件的传值
- 子组件传值给父组件，子组件使用$emit+[事件名]对父组件进行传值，父组件通过@[事件名]=[处理子组件事件名]处理子组件的传值
- 组件中可以使用$parent、$children获取父组件和子组件实例，进而获取数据
- 使用$attrs和$listeners，在对一些组件进行二次封装时可以方便传值，例如A->B->C
- 使用$refs获取组件实例，进而获取数据
- 使用Vuex进行状态管理
- 使用eventBus进行跨组件触发事件，进而传递数据
- 使用provide和inject
- 使用浏览器本地缓存，例如localStorage

# 10) 路由模式有哪些模式？又有什么不同？

- hash模式：通过#[router-name]中路由的更改，触发hashChange事件，实现路由切换
- history模式：通过pushState和replaceState切换url，实现路由切换，需要后端适配

# 11) 如何设置动态class，动态style？

- 动态class对象：`<div :class="{ 'is-active': true, 'red': isRed }"></div>`
- 动态class数组：`<div :class="[ 'is-active', isRed? 'red': '']"></div>`
- 动态style对象：`<div :style="{ color: textColor, fontSize: '18px'}"></div>`
- 动态style数组：`<div :style-"[{ color: textColor, fontSize: '18px'}, { fontWeight: '300' }]"></div>`

# 12) v-if和v-show有何区别？

- v-if是通过控制DOM元素的删除和生成来控制显隐，每一次显隐都会使组件重新跑一遍生命周期，因为显隐决定了组件的生成和销毁
- v-show是通过控制DOM元素的css样式来控制显隐，不会销毁
- 频繁控制显隐使用v-show，否则使用v-if

# 13) computed和watch有何区别？

- computed是依赖已有的变量来计算一个目标变量，通常是多个变量决定一个变量的变化。由于computed存在缓存机制，依赖值不变的情况下会直接读取缓存进行复用，且computed不能进行异步操作
- watch是监听某个变量的变化，并执行相应的回调函数，通常是一个变量决定多个变量的变化。watch可以进行异步操作

# 14) Vue生命周期

![](https://s2.loli.net/2022/03/06/idKkuZ7ajLmg8JQ.png)

- beforeCreate：实例化Vue但还没进行数据的初始化与响应式处理
- created：数据已被初始化和响应式处理，在这里可以访问到数据，也可以修改数据
- beforeMount：render函数在这里被调用，生成虚拟DOM，但是还没转成真实DOM并替换到el
- mounted：在这里，真实DOM挂载完毕
- beforeUpdate：数据更新后，新的虚拟DOM生成，但还没跟旧的询DOM对比打补丁
- update：新旧虚拟DOM对比打补丁后，进行真实DOM的更新
- activated：被keep-alive缓存的组件被激活时调用
- deactivated：被keep-alive缓存的组件停用时调用
- beforeDestroy：实例被销毁之前调用，在这一步，依然可以访问到数据
- destroyed：实例被销毁后调用，该钩子被调用后，对应Vue实例的所有指令都被解绑，所有的事件监听被移除，所有的子实例也都被销毁
- errorCaptured：当捕获一个来自子孙组件的错误时被调用，此钩子会接收三个参数：错误对象、发生错误的组件实例以及一个包含错误来源信息的字符串，此钩子可以返回false
  以阻止该错误继续向上传播。

# 15) 为什么v-if和v-for不建议用在同一标签？

- 在Vue2中，v-for优先级高于v-if，因此会先执行循环渲染DOM元素再控制DOM元素显隐，增加无用的DOM操作
- 建议先使用computed筛选出无需隐藏的列表再循环渲染

# 16) vuex有哪些属性？用处是什么？

![](https://s2.loli.net/2022/03/06/MQhkUVrL7gTEcSw.png)

- State：定义了应用状态的数据结构，可以在这里设置默认点的初始状态。
- Getter：允许组件从Store中获取数据，mapGetters辅助函数仅仅是将store中的getter映射到局部计算属性
- Mutation：是唯一更改store中状态的方法，且必须是同步函数
- Action：用于提交mutation，而不是直接变更状态，可以包含任意异步操作
- Module：允许将单一的Store拆分为多个store并且提示保存在单一的状态树中

# 17) 不需要响应式的数据应该怎么处理？

```javascript
// 方法一：将数据定义在data之外
data()
{
  this.list1 = {xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx}
  this.list2 = {xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx}
  this.list3 = {xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx}
  this.list4 = {xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx}
  this.list5 = {xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx}
  return {}
}

// 方法二：Object.freeze()
data()
{
  return {
    list1: Object.freeze({xxxxxxxxxxxxxxxxxxxxxxxx}),
    list2: Object.freeze({xxxxxxxxxxxxxxxxxxxxxxxx}),
    list3: Object.freeze({xxxxxxxxxxxxxxxxxxxxxxxx}),
    list4: Object.freeze({xxxxxxxxxxxxxxxxxxxxxxxx}),
    list5: Object.freeze({xxxxxxxxxxxxxxxxxxxxxxxx}),
  }
}
```

# 18) watch有哪些属性，分别有什么用？

- 监听一个基本数据类型（Boolean、Sting、Number、Null、Undefined、Symbol、BigInt）时：

```javascript
watch: {
  value()
  {
    // do something
  }
}
```

- 监听一个引用数据类型（Object）时：

```javascript
watch: {
  obj: {
    handler()
    {
      // 执行回调
      // do something
    }
    deep: true // 是否进行深度监听
    immediate: true // 是否初始执行handler函数
  }
}
```

# 19) 父子组件生命周期调用顺序

- 父组件beforeCreated
- 父组件created
- 父组件beforeMount
- 子组件beforeCreate
- 子组件created
- 子组件beforeMount
- 子组件mounted
- 父组件mounted

# 20) 对象属性无法更新视图，删除属性无法更新视图，为什么？怎么办？

- 原因：`Object.defineProperty`没有对对象的新属性进行属性劫持
- 对象新属性无法更新视图：使用`Vue.$set(obj, key, value)`，组件中`this.$set(obj, key, value)`
- 删除属性无法更新视图：使用`Vue.$delete(obj, key)`，组件中`this.$delete(obj, key)`

# 21) 直接arr[index] = xxx 无法更新视图怎么办？为什么？

- 原因：Vue没有对数进行`Object.defineProperty`的属性劫持，所以直接arr[index] = xxx是无法更新下视图的
- 使用数组的splice方法，`arr.splice(index, 1, item)`
- 使用`Vue.$set(arr, index, value)`

# 22) 自定义指令

[8个非常实用的Vue自定义指令](https://www.cnblogs.com/lzq035/p/14183553.html)

# 23) 插槽的使用及原理？

[「Vue源码学习」你真的知道插槽Slot是怎么“插”的吗](https://juejin.cn/post/6949848530781470733)

# 24) 为什么不建议用index作为key值，为什么不建议用随机数作为key值？

- 因为index和随机数变化较大，重新渲染比较损耗性能

# 25) 说说nextTick的用处？

- Vue采用的是异步更新策略，即同一事件循环内的多次修改，会统一进行一次视图更新，达到节省性能的目的
- 数据更新但视图还没更新，还是上一次的旧视图数据
- 使用nextTick可以拿到最新的视图数据

```javascript
this.$nextTick(() => {
  // do something
})
```

# 26) Vue的SSR是什么？有什么好处？

- SSR就是服务端渲染
- 基于nodejs serve服务端渲染，所有HTML代码在服务端渲染
- 数据返回给前端，前端进行“激活”，即可成为浏览器识别的HTML代码
- SSR首次加载更快，有更好的用户体验，有更好的seo优化，因为爬虫能看到整个页面的内容，如果是vue项目，由于数据还要经过解析，这就造成爬虫并不会等待你的数据加载完成，所以，Vue
  项目的seo体验并不是很好





# 参考

- [「百毒不侵」面试官最喜欢问的14种Vue修饰符](https://juejin.cn/post/6981628129089421326)
- [「自我检验」熬夜总结50个Vue知识点，全都会你就是神！！！](https://juejin.cn/post/6984210440276410399)

