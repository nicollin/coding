- 简单数据结构
    - 有序数据结构（ES Array）：栈、队列、链表，存储空间小。
    - 无序数据结构（ES Object）：集合、字典、散列表，读取时间快。
- 复杂数据结构
    - 树、堆
    - 图

Q：使用ECMAScript（JS）代码实现一个事件类Event，包含以下功能：绑定事件、解绑事件和派发事件

```javascript
class Event {
    constructor() {
        // 储存事件的数据结构
        // 为了查找迅速，使用了对象（字典）
        this._cache = {}
    }

    // 绑定
    on(type, callback) {
        // 为了按类查找方便和节省空间，
        // 将同一类型事件放到一个数组
        // 这里的数组是队列，遵循先进先出
        // 即先绑定的事件先触发
        let fns = (this._cache[type] = this._cache[type] || [])
        if (fns.indexOf(callback) === -1) {
            fns.push(callback)
        }
        return this
    }

    // 触发
    trigger(type, data) {
        let fns = this._cache[type]
        if (Array.isArray(fns)) {
            fns.forEach(fn => {
                fn(data)
            })
        }
        return this
    }

    // 解绑
    off(type, callback) {
        let fns = this._cache[type]
        if (Array.isArray(fns)) {
            if (callback) {
                let index = fns.indexOf(callback)
                if (index !== -1) {
                    fns.splice(index, 1)
                }
            } else {
                // 全部清空
                fns.length = 0
            }
        }
        return this
    }
}

// 测试用例
const event = new Event()
event.on('test', (a) => {
    console.log(a)
})

event.trigger('test', 'hello world') // hello world

event.off('test')
event.trigger('test', 'hello world') // Event {_cache: {test: []}}
```

- 算法复杂度
    - 空间复杂度
    - 时间复杂度（从小到大）
        - 常数阶（O(1)
        - 对数阶（O(logN)）
        - 线性阶（O(n)）
        - 线性对数阶（O(nlogN)）
        - 平方阶（O(n^2)）
        - 立方阶（O(n^3)）
        - ！k次方阶（O(n^k)）
        - 指数阶（O(2^n)）

- 算法复杂度分析
    - 看循环，一般一重循环是O(n)，两重循环是O(n^2)，以此类推
    - 二分查找：O(logN)
    - 保留最高项，去除常数项

Q：分析下面代码的算法复杂度

```javascript
let i = 0 // 语句执行一次
while (i < n) { // 语句执行n次
    console.log(`Current i is ${i}`) // 语句执行n次
    i++ // 语句执行n次
}
// 语句执行次数为3n+1，算法复杂度为O(n)
```

```javascript
let number = 1 // 语句执行一次
while (number < n) { //语句执行logN次
    number *= 2 // 语句执行logN次
}
// number的增长速度为2^n次方，循环代码实际执行logN次，语句执行2logN+1次，复杂度为O(logN)
```

```javascript
for(let i = 0; i< n; i++) { // 语句执行n次
    for(let j = 0;j< n;j++) { // 语句执行n^2次
        console.log('I am here!') // 语句执行n^2次
    }
}
// 语句执行次数为2n^2+n次，算法复杂度为O(n^2)
```

# 基础算法

- 枚举
- 递归
  - 递归主体，为了循环解决问题而编写的代码
  - 递归的结束条件，没有结束条件，容易造成死循环

Q：实现JS对象的深拷贝

- 深拷贝：拷贝数据的时候，将原数据的所有引用结构也拷贝一份
- 浅拷贝：拷贝数据的时候，只是复制其引用关系

```javascript
function deepClone(o1, o2){
    for( let k in o2){
        if(typeof o2[k] === 'object'){
            o1[k] = {}
            deepClone(o1[k], o2[k])
        } else {
            o1[k] = o2[k]
        }
    }
}

// 测试用例
let obj = {
    a: 1,
    b: [1,2,3],
    c: {}
}
let emptyObj = Object.create(null)
deepClone(emptyObj, obj)
console.log(emptyObj.a === obj.a) // true
console.log(emptyObj.b == obj.b) // false
```

