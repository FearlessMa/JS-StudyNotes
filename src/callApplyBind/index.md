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

    // 2.call 待参数实现
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
```