# call、apply、bind 实现

```js
    //1.call 不带参数基本实现
    const test = {
        name: 'test'
    };
    function testCall() {
        console.log(this.name);
    }

    Function.prototype.newCall = function (content) {
        // 将this指向改变到call的对象
        content.fn = this;
        // 在指向对象下调用call的方法获取上下文中的this
        content.fn();
        // 删除绑定的this
        delete content.fn;
    }
    testCall.newCall(test); //test

    // 2.call 带参数实现
    function testCall2(age, skill) {
        console.log('name', this.name, 'age', age, 'skill', skill);
    }
    Function.prototype.newCall2 = function (content) {
        content.fn = this;
        const arg = [... new Set(arguments)];
        content.fn(...arg.slice(1));
        delete content.fn;
    }
    testCall2.newCall2(test, 18, 'react'); //name test age 18 skill react

    // 3.apply的实现
    function testApply(age, skill) {
        console.log('name', this.name, 'age', age, 'skill', skill);
    }
    Function.prototype.newApply = function (content) {
        content.fn = this;
        const [arg] = Array.prototype.slice.call(arguments, 1);
        content.fn(...arg);
        delete content.fn;
    }
    testApply.newApply(test, [18, 'vue']);

    // 4.bind
    // const test = {
    //     name: 'test'
    // };
    function testBind(age, skill) {
        console.log('name', this.name, 'age', age, 'skill', skill);
    }
    Function.prototype.newBind = function (content) {
        content.fn = this;
        const arg = Array.prototype.slice.call(arguments, 1);
        return function () {
            content.fn.apply(null, arg);
            delete content.fn;
        }
    }
    testBind.newBind(test, 18, 'angular')();

    Function.prototype.newBind2 = function (content) {
        const _this = this;
        const arg = Array.prototype.slice.call(arguments, 1);
        return function () {
            _this.apply(content, arg);
        }
    }
    testBind.newBind2(test, 18, 'bind2')();
```