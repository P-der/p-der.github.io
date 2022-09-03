---
title: classNames源码
date: 2022-09-03 15:34:23
tags:
  - js
categories: 源码阅读笔记
---

- [掘金](https://juejin.cn/post/7139080024522670094)

### classNames 源码分析

- [仓库地址](https://github.com/JedWatson/classnames)
- 功能介绍
  > 这是一个常用的合并 css 中 class 名的工具函数库
- 提前准备 
  - git clone https://github.com/JedWatson/classnames
  - 或者查看 https://github.com/JedWatson/classnames/blob/master/index.js
  - 测试用例中的`assert.equal`为判断是否相等的函数，传入的删除相等的时候就会通过测试用例
- 手动实现：通过阅读 tests 文件夹中的测试用例我们可以逐条实现
    1. 传入参数为单一对象
        ``` js
        assert.equal(
            classNames({
            a: true,
            b: false,
            c: 0,
            d: null,
            e: undefined,
            f: 1,
            }),
            "a f"
        );
        ```
        观察测试可以看出，传入为一个对象，根据 key 对应的 value 选择是否保留 key 到返回值中，由此可以写出以下代码
        ```javascript
        function classNames(obj) {
            let classes = [];
            for (let key in obj) {
                if (obj.hasOwnProperty(key) && obj[key]) {
                    classes.push(key);
                }
            }
            return classes.join(" ");
        }
        ```
    2. 当传入参数为多个基础类型
        ``` js
        assert.equal(classNames("a", 0, null, undefined, true, 1, "b"), "a 1 b");
        ```
        观察测试可以看出，传入多个参数时，根据是否为真值，然后判断为字符串和数字，选择是否保留该参数，由此可以写出以下代码
        ``` js
        function classNames(...arg) {
            let classes = [];
            arg.forEach((element) => {
                if (element) {
                    if (typeof element === "string" || typeof element === "number") {
                        classes.push(element);
                    }
                }
            });
            return classes.join(" ");
        }
        ```
    3. 当传入参数为 1 和 2 两种情况的合集
        ```js
        assert.equal(classNames({ a: true }, "b", 0), "a b");
        ```
        根据测试用例，和前两步写的代码，对代码进行合并，可以写出以下代码
        ```js
        function classNames(...arg) {
            let classes = [];
            arg.forEach((element) => {
                if (element) {
                    if (typeof element === "string" || typeof element === "number") {
                        classes.push(element);
                    } else if (typeof element === "object") {
                        for (let key in element) {
                            if (element.hasOwnProperty(key) && element[key]) {
                                classes.push(key);
                            }
                        }
                    }
                }
            });
            return classes.join(" ");
        }
        ```
    4. 需要去掉两遍的空格
        ```js
        assert.equal(classNames("", "b", {}, ""), "b");
        ```
        因为我们之前的代码包括了处理假值的情况，所以无需修改代码 
    5. 在传入空对象时，返回空字符串    
        ```js
        assert.equal(classNames({}), "");
        ```
        运行下已经写的代码，满足条件进行下一条 
    6. 传入参数为一个字符串数组的时候，需要返回数组的 value 拼接
        ```js
        assert.equal(classNames(["a", "b"]), "a b");
        ```
        根据这种情况修改代码，可以写出以下代码
        ```js
        function classNames(...arg) {
            let classes = [];
            arg.forEach((element) => {
                if (element) {
                    if (typeof element === "string" || typeof element === "number") {
                        classes.push(element);
                    } else if (Array.isArray(element)) {
                        if (element.length) {
                            classes = classes.concat(element);
                        }
                    } else if (typeof element === "object") {
                        for (let key in element) {
                            if (element.hasOwnProperty(key) && element[key]) {
                                classes.push(key);
                            }
                        }
                        }
                    }
                });
            return classes.join(" ");
        }
        ```
    7. 传入参数为字符串数组和字符串
    ```js
    assert.equal(classNames(["a", "b"], "c"), "a b c");
    assert.equal(classNames("c", ["a", "b"]), "c a b");
    ```
    运行下已经写的代码，满足条件进行下一条
    8. 传入参数为多个字符串数组
        ```js
        assert.equal(classNames(["a", "b"], ["c", "d"]), "a b c d");
        ```
        运行下已经写的代码，满足条件进行下一条 
    9. 传入一个数组，并且每个值为基础类型时
        ```js
        assert.equal(classNames(["a", 0, null, undefined, false, true, "b"]), "a b");
        ```
        运行下已经写的代码，发现出现了问题，需要对数组的每一项进行校验，这时我们会有两种选择，因为我们处理过这种逻辑，所以可以抽离这个处理逻辑，这是一种处理方式，还有一种就是可以把数组作为参数，调用我们的 classNames，进行递归调用，这里我们选择第二种处理方式，防止在很多嵌套的情况，这里类似树结构的两种遍历方式，可以写出以下代码
        ```js
        function classNames(...arg) {
            let classes = [];
            arg.forEach((element) => {
                if (element) {
                    if (typeof element === "string" || typeof element === "number") {
                        classes.push(element);
                    } else if (Array.isArray(element)) {
                        if (element.length) {
                            let inner = classNames(...element);
                            classes.push(inner);
                        }
                    } else if (typeof element === "object") {
                        for (let key in element) {
                            if (element.hasOwnProperty(key) && element[key]) {
                                classes.push(key);
                            }
                        }
                    }
                }
            });
            return classes.join(" ");
        }
        ```
    10. 传入参数数组包含数组
        ```js
        assert.equal(classNames(["a", ["b", "c"]]), "a b c");
        ```
        运行下已经写的代码，满足条件进行下一条
    11. 传入数组中包含对象
        ```js
        assert.equal(classNames(["a", { b: true, c: false }]), "a b");
        ```
        运行下已经写的代码，满足条件进行下一条 
    12. 传入参数为数组并且嵌套
        ``` js
        assert.equal(classNames(['a', ['b', ['c', {d: true}]]]), 'a b c d');
        ``` 
        运行下已经写的代码，满足条件进行下一条 
    13. 传入参数中有空数组
        ``` js
        assert.equal(classNames('a', []), 'a');
        ```
        运行下已经写的代码，满足条件进行下一条 
    14. 传入参数中有嵌套空数组
        ``` js
        assert.equal(classNames('a', [[]]), 'a');
        ```
        运行下已经写的代码，发现出现了问题，函数运行结果为`'a '`，多了个空格，查看代码，发现存在递归数组为空的情况，不应该添加，修改代码
        ```js
        function classNames(...arg) {
            let classes = [];
            arg.forEach((element) => {
                if (element) {
                    if (typeof element === "string" || typeof element === "number") {
                        classes.push(element);
                    } else if (Array.isArray(element)) {
                        if (element.length) {
                            let inner = classNames(...element);
                            inner && classes.push(inner);
                        }
                    } else if (typeof element === "object") {
                        for (let key in element) {
                            if (element.hasOwnProperty(key) && element[key]) {
                                classes.push(key);
                            }
                        }
                    }
                }
            });
            return classes.join(" ");
        }
        ```
    15. 测试传入参数对象值为所有为`falsy`和`truthy`的情况
        ``` js
        assert.equal(classNames({
			// falsy:
			null: null,
			emptyString: "",
			noNumber: NaN,
			zero: 0,
			negativeZero: -0,
			false: false,
			undefined: undefined,

			// truthy (literally anything else):
			nonEmptyString: "foobar",
			whitespace: ' ',
			function: Object.prototype.toString,
			emptyObject: {},
			nonEmptyObject: {a: 1, b: 2},
			emptyList: [],
			nonEmptyList: [1, 2, 3],
			greaterZero: 1
		}), 'nonEmptyString whitespace function emptyObject nonEmptyObject emptyList nonEmptyList greaterZero');
        ```
        运行下已经写的代码，满足条件进行下一条 
    16. 当传入参数为对象并有`toString`方法的时候，应返回该方法的调用值
        ``` js
        assert.equal(classNames({
			toString: function () { return 'classFromMethod'; }
		}), 'classFromMethod');
        ```
        根据测试修改代码为以下代码
        ``` js
        function classNames(...arg) {
            let classes = [];
            arg.forEach((element) => {
            if (element) {
                if (typeof element === "string" || typeof element === "number") {
                    classes.push(element);
                } else if (Array.isArray(element)) {
                    if (element.length) {
                        let inner = classNames(...element);
                        inner && classes.push(inner);
                    }
                } else if (typeof element === "object") {
                    if(element.toString === Object.prototype.toString) { // 是否为原装
                        for (let key in element) {
                            if (element.hasOwnProperty(key) && element[key]) {
                                classes.push(key);
                            }
                        }
                    }else {
                        classes.push(element.toString());
                    }
                }
            }
            });
            return classes.join(" ");
        }
        ```
    17. 当传入参数为对象并继承`toString`方法的时候，应返回该方法的调用值
        ``` js
        var Class1 = function() {};
		var Class2 = function() {};
		Class1.prototype.toString = function() { return 'classFromMethod'; }
		Class2.prototype = Object.create(Class1.prototype);

		assert.equal(classNames(new Class2()), 'classFromMethod');
        ```
        运行下已经写的代码，满足条件
    - 根据这些测试用例，其实我们已经完成了`className`的所有功能，这其实就是测试驱动开发
- 手动实现与源码对比
  - 源码使用`apply`，强绑定了this，兼容性更好
  - 源码参数为空的时候提前返回，可以减少阅读者的理解成本
  - 源码对于多次调用的`typeof`判断进行抽离为变量，减少重复判断 
  - 源码提取`hasOwnProperty`方法，使用`call`，强绑定了this
  - 源码实现了一个umd的形式，去导出方法

### 总结
- 学习了源码的实现，使用了umd的导出方式
- 通过测试驱动的形式自己实现了`classNames`，测试可以极大程度上增加代码的健壮性