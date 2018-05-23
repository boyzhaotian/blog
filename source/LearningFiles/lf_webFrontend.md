title: "学习资料 - 前端"
date: 2018-05-08 18:29:58 +0800
update: 2018-05-23 12:40:00 +0800
author: me
cover: "-/images/example-en.png"
tags:
    - 学习
    - 前端
preview: LearningFiles 系列笔记 lf_webFrontend 前端笔记。欢迎大家参阅并提出宝贵意见。Github 仓库传送门：[前端基础知识汇总.md](前端基础知识汇总.md)

---

# 变量类型和计算
    JS 中使用 typeof 能得到哪些类型
    何时使用 === 何时使用 == 
    JS 中有哪些内置函数
    JS 变量按照存储方式区分为哪些类型，并描述特点
    如何理解JSON

## 变量类型
- 值类型 vs 引用类型
    ```js
    // 值类型
    var a = 100
    var b = a
    a = 200
    console.log(b)  // 100

    // 引用类型 
    // 对象、数组、函数，都具有对象特性，可无限扩展属性
    var a = {age:20}
    var b = a
    b.age = 21
    console.log(a.age)  // 21
    ```
- typeof 运算符详解
    ```js
    typeof undefined    // undefined
    typeof 'abc'    // string
    typeof 123    // number
    typeof true    // boolean
    // type 只能区分值类型
    typeof {}    // object
    typeof []    // object
    typeof null    // object
    // 不能区分引用类型
    typeof console.log()    // function
    ```
## 变量计算

- 强制类型转换
    - 字符串拼接
        ```js
        var a = 100 + 10    // 110
        var b = 100 + '10'  // '10010'
        ```
    - == 运算符 （ 判断值相等 ）
        ```js
        100 == '100'    // true
        0 == ''  // true
        null == undefined   //true
        ```
    - if 语句
        ```js
        var a = true
        if (a) {    // true
            // ...
        }
        var b = 100
        if (b) {    // true
            // ...
        }
        var c = ''
        if (c) {    // false
            // ...
        }
        ```
    - 逻辑运算
        ```js
        console.log(10 && 0)   // 0
        console.log('' && 'abc')   // 'abc'
        console.log(!window.abc)   // true

        // 判断一个变量会被当做 true 还是 false
        var a = 100
        console.log(!!a)   // true
        ```
> if中强制转换成false的有：0、''、NaN、null、false

### 何时使用 === 和 ==
```js
// 除了下面情况，全部使用 === 判断相等
if (obj.a == null) {
    // 这里相当于 obj.a === null || obj.a === undefined ，简写形式
    // 这是 jquery 源码中推荐的写法
}
```
> 看对象属性是否存在的时候用 == null（对象属性不存在不会报错）


### JS中的内置函数 —— 数据封装类对象
```js
Function
Array
Object
Date
RegExp
Boolean
Number
Error
```

### 内置对象：Math、JSON（两个API）
```js
// JSON 是一种数据格式 也是 JS 对象
JSON.stringify({a:10, b:20})
JSON.parse('{"a":10,"b":20}')
```


# 原型链
    如何准确判断一个变量是数组类型
    写一个原型链继承的例子
    描述 new 一个对象的过程
    zepto（ 或其他框架 ）代码中如何使用原型链
## 构造函数
```js
function Foo(name, age) {
    this.name = name
    this.age = age
    this.class = 'class-1'
    // return this  // 默认有这行
}
var f = new Foo('zhangsan', 20)
//var f = new Foo('lisi', 22)   // 创建多个对象
```
- new 的过程 this  
    ```js
    // 生成空对象 this ，添加属性，返回 this
    function Foo(name, age) {
        // this = {}
        this.age = age
        // return this  
    }
    ```
## 构造函数 - 扩展
- var a = {} 其实是 var a = new Object() 的语法糖
- var a = [] 其实是 var a = new Array() 的语法糖
- functino Foo(){...} 其实是 var Foo = new Function(...) 的语法糖
- 使用 instanceof 判断一个函数是否是一个变量的构造函数
## 原型规则和示例

- 所有的引用类型（数组、对象、函数），都具有对象是特性，即可以自由扩展属性（除了"null"以外）
    ```js
    var obj = {}; obj.a = 100;
    var arr = []; arr.a = 100;
    function fn () {}
    fn.a = 100;
    ```
- 所有的引用类型（数组、对象、函数），都有一个隐式原型__proto__属性，属性是一个普通对象。
    ```js
    console.log(obj.__proto__);
    console.log(arr.__proto__);
    console.log(fn.__proto__);
    ```
- 所有的函数，都有一个显式原型protoype属性，属性值也是一个普通对象。
    ```js
    console.log(fn.prototype);
    ```
- 所有的引用类型（数组、对象、函数），隐式原型__proto__属性指向它的构造函数的显式原型prototype属性。
    ```js
    console.log(obj.__proto__ === Object.prototype);
    ```
