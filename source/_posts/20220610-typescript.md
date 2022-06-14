---
title: Typescript
tags: ['学习']
categories: 学习
date: 2022-06-10
---

Typescript学习笔记。

<!--more-->

### 1. 基础类型

#### 1.1基础类型

基础类型可以直接用`:`连接。

```javascript
let isDone: boolean = false;
let decLiteral: number = 6;
let name: string = "bob";
```

#### 1.2 数组

数组可以有两种方法定义。第一种是在元素类型后面接`[]`，表示由此类型元素组成一个数组。

```javascript
let list:number[] = [1,2,3]
```

第二种方式是实用数组泛型。

```javascript
let list:Array<number> = [1,2,3]
```

### 1.3 元组

元组表示允许一个已知元素数量和类型的数组。各元素的类型不必相同。比如：

```javascript
let x:[string,number];
x = ['hello',1] // ok
x = [1,'hello'] //Error
```

#### 1.4 any

啥类型都行。尽量少用。

#### 1.5 Void

void类型像是与any类型相反。它表示没有任何类型。当一个函数没有任何返回时，可以使用`void`作为返回值。

```javascript
function error():void {
  console.log('this is an error')
}
```

#### 1.6 Object 

`object`表示非原始类型，也就是除`number`，`string`，`boolean`，`symbol`，`null`或`undefined`之外的类型。

使用`object`类型，就可以更好的表示像`Object.create`这样的API。例如：

```ts
declare function create(o: object | null): void;

create({ prop: 0 }); // OK
create(null); // OK

create(42); // Error
create("string"); // Error
create(false); // Error
create(undefined); // Error
```

### 2.接口

#### 2.1 定义

接口就是把要定义的type进行一个封装。例：

```typescript
function print (param:{label:string}) {
  console.log(param.lable)
}
let obj = {label:'size',size:10}
print(obj);
// 上面的例子用接口封装
interface Param {
  label:string;
}
function print (param:Param) {
  console.log(param.lable)
}
let obj = {label:'size',size:10}
print(obj);
```

#### 2.2 可选属性

可选属性是对一些可能存在可能不存在的值进行type限定。

```typescript
interface Bag {
  color?:string;
  book?:string;
  pen?:number;
}
function createBag (bag:Bag):{color:string;size:number}{
  let newBag = {color:'white',size:'big'};
  if (bag.color) {
    newBag.color = bag.color;
  }
  return newBag;
}
let myBag = createBag({color:'black'});
```

没加`?`的接口中的属性必须存在，不然就会报错。

如果赋值接口不存在的值，ts也会报错，但是也能运行。

```typescript
const test = reactive<{color?:string;book:string;pen:number}>({ book: 'hello' }) 
// 类型“{ book: string; }”的参数不能赋给类型“{ color?: string | undefined; book: string; pen: number; }”的参数。
// 类型"{ book: string; }"中缺少属性 "pen"，但类型 "{ color?: string | undefined; book: string; pen: number; }" 中需要该属性。ts(2345)
const test = reactive<{color?:string;book:string;pen?:number}>({ book: 'hello' }) // ok
test.test = 'hah' 
// 类型“{ color?: string | undefined; book: string; pen: number; }”上不存在属性“test”。ts(2339)
```

如果可能添加额外的属性，有三种方法。

1. 赋值给另一个变量

```typescript
interface test {
  color: string
  book: string
  pen: string
}
let a = { color: '1', book: '1', pen: '1', test: 'hello' }
const test = reactive<test>(a)
```

2. 断言

```typescript
interface test {
  color: string
  book: string
  pen: string
}
const test = reactive<test>({ color: '1', book: '1', pen: '1', test: 'hello' } as test)
```

3. 索引签名

需要注意的是，索引签名也会约束已经定义的类型。

```typescript
interface test {
  color: string
  book: string
  pen: string
  [x: string]: string
}
const test = reactive<test>({ color: '1', book: '1', pen: '1', test: 'hello' }) // ok

interface test {
  pen: number
  [x: string]: string
}
const test = reactive<test>({ pen: 1, test: 'hello' })
// 类型“{ pen: number; test: string; }”的参数不能赋给类型“test”的参数。
// 属性“pen”与索引签名不兼容。
// 不能将类型“number”分配给类型“string”。ts(2345)
```

#### 2.3 只读属性

使一些对象属性只能在对象刚创建时修改其值。可以在属性名县使用`readonly`来指定只读属性：

```typescript
interface Point {
    readonly x: number;
    readonly y: number;
}
let p1: Point = { x: 10, y: 20 };
p1.x = 5; // error!
```

最简单判断该用`readonly`还是`const`的方法是看要把它做为变量使用还是做为一个属性。 做为变量使用的话用 `const`，若做为属性则使用`readonly`。

#### 2.4 implements和extends

个人感觉，理解这两个作用的最简单的方式就是从字面理解。

implements 实现 就是 类 根据 接口/类 来实现具体的内容。

extends 继承 就是 接口/类 继承 （接口，类）/（类） 的本身属性。

原因是，类在构建过程中会自行创建一个类对应的接口，可以理解为接口是类的子集。

基于这个理解，我们可以得到下面的表：

|           | interface  | class              |
| --------- | ---------- | ------------------ |
| interface | extends    | extends            |
| class     | implements | implements/extends |

![implements](https://dqtwdd.top/cdn/img/implements.jpg)

### 3. class

关于类，看了一些资料，感觉比较好的一个说法是：

不要为了oop而使用class，大多数时候，函数式编程都可以解决我们的问题。学好 工厂函数/闭包 才是关键。

来一个简单的小例子：

```typescript
class Person {
  name: string;
  constructor(name:string) {
    this.name = name;
  }
  talk() {
    return 'I am ' + this.name
  }
}
let green:Person;
green = new Person('Green');
console.log(green.talk()); // I am Green

// 有一个小问题，关于this指向
let test = {}
test.func = green.talk;
test.func() // 'I am undefined'

// 解决方案1 显式调用bind
class Person {
  name: string;
  constructor(name:string) {
    this.name = name;
    this.talk = this.talk.bind(this);
  }
  talk() {
    return 'I am ' + this.name
  }
}
// 解决方案2 箭头函数
class Person {
  name: string;
  constructor(name:string) {
    this.name = name;
  }
  talk = () => {
    return 'I am ' + this.name
  }
}
class Person {
  name: string;
  constructor(name:string) {
    this.name = name;
    this.talk = () => {
    	return 'I am ' + this.name
  	}
  }
}
// 解决方案3 工厂函数
const Person = (name) => {
	return {
    name: name,
    talk: () => {
      console.log('I am ' + name)
    }
  }
}
```

### 4. 函数

为函数定义类型：

```typescript
// 基础
function add (x:number, y:number):number {
  return x + y;
}

// 可选参数和默认参数
function buildName(first:'bob',lastNmae?:string){
  
}

// 剩余参数
function buildName(first:string,args:string[]){
  
}

```

### 5. 泛型

泛型用来捕获用户传入的类型，之后我们就可以使用这个类型。

来一个简单的小例子：

```typescript
interface genericity<T> {
    par: T
  	test: T
}
const genericityTest = reactive<genericity<number>>({ par: 1, test: 2 }) // ok
const genericityTest = reactive<genericity<string>>({ par: '', test: '' }) // ok
const genericityTest = reactive<genericity<number>>({ par: '', test: 2 }) // error

// 函数泛型
function identity<T>(arg: T): T {
    return arg;
}
```

