# JavaScript基础

##Event Loop
1. 什么是Event Loop
```
    js的事件循环执行机制。
    js是单线程的，在执行上下文中会分为同步任务和异步任务。同步和异步任务会分别进入不同的“场所”。
    同步进入主线程，异步任务会进入Event Table并注册函数。
    当指定的事情完成，Event Table会将函数推进Event Queue。
    主线程内的任务执行完毕为空，会去Event Queue读取对应的函数，进入主线程执行。
    不断重复上述过程，称为Event Loop。
```
2. 异步任务有哪些
```
    异步任务分为两部分。macro-task（宏任务）、micro-task（微任务）。最新标准中，分别称为task/jobs。
    Macro-task大概包括： setTimeout、setInterval、setmmediate、I/O、UI rendering。
    Micro-task大概包括： process.nextTick、Promise、Object.obeserve(已废弃)、MutationObserver(html5新特性)。
```
3. 执行顺序
```
    首先会执行script整体代码，遇到异步任务时，会放到宏任务队列和微任务队列当中。
    当整体代码执行完成之后，会首先进入微任务队列，先进先执行，直到最后一个微任务结束完成，第一轮执行结束。
    再循环执行宏任务 -> 微任务。
    也就是说，script整体代码当中定义的setTimeout，会在第二轮开始执行。
```
4. setTimeout是如何执行的
```
    在js执行当中，会发现真正的延迟时间往往要比设置的延迟时间大。
    因为在setTimeout执行后function函数会进入Event Table并注册函数，计时开始。计时结束后，会将function函数推进Event Queue，但并未执行。
    会等到主线程中剩下的执行完成后，将从Event Queue把function函数推进主线程执行。
```

##原型链
1. 什么是原型
```
    将属性和方法定义在对象的构造函数的prototype属性上，并非实例对象本身，称为原型。
```
2. 什么是原型链
```
    通俗来说，就是找爸爸借钱的故事。我没有钱了，我会找我爸拿钱，我爸如果没有钱，会继续找我爸的爸爸拿钱。
    也就是说，每一个对象都拥有一个原型对象（[[Prototype]]）。对象与其原型为模板，从原型继承方法和属性。原型对象也可以拥有原型，并从中继承属性和方法，一层一层，最终指向null。这种关系称为原型链。
    __proto__ = [[Prototype]]
    __proto__属性在js中是浏览器所增加的，非w3c标准。在ES6时才被标准化，以确保web浏览器的兼容性，可以取到但不推荐使用。
    [[Prototype]]是对象的一个内部属性，外部代码无法直接访问。如果需要读取或修改对象[[Prototype]]属性，可以使用getPrototypeOf()/setPrototypeOf()
```
3. 如何继承自身的constructor和父级的原型。
```
    不规范的方式：
    function Parent() {
        this.name = "我爸";
    }
    function Child() {
        this.name = "我";
    }
    Child.prototype = new Parent();
    var c = new Child();
    这样会导致实例对象c本身没有constructor属性，是通过原型链向上查找__proto__，最终查找到constructor属性，该属性指向Parent。

    优秀的方式：
    function Parent() {
        this.name = "我爸";
    }
    function Child() {
        this.name = "我";
    }
    Child.prototyoe = Object.create(Parent.prototype);
    var c = new Child()
    c对象继承Child的construtor以及继承Parent构造函数的prototype。
```
4. Object.create做了什么事
```
    function create(proto, options) {
        // 创建一个空对象
        var tmp = {};

        // 让这个新的空对象成为父类对象的实例
        tmp.__proto__ = proto;

        // 传入的方法都挂载到新对象上，新的对象将作为子类对象的原型
        Object.defineProperties(tmp, options);
        return tmp;
    }
```