# null、undefined

## null

* null指的是空值(empty value)
    * null是指曾赋过值，目前没有值。
    * null是关键字，不是标识符。

```js
    var null = 'null'; //报错
    // null
    var a = null;
    var b = null;
    // null与null相等 基本类型值相等
    console.log('a===b',a===b);//true
    // typeof null为 object
    console.log('typeof null: ', typeof null);//typeof null:  object
```

## undefined

* undefined指的是没有值(missing value)
    * undefined是指从未赋值。
    * 可使用void 获得undefined ：void 0或void(0)
    * undefined 是一个标识符，可以被赋值。
    * undefined 可以通过void表达式获得，void在c、java中表示无返回值。
    * 在全局下给undefined赋值，结果为undefined。
    * 在函数作用域内部声明变量undefined，undefined会被赋值。

```js
// undefined
    var a1 = undefined;
    var b1 = undefined;
    console.log('typeof undefined: ', typeof undefined);//typeof null:  undefined
    console.log('a1===b1',a1===b1);//true
    var undefined = 1 ;
    console.log('undefined: ', undefined);//undefined
    function foo (){
        undefined = 2;
        console.log('undefined: ', undefined); //2
        var undefined = 3;
        console.log('undefined: ', undefined); //3
    }
    foo();
    console.log('undefined: ', undefined); //undefined
    function test(){
        return void 0;
    }
    console.log('test(): ', test());//test():  undefined
```