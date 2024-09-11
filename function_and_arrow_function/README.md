# 函式 & 箭頭函式
## 函式 (Function)
### Function的基本語法如下:  
1. 匿名函式
   ```
   function() {}
   ```

2. 有名稱的函式
   ```
   function foo() {}
   ```

function宣告有兩種方式  
1. 函式定義 (Function Declaration, FD)  
   一般功能函式的定義，通常用於在application中會重複用到的功能
   ```
   function sum(a, b) {
        return a + b
   }
   ```
   
2. 函式表達式 (Function Expression, FE)  
   較為簡潔(通常只會呼叫一次)
   ```
   const sum = function(a, b) {
        return a + b
   }
   ```
## 箭頭函式 (Arrow Function)
算是FE的簡寫語法，例子如下，下面這個例子等價於上面的FE  
```
const sum = (a, b) => a + b
```

用箭頭函式有以下好處:  
1. 語法簡單  
2. code可讀性提高，可以綁定詞法上的this

### 箭頭函式語法
語法大致如下  
```
(param1, param2, ... ) => {
    statements
}
```

箭頭函式有以下規則:  
1. 只有單行表達式且直接回傳運算結果才能省略 "{}"   
2. 有 "{}" 就要自己寫return，否則會回傳undefined
   ```
   const sumA = (a, b) => a + b
   const sumB = (a, b) => { a + b }

   sumA(1, 2)   //3
   sumB(1, 2)   //undefined
   ```
3. 只有在傳入單一參數時，可以省略 "()"   
   ```
   const funcA = x => x + 1     //ok
   ```

### this值的綁定
這邊直接看實際例子，用箭頭函式和一般函式的差別  

用箭頭函式:  
```
const obj = {a: 1}

function func() {
    setTimeout(() => {
        console.log(this.a) //this.a = 1
    }, 2000)
}

func.call(obj)
```
這裡'this'會以func中做為預設，因此func的上下文可以被呼叫到   
所以在call 'this.a' 的時候可以得到 1

將箭頭函式改為一般函式看看  
用一般函式:  
```
const obj = {a: 1}

function func() {
    setTimeout(function() {
        console.log(this.a) //this.a = undefined
    }, 2000)
}

func.call(obj)
```
這裡的'this'只能限定在func()中，所以這裡的'this.a' call不到obj的內容，所以會得到undefined

簡單總結this:  
* 一般函式的this是動態的，根據物件決定this能作用的範圍   
* 箭頭函式的this是詞法的(lexical)作用域決定的，也就是由周邊的作用域決定的

### 不能使用箭頭函式的情況
1. 物件字面文字定義物件時，物件中的方法
   因為箭頭函式的this會在定義時的上下文決定，因此this不是指向這個物件，例如
   ```
   const calculate = {
        array: [1, 2, 3],
        sum: () => {
            return this.array.reduce((result, item) => result + item)
        }
   }

   calculate.sum()
   ```
   上面這段會有TypeError，因為'this.array'是undefined  
   在定義物件的時候裡面也不是不能用箭頭函式，但不能用this (盡量不要在定義物件用箭頭函式)   

2. 在物件的prototype屬性中定義的方法  
   原因同第一點

3. DOM事件處理的監聽者 (事件函式處理)   
   ```
   const button = document.querySelector('button');

   button.addEventListener('click', () => {
        console.log(this); // `this` 指向全局物件或 undefined（在嚴格模式下）
   });
   ```
   應該使用一般function
   ```
   button.addEventListener('click', function () {
        console.log(this); // `this` 指向觸發事件的 DOM 元素（按鈕）
   });
   ```

4. 建構函式 (constructor)
   建構函式是使用new來建構物件時執行此函數  
   箭頭函式沒有constructor  
   ```
   const Message = (text) => {
        this.text = text
   }

   const helloMessage = new Message('Hello World!');
   ```

## 撰寫風格建議
1. callback (回調) 優先使用箭頭函式   
2. 箭頭函式如果只有單一參數時可以省略 "()" ，但建議無論什麼情況都加 "()"   
3. 避免合併使用箭頭函式和其他比較運算符(EX: >= , <=)   
4. 箭頭函式的flat arrow (=>) 前後都加一個空格
