# ES6 Learning Design Pattern
## Brief
《JavaScript 设计模式》读书笔记
## Constructor
ES6引入了类的概念，使得传统对象原型的写法变得更加清晰

Before ES6:
```javascript
function Stuff (name, age) {
    this.name = name;
    this.age = age;
}
Stuff.prototype.brief = function () {
    return 'name: ' + this.name + ', sex: ' + this.age;
};

// Usage:

var Lily = new Stuff('Lily', 20);
var Jack = new Stuff('Jack', 24);

console.log(Lily.brief());  // name: Lily , sex: 20
console.log(Jack.brief());  // name: Jack , sex: 24
```
ES6:
```javascript
class Stuff {
    constructor(name, age) {
    this.name = name;
    this.age = age;  
    }
    brief () {
        return 'name: ' + this.name + ', sex: ' + this.age;
    }
}

// Usage:

let Lily = new Stuff('Lily', 20);
let Jack = new Stuff('Jack', 24);

console.log(Lily.brief());  // name: Lily , sex: 20
console.log(Jack.brief());  // name: Jack , sex: 24
```
## Module
模块模式在JavaScript库或框架中得到广泛的运用,进一步模拟类的概念，实现单独的对象拥有公有/私有方法和变量，降低与其他脚本定义的函数的冲突可能性
```javascript
const complexModulePattern = (function () {
    // private
    let counter = 0;
    function print() {
        console.log(counter)
    }
    return {
        increment: function () {
            counter ++;
            print();
        },
        decrement: function () {
            counter --;
            print();
        }
    }
}) ();
// Usage:

complexModulePattern.increment(); // 1
```
## Revealing Module
通过定义一个匿名函数，返回一个公有指针指向私有的函数和属性，提高了可读性，在打补丁时候公有函数不能被覆盖
```javascript
let webWork = function () {
    let server = 'http://test.com';
    let browser = 'Chrome';

    function resourceSource () {
        return `fetching resource from ${server}`
    }

    function changeServer (str) {
        server = str
    }

    function getCurrentServer () {
        return resourceSource();
    }

    return {
        browser,
        changeServer,
        getCurrentServer
    }
}();
// Usage:
console.log(webWork.browser);   // Chrome
console.log(webWork.getCurrentServer());    // fetching resource from http://test.com 
webWork.changeServer('http://change_test.com');
console.log(webWork.getCurrentServer());    // fetching resource from http://change_test.com 
```
## Singleton
采用单例模式编写的类至多拥有一个实例
```javascript
let instance;
class SingleAdmin {
    constructor (name, uid) {

        if(!instance) {
            instance = this;
        }

        this.name = name;
        this.uid = uid;

        return instance
    }

    whoAmI(){
        console.log(this.name, this.uid);
    }
};

let user1 = new SingleAdmin('Alice', 2019);
let user2 = new SingleAdmin('Bob', 2020);
user1.whoAmI();     // Alice 2019
user2.whoAmI();     // Alice 2019
```
## Observer
Subject维护依赖于观察者的对象，将有关状态的变更通知给他们， `Subject（目标）`：维护一系列的观察者，可以进行观察者的添加与删除，`Observer（观察者）`：为目标状态发生改变时需要获得通知的对象提供更新接口，`ConcreteSubject（具体目标）`：状态发生改变的时候向 `Observer`发出通知，存储   `ConcreteObserver（具体观察者）`的状态存储一个指向`ConcreteSubject`的引用，实现`Observer`的    ` Update`接口，使得自身状态与目标一致

Subject:
```javascript
class ObserverList {
    constructor () {
        this.observerList = [];
    };
    add (obj) {
        this.observerList.push(obj)
    };
    remove (obj) {
        const removeIndex = this.observerList.findIndex(obs => {
            return observer === obs;
        });
        
        if (removeIndex !== -1) {
            this.observerList = this.observerList.slice(removeIndex, 1);
        }
    };
    empty () {
        this.observerList = []
    };
    count () {
        return this.observerList.length;
    }
    get (index) {
        if(index > -1 && index < this.observerList.length) {
            return this.observerList[index]
        }
    }
}

class Subject {
    constructor () {
        this.obervers = new ObserverList ();
    }
    addObserver (observer) {
        this.obervers.add(observer);
    };
    removeObserver (observer) {
        this.obervers.remove(observer);
    };
    notify(data) {
        if (this.observers.length > 0) {
          this.observers.forEach(observer => observer.update(data));
        }
      }
}
```
Observer:
```javascript
class Observer {
    constructor () {};
// need implement by concrete class
    update () {};
}
```
## Mediator
## Prototype
## Command
## Facade
## Factory
## Mixin
## Flyweight
## Composite
## Adapter
## Iterator
## Proxy
## Builder
