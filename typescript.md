## 快速入门TypeScrpit

### 什么是`TypeScript`?

---

首先，看下`TypeScript`定义:

>TypeScript is a typed superset of JavaScript that compiles to plain JavaScript. Any browser. Any host. Any OS. Open source<sup>[1]</sup>.

`TypeScript` 是 `JavaScript` 的强类型版本。然后在编译期去掉类型和特有语法，生成纯粹的`JavaScript` 代码。由于最终在浏览器中运行的仍然是 `JavaScript`，所以 `TypeScript` 并不依赖于浏览器的支持，也并不会带来兼容性问题。

`TypeScript` 是 `JavaScript` 的超集，这意味着他支持所有的 JavaScript 语法。并在此之上对 JavaScript 添加了一些扩展，如 class / interface / module 等。这样会大大提升代码的可阅读性。

强类型语言的优势在于静态类型检查，具体可以参见 http://www.zhihu.com/question... 的回答。概括来说主要包括以下几点<sup>[1]</sup>：

- 静态类型检查,减少编译阶段引起的错误
- 增强了编辑器和 IDE 的功能
- 代码重构
- 可读性

### `TypeScript`运行方式
----
`TypeScript`运行方式（运行环境）主要有一下三种：

#### 在线complier

这种方式最简单，不需在本地做任何配置安装，只需进入[Typescript的官网](http://www.typescriptlang.org/)，点击里面的`playground`就可以直接写代码了。但这种方式只适用于测试而不适用于开发。


#### 本地命令行编译

1. 在本地编译运行Typescript需要使用npm下载typescript, 执行命令: `npm install -g typescript`。
2. 下载完成后可以使用 `tsc -v`查看版本
3. 使用：如在本地创建Hello.ts

```javascript

export class Hello {

}
```

在命令行中 `tsc Hello.ts`
运行后就会发现在同一文件夹下生成了Hello.js

#### 使用IDE

可以使用vsCode，配置出`TypeScript`的运行环境，具体可参考：https://code.visualstudio.com/docs/typescript/typescript-tutorial


### `TypeScript`的基本用法

----

#### 基础数据类型


- boolean
- number
- string
- array
- tuple, 允许表示一个一直元素数量和类型的数组
- enum
- any
- void
- null 和 undefined

```javascript
// boolean
let isDone: boolean = false;

// number
let decLiteral: number = 6;

// string

let name: string = 'bob';

let sentence: string = `Hello, my name is ${ name }`;

// array

let list: number[] = [1, 2, 3];

let list: Array<number> = [1, 2, 3];

// tuple 

let x: [string, number];

x = ['hello', 10] // Ok

x = [10, 'hello'] // Error

// enunm

enum Color {Red, Green, Blue}

let c: Color = Color.Green; //1

let colorName: string = Color[2] // Blue

// any

let notSure: any = 4;

notSure = false;

// void

function warnUser() : void {
    console.log("This is my warning message");
}

let unusable: void = undefined;

// null undefined

let n: null = null;

let u: undefined = undefined;

// never

function infiniteLoop(): never {
    while (true) {
    }
}

// object

declare function create(o: object | null): void;

create({ prop: 0});

create(null);

// 类型断言

let someValue: any = "this is a string";

let strLength: number = (<string>someValue).length;

let strLength: number = (someValue as string).length;
```

#### 接口

 在`TypeScript`里，接口的作用就是为类型命名和为代码或第三方代码定义契约。

 ```javascript

interface SquareConfig {
    color?: string;
    width?: number;
    [propName: string]: any;
}

let squareOptions = { colour: "red", width: 100 };
let mySquare = createSquare(squareOptions);

```

#### 类

JavaScript的ES6，有class，也就是我们的类，虽然class实质上是 JavaScript 现有的基于原型的继承的语法糖。可以通过一个类new一个对象<sup>[3]</sup>。

```javascript
class Person {
    // protected表示只能被类内部，继承的类内部访问，不能被外部访问
    protected name: string;
    // 只读属性
    readonly gender: string = '女';
    // static表示所有实例共有的成员。
    static  workPlace: string = 'hikvision';
    // constructor为构造函数，在new一个类时，调用该方法
    protected constructor(theName: string) { this.name = theName; }
    // 抽象类是一种不能被实例化的类，它的目的就是用来继承的，抽象类里面可以有抽象的成员，就是自己不实现，等着子类去实现。
    abstract makeSound(): void;
}

// Employee 能够继承 Person
class Employee extends Person {
    // private表示只能被类内部访问，不能被类外部访问
    private department: string;
    // 存储器
    get age(): number {
        return this._age;
    }
    // 存储器
    set age(newName: number) {
        if(newName > 150){
            this._age = 150;
        }
        this._age = newAge;
    }
    constructor(name: string, department: string) {
        // super（)表示父类构造器
        super(name);
        this.department = department;
    }
    // 方法 public 默认的访问级别，不写默认就是public。
    public getElevatorPitch() {
        return `Hello, my name is ${this.name} and I work in ${this.department}.`;
    }
}

let howard = new Employee("Howard", "Sales");
let john = new Person("John"); // 错误: 'Person' 的构造函数是被保护的.

```


#### 函数

在JavaScript中，<s>函数是一等公民</s>，函数是JavaScript应用程序的基础，必须介绍。首先，定义函数的三种方式有：


1. 函数声明

```javascript
// 此时多传一个参数，或者少传参数，都不行
function sum(x: number, y: number): number {
    return x + y;
}
```
2. 函数表达式

```javascript

const sum3 = function(x: number, y: number): number {
    return x + y;
}

const sum4: (x: number, y: number) => number = function(x: number, y: number): number {
    return x + y;
}
```
3. 接口定义
```javascript
interface Function5 {
    (x: string, y: string): boolean
}

let function5: Function5 = (x: string, y: string) => {
    return x.search(y) > -1;
}
// 可选参数，可选参数必须跟在默认参数后面
function buildName(firstName: string, lastName?: string) {
    if (lastName)
        return firstName + " " + lastName;
    else
        return firstName;
}
```

### vue-cli中使用`TypeScript`

----


参考文献：


[1] http://www.typescriptlang.org/

[2] https://linux.cn/article-8774-1.html 介绍TypeScript编译原理

[3] https://juejin.im/post/5d25b3cce51d4510a37bac7a 介绍类



