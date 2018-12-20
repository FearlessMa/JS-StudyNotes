# new及Object.create的实现

```js
    // new的实现  通过Object.create实现新对象创建，然后复制属性到新对象中
    const newObj = function (name, age) {
        const _this = Object.create({});
        _this.name = name;
        _this.age = age;
        return _this;
    }
    const test = newObj('test', '18');
    console.log('test: ', test); //test:  {name: "test", age: "18"}
    // Object.create 实现  通过
    function newCreate(objPrototype) {
        function Fn() { }
        Fn.prototype = objPrototype;
        Fn.prototype.construct = Fn;
        return new Fn()
    }
    const testCreate = newCreate({ a: 1 });
    console.log('testCreate: ', testCreate); //testCreate:  Fn {} 

    // 柯里化实现new
    function Person(name, age) {
        this.name = name;
        this.age = age;
    }
    function NewFn(fn) {
        const newObj = {};
        fn.apply(newObj, Array.prototype.slice.call(arguments, 1));
        return newObj;
    }
    const newPerson = NewFn(Person,'ma',18);
    console.log('newPerson: ', newPerson);
```