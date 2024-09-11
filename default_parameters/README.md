# 傳入預設值 (Default Parameters)
parameter和argument的差異:  
1. parameter: 傳入的參數名稱定義

2. argument: 實際傳入的參數值

參數在未實際傳入時，一定是undefined

## ES6出現前的default parameters
在ES6出現前，default parameters是用 "||" (logical or) 或是typeof判斷實作  
用 "||" :  
```
const link = function (point, url){
    let point = point || 10
    let url = url || 'http://google.com'
}
```
用typeof:  
```
const link = function (point, url){
    let point = typeof point !== 'undefined' point : 10
    let url = typeof url !== undefined : 'http://google.com'
}
```

這裡要先提一下 "||"，因為實際上JS的logical OR和其他programming language不一樣  
* JS: "a || b" 表示當a為 "falsy" 時回傳b  
* Other programming language: "a || b" 會回傳bool值

\* falsy: 會導致回傳false的那些值稱為falsy，例如: 0, null, ...

用傳統default parameters的問題:  
1. 無法判斷object，例如: {}, []  
2. 0和null是falsy，但有時希望預設值是0或null

\* 補充: "&&" (logical AND)和 "||" 相反，"a && b" 若a為falsy則return a

## ES6的default parameters
用法和大多數的programming language一樣  
但有些情況要注意:  
1. TDZ (Temportal Dead Zone, 暫時死區)
   在default parameter中若有位初始化的parameter，會進入TDZ而產生錯誤
   ```
   function foo(x = y, y = 1){
        console.log(y)
   }

   foo(1)               //ok
   foo(undefined, 1)    //ReferenceError: y is not defined
   foo()                //ReferenceError: y is not defined
   ```

2. this
   ```
   function foo(x = this, y = this.value){
        console.log(x)
        console.log(y)
   }

   foo()                        //1
   foo({value: '= =+'})         //2
   foo.call({value: '^^y~'})    //3
   ```
   第1個: this會指到window或是全域物件，y沒這屬性存在，所以是undefined  
   第2個: x給了一個物件，y沒這屬性，所以一樣是undefined  
   第3個: 用了call，所以這個函式有裡面的物件，x是物件，y是 "^^y~"

3. 必要的default parameters  
   有兩個方法:  
   法1
   ```
   function func1(x){
        if(x === undefined)
            throw new Error('Missing parameter')
        
        //...
   }
   ```
   法2  
   ```
   function throwIfMissing() {
        throw new Error('Missing parameter')
   }

   function func1(x = throwIfMissing()) {
        //...
   }
   ```

4. default parameters沒有位置限制  
   以下情況是ok的  
   ```
   function func1(opts = {}, name) {
        //...
   }
   ```
   雖然不會報錯，但不建議這樣寫

5. Arrow function也可以有default parameters  
   ```
   const funct = (x = 100) => x*2
   ```

6. 可以用解構賦值  
   ```
   function func({a, b} = {a: 1, b: 2}) {
        console.log(a, b)
   }
   ```

## 撰寫風格建議
1. 避免default parameter修改到其他共享變數  
2. 有default值的parameter要放在沒有default的後面  
3. 不要re-assign參數值
