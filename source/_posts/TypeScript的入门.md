---
title: TypeScript
categories: ES6
date: 2020-11-01 10:00:00
updated: 2020-11-01 10:00:00
tag:
  - 前端
  - 语言
  - TypeScript
toc: true
thumbnail: "/images/brain.jpg"
---

##  关于TypeScript
`TypeScript`是`JavaScript`的超集，主要提供类型系统和对`ES6`的支持，由`Microsoft`开发，第一个版本发布于2012。
## TypeScript的优势
 - `TypeScript`增加了代码的可读性和可维护性
   - 大部分的函数函数看看类型的定义就已经知道该如何使用了
   - 可以在编译的阶段就发现了大量的错误，防止在运行中发现过多的错误
   - 增强了编辑器和`IDE`的功能。 
- `TypeScript `非常的包容
   - `TypeScript`是`JavaScript`的超集，`.js`文件可以直接将名字转换为`.ts`即可
   - 即使不显式的定义类型，也可以自动的做出类型的推论
   - 可以定义从简单到复杂的一切类型
   - 即使` TypeScript `编译报错，也可以生成` JavaScript `文件

   <!--more-->
## TypeScript的劣势
- 需要一定的学习成本，需要理解接口(`Interfaces`)、泛型(`Generics`)、类(`Classes`)、枚举类型(`Enums`)等前端工程师不是很熟悉的东西，而且现阶段的中文资料也不是很多
- 短期内可能增加一些开发成本，毕竟要多写一些类型的定义，不过对于一个需要长期进行维护的额项目。`TypeScript`能够减少维护的成本
- 集成到构建流程需要一定的工作量
- 可能和一些库结合的并不是很完美
## 原始数据类型
对于前端开发来说，主要的类型可以 分成两部分：原始数据类型和对象类型。原始数据类型包括：`布尔值`，`数值`，`字符串`，`null`，`undefined`以及ES6中的新类型`symbol`。现在我们主要看一下这些原始值在`TypeScript`中的应用。
#### 布尔值
布尔值是最基本的数据类型，在`TypeScript`中，使用`Boolean`定义布尔值类型：
``` TypeScript
let isTrue:boolean = false;

// 可以通过编译
// 对于后面来说，没有特殊强调错误代码片段，就默认为编译通过了。
```
注意一下，使用构造函数`Boolean`创建的对象不是布尔值：
```TypeScript
let createdByNewBoolean : boolean = new Boolean(1);

// index.ts(1,5): error TS2322: Type 'Boolean' is not assignable to type 'boolean'.
```
上面代码报错的原因是`new Boolean()`返回一个`Boolean`对象：
```TypeScript
let createdByNewBoolean : Boolean= new Boolean(1);
```
直接调用` Boolean `也可以返回一个 `boolean` 类型：
```TypeScript
let createdByBoolean: boolean = Boolean(1);
```
在` TypeScript `中，`boolean` 是 `JavaScript `中的基本类型，而` Boolean` 是 `JavaScript` 中的构造函数。其他基本类型（除了 `null `和 `undefined`）一样，不再赘述。
#### 数值
使用`nubmber`定义数值类型：
```TypeScript
let decLiteral:number=6;
let hexLiteral:number = 0xf00d;
let binaryLiteral:number = 0b1010; // ES6中的二进制表示法
let octalLiteral:number =0o744;// ES6中的八进制表示法
let notNumber:number =NaN;
let infinityNumber:number = Infinity;
```
上面的编译的结果是什么：
```
var decLiteral = 6;
var hexLiteral = 0xf00d;
var binaryLiteral = 10;
var octalLiteral= 484;
var notNumber = NaN;
var infinityNumber = Infinity;
```
其中` 0b1010` 和 `0o744` 是`ES6` 中的二进制和八进制表示法，它们会被编译为十进制数字。
#### 字符串
使用`string`定义字符串类型：
```
// 普通字符串
let myName:string = 'kim';
let myAge:number = 25;

// 模板字符串
let sentence:string = `Hello,my name is ${myName}.
I'll be ${myAge + 1} years old next month`;
```
编译结果：
```
var myName = 'kim';
var myAge = 25;

