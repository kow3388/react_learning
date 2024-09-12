# 物件增強語法和方法
物件類型以屬性(properties)和方法(methods)為主要組成  
* Properties: 可被描述的量化資料，例如: 人類的年齡  
* Methods: 物件可被反應的action，例如: 人類簽名  

## 物件字面語法 (Object Literals)
在JS中用於定義物件的語法，簡單介紹一下語法和特點:    
* 用 "{}" 作為區塊  
* 用 "."來存取物件的成員 (properties & methods)  
* 若給予不存在的成員(properties & methods)，則會自動擴充 (但建議是先定義好)

Object literal 範例:  
```
const aObj = {
    firstKey: 'foo'
    secondKey: 'bar'
}

bObj.thirdKey = 'yes'

console.log(bObj.firstKey)  //foo
console.log(bObj.thirdKey)  //yes
```

補充:  
* 其實可以用 "new" 來創建一個空的object，但不建議  
* object可以用array的語法來存取成員(object[prop])  
  但很少用，只有在特殊情況才會使用，例如: 動態屬性名稱  
* 基於object literal發展出JSON (JavaScript Object Notation)的資料定義格式

## ES6中的物件語法增強
### 屬性初始設定簡寫 (Property Initializer shorthand)
也有人稱作屬性值簡寫 (property value shorthand)  

Object literal可以用變數或常數當鍵值，且value為此變數或常數  
常用在fuction return object和object literal定義新object  
範例如下:
```
//正常寫法 (function return object)
function foo(a, b) {
    return {result: 'success', a: a, b: b}
}

//簡寫 (function return object)
function foo(a, b) {
    return {result: 'success', a, b}
}

//正常寫法 (define new object)
const a = 'success', b: 42, c = {}
const o = {a: a, b: b, c: c}

//簡寫 (define new object)
const a = 'success', b: 42, c = {}
const o = {a, b, c}
```

### 方法定義 (Method definitions)
object leteral定義方法時可以不用寫"function"這個關鍵字  
```
//一般寫法
const player = {
    fullName: 'Eddy',
    sayName: function(){
        console.log(this.fullName)
    }
}

//簡寫
const player = {
    fullName: 'Eddy',
    sayName(){
        console.log(this.fullName)
    }
}
```

\* 如果要在object leteral內中定義generator則不能少"function"關鍵字

### 計算得出屬性名稱 (Computed property names)
動態產生屬性名稱，範例如下:  
```
const prop = 'foo'

const o = {
    [prop]: 'hey',
    ['b' + 'ar']: 'there'
}

console.log(o)  //Object {foo: "hey", bar: "there"}
```
但用動態產生的屬性名，不能用前面提到property initializer shorthand
```
const prop = 'foo'

//錯誤
const o = {
    [prop]
}
```

### Object.assign
ES6的新方法，用於merge，mixin和shallow copy object  
```
//merge
const o1 = {a: 1}
const o2 = {b: 2}

const obj = Object.assign({}, o1, o2)
console.log(obj)    //Object {a: 1, b: 2}

//mixin，後面的會蓋掉前面
const o1 = {a: 1, b: 1}
const o2 = {b: 2}

const obj = Object.assign({}, o1, o2)
console.log(obj)    //Object {a: 1, b: 2}

//shallow copy
const o1 = {a: 1}
const obj = Object.assign({}, o1)
console.log(obj)    //Object {a: 1}
```
