### 1. Array.from() 将类数组对象转化为真正的数组。
    let arrayLike = {
      '0': 'a',
      '1': 'b',
      '2': 'c',
      length: 3
    };

    // ES5
    var arr1 = [].slice.call(arrayLike); 
    console.log(arr1); // ['a','b','c'];

    // ES6的写法
    let arr2 = Array.from(arrayLike);
    console.log(arr2);  // ['a', 'b', 'c']

    // 对于类似数组的对象是DOM操作返回的NodeList集合，以及函数内部的arguments对象。
    // Array.from都可以将它们转为真正的数组。
    let doms = document.querySelectorAll('p');
    Array.from(doms).forEach(function(p) {
      console.log(p); // 打印dom节点
    });
    // 上面的代码中，querySelectorAll方法返回一个类似的数组对象，只有将该返回类似数组对象转化为真正的数组对象，才能使用forEach.
    // arguments 对象
    function foo(arrs) {
      var args = Array.from(arguments);
      console.log(args);  // ['aa']
    }
    var obj = {
      "0": 'aa'
    };
    foo(obj);

    // 只要部署了Iterator接口的数据结构，比如字符串和Set结构，都可以使用Array.from将其转为真正的数组。
    // 字符串
    console.log(Array.from('hello')); // ['h', 'e', 'l', 'l', 'o']
    // set结构的
    let namesSet = new Set(['a', 'b']);
    console.log(Array.from(namesSet)); // ['a', 'b'];

    // 如果参数是一个真正的数组，那么使用Array.from 也会返回一个真正的数组。
    console.log(Array.from([1, 2, 3])); // [1, 2, 3]

    // 扩展运算符，也可以将其转化为数组的，如下
    // arguments对象
    function foo() {
      var args = [...arguments];
      console.log(args); // []
    }
    foo();

    // NodeList对象
    console.log(...document.querySelectorAll('p')); // 打印dom节点

    // Array.from 还可以接受第二个参数，作用类似于数组的map方法，用来对每个元素进行处理，将处理后的值放入返回的数组。
    // 形式如下：
    console.log(Array.from([1, 2, 3], (x) => x * x));  // [1, 4, 9]
    // 等价如下：
    console.log(Array.from([1, 2, 3]).map(x => x * x)); // [1, 4, 9]

    // 下面可以将数组中布尔值的成员转为0
    console.log(Array.from([1, , 2, , 3], (n) => n || 0)); // [1, 0, 2, 0, 3]
### 2. Array.of() 用于将一组值，转换为数组。
    console.log(Array.of(3, 10, 8)); // [3, 10, 8];
    console.log(Array.of(3)); // [3]
    console.log(Array.of(3).length); // 1

    console.log(Array.of()); // []
    console.log(Array.of(undefined)); // [undefined]
    console.log(Array.of(1)); // [1]
    console.log(Array.of(1, 2)); // [1, 2]

    // Array.of方法可以使用下面的代码模拟实现。
    function ArrayOf() {
      return [].slice.call(arguments);
    }
### 3. 数组实例 copyWithin() 
#### 该实例方法，在当前数组内部，将指定位置的成员复制到其他位置上(会覆盖原有的成员)， 然后返回当前的数组。
    Array.prototype.copyWithin(target, start = 0, end = this.length)
#### target (必须)：从该位置开始替换数据
#### start （可选): 从该位置开始读取数据，默认为0，如果为负值，表示倒数。
#### end(可选): 到该位置前停止读取数据，默认等于数组的长度，如果为负值，表示倒数。
    console.log([1, 2, 3, 4, 5].copyWithin(0, 3)); // [4, 5, 3, 4, 5]
    // 将3号位复制到0号位
    console.log([1, 2, 3, 4, 5].copyWithin(0, 3, 4)); // [4, 2, 3, 4, 5]

    // -2 相当于倒数第二个数字，-1相当于倒数最后一位
    console.log([1, 2, 3, 4, 5].copyWithin(0, -2, -1)); // [4, 2, 3, 4, 5]
### 4. 数组实例的 find()和findIndex() 
#### find()方法用于找出第一个符合条件的数组成员，该参数是一个回调函数，所有的数组成员依次执行该回调函数，直到找到第一个返回值为true的成员，
然后返回该成员，如果没有找到的话，就返回undefined
    console.log([1, 4, 5, 10].find((n) => n > 5)); // 10

    console.log([1, 4, 5, 10].find(function(value, index, arrs){
      return value > 9;
    }));  // 10
#### findIndex()方法 返回第一个符合条件的数组成员的位置，如果没有找到，就返回-1
    console.log([1, 5, 10, 15].findIndex(function(value, index, arrs) {
      return value > 9; // 2 
    }));
### 5. 数组实例 fill()
#### fill方法使用给定值，填充一个数组
    // fill 方法用于空数组初始化非常方便，数组中已有的元素，会被全部抹去
    console.log(['a', 'b', 'c'].fill(7)); // [7, 7, 7]

    // fill方法接收第二个和第三个参数，用于指定填充的起始位置和结速位置
    console.log(['a', 'b', 'c'].fill(7, 1, 2)); // ['a', '7', 'c']

