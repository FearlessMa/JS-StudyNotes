# JS基础总结

## js如何执行

* 对于常见编译型语言（例如：Java）来说，编译步骤分为：词法分析->语法分析->语义检查->代码优化和字节码生成。

* 对于解释型语言（例如 JavaScript）来说，通过词法分析 -> 语法分析 -> 语法树，就可以开始解释执行了。

## js解释执行过程

* js解释器执行代码->可执行代码
  * 词法分析
  * 语法分析
    * 词法语法分析完成后生成AST语法数([TODO]())
  * 生成可执行代码
  * 作用域规则确定
* js引擎->执行可执行代码
  * **内存中数据结构：**
    * 代码在内存中被执行，因为js没有严格的区分栈内存、堆内存，可以理解为js所有的数据都是保存在堆内存中，在一些知识中还是要区分堆、栈、列队。
    * 堆内存数据结构(heap)：堆数据结构是一种树状结构，[树](http://data.biancheng.net/view/23.html)。
    * 栈内存数据结构(stack)：先进后出数据结构。
    * 列队(queue):FIFO,先进先出数据结构。
  * **执行上下文：**
    * js代码被执行时，当前的执行环境可以理解为执行上下文。全局执行环境、函数执行环境、eval，多个执行上下文，js引擎会以栈的方式执行它们。
    * 栈底永远是全局执行上下文(global)
    * 当函数被执行时，函数被压入栈中。
  * 执行上下文生命周期(创建、执行)
    * 创建执行上下文：
      * 创建变量对象VO(Variable Object)：
        1. 创建arguments对象
        2. 创建function声明的函数：创建function声明的函数，若已有同名函数存在，替换同命函数的引用(更改指针为新函数地址)。
        3. 创建var声明变量：创建var声明变量并赋值undefined，若以存在与var声明变量同名function，跳过var变量声明。
      * 创建作用域链
        1. 作用域链：由当前环境与上一层环境的一系列变量对象组成，它保证了当前执行环境对符合访问权限的变量和函数的有序访问。
        2. scopeChain：[VO(current),VO(pre),...,VO(global)]
      * 确定this指向
      * **未进入执行阶段之前，变量对象中的属性都不能访问！但是进入执行阶段之后，变量对象转变为了活动对象，里面的属性都能被访问了，然后开始进行执行阶段的操作。**
        ```js
        //栗子
        function test() {
            var a = 1;
            function foo() {
                return 2;
            }
        }
        test();

        // 创建过程
        testEC = {
            // 变量对象
            VO: {},
            scopeChain: {}
        }
        //变量对象创建
        VO = {
            arguments:{...}, //注：在浏览器的展示中，函数的参数可能并不是放在arguments对象中，这里为了方便理解.
            foo:<foo reference>, //表示foo的引用地址
            a:undefined,
            this:window
        }
        ```
    * 执行(当函数处于栈顶)
      * 活动对象：VO->AO(Active Object)
      * 变量赋值
      * function执行
      * 执行其他代码
      ```js
        //VO->AO
        AO={
            arguments:{...},
            foo:<foo reference>,
            a:1,
            this:window
        }
      ```
* **事件循环(Event Loop)**
    * 当执行代码时，执行到setTimeout，setInterval,promise,process.nextTick时，回调会被放入事件循环队列中。
    * 事件循环分为两种
        * 宏任务(macro-task):script(整体代码), setTimeout, setInterval, setImmediate, I/O, UI rendering,等
        * 微任务(micro-task): process.nextTick, Promise, Object.observe(已废弃), MutationObserver(html5新特性),等
    * 事件循环执行顺序：
        * 先从script执行第一次循环，之后全局函数进入调用栈，其他函数被压入栈。
        * 在栈顶执行的函数中：
            * 执行到macro-task任务，该任务进入macro-task任务列队。
            * 执行到micro-task任务，该任务进入micro-task任务列队。
        * 执行栈被清空后：
            * 首先执行一遍微任务(micro-task)任务列队。微任务列队执行完毕。
            * 执行宏任务(macro-task)列队，这样一直循环下去.
```js

console.log(1);//同步执行

setTimeout(function(){
    console.log(2);
},1000);//回调函数进入宏任务

const newPromise = function(){
    return new Promise(function(resolve,reject){
    console.log(3); //同步代码直接执行
    resolve();
    console.log(4); //同步代码直接执行
})
}

//then回掉函数进入微任务队列
newPromise().then(function(){
    console.log(5);
})
console.log(6);//同步代码
//1,3,4,6,5,2

```
* **垃圾回收**
    
    * 栈顶函数执行完毕出栈，执行上下文失去引用，等待回收

* **[闭包](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Closures)**
    * 闭包： 执行上下中A中执行函数，该函数上下文B中引用了A变量对象中的值，就形成了闭包
    * 栈顶函数执行完毕出栈，执行上下文没有失去引用。

```js

function A(){
    var a= 1;
    return function B(){
        console.log(a);
    }
}

var b = A();
b();//1
```

##TODO