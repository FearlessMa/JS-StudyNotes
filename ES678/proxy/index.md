# Proxy、get、set

## 1.get、set

* Object.defineProperty() 方法会直接在一个对象上定义一个新属性，或者修改一个对象的现有属性， 并返回这个对象。

* 语法：Object.defineProperty(obj, prop, descriptor)

    * **configurable**
        * 当且仅当该属性的 configurable 为 true 时，该属性描述符才能够被改变，同时该属性也能从对应的对象上被删除。默认为 false。
    * **enumerable**
        * 当且仅当该属性的enumerable为true时，该属性才能够出现在对象的枚举属性中。默认为 false。
    * **value**
        * 该属性对应的值。可以是任何有效的 JavaScript 值（数值，对象，函数等）。默认为 undefined。
    * **writable**
        *当且仅当该属性的writable为true时，value才能被赋值运算符改变。默认为 false。
    * **get**
        * 一个给属性提供 getter 的方法，如果没有 getter 则为 undefined。当访问该属性时，该方法会被执行，方法执行时没有参数传入，但是会传入this对象（由于继承关系，这里的this并不一定是定义该属性的对象）。
        * 默认为 undefined。
    * **set**
        * 一个给属性提供 setter 的方法，如果没有 setter 则为 undefined。当属性值修改时，触发执行该方法。该方法将接受唯一参数，即该属性新的参数值。
        * 默认为 undefined
```js
    // es5 getter setter 可以在属性的存储过程附加其他操作
    var es5Obj = {
        name: 'es5'
    };
    (function () {
        var name = 'defineProperty';
        Object.defineProperty(es5Obj, 'name', {
            get: function () {
                console.log('es5 getter');
                return name;
            },
            set: function (value) {
                console.log('set value: ', value);
                name = 'setValue=' + value;
            }
        });
    })()
    console.log('es5Obj: ', es5Obj);
    console.log('es5Obj.name: ', es5Obj.name);
    //先通过getter函数获取name 然后打印name
    // es5 getter
    //es5Obj.name:  defineProperty
    es5Obj.name = '123';
    console.log('es5Obj.name: ', es5Obj.name);
    //es5 getter
    //es5Obj.name:  setValue=123

```