// 模板字符串
let sentence = "Hello,my name is"+myName+".\nI'will be"+(myAge+1)+"years old next month";
```
#### 空值
`JavaScript`没有空值(`void`)的概念，在`TypeScript`中，可以用`void`表示没有任何返回值的函数。
```
function akertName():void{
  alert('My name is kim');
}
```
声明一个`void`类型变量是没有什么用，因为我们只能将它赋值为`undefined`和`null`:
```
let unusable:void = undefined
```
#### Null和undefined
在`TypeScript`中，可以使用`null`和`undefined`来定义这两个原始数据类型：
```TypeScript
let u:undefined = undefined;
let n:null = null;
```
`undefined`只能赋值为`undefined`，`null`只能赋值为`null`。但是这二者和`void`还是有区别的，`null`和`undefined`是所有类型的子类型，也就是说`undefined`类型的变量，可以赋值给`number`类型的变量：
```TypeScript
//这样写也是没有问题的
let num:number =undefined;
let u: undefined;
let num: number = u;
```
但是`void`的类型并不是这样的，不能讲`void`类型的变量不能赋值给`number`类型的变量：
```TypeScript
let u: void;
let num: number = u;

//index.ts(2,5): error TS2322: Type 'void' is not assignable to type 'number'.
```
## 任意值
任意值（`Any`）用来表示允许赋值为任意类型。
#### 什么是任意值类型
如果是一个普通类型，在赋值过程中改变类型是不被允许的:
```TypeScript
let myFavoriteNumber:string ='seven';
myFavoriteNumber = 7;

// index.ts(2,1): error TS2322: Type 'number' is not assignable to type 'string'.
```
但如果是 `any` 类型，则允许被赋值为任意类型。
```TypeScript
let myFavoriteNumber : any ='seven';
myFavoriteNumber = 7;
```
#### 任意值的属性和方法
在任何值上访问任何属性都是允许的：
```TypeScript
let anyThing :any = 'hello';
console.log(anyThing.myName);
console.log(anyThing.myName.firstName);
```
也允许调用任何方法：
```TypeScript
let anyThing : any = 'kim';
anyThing.setName('Tom');
anyThing.setName('Tom').sayHello();
anyThing.myName.setFirstName('kim');
```
可以认为，声明一个变量为任意值之后，对它的任何操作，返回的内容的类型都是任意值。
#### 未声明类型的变量
变量如果在声明的时候，未指定其类型，那么它会被识别为任意值类型：
```TypeScript
let something;
something = 'seven';
something = 7;
something.setName('kim');
```
上面的代码等价于：
```TypeScript
let something:any;
something = 'seven';
something = 7;
something.setName('kim');
```
## 类型推论
如果没有明确的字段，那么`TypeScript`会依照类型推论的规则判断出一个类型。
下面的代码虽然没有指定类型，但是会在编译的时候报错：
```TypeScript
let myFavoriteNumber = 'seven';
myFavoriteNumber = 7;

// index.ts(2,1): error TS2322: Type 'number' is not assignable to type 'string'.
```
上面的代码等价于：
```TypeScript
let myFavoriteNumber:string  = 'seven';
myFavoriteNumber = 7;

// index.ts(2,1): error TS2322: Type 'number' is not assignable to type 'string'.
```
但是为什么这和`any`类型的还是有区别的，二者的区别在于在声明的时候没有赋值的话，就会判断成`any`类型，在后续就不会再进行检查。
## 联合类型
联合类型表示取值可以为多种类型中的一种。
让我们先举个小栗子：
```TypeScript
let myFavoriteNumber:string | number;
myFavoriteNumber = 'seven';
myFavoriteNumber = 7;
```
上面的例子说明`myFavoriteNumber`可以是`string`类型或是`number`类型的。但是如果我们给这个值给一个布尔值，就会报错。
```TypeScript
let myFavoriteNumber:string | number;
myFavoriteNumber = true;

