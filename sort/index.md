# js实现的排序

## 插入排序 
*  与元素左侧比较选择插入位置插入 从数组下标为1的位置开始于左侧元素比较找到大小的位置
```js
解析：
（1） 从第一个元素开始，该元素可以认为已经被排序
（2） 取出下一个元素，在已经排序的元素序列中从后向前扫描
（3） 如果该元素（已排序）大于新元素，将该元素移到下一位置
（4） 重复步骤3，直到找到已排序的元素小于或者等于新元素的位置
（5）将新元素插入到下一位置中
（6） 重复步骤2

const arr = [1, 4, 3, 2, 65, 43, 21];
function insertSort(arr) {
    if (!Array.isArray(arr)) throw new Error('err');
    for (let i = 1; i < arr.length; i++) {
        if (arr[i] < arr[i - 1]) {
            // 记住当前的值
            let current = arr[i];
            // 记住当前值的前一个位置 也就是比较的第一个位置
            // 插入排序是比较当前值左侧的所有值
            let j = i - 1;

            while (j >= 0 && current < arr[j]) {
                // 如果当前值（current）比 当前j位置的值小 j位置向右移动一位
                arr[j + 1] = arr[j];
                // 继续比较j位置的左侧值
                j--;
            }
            arr[j + 1] = current;
        }
    }
    return arr;

}
// [1, 3, 4, 2, 65, 43, 21];
// const arr = [1, 4, 3, 2, 65, 43, 21];
// let insert = insertSort(arr);
// console.log(insert)
```

## 冒泡排序

* 相邻比较换位置，没一轮最后一个位置是这一轮的最大或最小元素

```js
function popSort(arr) {
    if (!Array.isArray(arr)) throw new Error('err');
    for (let i = 0; i < arr.length - 1; i++) {
        for (let j = 0; j < arr.length - i - 1; j++) {
            if (arr[j] > arr[j + 1]) {
                let a = arr[j + 1];
                arr[j + 1] = arr[j];
                arr[j] = a;
            }
        }
    }
    return arr
}
// const arr = [1, 4, 3, 2, 65, 43, 21];
// let a = popSort(arr);
// console.log(a)
```

## 选择排序
* 记住最大或最小元素与每一轮比较的第一个元素换位置
* 解析:首先在未排序序列中找到最小（大）元素，存放到排序序列的起始位置，然后，再从剩余未排序元素中继续寻找最小（大）元素，然后放到已排序序列的末尾。
```js
function selectionSort(arr) {
    for (let i = 0; i < arr.length; i++) {
        // 记住当前最小值
        let min = arr[i];
        // 记住当前最小值位置
        let m = i;
        for (let j = i + 1; j < arr.length; j++) {
            if (min > arr[j]) {
                min = arr[j];
                m = j;
            }
        }
        arr[m] = arr[i];
        arr[i] = min;
    }
    return arr;
}

// const arr = [1, 4, 3, 2, 65, 43, 21];
// console.log(arr)
// let selection = selectionSort(arr);
// console.log(selection)
```