- 当试图得到一个对象的某个属性值时，如果这个对象本身没有这个属性，那么会去他的__proto__（即它的构造函数的prototype）中去找。
    ```js
    function Foo(name, age) {
        this.name = name
    }
    Foo.prototype.alertName = function () {
        alert(this.name)
    }
    // 创建实例
    var f = new Foo('zhangsan')
    f.printName = function () {
        console.log(this.log)
    }
    // 测试
    f.printName()
    f.alertName()
    ```
### 循环对象的自身属性
```js
var item
for (item in f) {
    // 高级浏览器已经在 for ... in 中屏蔽了来自原型的属性
    // 但是建议大家还是加上这个判断，保证程序的健壮性
    if (f.hasOwnProperty(item)) {
        console.log(item)
    }
}
```

## 原型链
<!-- ![image](https://boyzhaotian.netlify.com/原型链.png) -->
```js
function Foo(name, age) {
    this.name = name
}
Foo.prototype.alertName = function () {
    alert(this.name)
}
// 创建实例
var f = new Foo('zhangsan')
f.printName = function () {
    console.log(this.log)
}
// 测试
f.printName()
f.alertName()
f.toString()    // 要去 f.__proto__.__proto__中查找
```
> 原型链为防止死循环： Object.prototype.\_\_proto__ === null  

## instanceof
- f instanceof Foo 的判断逻辑是
    - f 的 __proto__ 一层一层往上，能否对应到 Foo.prototype
    - 再试着判断 f instanceof Object

> f instanceof Foo、Object都为true

## 解答
- 如何准确判断一个变量是数组类型？
    ```js
    var arr = []
    arr instanceof Array    // true
    typeof arr  // object，typeof 是无法判断是否是数组的
    ```
- 写一个原型链继承的例子
    ```js
    function Elem(id) {
        this.elem = document.getElementById(id)
    }
    Elem.prototype.html = function (val) {
        var elem = this.elem
        if (val) {
            elem.innerHTML = val
            return this //链式操作
        } else {
            return elem.innerHTML
        }
    }
    Elem.prototype.on = function (type, fn) {
        var elem = this.elem
        elem.addEventListener(type, fn)
        return this
    }
    var div1 = new Elem('div1')
    div1.html('<p>hello</p>').on('click', function(){
        console.log('clicked')
    }).html('<p>javascript</p>')
    ```
- 描述 new 一个对象的过程
    - 创建一个新对象
    - this 指向这个新对象
    - 执行代码，即对 this 赋值
    - 返回 this
- zepto（ 或其他框架 ）源码中如何使用原型链
    - 阅读源码是高效提高技能的方式
    - 但不能『埋头苦钻』有技巧在其中
    - 慕课网搜索『zeoto设计和源码分析』


# 作用域（Scope）和闭包（Closure）

    说一下对变量提升的理解
    说明 this 几种不同的使用场景
    创建 10 个 <a> 标签，点击的时候弹出来对应的序号
    如何理解作用域
    实际开发中闭包的应用

## 执行上下文
- 执行上下文深入理解
    
    [深入理解javascript原型和闭包（8）——简述【执行上下文】上](https://www.cnblogs.com/wangfupeng1988/p/3986420.html )
 
- 执行上下文和作用域的关系  
    在一个函数被执行时，创建的执行上下文对象除了保存了些代码执行的信息，还会把当前的作用域保存在执行上下文中。所以它们的关系只是存储关系。

```js
console.log(a)  // undefined
var a = 100

fn('zhangsan')  // 'zhangsan' 20
function fn (name) {
    age = 20
    console.log(name, age)
    var age
}
```
- 范围：一段\<script>或者一个函数  
- 全局：变量定义、函数声明（一段\<script>）  
- 函数：变量定义、函数声明、this赋值、arguments（函数）  
> 注意函数声明和函数表达式的区别

## THIS
- this 指向要在执行时才能确认值，定义时无法确认
    ```js
    var a = {
        name: 'A',
        fn: function () {
            console.log(this.name);
        }
    }
    a.fn()  // this === a
    a.fn.call({name: 'B'})  // this === {name: 'B'}
    var fn1 = a.fn
    fn1() // this === window
    ```
- 使用场景
    - 作为构造函数执行 -> 实例
    - 作为对象属性执行 -> 引用者
    - 作为普通函数执行 -> window
    - call、apply、bind -> 传入的第一个参数

## 作用域（Scope）
- JS没有块级作用域
    ```js
    // 无块级作用域
    if (true) {
        var name = 'zhangsan'
    }
    console.log(name);  // zhangsan
    ```
- 只有函数和全局作用域
    ```js
    // 函数和全局作用域
    var a = 100
    function fn() {
        var a = 200
        console.log('fn', a);
    }
    console.log('global', a);   // global 100
    fn()    // fn 200
    ```

## 作用域链
- 即自由变量的查找路径  
- 函数的父级作用域根据定义的位置来判断，并非执行位置。
### 自由变量  
```js
var a = 100
function F1() {
    var b = 200
    function F2() {
        var c = 300
        // 当前作用域没有定义的变量，即"自由变量"
        console.log(a); // a 是自由变量
        console.log(b); // b 是自由变量
        console.log(c);
    }
    F2()
}
F1()
```


## 闭包
- 使用场景
    - 函数作为返回值
        ```js
        function F1() {
            var a = 100
            // 返回一个函数（函数作为返回值）
            return funtion () {
                console.log(a)  // 自由变量，父作用域寻找
            }
        }
        // f1 得到一个函数
        var f1 = F1()
        var a = 200
        fn()    // 100
        ```
    - 函数作为参数传递  
        ```js
        function F1 () {
            var a = 100
            // 返回一个函数（函数作为返回值）
            return funtion () {
                console.log(a)  // 自由变量，父作用域寻找
            }
        }
        var f1 = F1()
        function F2（fn）{
            var a = 200
            fn()
        }
        F2(f1)
        ```
## 解题
- 说一下对变量提升的理解
    - 变量定义
    - 函数声明（ 注意和函数表达式的区别 ）
- 说明 this 几种不同的使用场景
    - 作为构造函数执行 -> 实例
    - 作为对象属性执行 -> 引用者
    - 作为普通函数执行 -> window
    - call、apply、bind -> 传入的第一个参数
- 创建 10 个 \<a> 标签，点击的时候弹出来对应的序号
    ```js
    var i, frag = document.createDocumentFragment()
    for (i = 0; i < 10; i++) {
        (function (i) {
            var a = document.createElement('a')
            a.innerHTML = i + '<br>'
            a.addEventListener('click', function (e) {
                e.preventDefault()
                alert(i)
            })
            frag.appendChild(a)
        })(i)
    }
    document.body.appendChild(frag)
    ```
- 如何理解作用域
    - 自由变量
    - 作用域链，即自由变量的查找
    - 闭包的两个场景
- 实际开发中的闭包应用
    - 封装变量
    - 收敛权限
    ```js
    function isFirstLoad() {
        var _list = []
        return function (id) {
            if (_list.indexOf(id) >= 0) {
                return false
            } else {
                _list.push(id)
                return true
            }
        }
    }
    var firstLoad = isFirstLoad()
    firstLoad(10) //true
    firstLoad(10) //false
    firstLoad(20) //true
    firstLoad(20) //false
    // 在 isFirstLoad 函数外面，根本不可能修改掉 _list 的值
    ```


# 异步和单线程  
    同步和异步的区别是什么？分别举一个同步和异步的例子  
    一个关于 setTimeout 的笔试题
    前端使用异步的场景有哪些
- 什么是异步（ 对比同步 ）
    ```js
    console.log(100)
    setTimeout(function () {
        console.log(200)
    }, 1000)    // 异步
    alert(300)  // 同步阻塞
    console.log(400)
    ```
    - 何时需要异步
        - 在可能发生等待的情况
        - 等待过程中不能像 alert 一样阻塞程序运行
        - 因此，所有的『等待的情况』都需要异步
- 前端使用异步的场景
    - 定时任务：setTimeout，setInterval  
    ```js
    setTimeout(function () {

    }, 0)
    ```

    - 网络请求：ajax请求，动态\<img>加载  
    ```js
    $.get('/test.json', function (data) {
        console.log(data)
    })
    var img = document.createElement('img')
    img.onload = function () {
        console.log('loaded')
    }
    ```
    - 事件绑定
    ```js
    var btn1 = document.getElementById('btn1')
    btn1.addEventListener('click', function (e) {
        console.log('clicked')
    })
    ```
## 解题
- 同步和异步的区别是什么？分别举一个同步和异步的例子  
    - 同步会阻塞代码执行，而异步不会
    - alert 是同步的，setTimeout 是异步的
- 一个关于 setTimeout 的笔试题

- 前端使用异步的场景有哪些
    - 定时任务：setTimeout，setInterval  
    - 网络请求：ajax请求，动态\<img>加载  
    - 事件绑定
# 日期和Math
    获取 2017-06-10 格式的日期
    获取随机数，要求是长度已知的字符串格式
    写一个能遍历对象和数组的通用 forEach 函数
## 日期
```js
Date.now()  //获取当前时间毫秒数
var dt = new Date()
dt.getTime()    //获取毫秒数
dt.getFullYear()    //年
dt.getMonth()    //月（0 - 11）
dt.getDate()    //日（0 - 31）
dt.getHours()    //小时（0 - 23）
dt.getMinutes()    //分（0 - 59）
dt.getSeconds()    //秒（0 - 59）
```
## Math
- 获取随机数 Math.random() -> [0-1]

# 数组和对象的API  
## 数组
- forEach 遍历所有元素  
    ```js
    var arr = [1,2,3]
    arr.forEach(function (item, index) {
        console.log(index, item)
    })
    ```
- every 判断所有元素是否都符合条件  
    ```js
    var arr = [1,2,3]
    var result = arr.every(function (item, index) {
        if (item < 2) {
            return true
        }
    })
    console.log(result) // false
    ```
- some 判断是否至少有一个元素符合条件  
    ```js
    var arr = [1,2,3]
    var result = arr.some(function (item, index) {
        if (item < 2) {
            return true
        }
    })
    console.log(result) // true
    ```
- sort 排序  
    ```js
    var arr = [1,4,2,3,5]
    var result = arr.sort(function (a, b) {
        return a - b    // 从小到大
    })
    console.log(result)
    ```
- map 对元素重新组装，生成新数组
    ```js
    var arr = [1,4,2,3,5]
    var arr2 = arr.map(function (item, index) {
        return '<b>' + item + '</b>'
    })
    console.log(result)
    ```
- filter 过滤符合条件的元素  
    ```js
    var arr = [1,4,2,3,5]
    var arr2 = arr.filter(function (item, index) {
        if (item >= 2) {
            return true
        }
    })
    console.log(result)
    ```
## 对象  
- for...in 遍历对象
    ```js
    var obj = {
        a: 100,
        b: 200,
        c: 300
    }
    for (key in obj) {
        if (obj.hasOwnProperty(key)) {
            console.log(key, obj[key])
        }
    }
    ```
## 解答
- 获取 2017-06-10 格式的日期
    ```js
    function formatDate(dt) {
        if (!dt) {
            dt = new Date()
        }
        var year = dt.getFullYear()
        var month = dt.getMonth() + 1
        var date = dt.getDate()
        if (month < 10) {
            month = '0' + month
        }
        if (date < 10) {
            date = '0' + date
        }
        return year + '-' + month + '-' + date
    }
    console.log(formatDate());
    ```
- 获取随机数，要求是长度已知的字符串格式
    ```js
    var random = Math.random()
    var random = random + '0000000000'  // 后面加上 10 个零
    var random = random.slice(0, 10)
    console.log(random)
    ```
- 写一个能遍历对象和数组的通用 forEach 函数
    ```js
    function forEach(obj, fn) {
        var key
        if (obj instanceof Array) {
            obj.forEach(function (item, index) {
                fn(index, item)
            })
        } else {
            for (key in obj) {
                if (obj.hasOwnProperty(key)) {
                    fn(key, obj[key])
                }
            }
        }
    }
    var arr = [1,2,3]
    forEach(arr, function (index, item) {
        console.log(index, item);
    })
    var obj = {x: 100, y:200}
    forEach(obj, function (key, value) {
        console.log(key, value);    
    })

    ```

# JS-WEB API
- JS基础知识：EMCA 262 标准（基础语法）
    - 变量类型
    - 原型
    - 作用域和异步
- JS-WEB-API：W3C标准（基础库）  
    - 浏览器操作页面的API
        - DOM操作  
        - BOD操作  
        - 事件绑定  
        - ajax请求（包括http协议）  
        - 存储  
    - 全局对象及函数  
        - Object, Array, Boolean, String, Math, JSON 等
        - window, document
        - navigator


## DOM操作（Document Object Model） 

    DOM 是哪种基本结构？
    DOM 操作常用 API 有哪些
    DOM节点的 attr 和 property 有何区别？  

- DOM 本质
> 就是将XML（或者HTML）内的节点定义成基本统一的对象数据可以供程序语言（如javaScript）控制的技术规范。
- DOM 节点操作
    - 获取 DOM 节点
        ```js
        var div1 = document.getElementById('div1')  // 元素
        var divList = document.getElementsByTagName('div')  // 集合
        var divList = document.getElementsByClassName('div')  // 集合
        var divList = document.querySelectorAll('div')  // 集合
        ```
    - prototype
        ```js
        var p = document.getElementById('p')  // 元素
        console.log(p.style.width)  // 获取样式
        p.style.width = '100px' // 修改样式
        ```
    - Attribute
        ```js
        var p = document.getElementById('p')  // 元素
        console.log(p.getAttribute('data-name'))  // 获取样式
        p.getAttribute('data-name') = 'p2' // 修改样式
        ```
- DOM 结构操作
    - 新增节点
        ```js
        var div1 = document.getElementById('div1')
        // 添加新节点
        var p1 = document.createElement('p')
        p1.innerHtml = 'this is p1'
        div1.appendChild(p1)    // 添加新创建的元素
        // 移动已有节点
        var p2 = document.getElementById('p2')
        div1.appendChild(p2)
        ```
    - 获取父元素
        ```js
        var parent = div1.parentElement
        ```
    - 获取子元素
        ```js
        var child = div1.childNodes
        ```
    - 删除节点
        ```js
        div1.removeChild(child[0])
        ```

### 解题
- DOM 是哪种基本结构？
    - 树
- DOM 操作常用 API 有哪些
    - 获取 DOM 节点，以及节点的 property 和 attibute
    - 获取父节点，获取子节点
    - 新增节点，删除节点

- DOM节点的 attr 和 property 有何区别？  
    - property 只是一个 JS 对象的属性的修改  
    - Attribute 是对 html 标签属性的修改

## BOM操作（Browser Object Model）

    如何检测浏览器类型 
    拆解url的各部分

- 如何检测浏览器类型  
    - navigator（userAgent）  
        ```js
        var ua = navigator.userAgent
        var isChrome = ua.indexOf('Chrome')
        ```
    - screen（width、height）  
        ```js
        console.log(screen.width)
        console.log(screen.height)
        ```
- 拆解url的各部分  
    - location（href、protocol、pathname、search、hash）  
        ```js
        console.log(location.href)
        console.log(location.protocol)
        console.log(location.host)
        console.log(location.pathname)
        console.log(location.search)
        console.log(location.hash)
        ```
    - history（back、forward）
        ```js
        history.back()
        history.forward()
        ```
## 事件  

    编写一个通用的事件监听函数
    描述事件冒泡流程
    冒泡的应用

- 编写一个通用的事件监听函数（ 通用事件绑定 ）
    ```js
    //通用事件绑定
    function bindEvent (elem, type, fn) {
        elem.addEventListener(type, fn)
    }
    //通用事件绑定 + 事件代理
    function bindEvent (elem, type, selector, fn) {
        if (fn == null) {
            fn = selector
            selector = null
        }
        elem.addEventListener(type, function (e) {
            var target
            if (selector) {
                target = e.target
                if (target.matches(selector)) {
                    fn.call(target, e)
                }
            } else {
                fn(e)
            }
        })
    }
    var div1 = document.getElementById('div1')
    bindEvent(div1, 'click', 'a', function(e) {
        console.log(this.innerHTML);
    })
    ```
    > 关于IE低版本的兼容性  
    IE低版本使用attachEvent绑定事件，和W3C标准不一样

- 描述事件冒泡流程（事件冒泡）  
    - DOM树形结构  
    - 事件冒泡  
    - 阻止冒泡  
        - event.stopPropagation() 不再派发事件。   
            终止事件在传播过程的捕获、目标处理或起泡阶段进一步传播。调用该方法后，该节点上处理该事件的处理程序将被调用，事件不再被分派到其他节点。
            ```html
            <body>
                <div id = 'div1'>
                    <p id = 'p1'>激活</p>
                    <p id = 'p2'>取消</p>
                    <p id = 'p3'>取消</p>
                    <p id = 'p4'>取消</p>
                </div>
                <div id = 'div2'>
                    <p id = 'p5'>取消</p>
                    <p id = 'p6'>取消</p>
                </div>
            </body>
            ```
            ```js
            var p1 = document.getElementById('p1')
            var body = document.body
            bindEvent(p1, 'click', function (e) {
                e.stopPropagation()
                alert('激活')
            })
            bindEvent(body, 'click', function (e) {
                e.stopPropagation()
                alert('取消')
            })
            ```
    - 冒泡的应用（代理）  
        - 代码简洁
        - 减少浏览器内存的占用
        ```html
        <div id = 'div1'>
            <a href='#'>a1</a>
            <a href='#'>a2</a>
            <a href='#'>a3</a>
            <a href='#'>a4</a>
        </div>
        ```
        ```js
        var div1 = document.getElementById('div1')
        bindEvent(div1, 'click', function (e) {
            var target = e.target
            if (target.nodeName === 'A') {
                alert(trget.innerHTML)
            }
        })
        ```

- 对于一个无限下拉加载图片的页面，如何绑定事件
    - 使用代理  
    - 知道代理的两个优点（绑定次数少，浏览器压力小）  

## AJAX
    手动编写一个ajax，不依赖第三方库
    跨域的几种实现方式    
- XMLHttpRequest  
    ```js
    var xhr = new XMLHttpRequest()
    xhr.open("GET", "./api", false)
    xhr.onreadystatechange = function () {
        //这里的函数异步执行
        if (xhr.readyState == 4) {
            if (xhr.status == 200) {
                console.log(xhr.responseText);
            }
        }
    }
    xhr.send(null)
    ```
    > 关于IE低版本的兼容性  
    IE低版本使用ActiveXObject，和W3C标准不一样
- 状态码说明
    - readyState  
        0 - （未初始化）还没有调用send()方法  
        1 - （载入）已调用send()方法，正在发送请求  
        2 - （载入完成）send()方法执行完成，已经接收到全部响应内容  
        3 - （交互）正在解析响应内容  
        4 - （完成）响应内容解析完成，可以在客户端调用了
    - status  
        2xx - 表示成功处理请求。如200  
        3xx - 需要重定向，浏览器直接跳转  
        4xx - 客户端请求错误，如404  
        5xx - 服务器错误  
- 跨域  
    - 定义：浏览器有同源策略，不允许ajax访问其他域的接口  
    - 跨域条件：协议、域名、端口，有一个不同就算跨域
    -  可以跨域的三个标签
        - \<img>  
            用于打点统计，统计网站可能是其他域  
            标签古老，不存在兼容性问题
        - \<link>、\<script>  
            可以使用CDN，CDN的也是其他域   
        - \<script>  
            可以用于JSONP

    > Tips: 所有的跨域请求都必须经过信息提供方允许  
    如果未经允许即可获取，那是浏览器同源策略出现漏洞

- JSONP实现原理
    - 加载http://coding.m.imooc.com/classindex.html  
    - 不一定服务器真正有一个classindex.html文件
    - 服务器可以根据请求，动态生成一个文件，返回  
    - 同理于\<script src = 'http://coding.m.imooc.com/api.js'>
        ```js
        window.callback = function (data) {
            //这是我们跨域得到的信息
            console.log(data);
        }
        <script src = 'http://coding.m.imooc.com/api.js'>
        <!-- 以上将返回 callback({x:100, y:200}) -->
        ```

- 服务器端设置 http header
    ```php
    // 第二个参数填写允许跨域的域名，不建议直接写 "*"
    response.setHeader("Access-Control-Allow-Origin", "http://a.com, http://b.com");
    response.setHeader("Access-Control-Allow-Headers", "X-Requested-With");
    response.setHeader("Access-Control-Allow-Methods", "PUT, POST, GET, DELETE, OPTIONS");

    // 接收跨域cookie
    response.setHeader("Access-Control-Allow-Credentials", "true");
    ```
- 解题
    - 手动编写一个ajax，不依赖第三方库
        ```js
        var xhr = new XMLHttpRequest()
        xhr.open("GET", "./api", false)
        xhr.onreadystatechange = function () {
            //这里的函数异步执行
            if (xhr.readyState == 4) {
                if (xhr.status == 200) {
                    console.log(xhr.responseText);
                }
            }
        }
        xhr.send(null)
        ```
    - 跨域的几种实现方式
        - JSONP
        - 服务器端设置 http header

## 存储

    请描述一下 cookie、 sessionStorage 和 localStorage 的区别？  
    > 容量、是否会携带到ajax中、API易用性  
    
- cookie  
    - 用于客户端和服务器通信  
    - 但是它有本地存储的功能，于是就被『借用』  
    - 使用 document.cookie = ... 获取和修改即可
    ### 缺点
    - 存储太小，只有4KB  
    - 所有http请求都带着，会影响获取资源的效率  
    - API简单，需要封装才能用 document.cookie = ...
-  localStorage 和 sessionStorage
    - HTML5 专门为存储而设计，最大容量 5M  
    - API简洁易用：  
        - localStorage.setItem(key, value);  
        - localStorage.getItem(key);

    > iOS safari 隐藏模式下 localStorage.getItem 会报错  
    建议统一使用 try-catch 封装

# 开发环境
## IDE（ 写代码效率 ）  
- webstorm 收费，功能强大
- sublime 免费，轻量级，性能好
- vscode　免费，轻量级，针对前端推出，微软出品
- atom　免费，GitHub出品
- 插件插件插件！！
## GIT（ 代码版本管理，多人协作开发 ）  
    正式项目都需要代码版本管理  
    大型项目需要多人协作开发  
    Git和Linux是一个作者
    网络Git服务器：coding.net、github.com  
    一般公司代码非开源，都有自己的Git服务器
- 常用Git命令
    ```js
    git add
    git checkout xxx //切换分支
    git commit -m "xxx"
    git push origin master
    git pull origin master
    git branch  //分支列表
    git chechout -b xxx / git chechout xxx
    git diff    //工作区与暂存区比较
    git status
    ```
- 多人协作
    ```js
    git checkout -b xxx //新建并切换到xxx分支  
    git merge xxx //合并当前分支和xxx分支
    ```
# JS模块化  

## 不使用模块化的情况
- utils.js getFormatDate函数  
- a-utils.js aGetFormatDate函数 使用getFormatDate  
- a.js aGetFormatDate  
```js
//util.js
function getFormatDate(date, type) {
    // type === 1 返回 2017-06-15
    // type === 2 返回 2017年06月15日 格式
    // ...
}

//a-utils.js
function aGetFormatDate(date) {
    // 要求返回 2017年06月15日 格式
    return getFormatDate(date, 2)
}

//a.js
var dt = new Date()
console.log(aGetFormatDate(dt));
```
```html
<script src="utils.js"></script>
<script src="a-utils.js"></script>
<script src="a.js"></script>

<!-- 1. 这些代码中的函数必须是全局变量，才能暴露给使用方。全局变量污染 -->
<!-- 2. a.js 知道要引用 a-utils.js，但是它知道还需要依赖于 utils.js吗？ -->
```
## 使用模块化
```js
// util.js
export {
    getFormatDate: function (date, type) {
        // type === 1 返回 2017-06-15
        // type === 2 返回 2017年06月15日 格式
        // ...
    }
}
// a-util.js
var getFormatDate = require('util.js')
export {
    aGetFormatDate: function (date) {
        // 要求返回 2017年06月15日 格式
        return getFormatDate(date, 2)
    }
}
// a.js
var aGetFormatDate = require("a-util.js")
var dt = new Date()
console.log(aGetFormatDate(dt));
```
## AMD
> 实际上AMD 是 RequireJS 在推广过程中对模块定义的规范化的产出 

require.js requirejs.org/  
全局 define 函数  
全局 require 函数  
依赖 JS 会自动、异步加载

```js
// 使用require.js
// util.js
define(function() {
    var util = {
        getFormatDate: function (date, type) {
            if (type === 1) {
                return '2017-06-15'
            }
            if (type === 2) {
                return '2018年05月10日'
            }
        }
    }
    return util
});
// a-util.js
define(['./util.js'], function (util) {
    var aUtil = {
        aGetFormatDate: function (date) {
            return util.getFormatDate(date, 2)
        }
    }
    return aUtil
});
// a.js
define(['./a-util.js'], function (aUtil) {
    var a = {
        printDate: function (date) {
            console.log(aUtil.aGetFormatDate(date));
        }
    }
    return a
});
// main.js
require(['./a.js'], function (a) {
    var date = new Date()
    a.printDate(date)
})
```
```html
<script src="http://cdn.bootcss.com/require.js/2.2.3/require.min.js" data-main="./main.js"></script>
```

## CommonJS
> nodejs 模块化规范  

前端开发依赖的插件和库，都可以从 npm 中获取  
构建工具的高度自动化，使得使用 npm 的成本非常低  
CommonJS不会异步加载JS，而是同步一次性加载出来

```js
// 使用CommonJS
// util.js
module.exports = {
    getFormatDate: function (date, type) {
        if (type === 1) {
            return '2017-06-15'
        }
        if (type === 2) {
            return '2018年05月10日'
        }
    }
}
// a-util.js
var util = require('util.js')
module.exports = {
    aGetFormatDate: function (date) {
        return util.getFormatDate(date, 2)
    }
}
```
## AMD 和 CommonJS 的使用场景
- 需要异步加载JS，使用AMD  
- 使用 npm 建议使用 CommonJS

# 打包工具  
- Grunt  
- Gulp  
- Fis3  
- Webpack

## 安装nodejs（移步nodejs官网）
```
> node -v
6.2.2
> npm -v
3.9.5
> sudo npm install http-server -g
> http-server -p 8881
```
## 安装依赖
```
> npm init
> npm install webpack --save-dev //安装webpack（保存，用于开发环境）
> npm i jquery --save
> npm i moment --save
```
## 卸载依赖
```
> npm uninstall moment --save
```
## 配置webpack
```js
// webpack.config.js
var path = require('path')
var webpack = require('webpack')

module.exports = {
    context: path.resolve(__dirname, './src')
    entry: {
        app: './app.js'
    }
    output: {
        path: path.resolve(__dirname, './dist'),
        filename: 'bundle.js'
    }
}
// package.json
{
    "script": {
        "start": "webpack"
    }
}
// command
> npm start
```

## 使用jquery
```js
// package.json 确认已安装jquery
{
    "dependencies": {
        "jquery": "^3.2.1"
    }
}
// app.js
var $ = require('jquery')
var root = $('#root')
$root.html('<p>这是jquery插入的文字</p>')
```
## 使用自定义CommonJS模块
```js
// 自定义模块 a-util.js
module.exports = {
    print: function () {
        console.log(123)
    }
}
// app.js
var aUtil = require('./a-util.js')
aUtil.print()
```
## 压缩JS
```js
// webpack.config.js
var path = require('path')
var webpack = require('webpack')

module.exports = {
    context: path.resolve(__dirname, './src')
    entry: {
        app: './app.js'
    }
    output: {
        path: path.resolve(__dirname, './dist'),
        filename: 'bundle.js'
    }
    plugins: [
        new webpack.optimize.UglifyJsPlugin()
    ]
}
// command
> npm start
```

# 上线和回滚
## 上线流程  
- 将测试完成的代码提交到git版本库的master分支
- 将当前服务器的代码全部打包并记录版本号，备份
- 将master分支的代码提交覆盖到线上服务器，生成新版本号
## 回滚流程
- 将当前服务器的代码全部打包并记录版本号，备份
- 将备份的上一个版本号解压，覆盖到线上服务器，并生成新的版本号
## linux基本命令  
    服务器使用linux居多，server版，只有命令行  
    测试环境要匹配线上环境，因此也是linux  
    经常登录测试机来自己配置、获取数据
```
> ssh name@server   //登录服务器
> mkdir xxx //新建文件夹
> ls    //查看文件名字列表
> ll    //查看文件详细列表
> pwd   //显示当前目录
> rm xxx    //删除文件
> rm -rf xxx    //删除文件夹
> cp xxx xxx    //拷贝
> mv xxx xxx    //移动
> cat xxx   //打印文件内容
> head xxx   //打印文件开头内容
    -n number   //选项，前几行
> tail xxx   //打印文件尾部内容
> grep xxx file //搜索
```
### VI、VIM命令
```
i 编辑模式
esc 退出编辑
w 保存
q 退出
```

# 运行环境
    浏览器可以通过访问链接来得到页面的内容  
    通过绘制和渲染，显示出页面的最终的样子  
    整个过程中，我们需要考虑什么问题？

## 页面加载过程  
-  从输入 url 到得到 html 的详细过程  

    - 加载资源的形式  
        - 输入url（或跳转页面）加载html  
        - 加载html中的静态资源  

    - 加载一个资源的过程  
        - 浏览器根据DNS服务器得到域名的IP地址  
        - 向这个IP的机器发用http(s)请求  
        - 服务器收到、处理并返回http(s)请求  
        - 浏览器得到返回内容  

    - 浏览器渲染页面的过程  
        - 根据 HTML 结构生成 DOM Tree  
        - 根据 CSS 生成 CSSOM  
        - 将 DOM 和 CSSOM 整合形成 RenderTree
        - 根据 RenderTree 开始渲染和展示  
        - 遇到 \<script> 时，会执行并阻塞渲染（因为JS有权改变DOM结构）  

### window.onload 和 DOMContentLoaded 的区别  

```js
window.addEventListener('load', function() {
    // 页面的全部资源加载完才会执行，包括图片、视频等
})
document.addEventListener('DOMContentLoaded', function() {
    // DOM渲染完即可执行，此时图片、视频还可能没有加载完
})
```
# 性能优化  
    多使用内存、缓存或者其他方法  
    减少CPU计算、减少网络请求

## 加载页面和静态资源  
- 静态资源的压缩合并
- 静态资源缓存
    ```html
    <!-- 通过链接名称控制缓存 -->
    <script src="abc_1.js"></script>
    <!-- 只有内容改变的时候，再改变链接名称 -->
    <script src="abc_2.js"></script>
    ```
- 使用CDN让资源加载更快（cdn.bootcss.com）
- 使用 SSR（Server-Side-Render） 后端渲染，数据直接输出到 HTML 中
    - Vue、React 提出了这样的概念
    - JSP、PHP、ASP 都属于后端渲染
## 页面渲染  
- CSS 放在 HEAD 中，JS 放在 BODY 后面  
- 懒加载（图片懒加载、下拉加载更多）
    ```html
    <img id="img1" src="preview.png" data-realsrc="abc.png"/>
    <script>
        var img1 = document.getElementById("img1")
        img1.src = img1.getAttribute('data-realsrc')
    </script>
    ```
- 减少 DOM 查询，对 DOM查询做缓存
    ```js
    // 未缓存 DOM 查询
    var i
    for (i = 0; i < document.getElementsByTagName('p').length; i++) {
        //todo
    }
    // 缓存了 DOM 查询
    var pList = document.getElementsByTagName('p')
    var i
    for (i = 0; i < pList.length; i++) {
        //todo
    }
    ```
- 减少 DOM 操作，多个操作尽量合并在一起执行
    ```js
    var listNode = document.getElementById('list')
    // 要插入 10 个 li 标签
    var frag = document.createDocumentFragment();
    var x, li;
    for (i = 0; i < 10; i++) {
        li = document.createElement("li")
        li.innerHTML = "List item " + x;
        frag.appendChind(li);
    }
    listNode.appendChild(frag)
    ```
- 事件节流
    ```js
    var textarea = document.getElementById('text')
    var timeoutId
    textarea.addEventListener('keyup', function () {
        if (timeoutId) {
            clearTimeout(timeoutId)
        }
        timeoutId = setTimeout(function () {
            // 触发 change 事件
        }, 100)
    })
    ```
- 尽早执行操作（如 DOMContentLoaded）
# 安全性
    综合性的问题：常见的前端安全问题有哪些？

## XSS 跨站请求攻击  
    在博客写一篇文章，同时偷偷插入一段 <script>  
    攻击代码中，获取 cookie ，发送到自己的服务器
    发布博客，有人查看内容
    会把查看者的 cookie 发送到攻击者的服务器中
- 替换关键字，例如替换 < 为 \&lt; >为 \&gt;
## XSRF 跨站请求伪造
    已登录一个购物网站，正在浏览商品
    该网站付费接口是 xxx.com/pay?id=100 但是没有任何验证
    然后你收到一封邮件，隐藏着 <img src="xxx.com/pay?id=100">
    你查看邮件的时候图片自动加载，就已经悄悄购买了
- 增加验证流程，如输入指纹、密码、短信验证码