// index.ts(2,1): error TS2322: Type 'boolean' is not assignable to type 'string | number'.
// Type 'boolean' is not assignable to type 'number'.
```
联合类型使用 | 分隔每个类型。
这里的`string|number`的意思是，允许`myFavoriteNumber `的类型是`string`或是`number`，但是不能是其他类型。
#### 访问联合类型的属性或方法
当`TypeScript`不确定一个联合类型的变量到底是哪个类型的时候，我们只能访问此联合类型的所有类型里共有的属性和方法：
```TypeScript
function getLength(something: string | number): number {
  return something.length;
}
// index.ts(2,20): error TS2339: Property 'length' does not exist on type 'string | number'.
// Property 'length' does not exist on type 'number'.
```
上例中，`length` 不是` string` 和 `number` 的共有属性，所以会报错。
访问 string 和 number 的共有属性是没问题的：
```TypeScript
function getString(something:string | number):string{
  return something.toString();
}
```
联合类型的变量在被赋值的时候，会根据类型推论的规则推断出一个类型：
```TypeScript
let myFavoriteNumber:string|number;
myFavoriteNumber = 'seven';
console.log(myFavoriteNumber.length); // 5
myFavoriteNumber = 7;
console.log(myFavoriteNumber.length); // 编译时报错
 
// index.ts(5,30): error TS2339: Property 'length' does not exist on type 'number'.
```
上例中，第二行的 `myFavoriteNumber `被推断成了 `string`，访问它的` length `属性不会报错。
而第四行的` myFavoriteNumber` 被推断成了 `number`，访问它的 `length`属性时就报错了。
## 对象的类型 -- 接口
在`TypeScript`中，我们使用接口来定义对象的类型。
#### 什么是接口
在面向对象的语言中，接口是一个很重要的概念，他是对行为的抽象，而具体如何行动需要类去实现。`TypeScript`中的接口就是一个很灵活的概念，除了可用于对类的一部分行为进行抽象以外，也常用于对【对象的形状】进行描述。
简单的例子：
```TypeScript
interface Person{
  name:string;
  age:number;
}
let kim:Person={
  name:'kim',
  age:18
}
```
在上面的例子中我们定义了一个接口`Person`，接着定义可一个变量`kim`，它的类型是`person`，这样，我们就约束了`kim`的形状和接口`Person`一致。
接口一般首字母大写。
接口的变量比接口少一些或是多一些属性是不允许的：
```TypeScript
interface Person {
  name: string;
  age: number;
}
 
let kim: Person = {
  name: 'kim',
};
 
// index.ts(6,5): error TS2322: Type '{ name: string; }' is not assignable to type 'Person'.
//   Property 'age' is missing in type '{ name: string; }'.


interface Person {
  name: string;
  age: number;
}
 
let kim: Person = {
  name: 'kim,
  age: 25,
  website: 'http://kim.com',
};
 
// index.ts(9,3): error TS2322: Type '{ name: string; age: number; website: string; }' is not assignable to type 'Person'.
//   Object literal may only specify known properties, and 'website' does not exist in type 'Person'.
```
#### 可选属性
有的时候我们希望不要完全匹配成一个形状，那么我们可用一些可选属性：
```TypeScript
interface Person {
  name: string;
  age?: number;
}
 
let kim: Person = {
  name: 'kim',
};

let kimi: Person = {
  name: 'kimi',
  age:28
};
```
可选属性的意思就是这个属性的值可以是不存在的，但是在这种情况下依旧不允许添加未定义的属性，我们在下面举个栗子：
```TypeScript
let Tom: Person = {
  name: 'Tom',
  age:28,
  website:'http://www.baidu.com',
};
// examples/playground/index.ts(9,3): error TS2322:
// Type '{ name: string; age: number; website: string; }' is not assignable to type 'Person'.
// Object literal may only specify known properties, and 'website' does not exist in type 'Person'.
```
#### 任意属性
有的时候我们希望几口允许任意的属性，可以使用如下的方式:
```TypeScript
interface Person{
  name:string;
  age?:number;
  [propName:string]:any;
}

