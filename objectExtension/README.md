### 1. 属性的简洁表示法
    var foo = 'bar';
    var baz = {foo};
    console.log(baz); // {foo: 'bar'}

    // 等价于 
    var baz = {foo: foo};

    // ES6 允许在对象之中，直接写变量，这时，属性名为变量名，属性值为变量值；如下：
    function f(x, y) {
      return {x, y};
    }
    console.log(f(5, 6)); // {x: 5, y: 6}

    // 除了属性简写，方法也可以简写
    var o = {
      method() {
        return 'Hello';
      }
    };
    // 上面的代码等价于如下代码：
    var o = {
      method: function() {
        return 'Hello';
      }
    };
    // 如下代码：
    var birth = '2017/1/1';
    var Person = {
      name: '张三',
      birth,
      hello() {
        console.log('我的名字是', this.name);
        console.log('我的生日是', this.birth);
      }
    }; 
    Person.hello(); // 我的名字是 张三, 生日是 2017/1/1

    // 用于函数的返回值，非常方便
    function getPoint() {
      var x = 1;
      var y = 10;
      return {x, y};
    }
    console.log(getPoint());// {x:1, y:10}
### 2. 属性名表达式
    // ES6 允许字面量 定义对象时，用作为对象的属性名，即把表达式放在括号内
    let propKey = 'foo';
    var obj = {
      propKey: true,
      ['a' + 'bc']: 123
    }
    console.log(obj.propKey); // true
    console.log(obj.abc); // 123

    var lastWord = 'last world';
    var a = {
      'first world': 'hello',
      [lastWord]: 'world'
    };
    console.log(a['first world']);  // hello
    console.log(a['last world']);   // world
    console.log(a[lastWord]);  // world

    // 表达式还可以定义方法名
    let obj2 = {
      ['h' + 'ello']() {
        return 'hi';
      }
    }
    console.log(obj2.hello()); // hi
### 3. Object.is()
#### ES5比较两个值是否相等，只有两个运算符，相等运算符("==")和严格相等运算符("==="), 他们都有缺点，前者会自动转换数据类型，后者NAN不等于自身，以及
+0 等于 -0；ES6有该方法是同值相等算法，Object.is()是比较两个值是否严格相等，和('===')是一个含义；
    console.log(Object.is('foo', 'foo')); // true

    console.log(Object.is({}, {}));  // false

    console.log(+0 === -0) // true
    console.log(NaN === NaN); // false

    console.log(Object.is(+0, -0)); // false
    console.log(Object.is(NaN, NaN)); // true
### 4. Object.assign()
    // Object.assign()方法用于对象的合并，将源对象的所有可枚举属性，复制到目标对象中。第一个参数是目标对象，后面的参数都是源对象。
    var target = { a: 1 };
    var s1 = { b: 2};
    var s2 = { c: 3};
    Object.assign(target, s1, s2);
    console.log(target); // {a:1, b:2, c:3}

    // 注意：如果目标对象与源对象有同名属性，或多个源对象有同名属性，则后面的属性会覆盖前面的属性。
    var target2 = { a: 1, b: 1};
    var s11 = { b:2, c:2 };
    var s22 = {c:3};
    Object.assign(target2, s11, s22);
    console.log(target2);  // { a: 1, b:2, c:3 }
#### 扩展运算符(...) 也可以用于合并两个对象，比如如下代码：
    let ab = { ...a, ...b }; 
#### 等价于
    let ab = Object.assign({}, a, b);
#### 
    // 如果该参数不是对象，则会先转成对象，然后返回
    console.log(typeof Object.assign(2)); // 'object'
#### 由于undefined和null无法转成对象，所以如果它们作为参数，就会报错。
    Object.assign(undefined) // 报错
    Object.assign(null) // 报错

    // Object.assign 方法实现的是浅拷贝，而不是深拷贝，如果对象某个属性值发生改变，那么合并对象的属性值也会发生改变。如下代码：
    var obj1 = {a: {b: 1}};
    var obj2 = Object.assign({}, obj1);
    obj1.a.b = 2;
    console.log(obj2.a.b); // 2

    // 同名属性覆盖
    var target = { a: {b: 'c', d: 'e' }};
    var source = {a: {b: 'hello'}};
    console.log(Object.assign(target, source)); // { a: {b: 'hello'}}

    // Object.assign 可以用来处理数组，但是会把数组视为对象, 
    console.log(Object.assign([1,2,3], [4, 5])); // [4, 5, 3]
    // 上面代码中，Object.assign把数组视为属性名0,1,2的对象，因此原数组的0号属性4覆盖了目标数组的0号属性。
### 5. 常见用途
#### 5-1 为对象添加属性
    // 如下代码：
    class Point {
      constructor(x, y) {
        Object.assign(this, {x, y});
      }
    }
    console.log(new Point(1, 2)); // {x:1, y:2}
#### 5-2 为对象添加方法
    // 2. 为对象添加方法
    var A = function() {};
    A.prototype = {
      init: function() {}
    }
    Object.assign(A.prototype, {
      test(a, b) {

      },
      test2() {

      }
    });

    var AInter = new A();
    console.log(AInter);  // {init: function(){}, test: function(a, b){}, test2: function(){} }
#### 5-3 克隆对象
    // 3. 克隆对象 将原始对象拷贝到一个空对象内，就得到了原始对象的克隆
    function clone(origin) {
      return Object.assign({}, origin);
    }
    var obj = {
      name: 'kongzhi'
    };
    console.log(clone(obj)); // {name: 'kongzhi'}
#### 采用这种方法克隆，只能克隆原始对象自身的值，不能克隆它继承的值，如果想要保持原型继承，如下代码：
    function clonePrototype(origin) {
      let originProto = Object.getPrototypeOf(origin);
      return Object.assign(Object.create(originProto), origin);
    }
    var AA = {
      init: function() {

      }
    }
    AA.prototype = {
      BB: function() {

      }
    }
    var AAP = clonePrototype(AA);
    console.log(AAP);  // {init: function(){}, prototype:xx}
#### 5-4 合并多个对象
### 6 Object.keys() 对象转为数组
    // ES5引入了Object.keys方法，返回一个数组(对象转为数组)，成员是参数对象自身的（不含继承的）所有可遍历属性的键名。
    var obj1 = {
      foo: 'bar',
      baz: 42
    };
    console.log(Object.keys(obj1));  // ['foo', 'baz']
### 6-2 Object.values()
#### 该方法返回一个数组，成员是参数对象自身(不含继承的)所有可遍历属性的键值。
    var obj2 = {
      foo: 'bar',
      baz: 42
    };

    console.log(Object.values(obj2)); // ["bar", 42];

    // 如果参数不是对象，Object.values 会先将其转为对象。最后返回空数组
    console.log(Object.values(42)); // []
    console.log(Object.values(true)); // []
### 6-3 Object.entries()
#### 该方法返回一个数组，成员是参数自身(不含继承的)所有可遍历属性的键值对数组。
    var obj3 = { foo: 'bar', baz: 42};
    console.log(Object.entries(obj3)); // [ ["foo", "bar"], ["baz", 42] ]




