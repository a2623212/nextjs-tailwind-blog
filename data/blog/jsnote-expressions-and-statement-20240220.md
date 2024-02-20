---
title: '[Note] 《JavaScript》 表達式和陳述句：實用筆記和重要概念'
date: '2024/2/20'
lastmod: '2024/2/20'
tags: [JavaScript, 表達式, 陳述句]
draft: false
summary: '這個「學習目的」困擾我一陣子，在沒有接觸 React 之前，以為知道自己寫的東西是什麼。結果，有鑑於`JSX`的限制，發現很多時候 bug 都是因為搞混了表達式（expression）和陳述句（statement）。'
images: ['/static/images/javaScript-note-03.png']

layout: PostLayout
---

最近學習用 React 做專案，在閱讀「JSX（HTML-like Markup）」文件時，看到這麼一句話：「Any **JavaScript expression** will work between curly braces。」，做了一下自我評量：**「JavaScript Expression」 是什麼** 👀？以及**「Expressions」和「Statement」有什麼不一樣** 👀？

🤔️ 發現自己也是一知半解，趕緊來做功課。

## 發動引擎 🚂

要釐清表達式（expression）和陳述句（statement）的差別，重點在知道一件事：

> **表達式會回傳一個值（value）** - 來自[MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Expressions_and_operators)的解釋。

**為什麼要了解兩者的差別？**

這個「學習目的」困擾我一陣子，在沒有接觸 React 之前，以為知道自己寫的東西是什麼。結果，有鑑於`JSX`的限制，發現很多時候 bug 都是因為搞混了表達式（expression）和陳述句（statement）。

## 表達式 (Expressions)

表達式又可以稱為「表示式」或「運算式」（中文真麻煩 😅，本文以下皆使用 expression）。

expression 會根據值、變數、運算符與邏輯**產生一個結果**，也就是回傳一個值，這個值可以是數字、字串，也可以是一個物件（object）或函式（function）。

舉例而言，這些都是 expression：

數字和字串：

```javascript
10 // 數字，expression ✅
3 + 5 // 加法運算符 = 8，expression ✅
;('Hello!') // 字串，expression ✅
'Hello'.toUpperCase() // 結果為 "HELLO"，expression ✅
```

條件運算符和布林：

```javascript
isOnline ? green : red // 回傳一個變數，expression ✅
false // Boolean，expression ✅
5 > 3 // returns true，expression ✅
```

變數與函式呼叫：

```javascript
firstName + ' ' + lastName // expression ✅
getTheUser() // 調用函式，expression ✅
```

那我宣告一個 function，讓他 return 一個值，會是 expression 嗎？

Well...不是，這是**陳述句（statements）**。

## 陳述句 (Statements)

陳述句，也可以稱為「陳述式」（中文真···😅，本文以下皆使用 statement）。

最簡單的 statement：**宣告一個變數**。但不止如此，JavaScript 程式是由一連串的 statements 所組成。每個 statement 都是給電腦的指令，用來告訴它執行特定的操作。

> statements 是程式中最基本的結構單位，它們通過組合和執行來實現程式的功能。

以下都是 statement：

宣告一個變數

```javascript
let myBirthDate = 'January 31, 1997'

function myBirth(myBirthDate) {
  console.log(myBirthDate)
}
```

條件 (if-else statements)

```javascript
if (myBirthDate === 'today') {
  //....
} else {
  //...
}
```

迴圈 (for/while 迴圈)

```javascript
while (age < 30) {
  //...
}
```

函式定義和函式呼叫?

```javascript
function add(emily, bob) {
  return emily + bob
}
```

> 如何快速分辨？

`console.log( *some* )`，如果 `*some*` 是 statement，就會得到一個「Syntax error」，反之，會得到一個值。

這是因為 `console.log( *some* )` 本身就是一個方法，裡面的 `*some*` 必須是 expression。

還有一個小觀察「`;`」分號會把 statement 區隔，雖然加分號與否不強制，但這也是一個可以觀察的重點。

## 隱身在 Statements 的 Expressions

```javascript
function add(emily, bob) {
  return emily + bob
}

add(3, 4) //是什麼
```

答案是：得到一個「7」，但沒有做任何事。

在這段程式碼中，有許多 expressions，「`emily`、`bob`、`+`、`add(3, 4)`」，所以 statements 裡包含了 expressions，`add(3, 4)` 其實有回傳一個值「7」，但因為没有告訴程式要接起來（或做任何事），雖然 expression 有產生一個值，但沒有下任何指令，所以就結束了。

也就是說，只有 expressions，xu duo shi hao 其實還不夠，下指令的工作是 statements 負責。這就呼應到：

「**statements 是程式中最基本的結構單位，它們通過組合和執行來實現程式的功能**」。

## 總結

每個 statement 都是給電腦的指令，用來告訴它執行特定的操作。expressions 會根據值、變數、運算符與邏輯**產生一個結果**，也就是回傳一個值，但進一步操作需要給予指令。

了解了兩者的差別就不會被突如其來的「Syntax Error」以及「不會動（例如上面的 `add(3, 4)` ）」的程式碼嚇到了。

## 參考資料

1. [Expressions and operators -MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Expressions_and_operators#comma_operator)
2. [Statements and declarations -MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements#difference_between_statements_and_declarations)
3. [Statements Vs. Expressions -Josh Comeau](https://www.joshwcomeau.com/javascript/statements-vs-expressions/#a-handy-trick-3)
4. [JavaScript Expressions and Statements -Madhu M](https://medium.com/launch-school/javascript-expressions-and-statements-4d32ac9c0e74)
