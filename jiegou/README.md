### 1. 变量的解构赋值
#### ES6可以写成这样，
    var [a, b, c] = [1, 2, 3];
    console.log(a);
    console.log(b);
    console.log(c);
#### 可以从数组中提取值，按照对应位置，对变量赋值。下面是一些使用嵌套数组进行解构的列子；
    let [foo2, [[bar2], baz]] = [1, [[2], 3]];
    console.log(foo2);  // 1
    console.log(bar2);  // 2
    console.log(baz);  // 3

    let [, , third] = ['foo', 'bar', 'baz'];
    console.log(third); // baz

    let [x1, , y1] = [1, 2, 3];
    console.log(x1); // 1
    console.log(y1); // 3

    let [head, ...tail] = [1, 2, 3, 4];
    console.log(head); // 1
    console.log(tail); // [2, 3, 4]

    let [x2, y2, ...z2] = ['a'];
    console.log(x2); // 'a'
    console.log(y2); // undefined
    console.log(z2); // []

    var [foo3] = [];
    var [bar4, foo4] = [1];
    console.log(foo3);  // undefined
    console.log(bar4);  // 1
    console.log(foo4);  // undefined
#### 另一种情况是不完全解构，即等号左边的模式，只匹配一部分等号右边的数组；如下代码：
    let [x3, y3] = [1, 2, 3];
    console.log(x3); // 1
    console.log(y3); // 2

    let [a2, [b2], d2] = [1, [2, 3], 4];
    console.log(a2); // 1
    console.log(b2); // 2
    console.log(d2); // 4
#### 如果左边是数组，右边不是一个数组的话，那么将会报错。
    // 报错
    let [f] = 1;
    let [f] = false;
    let [f] = NaN;
    let [f] = undefined;
    let [f] = null;
    let [f] = {};
### 2. 默认值
#### 解构赋值允许指定默认值。如下代码：
    var [foo = true] = [];
    console.log(foo); // true

    var [x, y = 'b'] = ['a'];
    console.log(x); // a
    console.log(y); // b

    var [x2, y2 = 'b'] = ['a', undefined];
    console.log(x2); // a
    console.log(y2); // b
#### ES6内部使用严格相等运算符(===)，判断一个位置是否有值，所以，如果一个数组成员不严格等于undefined，默认值是不会生效的。
    var [x = 1] = [undefined];
    console.log(x); // 1

    var [x2 = 2] = [null];
    console.log(x2); // null
#### 上面代码中，如果一个数组成员是null，默认值就不会生效的；因此x2为null；
#### 但是如果默认值是一个表达式，那么这个表达式是惰性求值的，即只有在用到的时候，才会求值的。如下代码：
    function f() {
      console.log('aaa');
    }
    let [x2 = f()] = [1]; 
    console.log(x2); // 1
### 3. 对象的解构赋值
#### 解构不仅可以用于数组，还可以用于对象。
    var { foo, bar } = { foo: 'aaa', bar: 'bbbb'};
    console.log(foo); // aaa
    console.log(bar); // bbbb
#### 对象的解构与数组有一个重要的不同，数组的元素是按顺序排序的，变量的取值是由他的位置决定的，而对象的属性没有顺序的，变量必须
和属性同名，才能取到正确的值。
    var { bar, foo } = { foo: "aaa", bar: "bbb" };
    console.log(foo); // "aaa"
    console.log(bar); // "bbb"

    var { baz } = { foo: "aaa", bar: "bbb" };
    console.log(baz); // undefined
### 4. 字符串的解构赋值
#### 字符串被转换成了一个类似数组的对象。
    const [a, b, c, d, e] = 'hello';
    console.log(a); // h
    console.log(b); // e
    console.log(c); // l
    console.log(d); // l
    console.log(e); // o
#### 类似数组的对象有一个length属性，还可以对这个对象进行解构赋值。
    let {length: len} = 'hello';
    console.log(len); // 5
### 5. 函数参数的解构赋值
#### 函数参数也可以使用解构赋值，如下代码：
    function add([x, y]) {
      return x + y;
    }
    console.log(add([1, 2])); // 3

    function move({x = 0, y = 0} = {}) {
      return [x, y];
    }
    console.log(move({x:3, y:8})); // [3, 8]
    console.log(move({x: 3})); // [3, 0];
    console.log(move({})); // [0, 0]
    console.log(move()); // [0, 0]
#### 但是下面的写法会得到不一样的结果，如下代码：
    function move({x, y} = {x: 0, y: 0}) {
      return [x, y];
    } 

    console.log(move({x: 3, y: 8})); // [3, 8]
    console.log(move({x:3})); // [3, undefined]
#### 因为move函数指定的参数有默认值，而不是给变量x和y指定默认值，所以会得到与前一种写法不同的结果；
### 5.变量解构的用途
#### 5-1 交换变量的值
    [x, y] = [y, x];
#### 从函数返回多个值
#### 函数只能返回一个值，如果需要返回多个值，只能将他们放在数组或者对象里面返回，但是有了解构赋值，取出这些值就非常方便了；
    // 返回一个数组
    function example() {
      return [1, 2, 3];
    }
    var [a, b, c] = example();
    console.log([a, b, c]); // [1, 2, 3]
    console.log(a); // 1
    console.log(b); // 2
    console.log(c); // 3

    // 返回一个对象
    function example2() {
      return {
        foo: 1,
        bar: 2
      }
    }
    var {foo, bar} = example2();
    console.log(foo); // 1
    console.log(bar); // 2
#### 5-2 函数参数的定义
#### 解构赋值可以方便地将一组参数与变量名对应起来
    // 参数是一组有次序的值
    function f([x, y, z]) { ... }
    f([1, 2, 3]);

    // 参数是一组无次序的值
    function f({x, y, z}) { ... }
    f({z: 3, y: 2, x: 1});
#### 5-3 提取JSON数据
    var jsonData = {
      id: 42,
      status: "OK",
      data: [867, 5309]
    };

    let { id, status, data: number } = jsonData;

    console.log(id, status, number); // 42, 'ok', [867, 5309]
#### 4. 函数参数的默认值，如下代码：
    function move({x = 0, y = 0} = {}) {
      return [x, y];
    }
    console.log(move({x:3, y:8})); // [3, 8]
    console.log(move({x: 3})); // [3, 0];
    console.log(move({})); // [0, 0]
    console.log(move()); // [0, 0]
#### 5 遍历map结构 如下代码：
    var map = new Map();
    map.set('first', 'hello');
    map.set('second', 'world');

    for (let [key, value] of map) {
      console.log(key + " is " + value);
    }

    // first is hello
    // second is world
### 6 输入模块的指定方法
#### 加载模块的时候，往往需要指定输入那些方法，解构赋值可以做这一点
    const { Afun, Bfun } = require('./A')