let kim :Person ={
  name:'kim',
  website:'http://www.baidu.com',
};
```
我们对于上面的代码进行解析，使用`[propName:string]`定义了任意属性取`string`类型的值。但是需要注意的是，一旦定义了任意属性，那么确定属性和可选属性都必须是它的子属性：
```TypeScript
interface Person{
  name:string;
  age?:number;
  [propName:string]:string;
}

let kim :Person ={
  name:'kim',
  age:25,
  website:'http://www.baidu.com',
}

// index.ts(3,3): error TS2411: 
//Property 'age' of type 'number' is not assignable to string index type 'string'.
// index.ts(7,5): 
//error TS2322: Type '{ [x: string]: string | number; name: string; age: number; website: string; }' is not 
//assignable to type 'Person'.
// Index signatures are incompatible.
//Type 'string | number' is not assignable to type 'string'.
//Type 'number' is not assignable to type 'string'.
```
上例中，任意属性的值允许是 `string`，但是可选属性 `age` 的值却是` number`，`number` 不是` string `的子属性，所以报错了。

另外，在报错信息中可以看出，此时 `{ name: ‘Xcat Liu’, age: 25, website: ‘http://xcatliu.com’ }` 的类型被推断成了 `{ [x: string]: string | number; name: string; age: number; website: string; }`，这是联合类型和接口的结合。
#### 只读属性
有时候我们希望对象中的一些字段只能在创建的时候被赋值，那么可以用`readonly`定义只读属性：
```TypeScript
interface Person {
  readonly id: number;
  name: string;
  age?: number;
  [propName: string]: any;
}
 
let xcatliu: Person = {
  id: 89757,
  name: 'kim',
  website: 'http://www.baidu.com',
};
 
xcatliu.id = 9527;
 
// index.ts(14,9): error TS2540: Cannot assign to 'id' because it is a constant or a read-only property.
```
上面的id属性是只读属性，`id`在初始化之后，又被赋值了，所以会报错。只读的约束存在于第一次给对象赋值的时候，而不是第一次给只读属性赋值的时候。
```TypeScript
interface Person {
  readonly id: number;
  name: string;
  age?: number;
  [propName: string]: any;
}
 
let xcatliu: Person = {  // 没有给id进行赋值
  name: 'Xcat Liu',
  website: 'http://xcatliu.com',
};
 
xcatliu.id = 89757; // 错误
 
// index.ts(8,5): error TS2322: Type '{ name: string; website: string; }' is not assignable to type 'Person'.
//   Property 'id' is missing in type '{ name: string; website: string; }'.
// index.ts(13,9): error TS2540: Cannot assign to 'id' because it is a constant or a read-only property.
```
## 数组的类型
#### [类型+方括号]表示法
最简单的方法是使用`[类型+方括号]`来标识数组：
```TypeScript
let arr:number[] =[1,2,3,4,5]
```
数组的项中不允许出现其他的类型：
```TypeScript
let arr:number[] =[1,'2',3,4,5];

//index.ts(1,5): error TS2322: Type '(number | string)[]' is not assignable to type 'number[]'.
//Type 'number | string' is not assignable to type 'number'.
//Type 'string' is not assignable to type 'number'.
```
现在的`[1,'2',3,4,5]`的类型被推断称为`(number|string)[]`,这个是联合类型和数组的结合。
``` TypeScript
let arr2: number[] = [1,2,3,4,5,6,7];
arr2.push('8')

//index.ts(2,16): error TS2345: Argument of type 'string' is 
//not assignable to parameter of type 'number'.
```
#### 数组泛型
也可以使用数组泛型（`Generic`） `Array<elemType>` 来表示数组：
```
let arr3: Array<number> = [1, 1, 2, 3, 5];
```
#### 用接口表示数组
接口也可以用来描述数组：
```TypeScript
interface NumberArray{
  [index:number]:number;
}
let arr4 : NumberArray=[1,1,2,3,4];
```
`NumberArray` 表示：只要 `index` 的类型是 `number`，那么值的类型必须是 `number`。
#### any 在数组中的应用
一个比较常见的做法是，用`any`表示数组中允许出现任意类型：
```TypeScript
let list : any[] =['kim',25,{website:'www.baidu.com'}];
```
#### 类数组
类数组不是数组类型，比如`arguments`：
```TypeScript
function sum(){
  let args:number[] = arguments;
}
// index.ts(2,7): error TS2322: Type 'IArguments' is not assignable to type 'number[]'.
// Property 'push' is missing in type 'IArguments'.
```
事实上常见的类数组都有自己的接口定义，如 `Arguments`,` NodeList`, `HTMLCollection` 等：
```TypeScript
function sum() {
  let args: IArguments = arguments;
}
```
## 函数的类型
函数的声明
在`JavaScript`中，有两种常见的定义函数的方式--函数声明（`Function Declaration`）和函数表达式（`Function Expression`）：
```
// 函数声明（Function Declaration）
function sum (x,y){
  return x+y;
}
//函数表达式（Function Expression）
let mySum = function （x,y）{
  return x + y;
}
```
一个函数有输入和输出，要在`TypeScript`中对齐进行约束，需要把输入和输出都考虑到，其中函数声明的类型定义比较简单：
```TypeScript
function sum (x:number,y:number):number{
  return x+y;
}
```
但是输出多余的(或少于要求)参数，是不被允许的：
```TypeScript
function sum (x:number,y:number):number{
  return x+y;
}
sum (1,2,3);
// index.ts(4,1): error TS2346: Supplied parameters do not match any signature of call target.

sum (1);
//index.ts(4,1): error TS2346: Supplied parameters do not match any signature of call target.
```
#### 函数表达式
如果我们现在想要写一个函数表达式的定义，我们可能会这样写：
```TypeScript
let muSum = function(x:number,y:number):number{
  return x+y;
}
```
这个是可以通过编译的，但是在事实上，上面的代码只对等号右侧的匿名行数进行了类的定义，而等号左侧的`mySum`，是通过赋值操作对类型进行推断出来的。如果需要我们手动给`mySum`添加类型，代码应该只这样写的：
```TypeScript
let mySum:(x number,y:number) =>number = function (x:number,y:number):number{
  return x+y;
}
```
在`TypeScript`的类型定义中，`=>`用来表示函数的定义，左边是输入类型，需要用括号括起来，右边是输出类型，应用是十分广泛的。
#### 接口中函数的定义
我们也可以使用接口的当时来定义一个函数需要符合的形象：
```
interface SearchFunc{
  (source:string,subString:string):boolean
}

let mySearch : SearchFunc;
mySearch  = function (source:string,subString:string){
  return source.search(subString) !== -1;
}
```
#### 可选参数
前面，输入多余的(或是少于要求的)的参数，是不被允许的，那么怎么定义可选的参数呢？与接口中的可选属性相似，也是使用`？`表示可选参数：
```TypeScript
function buildName(firstName: string, lastName?: string) {
  if (lastName) {
    return firstName + ' ' + lastName;
  } else {
    return firstName;
  }
}
let kimXue= buildName('Xcat', 'Xue');
let kim= buildName('kim');
```
需要注意的是，可选参数必须接在必需参数后面。换句话说，可选参数后面不允许再出现必须参数了：
```TypeScript
function buildName(firstName?: string, lastName: string) {
  if (firstName) {
    return firstName + ' ' + lastName;
  } else {
    return lastName;
  }
}
let kimXue= buildName('Xcat', 'Xue');
let kim= buildName('kim');
 
// index.ts(1,40): error TS1016: A required parameter cannot follow an optional parameter.
```
上面的代码会报错，因为在first那么这个属性的后面还有必须的属性，因此报错。
#### 参数默认值
在`ES6`中，我们允许给函数的参数添加默认值，`TypeScript`会将添加了默认值的参数识别为可选参数：
``` 
function buildName(firstName: string, lastName: string = 'Xue') {
  return firstName + ' ' + lastName;
}
let kimXue= buildName('Xcat', 'Xue');
let kim= buildName('kim');
```
此时就不受「可选参数必须接在必需参数后面」的限制了：
```
function buildName(firstName: string = 'kim', lastName: string) {
  return firstName + ' ' + lastName;
}
let kimXue= buildName('Xcat', 'Xue');
let kim= buildName('kim');
```
#### 剩余参数
`ES6`中，可以使用`...rest`的方式获取函数中的剩余参数(`rest`参数)：
```
function push(array,...items){
  items.forEach(function (item){
    array.push(item);
  })
}

let a = [];
push(a,1,2,3);
```
事实上，`items`是一个数组，所以我们可以用数组的类型去定义它：
```TypeScript
function push(array:any[],...items:any[]){
  items.forEach(function (item){
    array.push(item);
  })
}

let a = [];
push(a,1,2,3);
```
和上面的代码一样，`rest`参数只能是最后一个参数。
#### 重载
重载允许一个函数接受不同数量或是类型的参数，并作出不同的处理。
比如，我们需要实现一个函数`reverse`，输入数字`123`的时候，输出的就是数字`321`，输入字符串`‘hello’`的时候，输出翻转的字符串`‘olleh’`，利用联合类型，我们可以这么实现：
```TypeScript
function reverse(x: number | string): number | string {
  if (typeof x === 'number') {
    return Number(x.toString().split('').reverse().join(''));
  } else if (typeof x === 'string') {
    return x.split('').reverse().join('');
  }
}
```
然而这样写的话，还是有一个缺点，就是不能够精确地表达，输入为数字的时候，输出的也应该是数字，输入为字符串的时候，输出也应该为字符串。这时，我们可以用重载来实现多个这样的函数：
```TypeScript
function reverse(x: number): number;
function reverse(x: string): string;
function reverse(x: number | string): number | string {
  if (typeof x === 'number') {
    return Number(x.toString().split('').reverse().join(''));
  } else if (typeof x === 'string') {
    return x.split('').reverse().join('');
  }
}
```
## 类型断言
类型断言可以用来绕过编译器的类型判断，手动指定一个值的类型。
#### 语法
```
<类型>值
// 或
值 as 类型
// 在TSX语法 (React的JSX语法的TS版）中必须用后一种
```
**例子：将一个联合类型的变量指定为一个更加具体的类型**

当 `TypeScript` 不确定一个联合类型的变量到底是哪个类型的时候，我们只能访问此联合类型的所有类型里共有的属性或方法：
```TypeScript
function getLength(something: string | number): number {
  return something.length;
}
 
//index.ts(2,20): error TS2339: Property 'length' does not exist on type 'string | number'.
//Property 'length' does not exist on type 'number'.
```
这样的时候我们就需要使用类型断言，现将`something`断言成`string`：
```TypeScript
function getLength(something: string | number): number {
  if ((<string>something).length) {
    return (<string>something).length;
  } else {
    return something.toString().length;
  }
}
```
类型断言不是类型转换，断言成一个联合类型中不存在的类型是不允许的：
```
function toBoolean(something: string | number): boolean {
  return <boolean>something;
}
 
// index.ts(2,10): error TS2352: Type 'string | number' cannot be converted to type 'boolean'.
//   Type 'number' is not comparable to type 'boolean'.
```
## 声明文件
当我们使用第三方库时，我们需要引用它的声明文件：
#### 声明语法
当我们使用第三方库，比如`jQuery`，我们通常这样获取一个`id`是`foo`元素：
```
$('#foo');
// or
jQuery('#foo');
```
但是在 `TypeScript` 中，我们并不知道` $` 或` jQuery `是什么东西。这时，我们需要使用 `declare `关键字来定义它的类型，帮助 `TypeScript` 判断我们传入的参数类型对不对。
```
declear var jQuery:(string) =>any;
jQuery('#foo');
```
`declare` 定义的类型只会用于编译时的检查，编译结果中会被删除。
#### 声明文件
通常我们会把类型声明放在一个单独的文件中，这就是声明文件：
```
// jQuery.d.ts
 
declare var jQuery: (string) => any;
```
我们约定声明文件以` .d.ts `为后缀。
然后在使用到的文件的开头，用`「三斜线指令」`表示引用了声明文件：
```TypeScript
/// <reference path="./jQuery.d.ts" />
jQuery('#foo');
```