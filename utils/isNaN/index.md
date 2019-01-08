# NaN (not a number)

* NaN ：不是一个数字，当数学运算后无法返回一个有效的数字，就会返回NaN。
    * NaN 是number类型。

    * NaN 与自身不相等。

```js
// NaN
    var a = NaN;
    var b = NaN;
    console.log('typeof NaN: ', typeof NaN);//number
    //NaN与自身不相等
    console.log('a===b',a===b); //false
    Object.prototype.toString.call(null,NaN);//"[object Number]"
```

## isNaN实现

* window.isNaN无法判断字符串类型

```js
    //window.isNaN
    console.log('isNaN("a1"): ', isNaN("a1"));//true
```

* NaN与自身不相等

```js
//方法1
    //NaN与自身不相等
    function nweIsNaN(num) {
        return  num !== num;
    }
    console.log('nweIsNaN(1): ', nweIsNaN(1)); //false
    console.log(' nweIsNaN("a1"): ',  nweIsNaN("a1"));//false
    console.log('nweIsNaN(NaN): ', nweIsNaN(NaN));//true
// 方法2
    function nweIsNaN2(num){
        return typeof num === 'number' && window.isNaN(num);
    }
    console.log('nweIsNaN2(1): ', nweIsNaN2(1)); //false
    console.log(' nweIsNaN2("a1"): ',  nweIsNaN2("a1"));//false
    console.log('nweIsNaN2(NaN): ', nweIsNaN2(NaN));//true

```

