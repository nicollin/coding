# 1.拿到某个元素在数组里的下标（索引）

- 元素在数组内，返回大于等于0的数组下标；
- 元素不在数组内，返回-1.

## 简单数组

- 内容是基本类型（Boolean、String、Number、Null、Undefined）
- 使用 index = arr.indexOf(ele)

```javascript
const simpleList = [1, 2, 3, 4, 5]
const findSimpleEleOne = 2, findSimpleEleTwo = 8
console.log(simpleList.indexOf(findSimpleEleOne)) // 1
console.log(simpleList.indexOf(findSimpleEleTwo)) // -1
```

## 复杂数组

- 内容是引用类型（Object，Array）
- 使用 index = arr.findIndex(item => func(item))

```javascript
const complexList = [
    {id: 1, message: '111'},
    {id: 2, message: '222'},
    {id: 3, message: '333'}
]
const findComplexEleOne = {message: '111'}, findComplexEleTwo = {message: '999'}
const findIndexOne = complexList.findIndex(item=> {
    return item.message === findComplexEleOne.message
})
const findIndexTwo = complexList.findIndex(item => {
    return item.message === findComplexEleTwo.message
})
console.log(findIndexOne, findIndexTwo) // 0 -1
```