# Data type, let & const

## Data type
JavaScript中的資料型態大致分為以下7種:
1. number  
2. string  
3. boolean  
4. null  
5. undefined
6. symbol
7. object

其中1. ~ 5.為primitive data type(原始資料型態)，6. 為進階data type(暫不討論)，7. 為複合型  

JavaScript是loosely typed的程式語言，因此不用一開始指定data type(和python類似)
```
let foo = 42 //foo is Number
```

## typeof
用來判斷data type的，但其實在判斷複合型的data type是有些不足之處  
以下是typeof的例子
```
typeof 37   		//number
typeof ''   		//string
typeof true 		//boolean
typeof {}   		//object
typeof []   		//object
typeof null 		//object
typeof function() {}    //function
```
從上面的例子我們可以看到，typeof有兩個例外:  
1. null會被判定成object (null是primitive data type)   
2. function會被判定成function (function是object的一種)

而且在複合型態的data type中，typeof只會回傳object，因此需要其他方式來判斷

## let & const
let & const用來取代var ***(不要在用var了)***

var是funciton scope，let和const是block scope  
這邊不多解釋為什麼不要用var

### const
const是指固定值的變數(一開始就要指定好值得變數，且不能re-assignment)
```
const a = 10
a = 20		//TypeError
```
但不代表const這個reference的值是不可改變的
```
const a = []
a[0] = 1	//ok
```

### let
let是一般變數和const差不多，但差在可以敗re-assignment

## 撰寫風格建議
1. 不要在用var，用let & const，且盡量用const  
2. 不要用 "," 在一行宣告多個變數，一行一行宣告  
   ```
   let a = 1, b = 2	//建議不要
   ```
3. 宣告變數在第一次使用的前一行 (和其他language一樣)
