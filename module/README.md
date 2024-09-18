# 模組 (Module)
當application規模越來越大時，需要模組化的管理code的方式，能夠明確區分出每個獨立的作用域

ES6的模組化基本上有三個基本重點:  
1. ES6的模組code會自動變成strict mode  
2. ES6的模組分隔原則是一個檔案一個模組  
3. 要用"export"輸出和"import"匯入  

## Module name
這裡的module name是指路徑 + 檔名  
例如要匯入utils.js這個檔案，而且與你要import utils.js的檔案在同一目錄下  
```
import * from './utils'
```
若utils.js在components這個目錄下則如下
```
import * from './components/utils'
```

## Import & Export
### Export
要輸出的東西前面要有"export"這個關鍵字才能讓其他檔案匯入

export可以加在object, class, function, function\*, variable和const variable  
例如
```
//const variable
export const a = 'test'

//function
export function b() {
	console.log('test')
}

//object
export const c = {val: 1}

//class
export class d {
	constructor(name, age){
		this.name = name
		this.age = age
	}
}
```
如果只有單一輸出，通常會加上"default"這個關鍵字在檔案的最後或是要輸出的東西前  
加在最後範例如下  
```
function a(param) {
	return param * param
}

export default a
```
加在要輸出的東西前
```
export default function (param) {
	return param * param
}
```
\*var, let或const不能使用export default

### Import
要匯入module有兩種方式:  
1. 用 "{}" 表示要import的東西，但若是只有import一個東西則可以省略 "{}"  
   ```
   import {a, b, c} from './lib'
   import aFunction from './module1'
   ```
2. 用 "\*" 表示import所有東西，但是後面要加上一個自定義的name  
   ```
   import * as myModule from './lib'
   ```

## 撰寫風格建議
1. 不要使用"\*"來import  
2. 只在檔案的最上面import  
3. 不要export變數，class和function則要小心被re-assignment  
4. 如果只有單一輸出，優先使用"default"
