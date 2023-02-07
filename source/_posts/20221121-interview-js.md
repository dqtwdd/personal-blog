---
title: 面试准备
tags: ['工作']
categories: 工作
date: 2021-01-06
---

记录工作中想到的面试应该会的问题。

<!--more-->

## 2.JS 基础和 ES6 相关知识

### 2.1 闭包

- 闭包是指有权访问另外一个函数作用域中的变量的函数

- 闭包的特性：
  
  - 函数内再嵌套函数
  - 内部函数可以引用外层的参数和变量
  - 参数和变量不会被垃圾回收机制回收

- 说说你对闭包的理解
  
  - 使用闭包主要是为了设计私有的方法和变量。
  - 闭包的优点是可以避免全局变量的污染。
  - 缺点是闭包会常驻内存，会增大内存使用量，使用不当很容易造成内存泄露。

- 闭包的使用场景
  
  - 防抖、节流函数
    
    ```javascript
    // 防抖
    export function debounce(fn, wait, immediate) {
      let timeout;
      const immediates = immediate || false;
      return function () {
        const context = this;
        const args = arguments;
        const later = function () {
          timeout = null;
          if (!immediates) fn.apply(context, args);
        };
        const call = immediates && !timeout;
        clearTimeout(timeout);
        if (call) return fn.apply(context, args);
        timeout = setTimeout(later, wait);
      };
    }
    // 使用
    function a(b, c) {
      console.log(b, c);
      return b + c;
    }
    let de = debounce(a, 3, true);
    console.log(de(1, 2));
    
    // 节流
    export function throttle(fn, wait) {
      let timeout;
      return function () {
        const context = this;
        const args = arguments;
        if (timeout) return;
        timeout = setTimeout(function () {
          fn.apply(context, args);
          timeout = null;
        }, wait);
      };
    }
    ```

### 2.2 内存泄漏

- 概念
  
  当一个对象已经不需要再使用本该被回收时，另外一个正在使用的对象持有它的引用从而导致它不能被回收，这导致本该被回收的对象不能被回收而停留在堆内存中，这就产生了内存泄漏。

- 造成内存泄露的原因：
  
  - 意外的全局变量(在函数内部没有使用 var 进行声明的变量)
  - console.log
  - 闭包
  - 对象的循环引用
  - 未清除的计时器
  - DOM 泄露(获取到 DOM 节点之后，将 DOM 节点删除，但是没有手动释放变量，拿对应的 DOM 节点在变量中还可以访问到，就会造成泄露)

- 例子
  
  ```javascript
  // DOM泄露
  //这段代码会导致内存泄露
  window.onload = function () {
    var el = document.getElementById('id');
    el.onclick = function () {
      alert(el.id);
    };
  };
  
  //解决方法为
  window.onload = function () {
    var el = document.getElementById('id');
    var id = el.id; //解除循环引用
    el.onclick = function () {
      alert(id);
    };
    el = null; // 将闭包引用的外部函数中活动对象清除
  };
  // 闭包导致的内存泄漏
  var fn = function () {
    var sum = 0;
    return function () {
      sum++;
      console.log(sum);
    };
  };
  fn1 = fn();
  fn1(); //1
  fn1(); //2
  fn1(); //3
  // 这之前，如果fn1一直存在，闭包中的sum就一直不会被内存回收，就造成了内存泄露
  fn1 = null; // fn1的引用fn被手动释放了 这样就被回收了
  fn1 = fn(); //num再次归零
  fn1(); //1
  ```

### 2.3 事件循环机制-----宏任务（macrotask ）和微任务（microtask ）

**下面的重点，只在 node 版本 11 之前有体现！！！！！**

**在 node 11 版本中，node 下 Event Loop 已经与浏览器趋于相同。
在 node 11 版本中，node 下 Event Loop 已经与浏览器趋于相同。
在 node 11 版本中，node 下 Event Loop 已经与浏览器趋于相同。 **

**重点来啦：**node 环境和浏览器环境对于微任务和宏任务的执行顺序是不一样的，主要体现在：

- node 环境中，微任务是事件循环的各个阶段之间执行。
- 浏览器环境中，微任务在事件循环的宏任务执行完成后执行。

