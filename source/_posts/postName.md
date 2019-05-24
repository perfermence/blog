---
title: 浅谈javascript中的数据类型
date: 2019-04-30 18:47:59
tags:
   - javascript 
categories:
   - 笔记
---
### JS中的数据类型（重点）
>- 基本数据类型（值类型）
>  + number:数字
>  + string:字符串
>  + boolean:布尔
>  + null：空对象指针
>  + undefined:未定义
> - 引用数据类型
>  + object 对象数据类型
>      + {}普通对象
>      + []数组
>      + /^$/正则
>      + ...
>   + function 函数数据类型


```javascript
12 12.5 -12.5 0
'fhjf' "祝福" =>单双引号包裹起来的都是字符串（单双引号没有区别）
true false=> 布尔类型：只有两个值;
null
undefined

{name:'dfd','dfdf'}
[12,23,34]
/^-?(\d|([1-9]\d+))(\.\d+)?$/
function fn(){}

```
这么多数据类型JS如何去检测？(背)
- typepof: 检测数据类型的运算符
- instanceof: 检测某个实例是否属于这个类
- constructor: 获取当前实例的构造器
- Object.prototype.toString.call: 获取当前实例的所属类信息

**typeof**
> 使用typeof检测，返回的结果是一个字符串，字符串中包含的内容证明了值是属于什么类型的
>
> [局限性]
> 1.typeof null 不是'null'而是'object' : 因为Null虽然是单独的一个数据类型，但是它原本意思是空对象指针，浏览器使用typeof检测的时候会把它按照对象来检测
> 2.使用typeof 无法具体细分出到底是数组还是正则，因为返回的结果都是'object'
```javascript
typeof 12;  =>"number"

var num=13;
typeof num;=>"number" //实际检测的是13的数据类型，num 无意义
```
![Alt text](./1550739939224.png)
![Alt text](./1550740266812.png)

腾讯面试题：
```javascript
console.log(typeof typeof []);=>"string"
typeof [] ->"object"   //因为typeof返回的值就是一个字符串
typeof "object" ->"string" //检测的类型是字符串
```

<!-- more -->

----------

### 布尔
**`Boolean()  `**
> 把其他数据类型的值转换为布尔类型
>  
>  只有‘0 、NaN、 空字符串、 null、undefined’ 这五个数值转换为布尔类型的false，其余的都会变为true。

**` !`**
> !=: 不等于
> 叹号在JS中还有一个作用：`取反`，先把值转换为布尔类型，然后再去取反

**` !!`**
> 在一个叹号取反的基础上再取反，取两次反相当于没有做操作，但是却已经把其它类型值转换为布尔类型了，和Boolean是相同的效果

-----------
### 字符串
> 在JS中， 单引号和双引号包起来的都是字符串
```javascript
12->number;
'12' ->string;
'[12,23]'->srting;
```

常用方法：
charAt  charCodeAt  substr  substring slice  toUpperCase toLowerCase  indexOf  lastIndexOf 
 spilt 
 replace
 match
 ...

------------------
### number数字
> 0 12 -12 23.5   JS中新增加了一个number类型的数据：`NaN`
> typeof NaN  -> "number"

**`NaN`**
> not a number :不是一个数（即除了是数，其他什么都可以），但是属于number类型
>  NaN== NaN: false（左侧可以是除数以外的东西，右侧可以是其他的，所以不等）,  NaN和任何其他值都不相等

**`isNaN()`**
> 用来检测当前这个值是否是非有效数字，如果不是有效数字检测的结果是TRUE，反之有效数字则为false。

```javascript
isNaN(0) ->FALSE
isNaN(NaN) ->TRUE

isNaN('12') ->false
//当我们使用isNaN检测值的时候，检测的值不是number类型的，浏览器会默认的把值先转换为number类型，然后再去检测

isNaN([]) ->FALSE  //非number类型，先使用Number([])转换为0 ，0是有效数字，则答案为false
```

**`Number()`**
> 把其他数据类型值转化为number类型的值
```javascript
Number('12') ->12
Number('12px') ->NaN
//在使用Number转换的时候只要字符串中出现任何一个非有效数字字符，最后的结果都是NaN

Number（true） ->1
Number（false） ->0
Number（null） ->0
Number（undefined） ->NaN

Number（[]） ->NaN   //把引用数据类型转换为number，首先需要把引用数据类型转换为字符串（toString）,在把字符串转换为number即可。例如：[]->'' ''->0
Number（[12]）=>[12]->'12' '12'->12

Number（[12,23]）=>[12,23]->'12,23' '12,23'->NaN


```
**`parseInt()`**
> 也是把其他数据类型值转换为number，和Number方法在处理字符串的时候有所区别
```javascript
Number('12px') ->NaN
parseInt('12px') ->12
parseInt('12px13') ->12 提取规则：从左到右依次查找有效数字字符，直到遇见非有效数字字符为止（不管后面是否还有，都不找了），把找到的转换为数字
parseInt('px12') ->NaN

parseInt('12.5') ->12(因为int是整型)
```

**`parseFloat()`**
> 在parseInt的基础上可以识别小数点

```javascript
parseFloat('12.5') ->12.5

```

