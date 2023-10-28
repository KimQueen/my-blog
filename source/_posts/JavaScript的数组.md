---
title: JavaScript的数组
categories: 数据结构
date: 2020-11-01 10:00:00
updated: 2020-11-01 10:00:00
tag:
  - 前端
  - JavaScript
  - 数组
  - 数据结构
toc: true
thumbnail: "/images/handle.jpg"
---

使用数组的目的：数组用来存储一系列同一种数据类型的值。但是在`JavaScript`中我们可以存储不同类型的值。
<!--more-->
## 创建和初始化数组
```
//三种声明数组的方式，使用new的方式进行声明
let dayOfWeek = new Array();
let dayOfWeek = new Array(7);
let dayOfWeek = new Array('Sunday','Monday','Tuseday','Wednesday',
'Thursday','Friday','Saturday');

// 两种声明数组的方式，使用[]的方式进行声明
let dayOfWeek = [];
let dayOfWeek =['Sunday','Monday','Tuseday','Wednesday',
'Thursday','Friday','Saturday'];

//数组可以使用length描述数组的长度
console.log(dayOfWeek.length); // 输出：7
```


## 添加元素
从数组中添加和删除元素也是很容易的，但有时也会很棘手，假设我们有一个数组的`numbers`，初始化成0到9：
```
let numbers = [0,1,2,3,4,5,6,7,8,9];
```
如果想要给数组添加一个元素(比如说10)，只要把值赋给数组中最后一个空位上的元素即可。
```
numbers[numbers.length] = 10;
```
#### 使用push方法
可以使用`push`的方法，能把元素添加到数组的末尾。通过`push`方法，能添加任意个元素:
```
numbers.push(11);
numbers.push(12,13);
```
输出的结果是0,1,2,3,4,5,6,7,8,9,10,11,12,13
#### 插入元素到数组的首位（传统方法）
如果我们想要想数组的最前面插入一个值，而不是像之前一样插入到后面，为了实现这个需求，我们首先要腾出数组第一位的位置，把所有元素的位置向右移动一位，我们可以循环数组中的元素，从最后我们把值赋值到第一位上面，下面具体讲一下具体实现的逻辑：
```
for(let i = numbers.length ; i >= 0; i--){
  numbers[i] = number[i-1];
}
numbers[0] = -1;
```
#### 使用unshift方法
在`JavaScript`中，数组中有一个方法叫做`unshift`，可以直接将数值插入到数组的首部：
```
numbers.unshift(-2);
numbers.unshift(-4,-3);
```
输出的结果为`[-4, -3, -2, -1, 0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10]`。
## 删除元素
如果我们要删除数组中最后的一个元素，我们可以使用`pop`方法来进行操作,具体的调用的方法就是：
```
numbers.pop();
```
#### 从数组的首位删除元素（传统方法）
如果我们想要从数组的第一个元素，可以使用下面的代码：
```
for(let i = 0 ; i < numbers.length; i++){
  numbers[i] = number[i+1];
}
```
#### 使用shift方法
如果想要删除数组的第一个元素，可以使用`shift`方法来实现：
```
numbers.shift();
```
## 在任意的位置添加和删除元素 
我们可以使用`splice`方法，简单的通过指定`位置/索引`，就可以删除相应位置和数量的元素：
```
numbers.splice(5,3);
```
这行代码的意思就是从数组索引5开始的3个元素，也就意味着`numbers[5]`,`numbers[6]`,`numbers[7]`从数组中删除。
如果我们想把数字2,3,4插入到数组里，放在之前删除的位置上面，我们可以再一次使用`splice`方法:
```
numbers.splice(5,0,2,3,4);
```
那我们介绍一下`splice`各个参数的意义：`splice`的第一个参数，表示想要删除或是插入元素的索引值。第二个参数是删除的元素的个数，第三个参数往后，就是要添加到数组里的值。
## 二维和多维数组
如果我们想要记录数天每小时的气温，我们使用数组来保存这些数据，我们应该怎么记录，我们可以这样记录：
```
var averageTempDay1 = [72,75,79,81,81,80];
var averageTempDay1 = [81,79,75,75,73,72];
```
但是这不是最好的方法，我们可以做到的更好，我们可以使用矩阵(二维矩阵)来存储这些信息，矩阵的行保存每一天的数据，列对应小时级别的数据：
```
let averageTemp = [];
averageTemp[0] =  [72,75,79,81,81,80];
averageTemp[1] =  [81,79,75,75,73,72];
```
`JavaScript`只支持一维数组，并不支持矩阵。但是，我们可以像上面的代码一样，用数组的嵌套来实现一维或是多维数组，代码也可以这样写：
```
let averageTemp = [];
// day 1
averageTemp[0] = [];
averageTemp[0][0] =72;
averageTemp[0][1] =75;
averageTemp[0][2] =79;
averageTemp[0][3] =81;
averageTemp[0][4] =81;
averageTemp[0][5] =80;
// day 2
averageTemp[1] = [];
averageTemp[1][0] =81;
averageTemp[1][1] =79;
averageTemp[1][2] =75;
averageTemp[1][3] =75;
averageTemp[1][4] =73;
averageTemp[1][5] =72;
```
#### 迭代二维数组的元素
如果我们想看一个二维数组的输出，我们可以创建一个通用的函数，专门输出里面的值：
```
function printMatrix(myMarix){
  for(let i = 0; i< myMarix.length; i++){
    for(let j = 0; j < myMarix[i].length; j++){
      console.log(myMarix[i][j]);
    }
  }
}
```
#### 多维数组
假如我们需要创建一个`3X3X3`的矩阵，每一格里包含矩阵的`i`（行）`j`（列）以及`z`（深度）之和：
```
let matrix3x3x3 = [];
for(let i =0; i < 3; i++){
  matrix3x3x3[i] = [];
  for(let j =0; j < 3; j++){
      matrix3x3x3[i][j] = [];
      for(let z =0; z < 3; z++){
          matrix3x3x3[i][j][z] = i + j + z;
    }
  }
}
```
我们如果要遍历这个三维数组的话，我们具体的操作如下：
```
for(let i =0; i < matrix3x3x3.length; i++){
  for(let j =0; j < matrix3x3x3[i].length; j++){
      for(let z =0; z <matrix3x3x3[i][j].length; z++){
          matrix3x3x3[i][j][z] = i + j + z;
    }
  }
}
```
## JavaScript的数组方法参考
#### 数组合并
我们设想一下下面的场景：有多个数组，需要合并起来变成一个数组，我们可以迭代各个数组，然后把每一个元素加入最终的数组，现在`JavaScript`已经为我们提供了方法，叫做`concat`方法：
```
let zero = 0;
let positiveNumbers = [1,2,3];
let negativeNumbers = [-3,-2,-1];
let numbers = negativeNumbers .concat(zero,positiveNumbers )
```
上面的数组里面的数是从-3到3，在这个例子里面，`zero`将被合并发哦`negativeNumbers `数组的后面，然后`positiveNumbers` 也将会被合并到已经合并了`zero`的`negativeNumbers `数组里面。
#### 迭代器函数
加入有一个数组，他的值是从1到15，如果数组中的元素可以被2整除（偶数），函数返回`true`，否则就要返回`false`。
```
let isEven = function(x){
  console.log(x);
  return (x % 2 == 0) ? true :false;
}
let numbers = [1,2,3,4,5,6,7,8,9,10,11,12,13,14,15];
```
1.使用`every`方法进行迭代
`every`方法会迭代数组中的每一个元素，直到返回`false`。
```
numbers.every(isEven)
```
这个例子里面，数组的第一个元素是1，不是2 的倍数，因此`isEven`函数返回`false`，然后`every`就执行结束了。
2.用`some`方法进行迭代
它和`every`的行为类似，不过`some`方法会迭代数组的每一个元素，直到函数返回`true`。
```
numbers.some(isEvent)
```
在我们的例子中，`numbers`数组的第一个偶数是2(第二个元素)，第一个被迭代的元素是1，`isEven`会返回false，第二个被迭代的元素是2，`isEven`返回`tru`e，`isEven`返回`true`-- 迭代结束。
3.`forEach`方法迭代
如果要迭代整个数组的话，可以使用`forEach`方法，它和使用`for`循环的结果是一样的：
```
numbers.forEach (function(x){
  console.log((x%2 == 0))；
})；
```
4.使用`map`和`filter`方法
`JavaScript`还有两个会返回新数组的遍历方法，第一个是`map`：
```
let myMap = numbers.map(isEven);
```
现在数组`myMap`里面的值是`[false,true,false,true,false,true,false,true,false,true,false,true,false,true,false]`。这个数组保存的是传入`map`方法的`isEven`函数的运行结果，这样就很好的判断一个元素是不是偶数，`myMap[0]`是`false`，所以`myMap[0]`不是偶数，`myMap[1]`是`true`，因此`myMap[1]`是偶数。
5.使用`reduce`方法
`reduce`方法接受一个函数作为参数，这个函数有4个参数：`previuosValue`，`currentValue`，`index`和`array`。这个函数会返回一个将叠加到累加器的值。`reduce`方法停止执行后会返回这个累加器。如果要对一个数组中的所有元素求和，这就很有用：
```
numbers.reduce(function (previous,current,index){
  return previous + current;
})
```
![reduce参数详情](https://upload-images.jianshu.io/upload_images/13681871-56e43dc408939eab.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
## ECMAScript 6和数组的新功能
1.使用`forEach`和箭头函数迭代
箭头函数可以简化使用`forEach`迭代数组元素的做法，我们给出例子如下：
```
numbers.forEach(function (x){
  console.log( x % 2 === 0);
})

// 进行代码的简化
numbers.forEach( x =>{
  console.log( x % 2 === 0);
})
```
2.使用`for...of`循环
```
for(let n of numbers){
  console.log( (n%2 == 0 ) ? 'even' : 'odd');
}
```
3.使用`ES6`新的迭代器(`@@iterator`)
`ES6`还为`Array`类增加了一个`@@iterator`属性，需要通过`symbol.iterator`来访问。代码如下：
```
let iterator = numbers[Symobol.iterator]();
console.log(iterator.next().value); //1
console.log(iterator.next().value); //2
console.log(iterator.next().value); //3
console.log(iterator.next().value); //4
console.log(iterator.next().value); //5
```
只要不断的调用迭代器的`next`方法，就能一次得到数组中的值。`numbers`数组中有15个值，因此需要调用15次`iterator.next().value`。数组中的所有值都迭代完之后，`iterator.next().value`会返回`undefined`。
数组的`entries`、`key`和`values`
- 我们先来看一下`entries`方法，`entries`方法返回包含键值对的`@@iterator`，下面我们对这个方法进行演示：
```
let aEntries = numbers.entries(); //得到键值对的迭代器
console.log(aEntries .next().value); //[0,1] - 位置0的值为1
console.log(aEntries .next().value); //[1,2] - 位置1的值为2
console.log(aEntries .next().value); //[2,3] - 位置2的值为3
```
在`numbers`数组中都是数字，`key`对应数组中的位置，`value`是保存在数组索引的值。
- `keys`方法返回包含数组索引的`@@iterator`，下面我们对这个方法进行演示：
```JavaScript
let akeys = numbers.keys();
console.log(akeys.next()); //{value:0,done:false}
console.log(akeys.next()); //{value:1,done:false}
console.log(akeys.next()); //{value:2 ,done:false}
```
`keys`方法会返回`numbers`数组的所以，一旦没有可迭代`akeys.next()`就会返回一个`value`属性为undefined，`done`属性为`true`的对象。如果`done`属性的值为`false`，就意味着还有可以迭代的值。
- `values`方法返回`@@iterator`则包含数组的值，使用这个方法的代码示例如下：
```JavaScript
let aValues = numbers.values();
console.log(aValues .next()); //{value:1,done:false}
console.log(aValues .next()); //{value:2,done:false}
console.log(aValues .next()); //{value:3 ,done:false}
```
4.使用`from`方法
`Array.from`方法根据已有的数组创建一个新的数组，比如，要复制`numbers`数组，可以这么做：
```
let numbers = Array.from (numbers);
// 还可以传入一个过滤函数
let evens = Array.from (numbers, x=>(x % 2 == 0));
```
5.使用`Array.of`方法
`Array.of`方法根据传入的参数创建一个新的数组，以下面的数组为例：
```
let numbers3 = Array.of(1);
let numbers4 = Array.of(1,2,3,4,5,6,7);

//代码等效于
let numbers3 = [1];
let numbers4 =[1,2,3,4,5,6,7];

// 我们也可以根据这个方法赋值已有的数组
let numbersCopy = Array.of(...numbers4);
```
6.使用`fill`方法
`fill`方法用静态值来进行数组的填充，下面的代码为例：
```
let numbersCopy = Array.of(1,2,3,4,5,6,7);
numbersCopy.fill(0); // [0,0,0,0,0,0,0]
numbersCopy.fill(2,1); // [0,2,2,2,2,2,2]
numbersCopy.fill(1,3,6); // [0,2,2,1,1,1,2]
```
7.使用`copyWithin`方法
`copyWithin`方法复制数组中的一系列元素到同一数组指定的起始位置。
```
let numbers4 =[1,2,3,4,5,6];
numbers4.copyWithin(0,3) // 把4.5.6三个值复制到数组的前三个位置

let numbers5 =[1,2,3,4,5,6];
numbers5.copyWithin(1,3,5) //把4.5.6三个值复制到数组的1,2两个位置上
```
#### 排序元素
-`reverse()`
reverse方法让元素中的数组反序.
```
let numbers = [1,2,3,4,5,6];
numbers.reverse(); // [6,5,4,3,2,1]
numbers.sort() //[1,2,3,4,5,6];
```
- 自定义排序
我们可以对任何对象类型的数组进行排序，也可以创建`compareFunction`来比较元素。例如对象`Person`有名字和年龄属性，我们希望可以按照年龄进行排序。
```
let persons = [
  {name:'John',age:30},
  {name:'Ana',age:20},
  {name:'Chris',age:25},
];
function compareFunction(a,b){
  if(a.age < b.age){
    return -1;
  }
 if(a.age > b.age){
    return 1;
  }
  return 0;
}
console.log(persons.sort(compareFunction));
```
 - 字符串排序
```
let names = ['Ana','ana','john','John'];
console.log(names.sort());
//输出： Ana John ana john
```
输出上面的情况是因为支付串的排名是按照`ASCII`值来进行排序的。现在如果我们输入一个忽略大小写的比较函数：
```
name.sort(function(a,b){
  if(a.toLowerCase() < b.toLowerCase()){
    return -1;
  }
  if(a.toLowerCase() > b.toLowerCase()){
    return 1;
  }
  return 0;
})
```
#### 搜索
- 搜索有两个方法：`indexOf`方法返回与参数匹配的第一个元素的索引，`lastIndexOf`返回与参数匹配的最后一个元素的索引。
- 在`ECMAScript6`新增了方法-- `find`和`findIndex`方法
```
let numbers =[1,2,3,4,5,6,7,8,9,10,11,12,13,14,15];
function multipleOf13(element,index,array){
  return (element % 13 == 0) ? true :false;
}
console.log(numbers.find(multipleOf13)); // 返回第一个满足条件的值
console.log(numbers.findIndex(multipleOf13));//返回第一个满足条件的值的索引
```
- `ECMAScript7`中又议案家了`includes`方法
这个方法的使用方式是如果数组中存在某个元素，`includes`方法会直接返回`true`，否则就会返回`false`，现在我们通过一段代码进行具体的阐述：
```
console.log(numbers.includes(15)); // true
let numbers = [7,6,5,4,3,2,1];
// 因为查找是从数组的索引为5之后开始的，所以没有4
console.log(numbers.includes(4,5)); // false
```
