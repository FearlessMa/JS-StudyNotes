# 变量对象(Variable Object )


![](http://upload-images.jianshu.io/upload_images/599584-ab88faf1cbf625b6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

* 当一个函数被执行，就会在栈中生成一个该函数的执行上下文，执行上下文的声明周期分为2个阶段
![](http://upload-images.jianshu.io/upload_images/599584-391af3aad043c028.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
  * 创建阶段：创建阶段,执行上下文会分别创建变量对象(VO),建立作用域,以及确认this指向。
  * 执行阶段：上下文创建完成后就会执行，执行阶段会完成变量赋值，函数引用,以及执行其他代码,变量对象变为活动对象(AO)。

**变量对象（Variable Object）**

* 变量对象的创建，依次经历了以下几个过程。
1. 建立arguments对象。检查当前上下文中的参数，建立该对象下的属性与属性值。
2. 检查当前上下文的函数声明，也就是使用function关键字声明的函数。在变量对象中以函数名建立一个属性，属性值为指向该函数所在内存地址的引用。如果函数名的属性已经存在，那么该属性将会被新的引用所覆盖。
3. 检查当前上下文中的变量声明，每找到一个变量声明，就在变量对象中以变量名建立一个属性，属性值为undefined。如果该变量名的属性已经存在，为了防止同名的函数被修改为undefined，则会直接跳过，原属性值不会被修改


```js
function foo() { console.log('function foo') }
var foo = 20;

console.log(foo); // 20 AO执行过程变量赋值

// 分解上下文过程 
// 创建过程 VO阶段 创建变量
function foo() { console.log('function foo') }
var foo = undefined;
// 执行过程 VO->AO
function foo() { console.log('function foo') }
var foo = undefined;
foo = 20; //变量赋值
console.log(foo);

```
```js
function foo() { console.log('function foo') }
var foo ; //foo以存在跳过赋值
console.log(foo); // ƒ foo() { console.log('function foo') }

// 分解上下文过程 
// 创建过程 VO阶段 创建变量
function foo() { console.log('function foo') }
// var foo = undefined;被跳过
// 执行过程 VO->AO
function foo() { console.log('function foo') }
console.log(foo);
```

## 变量对象创建过程

![](http://upload-images.jianshu.io/upload_images/599584-7d131cfe82a20d37.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

栗子：
```js
// demo01
function test() {
    console.log(a);
    console.log(foo());

    var a = 1;
    function foo() {
        return 2;
    }
}

test();

```

变量对象创建过程
```js
// 创建过程
testEC = {
    // 变量对象
    VO: {},
    scopeChain: {}
}

// 因为本文暂时不详细解释作用域链，所以把变量对象专门提出来说明

// VO 为 Variable Object的缩写，即变量对象
VO = {
    arguments: {...},  //注：在浏览器的展示中，函数的参数可能并不是放在arguments对象中，这里为了方便理解，我做了这样的处理
    foo: <foo reference>  // 表示foo的地址引用
    a: undefined
}
```
未进入执行阶段之前，变量对象中的属性都不能访问！但是进入执行阶段之后，变量对象转变为了活动对象，里面的属性都能被访问了，然后开始进行执行阶段的操作。

```

这样，如果再面试的时候被问到变量对象和活动对象有什么区别，就又可以自如的应答了，他们其实都是同一个对象，只是处于执行上下文的不同生命周期。不过只有处于函数调用栈栈顶的执行上下文中的变量对象，才会变成活动对象。

```
```js
// 执行阶段
VO ->  AO   // Active Object
AO = {
    arguments: {...},
    foo: <foo reference>,
    a: 1,
    this: Window
}

```
test栗子就变为这样
```js
function test() {
    function foo() {
        return 2;
    }
    var a;
    console.log(a);
    console.log(foo());
    a = 1;
}

test();

```
**全局上下文的变量对象**

以浏览器中为例，全局对象为window。全局上下文有一个特殊的地方，它的变量对象，就是window对象。而这个特殊，在this指向上也同样适用，this也是指向window。
```js
// 以浏览器中为例，全局对象为window
// 全局上下文
windowEC = {
    VO: Window,
    scopeChain: {},
    this: Window
}
```


参考资料：

[变量对象详解](https://segmentfault.com/a/1190000012646211)