# 解構賦值 (destructuring assignment)
Destructuring assignment是JS的expression  
可以從array或object extract出資料，這個語法如同鏡子一樣，將array和object的內容一一對應  

destructuring assignment仍是一種assign運算

## Array destructuring
基本用法如下:  
```
//基本用法
const [a, b] = [1, 2]   //a = 1, b = 2

//先宣告後assign要用let
let a, b
[a, b] = [1, 2]

//略過某些值
const [a, , b] = [1, 2, 3]  //a = 1, b = 3

//多維複雜array
//a = 1, b = 2, c = [[3, 4], 5], d = 6
const [a, [b, [c, d]]] = [1, [2, [[[3, 4], 5], 6]]]
```
比較特別的點有3個:  
1. 可以用rest operator
   ```
   const [a, ...b] = [1, 2, 3]  //a = 1, b = [2, 3]
   ```

2. 可以用swap array中的值語法
   ```
   let a = 1, b = 2
   [b, a] = [a, b]  //a = 2, b = 1
   ```

3. 可以用string (因為string其實是char的array)
   ```
   //a = 'h', b = 'e', c = 'l', d = 'l', e = 'o'
   const [a, b, c, d, e] = 'hello'
   ```

## Object destructuring
object除了用 "{}" 定義外還有property，property也要對應
```
//基本用法
const {a: x} = {a: 5}   //x = 5

//property賦值
const {a: a, b: b} = {a: 5, b: 10}  //a = 5, b = 10

//上面的簡寫法
const {a, b} = {a: 5, b: 10}

//rest operator
//a = 1, b = 2, rest = {c: 3, d: 4}
const {a, b, ...rest} = {a: 1, b: 2, c: 3, d: 4}
```
但要注意，下面這個會syntax error
```
//syntax error
let a, b
{a, b} = {a: 1, b: 2}
```
原因是因為 "{}" 除了是object宣告的符號外，他還是區塊語句的符號，所以以上面這個例子被當成區塊而不是object

## Primitive data type destructuring
會有問題，因為destructuring assignment是為了獲取array or object的value，所以用於primitive data type，不是會有error就是獲得undefined

唯一例外如下:
```
const [c] = 'hello' //c = h
```
原因如同我們前面所說，string是char的array，所以在[c]中被c獲取了 "hello" 的h後面的被略過  
但這通常也不是我們期望的結果  

## Destructuring assignment的default value
可以先給default value，若沒有東西對應則會得到default value
```
const {x = 3} = {}
console.log(x) //x = 3
```

## Function傳入參數中的destructuring assignment
和前面其實沒什麼不同
```
function func({a = 3, b = 5}){
    return a + b
}

func({a: 1, b: 2})  //3
func({a: 1})    //6
func({b: 2})    //5
func({})    //8
func()  //TypeError
```
這邊可以看一下最後一個例子，因為他相當於傳了一個undefined進入function，如下  
```
let {a = 3, b = 5} = undefined
```
解法也很簡單，就是給傳入參數一個預設值
```
function func({a = 3, b = 5} = {}){
    return a + b
}

func()  //8
```
這樣最後一個就不會出錯  

這邊還有提一點，如果assign時給的是null會被當成0  
而傳入undefined則會變成預設值
```
function func({a = 3, b = 5} = {}){
    return a + b
}

func({a: null, b: 3})   //3
func({a: undefined, b: 3})  //6
```

## 撰寫風格建議
1. Destructuring assignment優先使用const  
2. Destructuring assignment時不要包含空樣式 (empty array or empty object)  
3. 函式傳入參數或回傳值中要做destructuring assignment時優先使用object
