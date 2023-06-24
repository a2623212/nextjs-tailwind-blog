---
title: '[Note]《JavaScript》執行順序？究竟什麼是 Event Loop'
date: '2023/6/25'
lastmod: '2023/6/25'
tags: [JavaScript, event loop]
draft: false
summary: '為了能打造出一個更好的產品，必須對使用的語言與工具更加熟悉，本篇文章將整理 JavaScript 中的 Event Loop 以及 非同步 (asynchronous)。'
images: ['/static/images/javaScript-note-02.png']

layout: PostLayout
---

為了能打造出一個更好的產品，必須對使用的語言與工具更加熟悉，本篇文章將整理 JavaScript 中的 Event Loop 以及 非同步 (asynchronous)。

> 本文首次發文為 2021 年 11 月，因以主題區分兩個部落格，重新整理再發布。

## 奇怪的順序排列

`setTimeout()`是一個依設定時間延後執行函式或特定程式的方法。第一個參數可以是一個 function ，第二個參數則是時間，例如**1 秒鐘後印出`Hi`**：

```javascript
setTimeout(()=>{console.log('Hi')}, 1000))
```

如果在沒有延遲時間時，即當第二個參數是 0 ，又會怎麼樣呢?

用 replit 測試之後產生下圖的結果，第 3 行會先被 console，明明沒有時間差啊？這是為什麼呢？

![event-loop](/static/images/jsnote2-picture001.jpeg)

## 同步與非同步

要解釋上面的原因，就必須先回到 JavaScript 語言的**同步**特性。

### 同步

JavaScript 是一種同步的語言，又稱為單執行緒(single-threaded)，指一次只執行一個動作。也就是說，在進行下一個動作時，必須等上一個動作完成，這種情形就叫「同步 (synchronous)」，我個人覺得是很反直覺的解釋。

單執行緒的語言雖然能夠讓開發者更專心執行一個程式，不用擔心發生併發 (concurrency) 事件；但當遇到複雜的情況，如要對外請求大量資料的情形，就會造成阻塞(block)，讓後續無法渲染，造成網頁卡住現象，使用者只能無止盡的等待。

### 非同步

非同步 (asynchronous)，又稱異步，是只一次可以同時處理很多事情（再次覺得反直覺）。

用煮飯來比喻的話，同步就是先煮第一道菜(備料 → 煮飯 → 盛盤)，再煮第二道菜(備料 → 煮飯 → 盛盤)，...以此類推。非同步則是先備料，用電鍋煮第一道菜，同時用爐子煮第二道菜，一邊準備飯後水果。

![image alt](https://assets-lighthouse.alphacamp.co/uploads/image/file/5072/ExportedContentImage_02.png)
Reference from: Alpha Camp

前提概念 OK 之後，那什麼是 **Event Loop** 呢？

## JavaScript 執行環境

前面有提到，JavaScript 的語言特性是同步，那「為什麼還能夠執行非同步事件」呢？

JavaScript 自始至終都是一個單執行序的程式，當他用了一些「工具」，來處理瀏覽器內執行，由瀏覽器提供的許多 Web API 工具， 如 AJAX、DOM Events、上面提到的`setTimeout()`都屬之。

> There are APIs in the browser that have been used by almost any JavaScript developer out there (e.g. “setTimeout”). Those APIs, however, are not provided by the Engine.
> --Reference from: [SessionStack Blog](https://blog.sessionstack.com/how-does-javascript-actually-work-part-1-b0bacc073cf)

所以，接下來要了解一下 JavaScript 的執行環境。

### 部門介紹

JavaScript 的執行環境，包括以下區域： JS Engine、Web APIs 以及 Callback Queue， JS Engine 是用來解析程式的工具，每一個瀏覽器有其自己的 JS Engine，Google Chrome 使用的叫做 V8 JS Engine。
![image alt](https://miro.medium.com/max/3000/1*4lHHyfEhVB0LnQ3HlhSs8g.png)
Reference from: [SessionStack Blog](https://blog.sessionstack.com/how-does-javascript-actually-work-part-1-b0bacc073cf)

- Memory Heap
  屬於 JS Engine 的一部份，在程式中宣告、定義變數、函式...等的記憶體儲存空間。
- Call Stack
  屬於 JS Engine 的一部份，當 JavaScript 開始執行時，程式要執行的內容就會被推進 (pop up)，一次執行一個，執行完後會跳出 (pop out)。:bulb: Call stack 的資料結構為 LIFO (last in first out)，只有在 stack 最上層的函式才會執行，下層的必須等上層跳出之後才會繼續。

- Web APIs
  瀏覽器提供許多不同的 API 工具 (event listeners 、HTTP/AJAX requests 或 timing functions)，呼叫這些函式之後，會把「callback function」放進 Callback Queue。

- Callback Queue
  這裡就是一個 callback 等待區，專門放置具有 callback 功能的函式。在這裡的函式會聽從 Event loop 的通知，將函式推進去空的 stack 執行。
  :bulb: 和 Call stack 不同，Callback Queue 的資料結構為 FIFO (first in first out)，當 stack 空之後會將最早放進 Callback Queue 的函式送進 stack，等 stack 又空了，才會送第 2 早的。

### Event loop

我們主角 Event loop， 就是一個在 JavaScript 執行環境中負責檢查 call stack 和 callback queue 機制。如果他發現 call stack 空了，他就會通知 callback queue 將下一個 callback function 送過來。

:point_down: 可以以下圖來解釋整個 JavaScript 執行環境和 Event loop

![image alt](https://i.stack.imgur.com/2DRQc.gif)
Reference from: [here](https://gist.github.com/thinker3197/473ea38daaec4f6b5d7783e0ffc57a21)

## 結論

閱讀完這些資料之後，我們可以知道`setTimeout()`不在 JS Engine 裡，而是一個 Web API，在 JavaScript 執行環境的過程中會先將第 1 行從 Call stack 移到 Web APIs Container，延遲 0 秒之後，將 callback function 傳到 Callback queue；此時第 3 行會被推進 Call stack，直接 `console.log('Good Morning')` 再推出，這時一直在工作的 **Event Loop** 會發現 Call stack 空了，就會通知 Callback queue 將 callback function 推進 Call stack，才會 `console.log('Hi')`。

其實，說 JavaScript 有非同步功能並不太精確，他本身依然秉持著單執行緒(single-threaded)的特性，而是依賴著 Web APIs Container 將 callback 增加到 callback queue 中，Event Loop 再伺機通知 queue 將 callback 增加到 stack 中。好神奇喔！

## 參考資料

### 影片參考

本篇文章主要參考 **What the heck is the event loop anyway?** | Philip Roberts | JSConf EU 這部影片內容，Philip 大大很完整的將整個 Event Loop 搭配例子講解了一回。為了讓自己更了解，才會產生這一篇筆記。聽說是業界必修，務必搭配服用！

[影片來源](https://youtu.be/8aGhZQkoFbQ)

### 閱讀資料

1. [Understanding Asynchronous JavaScript](https://blog.bitsrc.io/understanding-asynchronous-javascript-the-event-loop-74cd408419ff)
2. [How JavaScript works: an overview of the engine, the runtime, and the call stack](https://blog.sessionstack.com/how-does-javascript-actually-work-part-1-b0bacc073cf)
3. [[JavaScript] Javascript 的事件循環 (Event Loop)、事件佇列 (Event Queue)、事件堆疊 (Call Stack)：排隊](https://medium.com/itsems-frontend/javascript-event-loop-event-queue-call-stack-74a02fed5625)
