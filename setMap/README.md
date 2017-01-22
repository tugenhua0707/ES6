### Set
#### set类似于数组，但是成员值都是唯一的，没有重复的值。
#### set 有如下方法
#### 1. add(value): 添加某个值，返回Set结构本身
#### 2. delete(value): 删除某个值，返回一个布尔值，表示删除是否成功。
#### 3. has(value): 返回一个布尔值，表示该值是否为Set的成员。
#### 4. clear() 清除所有的成员，没有返回值。
    var s = new Set();
    [2, 3, 5, 4, 5, 2, 2].map(x => s.add(x));
    for (let i of s) {
      console.log(i); // 2, 3, 5, 4
    }

    // Set函数可以接受一个数组(或类似数组的对象)作为参数，用来初始化。
    var set = new Set([1, 2, 3, 4, 4]);
    console.log([...set]); // [1, 2, 3, 4]

    var items = new Set([1, 2, 3, 4, 4, 5, 6, 5]);
    console.log(items.size); // 6

    function getElems() {
      return [...document.querySelectorAll('p')];
    }
    var s2 = new Set(getElems());
    console.log(s2.size); // 2

### 去除数组重复的成员
    var arrs = [1, 2, 3, 1, 3, 4];
    console.log([...new Set(arrs)]); // [1, 2, 3, 4]

    // 向Set加入值的时候，不会发生类型转换，因此 5 和 "5" 是两个不同的值，但是NaN等于NaN, 如下：
    let s3 = new Set();
    let a = NaN;
    let b = NaN;
    s3.add(a);
    s3.add(b);
    console.log(s3); // {NaN}

    // 但是两个对象是不相等的
    let s4 = new Set();
    s4.add({});
    console.log(s4.size); // 1

    s4.add({});
    console.log(s4.size); // 2

    var s5 = new Set();
    s5.add(1).add(2).add(2);
    console.log(s5.size); // 2
    console.log(s5.has(1)); // true
    console.log(s5.has(2)); // true
    console.log(s5.has(3)); // false
    s5.delete(2);
    console.log(s5.has(2)); // false

    // Array.from 方法也可以将Set结构转为数组
    var items2 = new Set([1, 2, 3, 4, 5]);
    var array = Array.from(items2);
    console.log(array); // [1, 2, 3, 4, 5]

### 去除数组重复的另外一种方法
    function unique(array) {
      return Array.from(new Set(array));
    }
    console.log(unique([1, 2, 2, 3])); // [1, 2, 3]
### 遍历操作
#### Set结构的实列有四个遍历方法
#### 1. keys() 返回键名的遍历器。
#### 2. values() 返回键值的遍历器。
#### 3. entries() 返回键值对的遍历器。
#### 4. forEach() 使用回调函数遍历每个成员。
    let set = new Set(['red', 'green', 'blue']);
    for (let item of set.keys()) {
      console.log(item); // red, green blue
    }

    for (let item of set.values()) {
      console.log(item); // red, green blue
    }

    for (let item of set.entries()) {
      console.log(item); // ['red', 'red']  ['green', 'green'] ['blue', 'blue']
    }

    // Set结构的实列默认可遍历，它的默认遍历生成函数就是它的values方法
    // 因此可以省略values方法，直接使用for.. of 循环遍历Set
    let set2 = new Set(['red', 'green', 'blue']);
    for (let x of set2) {
      console.log(x); // red green blue
    }

    // forEach()方法
    let s3 = new Set([1, 2, 3]);
    s3.forEach((value, key) => console.log(value * 2)); // 2 4 6

    // 遍历的应用 
    // 扩展运算符(...) 内部使用 for...of循环，
    let s6 = new Set(['a', 'b', 'c']);
    let arr6 = [...s6];
    console.log(arr6); // ['a', 'b', 'c']

    // 使用Set可以很容易地实现并集， 交集和差集
    let a = new Set([1, 2, 3]);
    let b = new Set([4, 3, 2]);
    // 并集
    let union = new Set([...a, ...b]);
    console.log(union); // {1, 2, 3, 4}

    // 交集
    let jj = new Set([...a].filter(x => b.has(x)));
    console.log(jj); // {2, 3}

    // 差集
    let diff = new Set([...a].filter(x => !b.has(x)));
    console.log(diff); // {1}