思考：parseInt常用的只需要传递一个值做参数即可，但是它支持多个参数，其他参数的意思
例如：parseInt('12.5',10)
parseInt(string, radix) 
1.parseInt方法还可以接受第二个参数（2到36之间），表示被解析的值的进制，返回该值对应的十进制数。默认情况下，parseInt的第二个参数为10，即默认是十进制转十进制
2.如果第二个参数不是数值，会被自动转为一个整数。这个整数只有在2到36之间，才能得到有意义的结果，超出这个范围，则返回NaN。如果第二个参数是0、undefined和null，则直接忽略。
3.当参数 radix 的值为 0，或没有设置该参数时，parseInt() 会根据 string 来判断数字的基数。
举例，如果 string 以 "0x" 开头，parseInt() 会把 string 的其余部分解析为十六进制的整数。
parseInt("10");			//返回 10
parseInt("19",10);		//返回 19 (10+9)
parseInt("11",2);		//返回 3 (2+1)
parseInt("17",8);		//返回 15 (8+7)
parseInt("1f",16);		//返回 31 (16+15)
parseInt("010");		//未定：返回 10 或 8

--------------------------------------------

### null和undefined
> null : 空，没有
> undefined: 未定义，没有
>  
>  '': 空字符串，没有
>  0 : 也可以理解为没有

`空字符串和null的区别`
> 比如，都是去种树，空字符串属于挖了一个坑，但是没有种任何东西，而null连坑都没挖
>  
>  空字符串相对于null来说开辟了内存，消耗了那么一丢丢的性能


`null和undefined的区别`
> null一般都是暂时没有，预期中以后会有的（可能以后也没有达到预期）：在JS中null一般都是手动先赋值为null，后期我们再给其赋具体值
>  
>  undefined: 完全没在预料之内的
> 例如：他是一个帅气的男孩子，正常情况下，他现在的女朋友是null，他的男朋友是undefined。


---------------------
### 对象数据类型object
这里先只介绍{}普通对象
> var obj={name:"fa",age:8};
> 每一个对象都是由零到多组`属性名（key键）：属性值（value值）`组成的，或者说有多组 键值对组成的，每一组键值对中间用逗号分隔
>  
>  属性：描述这个对象特点特征的。
>  对象的属性名一般是字符串或者数字格式的，存储的属性值可以是任何的数据类型
>   
>   获取操作方式 ：
>  -  1.对象名.属性名  ：忽略了属性名的单双引号         
>  -  2.对象名[属性名] ：不能忽略单双引号

```javascript
 var obj={name:"fa",age:8,friend:['习大大','彭麻麻'],0:100};
 //=>获取某个属性名对应的属性值
 obj.name;
 obj['name']
 //->如果属性名是数字如何操作
 //object.0  语法不支持
 // object[0] / object['0']  两者都可以

//->如果操作的属性名在对象中不存在，获取的结果是undefined
obj.sex  =>undefined

// =>设置/修改：一个对象的属性名是不能重复的（唯一性），如果之前存在就是修改属性值的操作，反之不存在就是新设置属性的操作
obj.sex="男";  //此处是设置，因为之前没有sex属性
obj['age']='9'  //此处是修改，因为之前存在age属性

//=> 删除
//-> 假删除：让其属性值赋值为null，但是属性还在对象中
obj.sex=null;
//-> 真删除：把整个属性都在对象中暴力删除
delete obj.sex;
```
思考题：obj[age]和obj['age']有和区别？
```javascript
var obj={name:'af',age:8};
var age='9';
obj[age]; //->undefined
obj['age'];//->8
```

-----------------
###基本数据类型和引用数据类型的区别(important)
> JS是运行在浏览器中的（内核引擎），浏览器会为JS提供赖以生存的环境（提供给JS代码执行的环境） =》 全局作用域  window(前端称)  global（后台称）

```javascript
var a = 12;
var b = a //=>把A变量存储的值赋值给B
b=13;
console.log(a); //=> 12



var n = {name:'zhufeng'};
var m=n; 
m.name='jk';
console.log(n.name); //=>jk

```
`基本数据类型是按值操作的`：基本数据类型在赋值的时候是直接把值赋值给变量即可

`引用数据类型是按照空间地址（引用地址）来操作的`：
var n = {name:'珠峰'};
引用数据类型的操作：
1.先创建一个变量n
2.浏览器首先会开辟一个新的存储空间（内存空间），目的是把对象中需要存储的内容（键值对）分别的存储在这个空间当中。为了方便后期找到该空间，浏览器给空间设定了一个地址（16进制的）
3.把空间地址赋值给了变量


-----------------------
### 函数数据类型
> 函数数据类型也是按照引用地址来操作的（其隶属于引用数据类型）
>  
>  函数：具备一定功能的方法

```javascript
//=> 创建函数：相当于生产了一台洗衣机
function 函数名(){
       //=> 函数体：实现某一个功能的具体JS代码

}

// => 执行函数：相当于使用洗衣机洗衣服（如果函数只创建但是没有执行，那么函数没有任何的意义）
函数名();

```

```javascript
function fn(){
	console.log(1+1);
}

fn;        //=>输出函数本身
fn();     //=>把函数执行（把函数体中实现功能的代码执行）
```
![Alt text](./1550822696428.png)

```javascript
//=> 形参：形式参数（变量），函数的入口
//=> 当我们创建一个函数想要实现某个功能的时候，发现有一些材料并不清楚，只有当函数运行的时候，别人传递给我才知道，此时我们就需要设定入口，让用户执行的时候通过入口把值给我们
function fn(num1,num2){
    console.log(num1+num2);

}

//=> 实参：函数执行传递给函数的具体值就是实参
fn(1,3);
fn(1,4);

```