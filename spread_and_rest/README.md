# 展開與其餘運算符
展開運算符(Sperad Operator)和其餘運算符(Rest Operator)是ES6的兩個新特性  
這邊簡單介紹一下特點:  
1. 符號都是 "..."  
2. 都是在陣列值運算   
3. Spread => 展開array中的值  
   Rest   => 集合其餘的值

## Spread operator
把一個array展開成個別的值的速寫  
展開可以用作shallow copy，例子如下:   
```
const arr1 = [1, 2, 3]
const arr2 = [...arr1]	//arr2 = [1, 2, 3]

arr2.push(4)		//不會影響arr1
```
還有把array like的物件轉成array的功用，如下以string為例
```
const a = "foo"
const a_char = [...a]	//["f", "o", "o"]
```

## Rest operator
其餘參數設計出來一個很明確的目的就是為了取代函式中的隱藏"偽陣列"物件arguments(暫時不介紹)  
把剩餘的值轉變成array，主要用途有兩個:  
1. 函式傳入參數  
   ```
   function sum(...number){
	const res = 0
	
	number.forEach(function (number){
		res += number
	})

	return res
   }

   sum(1)		//res = 1
   sum(1, 2, 3, 4, 5)	//res = 15
   ```
   若沒有實際傳入值，則會變成一個empty array "[]"
   ```
   function aFunc(x, ...y){
	console.log('x = ', x, 'y = ', y)
   }

   aFunc(1, 2, 3)	//x = 1, y = [2, 3]
   aFunc()		//x = undefined, y = []
   ```
2. 解構賦值  
   後面章節會講的更詳細，這邊先看例子，基本上和傳入參數用法差不多
   ```
   const [x, ...y] = [1, 2, 3]

   console.log(x)	//1
   console.log(y)	//[2, 3]
   ```
   也可以函式傳入中做解構賦值  
   ```
   function f(...[a, b, c]){
	return a + b + c
   }

   f(1)			//NaN (1 + undefined + undefined)
   f(1, 2, 3)		//6
   f(1, 2, 3, 4)	//6 (第四個傳入值沒有對應的變數，所以跳過)
   ```

\* 這邊要特別注意，不論是傳入參數或是解構賦值，***只能用一個其餘參數，且要放在最後一位***

## 撰寫風格建議
1. 不要用arguments，用其餘參數語法取代  
2. 不要在spread或rest operator後加空格
   ```
   ... number	//不建議
   ```
3. 用spread operator做array copy  
4. 用spread operator取代函式中的appy語法，做不定個數傳入  