### Map
    // Object结构提供了 '字符串-值'的对应，Map结构提供了值-值的对应，但是'键'的范围不限于字符串，各种类型的值(包括对象)都可以当做键

    var m = new Map();
    var o = {p: 'hello world'};

    m.set(o, 'content');
    console.log(m.get(o)); // 'content'
    console.log(m.has(o)); // true
    console.log(m.delete(o)); // true
    console.log(m.has(o)); // false

    // 作为构造函数，Map也可以接受一个数组作为参数，该数组的成员是一个个表示键值对的数组。
    var map = new Map([
      ['name', '张三'],
      ['title', 'aa']
    ])
    console.log(map.size); // 2
    console.log(map.has('name')); // true
    console.log(map.get('name')); // 张三
    console.log(map.has('title')); // true
    console.log(map.get('title')); // aa

    // 字符串true 和 布尔值true是两个不同的键
    var m2 = new Map([
      [true, 'foo'],
      ['true', 'bar']
    ]);
    console.log(m2.get(true)); // 'foo'
    console.log(m2.get('true')); // 'bar'

    // 如果对同一个键多次赋值，后面的值将覆盖前面的值
    let m3 = new Map();
    m3.set(1, 'aaa').set(1, 'bbb');
    console.log(m3.get(1)); // 'bbb'

    // 如果读取一个未知的键，则返回undefined
    console.log(new Map().get('aaaaassd')); // undefined

    // 对同一个对象引用是读取不到值的，因为对象是比较内存地址的
    var m4 = new Map();
    m4.set(['a'], 555);
    console.log(m4.get(['a'])); // undefined

    m4.set(NaN, 123);
    console.log(m4.get(NaN)); // 123
    m4.set(-0, 123);
    console.log(m4.get(+0)); // 123

    // map 提供三个遍历器生成函数和一个遍历方法
    // 1. keys()， 2. values(), 3. entries(). 4. forEach() Map遍历的顺序就是插入的顺序
    let m5 = new Map([
      ['a', 'no'],
      ['y', 'yes']
    ]);
    for (let key of m5.keys()) {
      console.log(key); // a，y
    }

    for(let value of m5.values()) {
      console.log(value); // 'no', 'yes'
    }

    for (let item of m5.entries()) {
      console.log(item[0], item[1]); // 'a' 'no' 'y' 'yes'
    }

    // Map结构的默认遍历器接口 (Symbol.iterator属性) 就是 entries方法， map[Symbol.iterator] === map.entries
    // Map结构转为数组结构，比较快速的方法可以使用扩展运算符(...)
    let m6 = new Map([
      [1, 'one'],
      [2, 'two'],
      [3, 'three']
    ]);
    console.log([...m6.keys()]); // [1, 2, 3]

    console.log([...m6.values()]); // ['one', 'two', 'three']

    console.log([...m6.entries()]); //[[1, 'one'], [2, 'two'], [3, three]]

    console.log([...m6]); //[[1, 'one'], [2, 'two'], [3, three]]

    // 结合数组的map方法和filter方法，可以实现map的遍历和过滤
    let m0 = new Map().set(1, 'a').set(2, 'b').set(3, 'c');
    let m00 = new Map([...m0].filter(([k, v]) => k < 3)); // {1 => 'a', 2 => 'b'}

    let m11 = new Map([...m0].map(([k, v]) => [k*2, '_'+v])); 
    // {2 => '_a', 4 => '_b', 6 => '_c'}

    // Map转为数组
    let amap = new Map().set(true, 7).set({foo: 3}, ['abc']);
    console.log([...amap]); // [ [true, 7], [{foo: 3}, ['abc'] ] ]

    // 数组转为Map
    console.log(new Map([[true, 7],[{foo:3}, ['abc']]]));// Map {true => 7, Object {foo: 3} => ['abc']}

    // Map转为对象
    function strMapToObj(strMap) {
      let obj = Object.create(null);
      for (let [k, v] of strMap) {
        obj[k] = v;
      }
      return obj;
    }
    let myMap = new Map().set('yes', true).set('no', false);
    console.log(strMapToObj(myMap)); // {yes: true, no: false}

    // 对象转为map
    function objToStrMap(obj) {
      let strMap = new Map();
      for (let k of Object.keys(obj)) {
        strMap.set(k, obj[k]);
      }
      return strMap;
    }
    console.log(objToStrMap({yes: true, no: false})); // [ ['yes', ture], ['no', false]]

    // Map转为json
    // Map转为JSON要区分二种情况，一种情况是,Map的键名都是字符串，这时可以选择转为对象的JSON
    function strMapToJson(strMap) {
      return JSON.stringify(strMapToObj(strMap));
    }
    let myMap2 = new Map().set('yes', true).set('no', false);
    console.log(strMapToJson(myMap2)); // '{"yes": true, "no": false}'

    // 第二种情况是，Map的键名有非字符串，这时候可以选择转为数组的JSON
    function mapToArrayJson(map) {
      return JSON.stringify([...map]);
    }
    let myMap3 = new Map().set(true, 7).set({foo: 3}, ['abc']);
    console.log(mapToArrayJson(myMap3)); // '[[true, 7], [{"foo": 3}, ["abc"]]]'

    // JSON转为Map  
    // 正常情况下，所有键名都是字符串
    function jsonToStrMap(jsonStr) {
      return objToStrMap(JSON.parse(jsonStr));
    }
    console.log(jsonToStrMap('{"yes": true, "no": false}')); // Map {'yes' => true, 'no' => false}

    // 有一种特殊情况，整个JSON就是一个数组，且每个数组成员本身，又是一个有两个成员的数组。这时，它可以一一对应地转为Map。这往往是数组转为JSON的逆操作。
    function jsonToMap(jsonStr) {
      return new Map(JSON.parse(jsonStr));
    }

    jsonToMap('[[true,7],[{"foo":3},["abc"]]]')
    // Map {true => 7, Object {foo: 3} => ['abc']}





