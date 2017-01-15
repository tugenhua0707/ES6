### 1. let的含义及let与var的区别：
#### let 声明的变量只在它所在的代码块有效；如下：
    for (let i = 0; i < 10; i++) {
      console.log(i);
    }
    console.log('aaa');
    console.log(i); // i is not defined
#### 上面代码中，计数器i只在for循环体内有效，在循环体外引用就会报错。如下var代码：
    var a = [];
    for (var i = 0; i < 10; i++) {
      a[i] = function() {
        console.log(i);
      }
    }
    a[6](); // 10
#### 变量i是var声明的，在全局范围内都有效，所以每一次循环，新的i值都会覆盖旧值，导致最后输出的是最后一轮i的值。
#### 但是如果使用let，声明的变量仅在块级作用域内有效，最后输出的是6. 如下：
    var b = [];
    for (let j = 0; j < 10; j++) {
      b[j] = function() {
        console.log(j);
      }
    }
    b[6](); // 6
### 2. 不存在变量提升
#### let 不像var 那样会发生 '变量提升' 现象，因此，变量需要先声明然后再使用，否则报错；
    // var 的情况
    console.log(foo);  // undefined
    var foo = 2;

    // let的情况；
    console.log(bar);  // 报错
    let bar = 2;
#### 3. 暂时性死区
#### 快级作用域内存在let命令，它所声明的变量就绑定在这个区域，不再受外部影响；如下代码：
    var tmp = 123;
    if (true) {
      tmp = 'abc';
      let tmp;
      console.log(tmp); // tmp is not defined
    }
#### 上面代码定于全局变量tmp，但是在快级作用域内let又声明了一个局部变量tmp，导致绑定了这个快级作用域；因此打印出tmp会报错。
### 3. 不允许重复声明
#### let 不允许在相同作用域内，重复声明同一个变量。如下代码排错
    function a() {
      let a = 10;
      var a = 1;
      console.log(a);
    }
    a();
    function a() {
      let a1 = 10;
      let a1 = 1;
      console.log(a1);
    }
    a();
#### 也不能在函数内部重新声明参数。
    function func1(arg) {
      let arg; // 报错
    }
    function func2(arg) {
      {
        let arg; // 不报错
      }
    }
### 4. ES6的块级作用域
    function f1() {
      let n = 5;
      if (true) {
        let n = 10;
      }
      console.log(n); // 5
    } 
    f1()
#### 上面的代码有2个代码块，都声明了变量n，运行后输出5，说明了外层代码块不受内层代码块的影响，如果使用了变量var，那么输出的就是10；
### 二：const命令
#### const 声明一个只读的常量，一旦声明，常量的值就不允许改变。如下代码：
    const a = 1; 
    a = 2; 
    console.log(a);  //报错
#### 也就是说 const 一旦声明了变量，就必须初始化，不能留到以后赋值。如果使用const声明一个变量，但是不赋值，也会报错；如下代码：
    const aa;  // 报错
### 2-1 const的作用域与let命令相同；只在声明所在的块级作用域内有效。
    if (true) {
     const aa = 1;
    } 
    console.log(aa);  // 报错
### 2-2 不可重复声明 (和let一样)
    var message = "Hello!";
    let age = 25;
    // 以下两行都会报错
    const message = "Goodbye!";
    const age = 30;
#### 但是对于复合类型的变量，比如数组，存储的是一个地址，不可改变的是这个地址，即不能把一个地址指向另一个地址，但是对象本身是可变的，比如可以给它添加新的
属性；如下代码：
    const a33 = [];
    a33.push('Hello'); // 可执行
    a33.length = 0;    // 可执行
    a33 = ['55']  // 报错