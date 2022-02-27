# 1. this指向问题

**this永远指向最后调用它的那个对象**

例1：

```javascript
var name = 'windowsName'

function a() {
    var name = 'Cherry'
    console.log(this.name)
    console.log('inner:' + this)
}

a()
console.log('outer:' + this)
```

Desc：最后调用方法a的地方`a()`，前面没有调用的对象那么this指向的就是全局对象`window`，相当于`window.a()`。因此输出结果为

```text
windowsName
inner:[Object Window]
outer:[Object Window]
```

Tips：使用严格模式后，全局对象指向`undefined`，此时执行以上代码会报错：`Uncaught TypeError: Cannot read properties of undefined (reading 'name')`

***

例2：

```javascript
var name = 'windowsName'
var a = {
    name: 'Cherry',
    fn: function () {
        console.log(this.name)
    }
}
a.fn()
```

Desc：本例中，函数fn被对象a调用，a.fn执行时this指向对象a，所以打印的值就是a中name的值。因此输出结果为`Cherry`

***

例3：

```javascript
var name = 'windowsName'
var a = {
    name: 'Cherry',
    fn: function () {
        console.log(this.name)
    }
}
window.a.fn()
```

Desc：本例中，函数fn被对象window.a调用，window.a.fn执行时this指向对象a，打印的值依然是a中name的值。因此输出结果为`Cherry`

***

例4：

```javascript
var name = 'windowsName'
var a = {
    fn: function () {
        console.log(this.name)
    }
}
window.a.fn()
```

Desc：本例中，函数fn依然被对象window.a调用，window.a.fn执行时this指向对象a，但对象a并未对name进行定义和赋值。因此输出结果为`undefined`

***

例5：

```javascript
var name = 'windowsName'
var a = {
    name: null,
    fn: function () {
        console.log(this.name)
    }
}
var f = a.fn
f()
```

Desc：函数fn最终被window.f调用，f执行时this指向window。因此输出结果为`windowsName`

***

例6：

```javascript
var name = 'windowsName'

function fn() {
    var name = 'Cherry'
    innerFunction()

    function innerFunction() {
        console.log(this.name)
    }
}

fn()
```

Desc：本例乍一看，innerFunction是定义在fn内部的函数，但函数fn被window.fn调用，fn执行时this依旧指向window。因此输出结果为`windowsName`

# 2. 怎么改变this指向

1. **使用ES6的箭头函数**
2. **在函数内部使用_this = this**
3. **使用apply、call、bind**
4. **new实例化一个对象**

例7：

```javascript
var name = 'windowsName'
var a = {
    name: 'Cherry',
    func1: function () {
        console.log(this.name)
    },
    func2: function () {
        setTimeout(function () {
            this.func1()
        }, 100)
    }
}
a.func1()
a.func2()
```

Desc：本例中，a.func1执行时this指向对象a。因此`a.func1()`的输出结果为`Cherry`。a.func2中有一个延时函数`setTimeout()`，但由于***
setTimeout方法是挂在window对象下的***`（超时调用的代码都是在全局作用域中执行的，因此函数中this的值在非严格模式下指向window对象，在严格模式下是undefined）`
。所以a.func2执行时this指向window，又因为window下并未定义func1函数。因此**a.func2()**的输出结果为`Uncaught TypeError: this.func1 is not a function`

## 2.1 箭头函数

1. **箭头函数的this始终指向函数定义时的this，而非执行时**
2. 箭头函数中没有this绑定，必须通过查找作用域链来决定其值
3. 如果箭头函数被非箭头函数包含，则this绑定的是最近一层非箭头函数的this。否则，this为undefined

例8：

```javascript
var name = "windowsName";
var a = {
    name: "Cherry",
    func1: function () {
        console.log(this.name)
    },
    func2: function () {
        setTimeout(() => {
            this.func1()
        }, 100);
    }

};
a.func2() 
```

Desc：本例中，a.func2执行时，由于setTimeout内使用了箭头函数，this指向对象a。因此`a.func2()`的输出结果为`Cherry`

***

例9：

```javascript
var name = "windowsName";
var a = {
    name: "Cherry",
    func1: function () {
        console.log(this.name)
    },
    func2: function () {
        this.func1 = function () {
            console.log('func2.func1')
        },
            setTimeout(() => {
                this.func1()
            }, 100);
    }

};
a.func2() 
```

Desc：本例中，箭头函数的this指向最近一层的this，this指向a.func2函数。因此`a.func2()`的输出结果为`func2.func1`

## 2.2 在函数内部使用_this = this

由于执行了_this = this，this对象被保存在变量_this中，在函数内使用的_this，_this的指向不会再改变

例10：

```javascript
var name = "windowsName";
var a = {
    name: "Cherry",
    func1: function () {
        console.log(this.name)
    },
    func2: function () {
        var _this = this;
        setTimeout(function () {
            _this.func1()
        }, 100);
    }
};
a.func2()
```

Desc：本例中，_this被固定指向了对象a，因此`a.func2()`的输出结果为`Cherry`

## 2.3 使用apply、call、bind

例11：

```javascript
// 使用apply
var a = {
    name : "Cherry",
    func1: function () {
        console.log(this.name)
    },
    func2: function () {
        setTimeout(function (){
            this.func1()
        }.apply(a), 100)
    }
}
a.func2() 
```

Desc：[apply](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Function/apply) 的用法为`func.apply(thisArg, [argsArray])`中`thisArg`为func函数运行时指定的this，`argsArray`为func函数的入参。本例中，this指向对象a。因此`a.func2()`输出结果为`Cherry`

***

例12：

```javascript
// 使用call
var a = {
    name : "Cherry",
    func1: function () {
        console.log(this.name)
    },
    func2: function () {
        setTimeout(function (){
            this.func1()
        }.call(a), 100)
    }
}
a.func2()
``` 

Desc：[call](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Function/call) 的用法为`func.call(thisArg, arg1, arg2, ...)`，第一个参数`thisArg`同样为func函数运行时指定的this，`arg1, arg2, ...`为指定的参数列表，this指向对象a。因此`a.func2()`输出结果为`Cherry`

***

例13：

```javascript
// 使用bind
var a = {
    name : "Cherry",
    func1: function () {
        console.log(this.name)
    },
    func2: function () {
        setTimeout(function (){
            this.func1()
        }.bind(a), 100)
    }
}
a.func2()
```

Desc：[bind](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Function/bind) 的用法为`function.bind(thisArg[, arg1[, arg2[, ...]]])`，第一个参数`thisArg`同样为func函数运行时指定的this，与apply、call不同的是，bind不能自行调用，因此需要执行`(func.bind(thisArg[, arg1[, arg2[, ...]]]))()`，this指向对象a。因此`a.func2()`输出结果为`Cherry`

## 2.4 new实例化一个对象

例14：

```javascript
var name = 'windowsName'
function func() {
    var name = 'Chery'
    console.log(this)
    console.log(this.name)
}
var f1 = func()
var f2 = new func()
```

Desc：f1只是调用了func函数，这里的this指向window，因此输出结果依次为`window`、`windowsName`；f2是实例化对象func，这里的this指向了func本身，因此输出结果依次为`func{ }`、`undefined`

# 参考
[this、apply、call、bind-掘金](https://juejin.cn/post/6844903496253177863)