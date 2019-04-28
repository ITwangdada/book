# JavaScript基础

##Event Loop
1. 什么是Event Loop
    js的事件循环执行机制。
    js是单线程的，在执行上下文中会分为同步任务和异步任务。同步和异步任务会分别进入不同的“场所”。
    同步进入主线程，异步任务会进入Event Table并注册函数。
    当指定的事情完成，Event Table会将函数推进Event Queue。
    主线程内的任务执行完毕为空，会去Event Queue读取对应的函数，进入主线程执行。
    不断重复上述过程，称为Event Loop。
2. 异步任务有哪些
    异步任务分为两部分。macro-task（宏任务）、micro-task（微任务）。最新标准中，分别称为task/jobs。
    Macro-task大概包括： setTimeout、setInterval、setmmediate、I/O、UI rendering。
    Micro-task大概包括： process.nextTick、Promise、Object.obeserve(已废弃)、MutationObserver(html5新特性)。
3. 执行顺序
    首先会执行script整体代码，遇到异步任务时，会放到宏任务队列和微任务队列当中。
    当整体代码执行完成之后，会首先进入微任务队列，先进先执行，直到最后一个微任务结束完成，第一轮执行结束。
    再循环执行宏任务 -> 微任务。
    也就是说，script整体代码当中定义的setTimeout，会在第二轮开始执行。
4. setTimeout是如何执行的
    在js执行当中，会发现真正的延迟时间往往要比设置的延迟时间大。
    因为在setTimeout执行后function函数会进入Event Table并注册函数，计时开始。计时结束后，会将function函数推进Event Queue，但并未执行。
    会等到主线程中剩下的执行完成后，将从Event Queue把function函数推进主线程执行。

##原型链
1. 什么是原型
    将属性和方法定义在对象的构造函数的prototype属性上，并非实例对象本身，称为原型。
2. 什么是原型链
    通俗来说，就是找爸爸借钱的故事。我没有钱了，我会找我爸拿钱，我爸如果没有钱，会继续找我爸的爸爸拿钱。
    也就是说，每一个对象都拥有一个原型对象（[[Prototype]]）。对象与其原型为模板，从原型继承方法和属性。原型对象也可以拥有原型，并从中继承属性和方法，一层一层，最终指向null。这种关系称为原型链。
    __proto__ = [[Prototype]]
    __proto__属性在js中是浏览器所增加的，非w3c标准。在ES6时才被标准化，以确保web浏览器的兼容性，可以取到但不推荐使用。
    [[Prototype]]是对象的一个内部属性，外部代码无法直接访问。如果需要读取或修改对象[[Prototype]]属性，可以使用getPrototypeOf()/setPrototypeOf()
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

##js隐式转换规则
1. ToString
    * null: 转为"null"
    * undefined: 转为"undefined"
    * 布尔类型: true和false分别被转为"true"和"false"
    * 数字类型: 转为数字的字符串形式，如10转为"10",le21转为"le+21"
    * 数组：转为字符串是将所有元素按照","连接起来，相当于调用数组的Array.prototype.join()方法，如[1,2,3]转为"1,2,3"，空数组[]转为空字符串，数组中的null和undefined，会被当做空字符串处理。
    * 普通对象：转为字符串相当于直接使用Object.prototype.toString()，返回"[object Object]"

    ```
        String(null) // 'null'
        String(undefined) // 'undefined'
        String(true) // 'true'
        String(10) // '10'
        String(1e21) // '1e+21'
        String([1,2,3]) // '1,2,3'
        String([]) // ''
        String([null]) // ''
        String([1, undefined, 3]) // '1,,3'
        String({}) // '[object Objecr]'
    ```
2. ToNumber
    * null：转为0
    * undefined： 转为NaN
    * 字符串：如果是纯数字形式，则转为对应的数字，空字符转为0，否则一律按转换失败处理，转为NaN。
    * 布尔型：true和false被转为1和0
    * 数组：数组首先会被转为原始类型，也就是ToPrimitive，然后根据转换后的原始类型按照上面的规则处理。
    * 对象：同数组的处理

    ```
        Number(null) // 0
        Number(undefined) // NaN
        Number('10') // 10
        Number('10a') // NaN
        Number('') // 0 
        Number(true) // 1
        Number(false) // 0
        Number([]) // 0
        Number(['1']) // 1
        Number({}) // NaN
    ```

3. ToBoolean
    js中的假值只有false、null、undefined、空字符、0和NaN，其他值转为布尔型都为true

    ```
        Boolean(null) // false
        Boolean(undefined) // false
        Boolean('') // flase
        Boolean(NaN) // flase
        Boolean(0) // flase
        Boolean([]) // true
        Boolean({}) // true
        Boolean(Infinity) // true
    ```

