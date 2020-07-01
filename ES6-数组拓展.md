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


