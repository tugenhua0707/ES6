### ES6的class
    // ES5 定义类
    function Point(x, y) {
      this.x = x;
      this.y = y;
    }
    Point.prototype.toString = function() {
      return '(' + this.x + ', ' + this.y + ')';
    }

    var p = new Point(1, 2);
    console.log(p);

    // ES6的语法,方法之间不需要使用逗号分隔
    class Point2 {
      // 构造函数
      constructor(x, y) {
        
        this.x = x;
        this.y = y;
      }
      toString() {
        return '(' + this.x + ', ' + this.y + ')';
      }
    }

    var p2 = new Point2(1, 2);
    console.log(p2);

    // 类的数据类型就是函数，类本身就指向构造函数
    class Point3 {

    }
    console.log(typeof Point3); // 'function'
    console.log(Point === Point.prototype.constructor); // true

    // ES6 和 ES5 定义原型的区别如下：
    class Point4 {
      constructor(props) {
        
      }
      toString() {}
      toValue() {}
    }
    // 等价于如下ES5的代码
    Point4.prototype = {
      toString() {},
      toValue() {}
    }

    // 也可以使用Object.assign方法合并对象到原型去，如下代码：
    class Point5 {
      constructor(props) {
        
        // ...
      }
    }
    Object.assign(Point5.prototype, {
      toString() {},
      toValue() {}
    });
### class的继承
    // 父类
    class Point {
      init(x, y) {
        console.log('x:'+x, 'y:'+y);
      }
    }

    // 子类继承父类
    class ColorPoint extends Point {
      constructor(x, y, color) {
        super(x, y); // 调用父类的 constructor(x, y)
        this.color = color;
      }
    }
    var c = new ColorPoint(1, 2, 'red');
    console.log(c.init(1, 2)); // 继承父类，因此有init方法 x:1, y:2
    console.log(c.color); // 'red'

    // 子类必须在constructor方法中调用super方法，否则新建实例会报错，这是因为子类没有自己的this对象，而是继承父类的this对象，
    // 如果不调用super方法，子类就得不到this对象。如下代码：
    class Point2 {

    }
    class Color2 extends Point2 {
      constructor() {
        
      }
    }
    let cp = new Color2(); // 报错
#### 注意： 在子类构造函数中，只有调用super之后，才可以使用this关键字，否则会报错，只有super方法才能返回父类的实例。
### Object.getPrototypeOf()
#### 该方法可以用来从子类上获取父类；如下代码：
    // 父类
    class Point {
      init(x, y) {
        console.log('x:'+x, 'y:'+y);
      }
    }

    // 子类继承父类
    class ColorPoint extends Point {
      constructor(x, y, color) {
        super(x, y); // 调用父类的 constructor(x, y)
        this.color = color;
      }
    }
    console.log(Object.getPrototypeOf(ColorPoint) === Point); // true
### Class的静态方法
    // 如果在一个方法中，加上static关键字，就表示该方法不会被实例继承，而是直接通过类来调用
    class Foo {
      static init() {
        return 'hello'
      }
    }
    console.log(Foo.init()); // 'hello'

    var foo = new Foo();
    console.log(foo.init()); // 报错


    // 父类的静态方法，可以被子类继承
    class Foo {
      static init() {
        return 'hello';
      }
    }
    class Bar extends Foo {

    }
    console.log(Bar.init()); // 'hello'

    // 静态方法也可以从super对象上调用
    class Foo2 {
      static init() {
        return 'world';
      }
    }

    class Bar2 extends Foo2 {
      static init2() {
        return super.init() + ', hello';
      }
    }
    console.log(Bar2.init2()); // world, hello
### Class的静态属性和实例属性
    // 静态属性 其他的写法不可以
    class Foo {

    }
    Foo.prop = 1;
    console.log(Foo.prop); // 1

    // 类的实例属性 可以使用等式，写入类的定义之中
    class MyClass {
      myProp = 42;
      constructor() {
        console.log(this.myProp); // 42
      }
    }
    // react 的写法
    class ReactCounter extends React.Component {
      state = {
        count: 0
      };
    }
    // 相当于如下的写法
    class ReactCounter2 extends React.Component {
      constructor(props) {
        super(props);
        this.state = {
          count: 0
        }
      }
    }

    // 还可以使用如下新写法
    class ReactCounter3 extends React.Component {
      constructor(props) {
        super(props);
        this.state = {
          count: 0
        };
      }
      state;
    }




