---
title: '[Note]《JavaScript》 深入探究「解構賦值」'
date: '2023/6/8'
lastmod: '2023/6/8'
tags: [JavaScript, 解構賦值]
draft: false
summary: '第一次看到酷東西，叫做「解構賦值」，是 JavaScript ES6 後產生的語法糖，學會後寫 code 更快了。'
images: ['/static/images/javaScript-note-01.png']

layout: PostLayout
---

## 【 解構賦值 】？

解構賦值（Destructuring assignment）是 JavaScript ES6 後產生的語法糖，MDN 上的解釋：

> The **destructuring assignment** syntax is a JavaScript expression that makes it possible to **unpack values** from **arrays**, or **properties** from **objects**, into distinct variables.

**解構聽起來超酷，但就是「拆開」啦！**

也就是說，可以一次把 arrays 的 values（值） 或 objects 的 properties（屬性） 像鏡子一樣，按照順序「賦值」給`=`左邊的變數，而不會更動到原本的 arrays 或 objects。

使用解構賦值的優點在於，即可以**精簡**，又增加**可讀性**，尤其在需要從 Arrays 或 Objects 提取資料的時候。

在沒有解構賦值之前：

```javascript
let fruits = ['apple', 'orange', 'grape']
let monFruit = fruits[0]
let tueFruit = fruits[1]
let wedFruit = fruits[2]
```

在解構賦值之後：

```javascript
let [monFruit, tueFruit, wedFruit] = ['apple', 'orange', 'grape']
```

## 基本語法

### 應用在 Array 的解構賦值

- 基本中的基本：

```javascript
const fruits = (['apple', 'orange', 'grape'][(monday, tuesday, wednesday)] = fruits)
console.log(monday) //'apple'
console.log(tuesday) //'orange'
```

- 可以交換變數的值：

```javascript
let monday = 'apple'
let tuesday = ('orange'[(monday, tuesday)] = [tuesday, monday])
console.log(monday) //'orange'
console.log(tuesday) //'apple'
```

- 先用 `let` 宣告變數再賦值:

```javascript
let mondy, tuesday
;[monday, tuesday] = ['apple', 'orange']
console.log(monday) //'apple'
console.log(tuesday) //'orange'
```

- 略過一些值

```javascript
let [monday, , , thursday] = ['apple', 'orange', 'grape', 'watermelon']
console.log(monday) //'apple'
console.log(thursday) //'watermelon'
```

- 結合「Rest Operator `...` 」使用

```javascript
let [saturday, sunday, ...weekdays] = ['pear', 'melon', 'apple', 'orange', 'grape']
console.log(saturday) //'pear'
console.log(sunday) //'melon'
console.log(weekdays) //['apple','orange','grape']
```

:point_right: Rest Operator，又稱為「其餘運算子」，將其他數值結合成一個 array.

> :warning: 在使用「其餘運算子」，不能再使用逗號結尾 `,` 會跳出「語法錯誤」
> **_Uncaught SyntaxError: Rest element must be last element_** ⛔️

- 小結：
  在「陣列」的情形，基本上，只要等號 `=` 左右的模式相同就可以將對應的值 assign 給對應的變數。換言之，如果沒有找到值，就會得到 `undefined`。

### 應用在 Object 的解構賦值

- 基本中的基本

```javascript
let fruits = { monday: 'apple', tuesday: 'banana' }
let { monday, tuesday } = fruits
console.log(monday) //"apple"
console.log(tuesday) //"banana"

// 也可以寫這樣：
let { monday, tuesday } = { monday: 'apple', tuesday: 'banana' }
```

比較一下，原本將 `"apple"` 賦值給 `monday` 要寫成：

```javascript
let monday = fruits.monday //···解構賦值真的簡單很多。
```

- 屬性賦值

```javascript
let fruits = {monday:"apple",tuesday:"banana"}
let monday, tuesday;
({monday, tuesday}) = fruits;
console.log(monday) //"apple"
console.log(tuesday) //"banana"
```

- 重新取變數名
  原則上，object 的解構賦值，需要「等號左邊」的變數名 🟰 「等號右邊」 object 的 key，所以如果有重新命名的需求，可以這麼做：

```javascript
let fruits = { monday: 'apple', tuesday: 'banana' }
let { monday: morning, tuesday: noon } = fruits
console.log(morning) //"apple"
console.log(noon) //"banana"
```

- 巢狀用法

```javascript
let fruit = {
  name: 'apple',
  place: {
    country: 'Japan',
    city: 'Aomori',
  },
  colors: ['Red', 'Green'],
}

let {
  name: foo,
  place: { country: bar, city: x },
} = fruit

console.log(foo) //"apple"
console.log(bar) //"Japan"
```

