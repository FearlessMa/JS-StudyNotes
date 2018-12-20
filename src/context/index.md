# 执行上下文 : Execution Context
## 1.概念
* 执行上下文Execution Context，当js代码被执行的时候，当前代码的执行环境可以认为是执行上下文，会形成作用域，js作用域会有三种
    * 全局环境 ：Global Context
    * 函数环境 ：函数别调用是的上下文环境
    * eval
* js执行时是通过栈调用方式执行的
    * 栈底永远是 Golbal Context，函数在被执行的时候会被压入栈中，执行完成或者遇到return就会被弹出栈。
* 栗子：
```js
    function fn(a) {
        return function () {
            console.log(a);
        }
    }
    var a = fn(1);
    a();//1
```
    1、代码执行到fn(1)是 函数fn被压入栈，返回匿名函数被全局变量a引用 并没有被执行，此时形成闭包，fn调用结束 被弹出栈
    2、a()被执行，a引用的匿名函数被压入栈中，console被执行，打印1，匿名函数出站。
## 2.执行上下文（创建&执行）
* 执行上下文分为2个阶段
    * 创建上下文
        * 创建VO：变量对象（Variable Object）
            ```js
            1、建立arguments对象。检查当前上下文中的参数，建立该对象下的属性与属性值。
            2、检查当前上下文的函数声明，也就是使用function关键字声明的函数。在变量对象中以函数名建立一个属性，属性值为指向该函数所在内存地址的引用。如果函数名的属性已经存在，那么该属性将会被新的引用所覆盖。
            3、检查当前上下文中的变量声明，每找到一个变量声明，就在变量对象中以变量名建立一个属性，属性值为undefined。如果该变量名的属性已经存在，为了防止同名的函数被修改为undefined，则会直接跳过，原属性值不会被修改。
            ```
        * 确定作用域链
        * 确定this
    * 执行上下文
        * AO：变量对象变为活动对象 （Active Object）
        * 变量赋值
        * 函数引用
        * 执行代码
```js
    // 栗子
        function contextFn() {
            console.log(a);
            console.log(fn);
            var a = 'a';
            function fn() {

            }
            fn = 'fn';
            console.log(fn);
        }
        contextFn();
        // contextFn 被压入栈中是执行上下文创建阶段：作用域链指向golbal，this指向window
        function contextFn() {
            function fn() { }
            var a = undefined;
            console.log(a); // undefined
            console.log(fn); // fn(){}
            a = 'a';
            fn = 'fn';
            console.log(fn);//fn
        }
        // 理解上下文context就理解了变量提升
```