4. ToPrimitive
    * 当对象类型需要被转为原始类型时，它会先查找对象的valueOf方法，如果valueOf方法返回原始类型的值，则ToPrimitive的结果就是这个值。
    * 如果valueOf不存在或者valueOf方法返回的不是原始类型的值，就会尝试调用对象的toString方法，也就是会遵循对象的ToString规则，饭后使用toString的返回值作为ToPrimitive的结果。

    如果valueOf和toString都没有返回原始类型的值，则会抛出异常。

    ```
    Number([])
    ```
    * Number([])，空数组会先调用valueOf，但是返回的是数组本身，不是原始类型，所以会继续调用toString，得到空字符串，相当于Number('')，所以转换后的结果为"0"
    ```
    const obj1 = {
        valueOf() {
            return 100
        }
        toString() {
            return 101
        }
    }
    ```
    * Number(obj1)，obj1的valueOf返回原始对象类型100，所以ToPrimitive的结果为100。

5. 宽松相等（==）比较时的隐式转换规则
    布尔类型和其他类型的相等比较
    * 只要布尔类型参与比较，该布尔类型的值首先会被转为数字类型。
    ```
        false == 0 // true
        true == 1 // true
        true == 2 // false
    ```
    数字类型和字符串类型的相等比较
    * 当数字类型和字符串类型做相等比较时，字符串类型会被转为数字类型。
    * 根据字符串的ToNumber规则，如果是纯数字形式的字符串，则转为对应的数字，空字符转为0，否则一律按转换失败处理，转为NaN。
    ```
        0 == '' // true
        1 == '1' // true
        1e21 == '1e21' // true
        Infinity == 'Infinity' // true
        true == '1' // true
        false == '0' // true
        false == '' // true
    ```
    对象类型和原始类型的相等比较
    * 当对象类型和原始类型做相等比较时，对象类型会依照ToPrimitive规则转换为原始类型。

## 类型判断
typeof
运算符后可跟着一个操作数，用于判断该操作数的数据类型；typeof适用于boolean,undefined,string,number,symbol。
判断一个数据是不是对象，却不能判断它是对象的哪个“子类型”，比如说不能判断是否为数组，是否为日期等。
typeof null  // object
typeof function(){} // function

instanceof
判断一个对象的“子类型”。判断一个对象在其原型链中是否存在一个构造函数的prototype属性。

constructor
constructor的值是对构造函数本身的引用，而不是函数名的字符串。constructor属性返回的对象的构造函数，基本类型指向对象的内置对象的构造函数。null和undefined没有构造函数。

Object.prototype.toString.call()
Object.prototype.toString.call(data)返回一个表示data的原型对象的字符串（如果data不是对象，会先转化为对象，null和undefined除外）

##ajax / jsonp原理

    ajax: 可以在不刷新页面的情况下获取数据并展示出来，不是一门编程语言也不是工具库，是由js实现的。

1. 如何实现：
    * 创建XMLHTTPRequest对象
    * 使用open方法设置和服务器的交互信息
    * 设置发送的数据，开始喝服务器端交互
    * 注册事件

get请求
```
    //创建异步对象
    var ajax = new XMLHttpRequest();
    // 设置请求的url参数，参数一是请求的类型，参数二是请求的url，可以带参数，动态的传递参数name到服务端， 参数三是规定对请求进行异步（true）或（false）处理
    ajax.open('get', 'url?name='+name);
    // 发送请求
    ajax.send()
    // 注册事件
    ajax.onreadystatechange = function () {
        if(ajax.readyState == 4 && ajax.status == 200) {
            //数据返回
        }
    }
```
post请求
```
    // 创建异步对象
    var ajax = new XMLHttpRequest();
    // 设置请求的url参数，参数一是请求的类型，参数二是请求的url，可以带参数，动态的传递参数name到服务端
    // post请求一定要添加请求头才行不然会报错
    ajax.setRequestHeader("Content-type","application/x-www-form-urlencoded");
    ajax.open('get', 'url?name='+name);
    // 发送请求
    ajax.send()
    // 注册事件
    ajax.onreadystatechange = function () {
        if(ajax.readyState == 4 && ajax.status == 200) {
            //数据返回
        }
    }
```

注释：

1. open(method, url, async)
```
    * method: 发送请求所使用的方法
    * url: 规定服务器端脚本的URL
    * async: 对请求进行异步(true)或同步(false)处理
```
2. send() 方法可将请求送往服务器。
3. onreadystatechange 存有处理服务器响应的函数，每当readyState改变时，onreadystatechange函数就会被执行。
4. readyState 存有服务器响应的状态信息。
    0: 请求未初始化（代理被创建，但尚未调用open方法）
    1: 服务器连接已建立(open已经被调用)
    2: 请求已接受(send方法已经被调用，并且头部状态已经可获得)
    3: 请求处理中(下载中，responseText属性已经包含部分数据)
    4: 请求已完成，且响应已就绪(下载操作已经完成)
5. responseText: 获得字符串形式的响应数据。
6. setRequestHeader() : POST传数据时，用来添加HTTP头，然后send(data)，注意data格式