- 結合「Rest Operator `...` 」使用

```javascript
let fruit = {
  name: 'apple',
  place: {
    country: 'Japan',
    city: 'Aomori',
  },
  colors: ['Red', 'Green'],
  address: '125 Terasawa, Shimizu Tomita, Hirosaki-shi, Aomori',
}
let { name, colors, ...others } = fruit
console.log(name) //"apple"
console.log(colors) //["Red", "Green"]
console.log(others)
// {place: {country: "Japan", city: "Aomori" },
// address:'125 Terasawa, Shimizu Tomita, Hirosaki-shi, Aomori'}
```

### 解構賦值的預設值

解構賦值如果沒有找到值，就會得到 `undefined`，為了避免獲得一個 undefined 的值，可以事先給予變數預設值，假設週一預設吃蘋果 🍎，但有時候心情不錯，想吃香蕉 🍌，就可以用預設值表示：

```javascript
// Array
let [monday = 'apple', tuesday = 'apple'] = ['banana']
console.log(monday, tuesday) // 'banana', 'apple'

//Object
let { monday: fruitA = 'apple' } = {}
let { monday: fruitB = 'apple' } = { monday: 'banana' }
console.log(fruitA) //apple
console.log(fruitB) //banana
```

### 在函數參數中的應用

- Array 解構在 function 中的應用

```javascript
function getArray() {
  return ['apple', 'banana', 'grape', 'watermelon']
}
let [monday, tuesday] = getArray()

console.log(monday) //'apple'
console.log(tuesday) //'banana'
```

- Object 解構在 function 中的應用

```javascript
let fruit = {
  name: 'apple',
  place: 'Japan',
  address: '125 Terasawa, Shimizu Tomita, Hirosaki-shi, Aomori',
}

let fruitB = {
  place: 'Japan',
  address: '125 Terasawa, Shimizu Tomita, Hirosaki-shi, Aomori',
}

function showFruit({ name = 'grape', place }) {
  console.log(`${name} is from ${place}`)
}

showFruit(fruit) //'apple is from Japan'
showFruit(fruitB) //'grape is from Japan'
showFruit()
//語法錯誤：Uncaught TypeError: Cannot destructure property 'name' of 'undefined' as it is undefined.⛔️
```

## 解構賦值的注意點

### 從非 Array 或非 Object 解構賦值

要了解原理，先把下面的英文字母 a 到 h 全部 `console.log` 出來：

```javascript
let { a } = null
// TypeError: Cannot destructure property 'a' of 'null' as it is null.

let { b } = undefined
// TypeError: Cannot destructure property 'b' of 'undefined' as it is undefined.

let { c } = false
// undefine

let { d } = 10
// undefine

let { e } = 'world'
// undefine

let [f] = false
// TypeError: false is not iterable.

let [g] = 10
// TypeError: 10 is not iterable.

let [h] = 'world'
// w
```

只有字串可以進行解構，印出：`h`。

### 解構賦值在 Array 與 Object 的差別

Array 解構的對象是利用「可以迭代 iterable」的特性，按照**順序**（由左至右）將「值」賦值，常見的使用方法包含結合其他迭代器（例如：`for...of` 或 `Array.map()`）使用。

```javascript
let [one, two] = 1
// 會出現語法錯誤：TypeError: 1 is not iterable.
```

Object 解構的對象則是按照相同的「屬性名稱」，所以順序就不重要了，也因此可以重新命名，因為不是改值，而是「屬性」名稱，常見把 object 當作 function 的 parameter 傳遞所需要的值。

```javascript
let { one, two } = 1
// {one, two} 實際等於 {one: undefined, two: undefined}
// 不會出現語法錯誤
// console.log(one, two) = undefined undefined
```

## 結論

第一次看到解構賦值的想法「這是啥？😧」，我在做 Twitter clone 專案用到大量的解構賦值，尤其是從後段拿資料的時候，畢竟資料是以 object 呈現的，不用一個一個變數賦值，真是太方便了。

後來，做 React 專案時，因為 React 頻繁使用 `Array.map()` 的方法，解構賦值也是用好用滿，是一個了解之後會有效節省時間的工具。

### 參考資料

1. [Mozilla:Destructuring assignment](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment)
2. [变量的解构赋值 by 阮一峰](https://es6.ruanyifeng.com/#docs/destructuring#%E5%AF%B9%E8%B1%A1%E7%9A%84%E8%A7%A3%E6%9E%84%E8%B5%8B%E5%80%BC)
3. [[ES6] 分割代入（Destructuring assignment）を使いこなす](https://www.yoheim.net/blog.php?q=20160805)
