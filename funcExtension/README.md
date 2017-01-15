### 函数的扩展
#### 1. 函数参数的默认值
#### ES6允许为函数的参数设置默认值，即直接写在参数定义的后面。
    function log(x, y = 'world') {
      console.log(x, y);
    }
    log('Hello'); // Hello world
    log('Hello', 'China');  // Hello China
    log('Hello', ''); // Hello

    function Point(x = 0, y = 0) {
      this.x = x;
      this.y = y;
    }
    var p = new Point();
    console.log(p.x); // 0
    console.log(p.y); // 0
#### ES6的写法好处： 
##### 1. 阅读代码的人，可以立刻意识到那些参数是可以省略的，不用查看函数体或文档。
##### 2. 有利于将来的代码优化，即使未来的版本在对外接口中，彻底拿调这个参数，也不会导致以前的代码无法执行；
##### 参数变量默认声明的，所以不能用let或const再次声明。
    function foo(x = 5) {
      let x = 1; // error
      const x = 2; // error
    }
#### 2. 与解构赋值默认值结合使用。
#### 参数默认值可以与解构赋值的默认值，结合起来使用。
    function foo({x, y = 5}) {
      console.log(x, y);
    }
    foo({});  // undefined, 5
    foo({x: 1});  // 1, 5
    foo({x:1, y:2});  //1, 2
    foo(); // Uncaught TypeError: Cannot read property 'x' of undefined
#### 只有当函数的参数是一个对象时，变量x与y 才会通过解构赋值而生成，如果函数调用的不是一个对象的话，变量x就不会生成，从而导致出错;
#### 下面是另一个对象的解构赋值的列子
    function fetch(url, { body = '', method='GET', headers = {}}) {
      console.log(method);
    }
    fetch('http://www.baidu.com', {}); // 'GET'
    fetch('http://www.baidu.com'); // 报错  Uncaught TypeError: Cannot read property 'body' of undefined
#### 上面的代码不能省略第二个参数，如果结合函数参数的默认值，就可以省略第二个参数。这样就出现了双重默认值；如下代码：
    function fetch(url, { method = 'GET', body = '' } = {} ) {
      console.log(method);
    }
    fetch('http://www.baidu.com'); // GET
#### 再看下面两种写法有什么差别
    // 写法1
    function m1({x = 0, y = 0} = {}) {
      return [x, y];
    }

    // 写法2
    function m2({x, y} = { x: 0, y: 0}) {
      return [x, y];
    }

    // 函数没有参数的情况下 
    console.log(m1());  // [0, 0]
    console.log(m2());  // [0, 0]

    // x 和 y都有值的情况下
    console.log(m1({x:3, y: 8}));   // [3, 8]
    console.log(m2({x:3, y: 8}));   // [3, 8]

    // x有值，y无值的情况下 
    console.log(m1({x:3}));  // [3, 0]
    console.log(m2({x: 3}));  // [3, undefined]

    // x 和 y都无值的情况下 
    console.log(m1({}))  // [0, 0]
    console.log(m2({}))  // [undefined, undefined]

    console.log(m1({z:3}));  // [0, 0]
    console.log(m2({z:3}));  // [undefined, undefined]
#### 上面两种写法都对函数的参数设定了默认值，区别是写法一函数的默认值是空对象，但是设置了对象解构赋值的默认值，写法2函数的参数的默认值是一个有具体属性的对象，
但是没有设置对象解构赋值的默认值。
#### 3. 参数默认值的位置
#### 一般情况下，定义了默认值的参数，应该是函数的尾参数，因为这样比较容易看出来，到底省略了那些参数，如果非尾部参数设置默认值，这个参数是无法省略的。
    // demo 1
    function f(x = 1, y) {
      return [x, y];
    }
    console.log(f()); // [1, undefined]
    console.log(f(2)); // [2, undefined]
    f(, 1); // 报错

    // demo 2
    function f(x, y = 5, z) {
      return [x, y, z];
    }
    console.log(f());  // [undefined, 5, undefined]
    console.log(f(1));  // [1, 5, undefined]
    console.log(f(1, , 2)); // 报错
    console.log(f(1, undefined, 2));  // [1, 5, 2]

    function foo(x = 5, y = 6) {
      console.log(x, y);
    }
    foo(undefined, null);  // 5, null
#### 4. 函数的作用域
#### 如果参数默认值是一个变量，则该变量所处的作用域，与其他变量的作用域规则是一样的，则先是当前作用域，再是全局作用域。
    var x = 1;
    function f(x, y = x) {
      console.log(y);
    }
    console.log(f(2));  // 2
#### 如果在调用时候，函数作用域内部的变量x没有生成，结果不一样。
    let x = 1;
    function f(y = x) {
      let x = 2;
      console.log(y); // 1
    }
    f();
#### 但是如果，全局变量x不存在的话， 就会报错
    function f( y = x) {
      let x = 2;
      console.log(y);
    }
    f(); // 报错 x is not defined