![EventLoop](https://dqtwdd.top/cdn/img/EventLoop.png)

来一个挺经典的例子：

```javascript
setTimeout(() => {
  console.log('timer1');
  Promise.resolve().then(function () {
    console.log('promise1');
  });
}, 0);
setTimeout(() => {
  console.log('timer2');
  Promise.resolve().then(function () {
    console.log('promise2');
  });
}, 0);
// 浏览器 timer1 promise1 timer2 promise2
// node  timer1 timer2 promise1 promise2
```

可以看到以上代码在浏览器中和在 node 环境中的执行结果是不同的，具体用两张动图说明下原因：

![Browser](https://dqtwdd.top/cdn/img/Browser.gif)

![Node](https://dqtwdd.top/cdn/img/Node.gif)

- macrotask 和 microtask 表示异步任务的两种分类。
  
  在挂起任务时，JS 引擎会将所有任务按照类别分到这两个队列中，首先在 macrotask 的队列（这个队列也被叫做 task queue）中取出第一个任务，执行完毕后取出 microtask 队列中的所有任务顺序执行；之后再取 macrotask 任务，周而复始，直至两个队列的任务都取完。

- 宏任务
  
  | #                     | 浏览器 | Node |
  | --------------------- | --- | ---- |
  | setTimeout            | √   | √    |
  | setInterval           | √   | √    |
  | setImmediate          | x   | √    |
  | requestAnimationFrame | √   | x    |

- 微任务
  
  | #                          | 浏览器 | Node |
  | -------------------------- | --- | ---- |
  | process.nextTick           | x   | √    |
  | MutationObserver           | √   | x    |
  | Promise.then catch finally | √   | √    |

```javascript
//主线程直接执行
console.log('1');
//丢到宏事件队列中
setTimeout(function () {
  console.log('2');
  process.nextTick(function () {
    console.log('3');
  });
  new Promise(function (resolve) {
    console.log('4');
    resolve();
  }).then(function () {
    console.log('5');
  });
});
//微事件1
process.nextTick(function () {
  console.log('6');
});
//主线程直接执行
new Promise(function (resolve) {
  console.log('7');
  resolve();
}).then(function () {
  //微事件2
  console.log('8');
});
//丢到宏事件队列中
setTimeout(function () {
  console.log('9');
  process.nextTick(function () {
    console.log('10');
  });
  new Promise(function (resolve) {
    console.log('11');
    resolve();
  }).then(function () {
    console.log('12');
  });
});
// 打印结果：1 7 6 8 2 4 3 5 9 11 10 12
```

### 2.4 call、apply 和 bind

- 相同点

- 都改变了 this 的指向

- 区别
  
  - call/apply 返回的是函数执行的结果，bind 返回的是函数。
    
    ```javascript
    function personInit(name, age) {
      let person = {
        name: '',
        age: '',
        getInfo: function (nameParam, ageParam) {
          console.log(this.name, ' ', this.age, ' ', nameParam, ' ', ageParam);
        },
      };
      person.name = name;
      person.age = age;
      console.log(this, arguments);
      return person;
    }
    let father = new personInit('小王', 28);
    let daughter = new personInit('小小王', 2);
    
    daughter.getInfo(); // '小小王' 2
    daughter.getInfo.apply(father); // '小王' 28
    daughter.getInfo.bind(father)(); // '小王' 28
    ```
  
  - call、bind 传递第二个参数时可以直接传入，用','隔开，apply 传入第二个参数时必须传入[]
    
    ```javascript
    daughter.getInfo.call(father, '小董', 27); // 小王   28   小董   27
    daughter.getInfo.apply(father, ['小董', 27]); // 小王   28   小董   27
    daughter.getInfo.bind(father, '小董', 27)(); // 小王   28   小董   27
    daughter.getInfo.bind(father, ['小董', 27])(); // 小王   28   ["小董", 27]  undefined
    ```

### 2.5 原型与原型链

提一个方法：instanceof

语法：`object instanceof constructor`

作用： `instanceof` 运算符用来检测 `constructor.prototype `是否存在于参数 `object` 的原型链上。

- 原型
  
  - 定义
    
    **每一个 javascript 对象(除 null 外)创建的时候，就会与之关联另一个对象，这个对象就是我们所说的原型，每一个对象都会从原型中“继承”属性。**
    
    **原型是一个** **_对象_**。
    
    - Function => prototype
      
      Js 中每个函数都有一个 prototype 属性，这个属性指向函数的**原型** **_对象_**。
    
    ```javascript
    function personInit() {}
    personInit.prototype.name = '小王';
    let person = new personInit();
    person.name; // '小王'
    ```
    
    - Object => \_\_proto\_\_
      
      是每个对象(除 null 外)都会有的属性，叫做\_\_proto\_\_，这个属性会指向该对象的**原型** **_对象_**。
    
    ```javascript
    function personInit() {}
    let person = new personInit();
    console.log(person.__proto__ === Person.prototype); // true
    ```
    
    ![Object](https://dqtwdd.top/cdn/img/Object.png)
    
     Object 的 \_\_proto\_\_和 Function 的 prototype 都指向原型对象。
    
    - constructor
      
      每个原型都有一个 constructor 属性，指向该关联的构造函数。
      
      ```javascript
      function Person() {}
      
      var person = new Person();
      
      console.log(person.__proto__ == Person.prototype); // true
      console.log(Person.prototype.constructor == Person); // true
      console.log(person.constructor === Person); // true
      // 顺便学习一个ES5的方法,可以获得对象的原型
      console.log(Object.getPrototypeOf(person) === Person.prototype); // true
      ```
      
      ![Constructor](https://dqtwdd.top/cdn/img/Constructor.png)
    
    ![ProtoOfProto](https://dqtwdd.top/cdn/img/ProtoOfProto.png)
    
    Object 的 \_\_proto\_\_ 和 Function 的 prototype 都指向原型对象，修改原型对象参数时，原型对象的构造函数构造出来的对象也会改变。
    
    当读取实例的属性时，如果找不到，就会查找与对象关联的原型中的属性，如果还查不到，就去找原型的原型，一直找到最顶层为止。
    
    ```javascript
    function Person() {}
    
    var person = new Person();
    
    Person.prototype.__proto__.name = '小王';
    
    let a = {};
    
    console.log(a, a.name); // 小王
    ```

- 原型链
  
  - 概念
    
    每个函数都有一个 prototype 属性，每个函数构造出来的实例都有一个\_\_proto\_\_ 属性，他们都指向构造函数的原型对象，对应的，构造函数的原型对象也有自己的\_\_proto\_\_属性，指向自己的原型，这就是原型链。
    
    ![ProtoChain](https://dqtwdd.top/cdn/img/ProtoChain.png)
    
    ![proto](https://dqtwdd.top/cdn/img/proto.png)
    
    从上面的图片不难看出，Person 和 Object 这个两个构造函数都是 Object.prototype 的儿子。

### 2.6 继承

继承就是让子类继承父类的属性。

类是什么，类是对象的抽象（我觉得也是对象）。

继承有六种，分别是 原型链继承，构造继承，实例继承，拷贝继承，组合继承，寄生组合继承。

我们先构造一个父构造函数

```javascript
function Person(name, age) {
  this.name = name || '姓名';
  this.age = age || 999;
  this.getName = function () {
    console.log(this.name);
  };
}
// 原型方法
Person.prototype.getAge = function () {
  console.log(this.age);
};
// 实验一下，构造一个实例
let Wdd = new Person('小王', 27);
Wdd.getName(); // '小王'
Wdd.getAge(); // 27
```

#### 2.6.1 原型链继承

- 定义：将父类的实例作为子类的原型 。

```javascript
function Black() {
  this.skin = 'black';
  this.hobby = function () {
    console.log('R&B');
  };
}
Black.prototype = new Person();
let black = new Black(); // 可以看见，创建子类的实例时，无法向父类的构造函数传参。
black.getName(); // '姓名'
black.getAge(); // '999'
console.log(black.skin); //'black'
black.hobby(); // 'R&B'
black instanceof Black; // true
black instanceof Person; // true
```

可以看见 Black 的实例 black 继承了所有来自于父类的的属性。

- 特点
  - 简单，容易实现。
  - 原型对象的改变能影响所有后代实例。
- 缺点
  - 无法实现多继承。子类的构造函数只能继承一个父类的属性。
  - 创建子类的实例时，无法向父类的构造函数传参。
  - 来自原型对象的所有属性被所有后代实例共享。

#### 2.6.2 构造继承

- 定义：改变父类构造函数的 this 指向，使子类获得父类所有的属性和方法。
  
  区别于拷贝继承的是，构造继承只让子类获取了父类的属性和方法，没有继承父类原型的属性和方法。
  
  ```javascript
  function Black(name, age) {
    Person.call(this, name, age);
    this.skin = 'black';
    this.hobby = function () {
      console.log('R&B');
    };
  }
  let black = new Black('James', 5);
  black.getName(); // 'James'
  black.getAge(); // Uncaught TypeError: black.getAge is not a function
  console.log(black); //'black'
  black.hobby(); // 'R&B'
  black instanceof Black; // true
  black instanceof Person; // false
  ```

- 特点
  
  - 可以在子类创建时使用父类构造函数传递参数
  - 可以实现多继承
  - 子类的实例就是子类的实例，不是父类的实例。
  - 父类构造函数原型的改变不会影响子类。

- 缺点
  
  - 父类原型上的的属性和方法不会继承。
  - 无法实现多级集成：比如 祖父类=》父类 使用的是原型链继承，那么父类可以使用祖父的所有属性和方法，子类=》父类使用构造继承的话 子类是无法使用祖父类的方法和属性的。每一个子类如果想要使用祖父类和父类的话需要分别进行。

#### 2.6.3 实例继承（寄生继承）

- 定义：使用父类构造函数创建实例，为实例添加新特性后，作为子类实例返回。
  
  这个就很神奇，就如同它的名字一样，通过子类构造函数创造的实例却不是子类的实例
  
  ```javascript
  function Black() {
    let instance = new Person();
    instance.skin = 'black';
    instance.hobby = function () {
      console.log('R&B');
    };
    return instance;
  }
  let black = new Black();
  black.getName(); // 'James'
  black.getAge(); // Uncaught TypeError: black.getAge is not a function
  console.log(black); //'black'
  black.hobby(); // 'R&B'
  black instanceof Black; // false
  black instanceof Person; // true
  ```

- 特点
  
  - 创建的实例时父类的实例，不是子类的实例。

- 缺点
  
  - 不支持多继承。

#### 2.6.4 拷贝继承

- 定义：创建父类的实例后，遍历父级实例，将父级实例原型上的方法赋给子类构造函数的原型对象。
  
  ```javascript
  function Black() {
    let instance = new Person();
    for (let key in instance) {
      Black.prototype[key] = instance[key];
    }
    this.skin = 'black';
    this.hobby = function () {
      console.log('R&B');
    };
  }
  let black = new Black();
  black.getName(); // '姓名'
  black.getAge(); // '999'
  black.hobby(); // 'R&B'
  black instanceof Black; // true
  black instanceof Person; // false
  ```

- 特点
  
  - 支持多继承。
  - 可以继承父类的原型方法，但是不是父类的实例。

- 缺点
  
  - 执行效率低（因为需要遍历拷贝父类的每一条属性）。
  - 无法复制不可遍历属性。

#### 2.6.5 组合继承

- 定义：组合继承即组合了原型链继承和构造继承，使子类即可以向父类构造函数传递参数，又可以能继承父类原型的属性和方法。
  
  ```javascript
  function Black(name, age) {
    Person.call(this, name, age);
    this.skin = 'black';
    this.hobby = function () {
      console.log('R&B');
    };
  }
  Black.prototype = new Person();
  
  Black.prototype.constructor = Black; // 修改子类原型构造函数
  // 子类原型的构造函数不影响子类创建实例，但是我们一般会修正子类原型的构造函数使其指向本身。
  
  let black = new Black('Jone', 5);
  black.getName(); // 'Jone'
  black.getAge(); // '5'
  black.hobby(); // 'R&B'
  black instanceof Black; // true
  black instanceof Person; // true
  ```

- 特点
  
  - 子类即可以向父类构造函数传递参数，又可以能继承父类原型的属性和方法。
  - 既是子类实例，又是父类实例。

- 缺点
  
  - 调用了两次父类的构造函数，创建了两次父类的实例。

#### 2.6.6 寄生组合继承

- 定义：使用构造继承使子类可以使用父类的属性和方法，通过继承方法创建一个父类构造函数的副本使子类的原型指向这个副本，以此访问父类的原型。
  
  ```javascript
  function Black(name, age) {
    Person.call(this, name, age);
    this.skin = 'black';
    this.hobby = function () {
      console.log('R&B');
    };
  }
  function inherit(subType, superType) {
    //在new inheritFn 的时候将构造函数指向子类
    function inheritFn() {
      this.constructor = subType;
    }
    inheritFn.prototype = superType.prototype;
    //将子类的原型指向父类原型的一个副本
    subType.prototype = new inheritFn();
  }
  
  inherit(Black, Person);
  
  let black = new Black('Jone', 5);
  black.getName(); // 'Jone'
  black.getAge(); // '5'
  black.hobby(); // 'R&B'
  black instanceof Black; // true
  black instanceof Person; // true
  ```

- 特点
  
  - 可以实现多继承，但是原型只能指向一个父类的原型。

- 缺点
  
  - 实现比较麻烦。

### 2.7 深拷贝、浅拷贝

#### 2.7.1 基本数据类型：number string boolean undefined null object

- 简单数据类型：number string boolean undefined null
- 复杂数据类型：object

简单数据类型存放在**栈**中，负责数据类型存放在**堆**中。

复杂数据类型赋值时会指向同一个内存地址（既浅拷贝）：

```javascript
let objAncestor = {
  name: 'ancestor',
  age: 999,
};
let objGeneration = objAncestor;
console.log(objGeneration.name); // 'ancestor'
objAncestor.name = 'son';
console.log(objGeneration.name); // 'son'
```

简单数据类型就没有这样的问题：

```javascript
let a = 10;
let b = a;
a = 11;
console.log(a, b); // 11 10
```

#### 2.7.2 typeof 与 instanceof

- typeof 主要用于判断简单数据类型，只能返回 `'number' 'boolean' 'string' 'null' 'undefined' 'object'`
  
  ```javascript
  typeof ''; // 'string'
  typeof 1; // 'number'
  typeof []; // 'object'
  typeof null; // 'object' 历史遗留问题
  typeof true; // 'boolean'
  typeof undefinded; // 'undefined'
  typeof function () {}; //'function'
  ```

- instanceof 可以判断是否为某个构造函数的实例，但是对简单数据类型的判断不准确
  
  ```javascript
  [] instanceof Object  //true
  [] instanceof Array  //true
  {} instanceof Object  //true
  /\d/ instanceof RegExp  //true
  function(){} instanceof Object  //true
  function(){} instanceof Function  //true
  
  '' instanceof String  //false
  1 instanceof Number //false
  ```

- 比较好的办法是使用`Object.prototype.toString.apply()`来判断数据类型。
  
  ```javascript
  Object.prototype.toString.apply([1, 2]); //"[object Array]"
  Object.prototype.toString.apply('str'); //"[object String]"
  Object.prototype.toString.apply(1); //"[object Number]"
  Object.prototype.toString.apply(null); //"[object Null]"
  Object.prototype.toString.apply(); //"[object Undefined]"
  Object.prototype.toString.apply(function () {}); //"[object Function]"
  Object.prototype.toString.apply(true); //"[object Boolean]"
  Object.prototype.toString.apply(new Object()); //"[object Object]"
  ```

#### 2.7.3 深拷贝

- `Object.assign()`方法用于将所有可枚举属性的值从一个或多个源对象分配到目标对象。它将返回目标对象。 此方法可以实现对象内部第一层简单数据类型的深拷贝。

```javascript
let a = { a: 1, b: 2, c: { d: 1 } };
let b = Object.assign({}, a); // 参数中 {} 开辟了新的内存空间，将a中的简单数据类型拷过去了
a.a = 2;
a.c.d = 2;
console.log(a); // {a:2,b:2,c:{d:2}}
console.log(b); // {a:1,b:2,c:{d:2}} 可以看见第一层还是ok的，第二层不行。
```

- `JSON.stringify`和`JSON.parse`实现深拷贝：JSON.stringify 把对象转成字符串，再用 JSON.parse 把字符串转成新的对象；
  
  这个方法的缺点是只能实现转换成 Json 格式的才能用这种方式，如果源中含有 function 的这种方式是拷贝不过去的。
  
  不过这种方式工作中一般的情况都可以应付了。
  
  ```javascript
  // 例1：
  let a = { a: 1, b: 2, c: { d: 1 } };
  function clone(source) {
    let obj = JSON.parse(JSON.stringify(source));
    return obj;
  }
  let b = clone(a);
  a.a = 2;
  a.c.d = 2;
  console.log(a); // {a:2,b:2,c:{d:2}}
  console.log(b); // {a:1,b:2，c:{d:1}}
  // 例2：
  let a = {
    a: 1,
    b: 2,
    c: function () {
      console.log('this is a Function');
    },
  };
  function clone(source) {
    let obj = JSON.parse(JSON.stringify(source));
    return obj;
  }
  let b = clone(a);
  a.a = 2;
  a.c.d = 2;
  console.log(a); // {a:1,b:2,c:function(){console.log('this is a Function')}}
  console.log(b); // {a:1,b:2} 可以看见c直接蒸发了
  ```

- 递归实现深拷贝
  
  ```javascript
  let a = {
    a: 1,
    b: 2,
    c: function () {
      console.log('this is a Function');
    },
  };
  function deepClone(source) {
    let newObj;
    if (source !== null && typeof source === 'object') {
      newObj =
        Object.prototype.toString.call(source) === '[object Array]' ? [] : {};
      for (let key in source) {
        newObj[key] = deepClone(source[key]);
      }
    } else {
      newObj = source;
    }
    return newObj;
  }
  let b = deepClone(a);
  a.a = 2;
  a.c.d = 2;
  console.log(a); // {a:2,b:2,c:{d:2}}
  console.log(b); // {a:1,b:2，c:{d:1}}
  a.c = function () {
    console.log('this is not a Function');
  };
  a.c(); // 'this is not a Function'
  b.c(); // 'this is a Function'
  ```

附带一个自己做题时遇到的小问题：[一道简单小题引发的思考](https://dqtwdd.gitee.io/2020/08/12/20200812-mergeArr/)

### 2.8 new 关键字做了什么

1. 创建一个新对象。
2. 将新对象的*proto*指向构造函数的 prototype 对象。
3. 将构造函数的作用域赋值给新对象 （也就是 this 指向新对象）。
4. 执行构造函数中的代码（为这个新对象添加属性）。
5. 返回新的对象。

```javascript
let people = new Person()
==========================================
let people = {}
people.__proto__ = Person.prototype
Person.call(people)
return people
```

### 2.9 0.1 + 0.2 为什么不等于 0.3

因为 Js 采用的是**64 位双精度浮点数**编码，所以会有精度问题。

```javascript
0.1 + 0.2; // 0.30000000000000004  (15个0)
0.7 * 180; // 125.99999999998
1000000000000000128 === 1000000000000000129; //true  (15个0)
```

解决方案：小数计算时将小数转换成整数进行计算后再恢复成小数。

### 2.10 事件捕获和事件冒泡

事件流程处理分为三个阶段：

1. 事件捕获
2. 事件目标
3. 事件冒泡

```javascript
// addEventListener，他有一个参数，为true就是捕获阶段，为false就是冒泡阶段，默认为false
<div id="box1">
  <div id="box2">
    <div id="box3"></div>
  </div>
</div>;
// js 结构
document.getElementById('box1').addEventListener(
  'click',
  () => {
    console.log('box1事件捕获阶段');
  },
  true
);
document.getElementById('box1').addEventListener(
  'click',
  () => {
    console.log('box1事件冒泡阶段');
  },
  false
);
document.getElementById('box2').addEventListener(
  'click',
  () => {
    console.log('box2事件捕获阶段');
  },
  true
);
document.getElementById('box2').addEventListener(
  'click',
  () => {
    console.log('box2事件冒泡阶段');
  },
  false
);
document.getElementById('box3').addEventListener(
  'click',
  () => {
    console.log('box3事件捕获阶段');
  },
  true
);
document.getElementById('box3').addEventListener(
  'click',
  () => {
    console.log('box3事件冒泡阶段');
  },
  false
);
// 控制台打印
box1事件捕获阶段;
box2事件捕获阶段;
box3事件捕获阶段;
box3事件冒泡阶段;
box2事件冒泡阶段;
box1事件冒泡阶段;
```

- 事件捕获：点击一个 dom 后，事件会从文档的根节点流向目标节点，触发事件捕获（也就是`addEventListener true `时监听到的事件。）
- 事件目标：当事件到达目标节点后，就会触发目标节点所绑定的事件。然后会开始逆向回流，直到文档的最外层。
- 事件冒泡：目标节点绑定的事件触发后，并不会终止，而是会向文档的外层冒泡，依次触发外层 dom 所绑定的事件。

事件委托就是基于这个原理，当一个列表中有很多项时，我们可以不用为每一项绑定一个`@click`事件，而是可以直接在父节点绑定一个事件，传入`e`，然后对目标节点进行判断后做出回应：

```javascript
//html
<table @click="edit">
  <tr v-for="item in list">
    <td>{{item.name}}</td>
    ...
    <td>
      <button :data-id="item.id" title="eidt">编辑</button>
   </td>
  </tr>
</table>

//js
edit (event){
    if(event.target.title == "edit"){ //如果点击到了edit
        let id = evenr.target.dataset.id;
        //拿着id参数执行着相关的操作
    }
}
```

**无法在捕获阶段阻止事件冒泡！ **

vue 中可以使用修饰符来`@click.prevent` 阻止默认事件，使用` @click.stop`阻止事件冒泡。

### 2.11 ajax 原理

**ajax：** 既 _Asynchronous JavaScript and XML_ 异步的 Javascript 和 XML。

ajax 不是一种新技术，而是以下几种技术的组合：

1. 使用**HTML**和**Css**进行展示。
2. 使用**Dom**模型来交互和显示。
3. 使用**XMLHttpRequest**来请求。
4. 使用**JavaScript**来绑定和调用。

通过下图可以看出，以前的页面发送请求时是返回整个页面的信息，使用 Ajax 之后可以极大的降低网络请求传输的数据大小。

![ajax原理](https://dqtwdd.top/cdn/img/AjaxPrinciple.png)

### 2.12 前端模块化

- Node 采用 CommonJS 规范
  
  - 使用`module.exports`和`require()`配对
  - 特点：
    1. 每一个文件就是一个模块。
    2. 每个模块有自己的作用域。
    3. 每个模块内部的变量，方法，都是私有的，只能通过 exports 属性访问。
  
  看着眼熟不眼熟，面向对象啊！感觉每个 module 就是一个对象。
  
  ```javascript
  // example.js
  var x = 5;
  var addX = function (value) {
    return value + x;
  };
  module.exports.x = x;
  module.exports.addX = addX;
  
  // main.js
  var example = require('./example.js');
  
  console.log(example.x); // 5
  console.log(example.addX(1)); // 6
  ```

- ES6 模块规范
  
  - 使用`export`和`import`配对。
  - 特点：
    - `export`命令规定的是对外的接口，必须与模块内部的变量建立一一对应关系。
  
  ```javascript
  // 报错
  export 1;
  
  // 报错
  var m = 1;
  export m;
  
  // 写法一
  export var m = 1;
  
  // 写法二
  var m = 1;
  export {m};
  
  // 写法三
  var n = 1;
  export {n as m};
  ```
  
  // 报错
  function f() {}
  export f;
  
  // 正确
  export function f() {};
  
  // 正确
  function f() {}
  export {f};

```
### 2.13 ES6

#### 2.13.1 async await

- `async`是 Generator 函数的语法糖。
- `async`可以将异步函数改写为同步形式。
- `await` 后面跟一个函数。
- `async`函数返回一个 Promise 对象，可以使用`then`方法添加回调函数。
- 只有在 `async`内部函数执行完成后，函数才会继续往后执行。
- `await`后面如果是一个 Promise，会等待返回`resolve`或者`reject`之后再继续执行。
- `await`后面如果是一个普通函数，会立即执行。

```javascript
function fn1() {
console.log(1);
}
function fn2() {
console.log(2);
setTimeout(fn1, 10);
console.log(3);
}
function fn3() {
return new Promise(function (resolve) {
  console.log('4');
  resolve();
}).then(function () {
  console.log('5');
});
}
async function fn4() {
let res = await fn3();
fn1();
fn2();
}
async function fn5() {
let res = await fn2();
fn1();
fn2();
}
fn4(); // 4 5 1 2 3 1
fn5(); //2 3 1 2 3 1 1
```

#### 2.13.2 set map

1. set
   
   - `set` 类似于数组，与数组不同的是它内部的每个值都是唯一的。
   - `set`本身是一个构造函数，可以通过`new`关键字创建`set`对象。
   
   ```javascript
   const s = new Set();
   
   [2, 3, 5, 4, 5, 2, 2].forEach((x) => s.add(x));
   
   for (let i of s) {
     console.log(i);
   }
   // 2 3 5 4
   
   // 例一
   const set = new Set([1, 2, 3, 4, 4]);
   [...set];
   // [1, 2, 3, 4]
   
   // 例二
   const items = new Set([1, 2, 3, 4, 5, 5, 5, 5]);
   items.size; // 5
   ```
   
   - `set`的属性
     
     - `Set.prototype.size`用于查看`set`的总成员数，类似`Array.prototype.length`
     - `Set.prototype.constructor`用于查看`set`的构造函数。
   
   - `set`的方法
     
     - `Set.prototype.has`用于查看`set`是否含有某个元素。
       
       ```javascript
       let set = new Set([1, 2, 3, 3, 4, 5, 5, 6]);
       set.has(1); //true
       set.has('1'); //false
       ```
     
     - `Set.prototype.add`用于向`set`添加新元素。
       
       ```javascript
       console.log([...set]); // [1,2,3,4,5,6]
       set.add(9);
       console.log([...set]); // [1,2,3,4,5,6,9]
       set.add(1);
       console.log([...set]); // [1,2,3,4,5,6,9]
       ```
     
     - `Set.prototype.delete用于`set`删除元素。
       
       ```javascript
       console.log([...set]); // [1,2,3,4,5,6,9]
       set.delete(9);
       console.log([...set]); // [1,2,3,4,5,6]
       set.delete(1);
       console.log([...set]); // [2,3,4,5,6]
       ```
     
     - `Set.prototype.clear`用于清除`set`所有元素。

2. `map`
   
   - `map`类似于对象，与对象不同的是，传统的对象的`key`只能是 String，而`map`的`key`可以是任意类型（`Object Array String ... `）。
   
   - `map`的属性
     
     `size`属性返回 Map 结构的成员总数。
   
   - `map`的方法
     
     - `Map.prototype.set(key, value)`： `set`方法设置键名`key`对应的键值为`value`，然后返回整个 Map 结构。如果`key`已经有值，则键值会被更新，否则就新生成该键。
     - ` Map.prototype.has(key)`： `has`方法返回一个布尔值，表示某个键是否在当前 Map 对象之中。
     - `Map.prototype.get(key)`：`get`方法读取`key`对应的键值，如果找不到`key`，返回`undefined`。
     - ` Map.prototype.delete(key)` ： `delete`方法删除某个键，返回`true`。如果删除失败，返回`false`。
     - `Map.prototype.clear()：`： `clear`方法清除所有成员，没有返回值。

#### 2.13.3 let 和 const

先看一道经典的面试题

```javascript
for (var i = 0; i <10; i++) {
  setTimeout(function() {
    console.log(i);
  }, 0);
}
// 输出结果
10 10 10 10 10 10 10 10 10 10
```

以前啊，把这道题想简单了，以为知识简单的考`let`和`var`的区别，昨天跟小伙伴讨论的时候才意识到这道题考到了块级作用域和宏任务。现在把这道题改变一下，然后剖析一下。

```javascript
for (var i = 0; i <10; i++) {
  console.log('print',i)
  setTimeout(function() {
    console.log(i);
  }, 0);
}
// 输出结果
print1 print2 print3 print4 print5 print6 print7 print8 print9 10 10 10 10 10 10 10 10 10 10
```

结合后面提到的宏任务，我们可以分析，每一次执行循环的时候，都是先执行了一遍`console.log('print',i)`，然后把`setTimeout`的内容拿出来，放到宏任务的队列里面，由于`var`没有块级作用域，在执行完`for`循环后此时`i`的值为 10，然后再执行宏任务，打印 10 次 10。

而将`var`改为`let`之后，由于块级作用域的作用，每一次`for`的`i`不会互相影响， 所以打印结果就变成了`print1 print2 print3 print4 print5 print6 print7 print8 print9 1 2 3 4 5 6 7 8 9`

### 2.14 常用的`array string`方法

- `array`
  1. `Array.splice()` 截取数组中的一段，并返回新数组。
  2. `Array.concat()` 连接连个新数组。
  3. `Array.push()` 向数组最后添加一个元素。
  4. `Array.pop()` 删除并返回数组的最后一个元素。
  5. `Array.reverse()` 将数组倒叙排列。
  6. `Array.forEach()` 对数组中的每一个元素执行一遍函数，无返回值，更改原数组。
  7. `Array.every()` 检查数组中的每个元素是否都符合条件。
  8. `Array.map()` 对数组中的每一个元素执行一遍函数，返回一个新数组，不改变原数组。
  9. `Array.includes()` 查看数组是否包含某个元素。
  10. `Array.findIndex()` 查找数组中某个元素的索引。
  11. `Array.shift()` 从删除并返回一个元素。
  12. `Array.toString()` 将两个数组连接成一个新数组。
  13. `Array.sort()` 将数组排序。
  14. `Array.split()`从数组中添加或者删除元素。
- `string`
  1. `String.split()` 将字符串分割为数组。
  2. `String.replace()` 替换指定字符串。
  3. `String.slice()` 截取指定长度字符串并返回。
  4. `String.includes()` 判断字符串是否包含某个指定字符。
  5. `String.concat()` 将两个字符串链接成一个。
  6. `String.toLowerCase()` 转换小写。
  7. `String.toUpperCase()` 转换大写。
  8. `String.indexOf()` 查找指定字符第一次出现的位置。

### 2.15 Promise 的静态方法

1. `Promise.prototype.then()`
   
   `Promise.prototype.then()`是`Promise`返回`resolve`时的回调。

2. `Promise.prototype.catch()`
   
   `Promise.prototype.catch()`是`Promise`返回`reject`时的回调。

3. `Promise.prototype.finally()`
   
   `Promise.prototype.finally()`不关心`Promise`的返回是`resolve`还是`reject`，这个函数在`Promise`有返回值后执行。

4. `Promise.all()`
   
   `Promise.all()`方法用于将多个 Promise 实例，包装成一个新的 Promise 实例。
   
   `Promise.all()`的参数是一个数组，数组成员都是`Promise`实例。返回结果有两种情况：
   
   1. 参数中所有的`Promise`实例都返回`resolve`，则`Promise.all()`返回`fulfilled`，返货值为一个数组，数组成员为各个`Promise`实例`resolve`的值。
   2. 只要有一个返回`reject`，则直接执行`Promise.all()`的`.catch()`。
   
   ```javascript
   const p1 = new Promise((resolve, reject) => {
     resolve('hello');
   });
   
   const p2 = new Promise((resolve, reject) => {
     resolve('world');
   });
   
   const p3 = new Promise((resolve, reject) => {
     throw new Error('报错了');
   });
   
   Promise.all([p1, p2])
     .then((result) => console.log(result))
     .catch((e) => console.log('catch error:', e));
   // ["hello", "world"]
   
   Promise.all([p1, p3])
     .then((result) => console.log(result))
     .catch((e) => console.log('catch error:', e));
   // catch error: Error: 报错了
   ```
   
   **注意：**如果返回`reject`的`Promise`实例有自己的`.catch()`方法，那么并不会执行`Promise.all()`的`.catch()`方法，而是作为`fulfilled`执行`.then()`
   
   ```javascript
   const p1 = new Promise((resolve, reject) => {
     resolve('hello');
   })
     .then((result) => result)
     .catch((e) => e);
   
   const p2 = new Promise((resolve, reject) => {
     throw new Error('报错了');
   })
     .then((result) => result)
     .catch((e) => e);
   
   Promise.all([p1, p2])
     .then((result) => console.log(result))
     .catch((e) => console.log(e));
   // ["hello", Error: 报错了]
   ```

5. `Promise.race()`
   
   `Promise.race()`和`Promise.all()`接受的参数相同，也是一个`Promise`实例数组，不同的是，`Promise.race()`中只要有一个返回`resolve`或者`reject`就直接执行`Promise.race()`的`.then()`或者`.catch()`方法。不等待其他的实例完成。
   
   ```javascript
   const p1 = new Promise((resolve, reject) => {
     resolve('hello');
   });
   const p2 = new Promise((resolve, reject) => {
     resolve('world');
   });
   const p3 = new Promise((resolve, reject) => {
     throw new Error('报错了');
   });
   Promise.race([p1, p2, p3])
     .then((result) => console.log(result))
     .catch((e) => console.log('catch error:', e));
   // hello
   ```

6. `Promise.allSettled()`
   
   `Promise.allSettled()`和`Promise.all()`接受的参数相同，也是一个`Promise`实例数组，不同的是，`Promise.allSettled()`不关心参数中每个`Promise`实例返回的是`resolve`还是`reject`，关心的是数组中所有的`Promise`对象都完成了，该方法的返回值是参数中各个`Promise`实例的返回结果组成的数组。
   
   ```javascript
   const p1 = new Promise((resolve, reject) => {
     resolve('hello');
   });
   const p2 = new Promise((resolve, reject) => {
     resolve('world');
   });
   const p3 = new Promise((resolve, reject) => {
     throw new Error('报错了');
   });
   Promise.all([p1, p2, p3])
     .then((result) => console.log(result))
     .catch((e) => console.log('catch error:', e));
   // [{"status":"fulfilled","value":"hello"},{"status":"fulfilled","value":"world"},{"status":"rejected","reason":{}}]
   ```

7. `Promise.any()`
   
   `Promise.any()`和`Promise.all()`接受的参数相同，也是一个`Promise`实例数组，不同的是，`Promise.any()`中只要有一个`Promise`实例返回`resolve`，`Promise.any()`就返回`resolve`，并执行`.then()`后面的方法，所有`Promise`实例都返回`reject`，则返回`reject`，`Promise.any()`返回的错误是一个`AggregateError `错误实例数组。
   
   ```javascript
   const p1 = new Promise((resolve, reject) => {
     throw new Error('报错了');
   });
   const p2 = new Promise((resolve, reject) => {
     throw new Error('报错了');
   });
   const p3 = new Promise((resolve, reject) => {
     throw new Error('报错了');
   });
   Promise.any([p1, p2, p3])
     .then((result) => console.log(result))
     .catch((e) => console.log('catch error:', e));
   // catch error: AggregateError: All promises were rejected
   
   const p1 = new Promise((resolve, reject) => {
     throw new Error('报错了');
   });
   const p2 = new Promise((resolve, reject) => {
     throw new Error('报错了');
   });
   const p3 = new Promise((resolve, reject) => {
     resolve('hello');
   });
   Promise.any([p1, p2, p3])
     .then((result) => console.log(result))
     .catch((e) => console.log('catch error:', e));
   // hello
   ```

### 2.16 Js 包含哪几部分？

核心（ECMAScript）、文档对象模型（DOM）、浏览器对象模型（BOM）

常见的 BOM 对象：

window：代表整个浏览器窗口（window 是 BOM 中的一个对象，并且是顶级的对象）

Navigator ：代表浏览器当前的信息，通过 Navigator 我们可以获取用户当前使用的是什么浏览器

Location： 代表浏览器当前的地址信息，通过 Location 我们可以获取或者设置当前的地址信息

History：代表浏览器的历史信息，通过 History 我们可以实现上一步/刷新/下一步操作（出于对用户的隐私考虑，我们只能拿到当前的浏览记录，不能拿到所有的历史记录）

Screen：代表用户的屏幕信息

### 2.17 设计模式：

#### 2.17.1 工厂模式

#### 2.17.2 观察者模式

#### 2.17.3 订阅发布模式

#### 2.17.4 单例模式

#### 2.17.5 装饰器模式

### 2.18 for in 与 for of

for in 遍历的是 key，for of 遍历的是 value。

### 2.19 执行上下文

JavaScript 有三种执行上下文类型：

- 全局执行上下文
  
  一个程序中只会有一个全局执行上下文，它会执行两件事情：
  
  1. 创建一个全局的 window 对象（浏览器情况下；
  2. 设置 this 的值等于这个全局对象。

- 函数执行上下文
  
  每当一个函数被调用时，都会为该函数创建一个新的上下文，每个函数都有它自己的执行上下文，不过是在函数调用的时候创建的；函数的上下文可以有任意多个，每当一个新的执行上下文被创建，它会按照定义的顺序执行一系列步骤。

- Eval 函数执行上下文

上下文有两个步骤：

1. 创建上下文
   
   创建上下文分三步：
   
   1. this绑定
   
   2. 创建词法环境：确定词法环境是*全局环境*还是*函数环境*
   
   3. 创建变量环境：确定变量环境是*全局环境*还是*函数环境*

2. 执行
   
   看一下2.3的经典小题。

### 2.20 JS GC（Garbage Collection）垃圾回收机制

- 可达性
  
  js 中“可达性”值就是那些可以以某种方式访问或可用的值，他们被保证储存在内存中。

- 垃圾是什么
  
  一般来说，没有被引用到的对象就是垃圾，就是需要被清除的，如果几个对象引用成一个环，互相引用，但根访问不到他们，这几个对象也是垃圾，也要被清除。

- 标记-清除 算法
  
  1. 标记阶段：从根集合出发，将所有活动对象及其子对象打上标记。
  2. 清除阶段：遍历堆，将非活动对象（未打上标记）清除。

参考：[思否-垃圾回收机制](https://segmentfault.com/a/1190000018605776)

### 2.21 Polyfill

Polyfill 字面意思是垫片，意为用于实现浏览器并不支持的原生 AP 的代码。

常见的 Polyfill 有比如 new 函数，debounce 防抖函数，throttle 节流函数等。

### 2.22 函数柯里化

柯里化是一种函数的转变，它指将一个函数从可调用的 f(a,b,c)转换为可调用的 f(a)(b)(c)。

柯里化不会调用函数，他只是对函数进行转换。

柯里化的核心思想是：降低通用性，提高适用性。

例：正则判断

如下，封装一个正则判断的函数，传入正则表达式和目标字符串，返回判断结果。没有问题。

```jsx
function check(targetString, reg) {
  return reg.test(targetString);
}

check(/^1[34578]\d{9}$/, '14900000088');
check(/^(\w)+(\.\w+)*@(\w)+((\.\w+)+)$/, 'test@163.com');
```

当业务需求只是判断手机号或判断邮箱时，仍需要每次都传入相应的正则这就很低效，因此可以使用柯里化函数再次进行封装。

```jsx
function curring(reg) {
  return (str) => {
    return reg.test(str);
  };
}
var checkPhone = curring(/^1[34578]\d{9}$/);
var checkEmail = curring(/^(\w)+(\.\w+)*@(\w)+((\.\w+)+)$/);

console.log(checkPhone('183888888')); // false
console.log(checkPhone('17654239819')); // true
console.log(checkEmail('exy@163.com')); // true
```
