### 引言
 最初接触es6class是react全面向es6兼容的时候，那时候class类属性跟方法的写法已经this指针的指向弄得焦头烂额，后来学会了用箭头函数去绑定this，开始渐渐解决了this的问题。随着react熟练度的增加，对类的'熟练度'看似增加了，实际还停留在不懂的阶段，这次趁重学前端的机会，扎扎实实的去看待一下这个有用的语法糖。
### 理理常见class的常见用法

```
class Person {
    constructor(name) {    // 1
        console.log(this)
        this.name = name
    }
    
    static state={}        // 2
    
    state1={}              // 3
    
    sayname() {            // 4
        console.log(this)
    }
    
    sayname2 = () => {          // 5
        console.log(this)
    }
    
    static sayname3() {         // 6
        console.log(this)
    }
    
    get html() {                // 7
        console.log(this)
        return 'html'
    }
    
    set html(name) {            // 8
        console.log(this)
        this.name = name
    }
}

const person = new Person('Jim')
```
es6的class是可以看作只是一个语法糖，它的大部分功能都能用es5语法重写。我们平时用到的类的写法大概有以上几种，那上面的类转化成es5的语法要怎么写？我们从两个方面来理解：

1. 从类属性或者方法归属来看：
 >
 - 1 很好理解代表类的构造函数
 - 2 静态方法state，通过Person.state 访问
 - 3 state1，是this.state1={} 的简写，es7的语法，是实例person的私有属性，通过person.state1访问
 - 4 sayname相当于Person.prototype.sayname,挂在Person的原型链上，可通过Person.prototype.sayname或者person.sayname访问
 - 5 sayname2同3，相当于this.sayname2=()=>{},属于实例的私有属性
 - 6 sayname3同2，Person的静态属性通过Person.sayname3 访问
 - 7.8 访问器属性get/set ，挂在Person.prototype上，可通过Person.prototype.sayname或者person.sayname访问
 
2. 从访问内部this指针来看
> 
- 1 constructor里的this在实例初始化的时候生成，指向实例
- 4 sayname相当于Person.prototype.sayname(),指向Person.prototype，person.sayname()实例调用时指向person
- 5 sayname2相当于this.sayname2,所以只能在实例中调用，this指向person
- 6 sayname3是静态属性，Person.sayname3(),自然指向的是父类Person
- 7/8 同4，定义在prototype上
- 
3. 最后我们用es5的语法来实现以下以上的Person
4. 
```
let Person = (function(){
    'use strict'
    // constructor
    const Person = function(name) {
        // 元属性new.target确保是通过new调用的
        if(typeof new.target === 'undefined') {    
            throw new Error('必须通过new关键字调用')
        }

        this.name = name
        this.state1 = {}
        this.sayname2 = (function() {}).bind(this)
    }
    
    // static state
    Person.state = {} 
    // static sayname3
    Person.sayname3 = function() {}
    // sayname
    Object.defineProperties(Person.prototype, 'sayname', {
        value: function() {
            // 元属性new.target确保是通过new调用的
            if(typeof new.target === 'undefined') {    
                throw new Error('必须通过new关键字调用')
            }
            console.log(this)
        },
        enumerable: false,
        writable: true,
        configurable: true
    })
    // html set
    Object.defineProperties(Person.prototype, 'html', {
        get: function() {
            console.log(this)
            return 'html'
        },
        set: function(name) {
            console.log(this)
            this.name = name
        },
        enumerable: false,
        configurable: true
    })
})()
```
