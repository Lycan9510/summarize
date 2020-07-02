# ES6-数组拓展
>本文主要是学习[ECMAScript 6 入门](https://es6.ruanyifeng.com/#docs/array#Array-from)的个人总结。

1. 拓展运算符

    扩展运算符（spread）是三个点（...）。该运算符主要用于函数调用。

    应用：
    1. 复制数组
        ```
        const a1 = [1, 2];
        // 写法一
        const a2 = [...a1];
        // 写法二
        const [...a2] = a1;
        ```
        > **注意:** 上面的两种写法，a2都是a1的克隆,再修改a2就不会对a1产生影响。
    2. 合并数组
        ```
        const arr1 = ['a', 'b'];
        const arr2 = ['c'];
        const arr3 = ['d', 'e'];

        // ES5 的合并数组
        arr1.concat(arr2, arr3);
        // [ 'a', 'b', 'c', 'd', 'e' ]

        // ES6 的合并数组
        [...arr1, ...arr2, ...arr3]
        // [ 'a', 'b', 'c', 'd', 'e' ]
        ```
        > **注意:** 上面代码中，用两种不同方法合并而成的新数组，但是它们的成员都是对原数组成员的引用，这就是`浅拷贝`。如果修改了引用指向的值，会同步反映到新数组。
    3. 与解构赋值结合

        扩展运算符可以与解构赋值结合起来，用于生成数组。
        ```
        const [first, ...rest] = [1, 2, 3, 4, 5];
        first // 1
        rest  // [2, 3, 4, 5]

        const [first, ...rest] = [];
        first // undefined
        rest  // []

        const [first, ...rest] = ["foo"];
        first  // "foo"
        rest   // []
        ```
         > **注意:** 如果将扩展运算符用于数组赋值，只能放在参数的最后一位，否则会报错。
    4. 字符串

        扩展运算符还可以将字符串转为真正的数组。其能够正确识别四个字节的 Unicode 字符。
        ```
        [...'hello']
        // [ "h", "e", "l", "l", "o" ]

        'x\uD83D\uDE80y'.length // 4
        [...'x\uD83D\uDE80y'].length // 3

        let str = 'x\uD83D\uDE80y';
        str.split('').reverse().join('')
        // "y��x"
        [...str].reverse().join('')
        // "y🚀x"
        ```
    5. 转换类数组对象为数组

        任何定义了遍历器（Iterator）接口的对象，都可以用扩展运算符转为真正的数组。
        ```
        let nodeList = document.querySelectorAll('div');
        let array = [...nodeList];
        ```
    6. 转换可遍历对象为数组 
    
        只要具有 Iterator 接口的对象，都可以使用扩展运算符。例如 Map 和 Set 结构，Generator 函数。
        ```
        let map = new Map([
            [1, 'one'],
            [2, 'two'],
            [3, 'three'],
        ]);

        let arr = [...map.keys()]; // [1, 2, 3]

        const go = function*(){
            yield 1;
            yield 2;
            yield 3;
        };

        [...go()] // [1, 2, 3]
        ```

2. Array.from()

    Array.from方法用于将两类对象转为真正的数组：类似数组的对象（array-like object）和可遍历（iterable）的对象（包括 ES6 新增的数据结构 Set 和 Map）。

    Array.from还可以接受第二个参数，作用类似于数组的map方法，用来对每个元素进行处理，将处理后的值放入返回的数组。如果map函数里面用到了this关键字，还可以传入Array.from的第三个参数，用来绑定this。

    应用：
    1. DOM 操作返回的 NodeList 集合、函数内部的arguments对象 转为 真正的数组。
        ```
        // NodeList对象
        let ps = document.querySelectorAll('p');
        Array.from(ps).filter(p => {
        return p.textContent.length > 100;
        });

        // arguments对象
        function foo() {
        var args = Array.from(arguments);
        // ...
        }
        ```
    2. 类似数组的对象（必须有length属性） 转为 真正的数组。
        ```
        Array.from({ length: 3 });
        // [ undefined, undefined, undefined ]
        ```

3. Array.of()

    Array.of方法用于将一组值，转换为数组。
    
    Array.of总是返回参数值组成的数组。如果没有参数，就返回一个空数组。Array.of基本上可以用来替代Array()或new Array()，并且不存在由于参数不同而导致的重载。它的行为非常统一。
    ```
    Array() // []
    Array(3) // [, , ,]
    Array(3, 11, 8) // [3, 11, 8]

    Array.of() // []
    Array.of(undefined) // [undefined]
    Array.of(1) // [1]
    Array.of(1, 2) // [1, 2]
    ```

4. 数组实例的 copyWithin()

    数组实例的copyWithin()方法，在当前数组内部，将指定位置的成员复制到其他位置（会覆盖原有成员），然后返回当前数组。

    它接受三个参数。

    - target（必需）：从该位置开始替换数据。如果为负值，表示倒数。
    - start（可选）：从该位置开始读取数据，默认为 0。如果为负值，表示从末尾开始计算。
    - end（可选）：到该位置前停止读取数据，默认等于数组长度。如果为负值，表示从末尾开始计算。
    ```
    // 将3号位复制到0号位
    [1, 2, 3, 4, 5].copyWithin(0, 3, 4)
    // [4, 2, 3, 4, 5]

    // -2相当于3号位，-1相当于4号位
    [1, 2, 3, 4, 5].copyWithin(0, -2, -1)
    // [4, 2, 3, 4, 5]

    // 将3号位复制到0号位
    [].copyWithin.call({length: 5, 3: 1}, 0, 3)
    // {0: 1, 3: 1, length: 5}

    // 将2号位到数组结束，复制到0号位
    let i32a = new Int32Array([1, 2, 3, 4, 5]);
    i32a.copyWithin(0, 2);
    // Int32Array [3, 4, 5, 4, 5]
    ```

5. 数组实例的 find() 和 findIndex()

    数组实例的find方法，用于找出第一个符合条件的数组成员。如果没有符合条件的成员，则返回undefined。

    数组实例的findIndex方法，返回第一个符合条件的数组成员的位置，如果所有成员都不符合条件，则返回-1。

    回调函数可以接受三个参数，依次为当前的值、当前的位置和原数组。

    这两个方法都可以接受第二个参数，用来绑定回调函数的this对象。

    > **注意:** 这两个方法都可以发现NaN，弥补了数组的indexOf方法的不足。
    ```
    [NaN].findIndex(y => Object.is(NaN, y)) //0
    ```