#### 5. rest参数
#### ES6引入rest参数(形式为 "...变量名")，用于获取函数的多余参数，rest参数搭配的变量是一个数组，该变量将多余的参数放入一个数组中。
    function add(...values) {
      let sum = 0;
      for(let i of values) {
        sum += i;
      }
      return sum;
    }
    console.log(add(2, 3, 5)); // 10
#### rest参数中的变量代表一个数组，所以数组特有的方法都可以用于这个变量。下面是一个利用rest参数改写数组push方法的例子。
    function push(array, ...items) {
      items.forEach(function(item) {
        array.push(item);
        console.log(item);
      });
    }

    var a = [];
    push(a, 1, 2, 3);  // 1,2,3
#### 注意，rest参数之后不能再有其他参数（即只能是最后一个参数），否则会报错。
    // 报错
    function f(a, ...b, c) {
      // ...
    }
#### 6. 扩展运算符
#### 扩展运算符(spread) 是三个点 (...)， 它好比rest参数的逆运算，将一个数组转为用逗号分隔的参数序列。
    console.log(...[1, 2, 3]); // 1 2 3
    console.log(1, ...[2, 3, 4], 5); // 1 2 3 4 5

    function add(x, y) {
      return x + y;
    }

    var numbers = [4, 38];
    console.log(add(...numbers)); // 42
#### 上面的代码 使用了扩展运算符，该运算符是一个数组，变为参数序列。
#### 替代数组的apply方法
#####扩展运算符可以展开数组，所以不需要apply方法，将数组转为函数的参数。
    // ES5
    function f(x, y, z) {
      console.log(x); // 0
      console.log(y); // 1
      console.log(z); // 2
    }
    var args = [0, 1, 2];
    f.apply(null, args);

    // ES6
    function f2(x, y, z) {
      console.log(x); // 0
      console.log(y); // 1
      console.log(z); // 2
    }
    var args = [0, 1, 2];
    f(...args); 

    // ES5的写法
    console.log(Math.max.apply(null, [14, 3, 77]))  // 77
    // ES6的写法
    console.log(Math.max(...[14, 3, 77]))  // 77
    // 等同于
    console.log(Math.max(14, 3, 77));  // 77

    // ES5的写法
    var arr1 = [0, 1, 2];
    var arr2 = [3, 4, 5];
    Array.prototype.push.apply(arr1, arr2);
    console.log(arr1); // 0,1,2,3,4,5

    // ES6的写法
    var arr1 = [0, 1, 2];
    var arr2 = [3, 4, 5];
    arr1.push(...arr2);
    console.log(arr1); // 0,1,2,3,4,5
#### 扩展运算符的运用
##### 1. 合并数组
    var arr1 = ['a', 'b'];
    var arr2 = ['c'];
    var arr3 = ['d', 'e'];

    // ES5 合并数组
    var rets1 = arr1.concat(arr2, arr3);
    console.log(rets1); // ["a", "b", "c", "d", "e"]

    // ES6合并数组
    var rets2 = [...arr1, ...arr2, ...arr3];
    console.log(rets2); // ["a", "b", "c", "d", "e"]
##### 2. 与解构赋值组合
##### 扩展运算符可以与解构赋值结合起来，用于生成数组。
    const [first, ...rest1] = [1, 2, 3];
    console.log(first); // 1
    console.log(rest1); // [2, 3]

    const [first2, ...rest2] = [];
    console.log(first2); // undefined
    console.log(rest2); // []

    const [first3, ...rest3] = ['foo'];
    console.log(first3); // foo
    console.log(rest3); // []
#### 如果将扩展运算符用于数组赋值，只能放在参数的最后一位，否则会报错
    const [...butLast, last] = [1, 2, 3, 4, 5]; // 报错
    const [first, ...middle, last] = [1, 2, 3, 4, 5]; // 报错
#### 扩展运算符可以将字符串转为真正的数组
    [...'hello']
    // [ "h", "e", "l", "l", "o" ]

    var f = v => v;
    console.log(f(2)); // 2

    // 等价于如下函数
    var f = function(v) {
      return v;
    }

    // 如果箭头函数不需要参数或需要多个参数的话，可以使用圆括号代表参数的部分
    var f2 = () => 5;
    // 等价于如下
    var f = function() {
      return 5;
    }

    var sum = (n1, n2) => n1 + n2;

    console.log(sum(1, 2)); // 3

    // 等价于 如下函数
    var sum = function(n1, n2) {
      return n1 + n2;
    }

    //  或者也可以这样写 
    var sum2 = (n1, n2) => {
      return n1 + n2;
    }
    console.log(sum2(2, 3)); // 5

    // 如果要返回一个对象的话， 需要使用圆括号括起来
    var getData = id => ({id: id});
    console.log(getData(1)); // {id: 1}

    // 正常函数的写法 
    [1, 2, 3].map(function(x) {
      return x * x;
    });

    // 箭头函数的写法 
    [1, 2, 3].map(x => x * x);
#### 7. 函数参数的尾逗号
##### ECMASCRIPT2017 允许函数的最后一个参数有尾逗号。
    // ES217
    function douhao(a, b,) {
      // 正常
    }
    console.log(douhao(1,2,))