Q：求斐波那契数列1,1,2,3,5,8,13,21,34,55,89...中的第n项
```javascript
let count = 0
function fn(n){
    let cache = {}
    function _fn(n) {
        if(cache[n]) {
            return cache[n]
        }
        count ++
        if(n == 1|| n == 2){
            return 1
        }
        let prev = _fn(n-1)
        cache[n-1] = prev
        let next = _fn(n-2)
        cache[n-2] = next
        return prev + next
    }
    return _fn(n)
}

let count2 = 0
function fn2(n) {
    count2 ++
    if(n == 1 || n == 2) {
        return 1
    }
    return fn2(n-1) + fn2(n-2)
}

console.log(fn(20), count) // 6765 20
console.log(fn2(20), count2) // 6765 13529
```

# 快速排序和二分查找

- 快速排序：算法复杂度O(logN)
  - 随机选择数组中的一个数A，以这个数为基准
  - 其他数字依次跟这个数进行比较，比这个数小的放在其左边，大的放到其右边
  - 经过一次循环之后，A左边为小于A的数，右边为大于A的数
  - 左边和右边的数组递归重复上面的过程
- 二分查找：算法复杂度O(logN)
  - 数组中排在中间的数字A，与要找的数字比较大小
  - 比A说明在A的前半部分，比A小说明在A的后半部分
  - 缩小范围继续查找

```javascript
// 划分操作函数
function partition(array, left, right){
    // 用index 取中间值而非splice
    const pivot = array[Math.floor((right+ left)/2)]
    let i = left,j = right
    while (i<= j){
        while (compare(array[i], pivot) === -1){
            i++
        }
        while (compare(array[j], pivot) === 1) {
            j--
        }
        if(i<= j){
            swap(array, i, j)
            i++
            j--
        }
    }
    return i
}

// 比较函数
function compare(a, b){
    if(a===b) {
        return 0
    }
    return a< b? -1 : 1
}

// 原地交换函数，而非用临时数组
function swap(array, a ,b) {
    ;[array[a], array[b]] = [array[b], array[a]]
}

function quick(array, left, right){
    let index
    if(array.length > 1) {
        index = partition(array, left, right)
        if(left < index -1) {
            quick(array, left, index -1)
        }
    
        if(index < right) {
            quick(array, index, right)
        }
    }
    return array
}

function quickSort(array) {
    return quick(array, 0, array.length - 1)
}

const Arr = [85, 24, 63, 45, 17, 31, 96, 50];
console.log(quickSort(Arr));  // [17, 24, 31, 45, 50, 63, 85, 96]
```

Q：在一个二维数组中，每一行都按照从左到右递增的顺序排序，每一列都按照从上到下递增的顺序排序。请完成一个函数，输入这样的一个二维数组 和一个整数，判断数组中是否含有该整数。
```javascript
function binarySearch(target, array){
    let i = 0
    let j = array[i].length - 1
    while (i< array.length && j >= 0){
        if(array[i][j] < target) {
            i++
        } else if(array[i][j] > target) {
            j--
        } else  {
            return true
        }
    }
    return false
}
```

Q：现在我有一个 1~1000 区间中的正整数，需要你猜下这个数字是几，你只能问一个问题：大了还是小了？问需要猜几次才能猜对？

最多需要猜log1000次，又因为2^9=512,2^10=1024，所以最多猜10次就能得到这个数

# 正则匹配解题

Q：求字符串中第一个出现一次的字符

```javascript
function find(str){
    for (var i = 0; i< str.length;i++){
        let char = str[i]
        let reg = new RegExp(char, 'g')
        let l = str.match(reg).length
        if(l == 1){
            return char
        }
    }
}
```

Q：将1234567变成1,234,567。即千分位标注

```javascript
function exchange(num){
    num += ''; // 转成字符串
    if(num.length <= 3) {
        return num
    }
    num = num.replace(/\d{1,3)(?=(\d{3})+$)/g, (v) => {
        console.log(v)
        return v + ','
    })
    return num
}

console.log(exchange(1234567))
```




