# 類別 (Class)
在ES6之前，是透過傳統函式來模擬建構函式去作為class使用  
範例如下:  
```
function Person(name, age) {
  this.name = name;
  this.age = age;
}

Person.prototype.greet = function() {
  console.log(`Hello, my name is ${this.name} and I am ${this.age} years old.`);
};

// 創建新物件
const p1 = new Person('Jane', 25);
p1.greet(); // 輸出: Hello, my name is Jane and I am 25 years old.
```
上面這個方法是用一般的function去模擬class  

有一個地方要注意，在上面的中間，我們在person的prototype上加入了一個greet的function，我們其實也可以加在function內  
但這樣在每創建一個物件時就會複製一份greet function，所以在prototype上加function，這樣所有物件就能共享  

但老實說在學過其他class-based的programming language後，會覺得prototype-based的JS各種奇怪，要實現一些功能(例如: 繼承)也很麻煩  
而且現在有被普遍使用prototype-based的programming language只有JS而已

為了讓user使用上能更接近其他programming language，所以開發出了class的語法糖 

## Class
ES6的class實際上還是跟其他class-based的programming language不同，他一樣是基於prototype去模擬class，但因為有了語法糖，所以使用上跟class-based較為接近

下面開始看class的語法以及JS如何模擬class的功能

/* class的命名用PascalCase命名方式 (大家習慣)

### 語法
直接看範例:  
```
class Person{
    constructor(name, age){
        this.name = name
        this.age = age
    }

    greet(){
        console.log(`Hello, my name is ${this.name} and I am ${this.age} years old.`);
    }
}

// 創建一個新的 Person 物件
const p1 = new Person('John', 30);
p1.greet(); // 輸出: Hello, my name is John and I am 30 years old.
```
這邊注意創建一個物件一定要加 "***new***"  

### this
這邊拿c++和JS的this進行比較，兩者最大的差別，c++的this是固定的，而JS的this是根據上下文決定  

c++: this會永遠指向當前物件的實例，假設用c++創了一個class Person，並創了一個實例p1，則this會固定指在p1

JS: 根據上下文來去決定this的，大致可以分成三個情況:
1. function呼叫: 此時的this取決於function的上下文，有可能是function所在的window，甚至是全域

2. constuctor呼叫: 此時的this指向新建立的實例

3. object呼叫method: 此時的this是指向此object的實例

### 建構式 (constructor)
跟c++的用法大同小異，但JS的constructor不具polmorphism或overriding  
JS和c++一樣都有預設的constructor，如果constroctor沒有要額外做什麼事，就不要寫constructor

### 私有成員 (private)
JS沒有分private, protected和public，全部都是public  

大家的習慣是在member前面加入 "_" 來代表private (例如: this._id)  
也有人用constructor來實作private，因為constructor有自己的作用域，但不建議

### getter與setter
JS利用get和set分別實現getter和setter的功能  
其實在JS中的一般property是不需要get和set就可以直接存取，但是用get和set有一些好處 (我的理解像是把property包裝像是function從而實現更多功能)

1. 使用 get 和 set，可以在存取屬性時進行額外的檢查或資料驗證
```
class Person {
  constructor(name, age) {
    this._name = name;
    this._age = age;  // 使用私有變數存儲
  }

 // Getter
  get age() {
    return this._age;
  }

  // Setter
  set age(value) {
    if (value < 0) {
      console.log("Age must be a positive number");
    } else {
      this._age = value;
    }
  }
}

const person1 = new Person("Alice", 30);
console.log(person1.age); // 30

person1.age = -5; // 輸出 "Age must be a positive number"
console.log(person1.age); // 還是 30
```

2. 可以避免property直接暴露在外
```
class Rectangle {
  constructor(width, height) {
    this.width = width;
    this.height = height;
  }
  
  // Getter 讓面積可讀但不可直接修改
  get area() {
    return this.width * this.height;
  }
}


const rect = new Rectangle(5, 10);
console.log(rect.area); // 50
rect.area = 100; // 嘗試直接修改，無效
console.log(rect.area); // 仍然是 50，因為面積是由 width 和 height 決定的
```

### 靜態成員 (static)
基本上和c++大同小異，static都是屬於class不屬於實例，都需要通過class來訪問static member，只有一些小小的使用上區別

### 繼承 (inherant)
在JS中用 "***extend***" 這個關鍵字來用作class的繼承，範例如下:  
```
// 基礎類別
class Animal {
  constructor(name) {
    this.name = name;
  }

  speak() {
    console.log(`${this.name} makes a sound.`);
  }
}

// 繼承自 Animal 的子類別
class Dog extends Animal {
  constructor(name, breed) {
    super(name);  // 調用基礎類別的 constructor
    this.breed = breed;
  }
  
    // 重寫基礎類別的方法
  speak() {
    console.log(`${this.name} barks.`);
  }

  // 新增的方法
  bark() {
    console.log(`${this.name} is barking.`);
  }
}

// 使用範例
const myDog = new Dog('Rex', 'German Shepherd');

myDog.speak();  // 輸出: Rex barks.
myDog.bark();   // 輸出: Rex is barking.
```

繼承有幾點要注意:  
1. 在constructor會多呼叫一個super，用於執行parent的consturctor，後續也指向parent，且super要放在constructor的第一行

2. 若child的property和method會蓋掉原本的parent的同名稱的property和method，若要呼叫parent的member要用super

3. JS不支持多class的繼承，只能從單一class繼承
