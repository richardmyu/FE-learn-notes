## 三.语法

### 1.组成

1).**ECMAScript**：定义了js里面的命名规范、变量、数据类型、基本语法、操作语句等最核心的东西；

2).**DOM**（document object modle）文档对象模型 ：DOM结构中提供了很多用来操作DOM元素的方法和属性（`api`）；

3).**BOM**（brower object modle）浏览器对象模型 ：提供一系列的方法（`api`）来操作浏览器；

### 2.命名规范

1).严格区分大小写；

2).使用驼峰命名法：

- --
 - a.首字母小写，其余每个有意义的单词首字母大写；
 - b.可以使用数字（但不能作为首位）、字母、下划线、`$`；
 - c.不能使用关键字（在js中有特殊意义的字）和保留字（未来可能成为关键字的词）；
- --

> 规范如下：
1.变量：匈牙利命名法
2.函数：Camel(第一个单词首字母小写，其他单词首字母大写)
3.属性：Camel

|类型 | 前缀|实例|
|--|:--:|--|
|Array|a|aNameList|
|Boolean|b|bVisible|
|Float|f|fMoney|
|Function|fn|fnMethod|
|Int|i|iAge|
|Object|o|oType|
|Regexp|re|rePattern|
|String|s|sName|

### 3.JavaScript中的变量 

#### 1.`var` 

1).用来声明一个变量（对于未经初始化的变量，会保存一个特殊的值:`undefined`），可以重复声明；

2).在全局域下用`var`关键字定义一个变量a或者不使用声明变量而直接写一个变量则表示该变量为全局变量，都相当于给`window`增加一个属性a，可以写成`window.a`；

3).用`var`操作符定义的变量将成为定义该变量的作用域中的局部变量，在该作用域之外无法访问该变量。也就是说，如果在函数中使用`var`定义一个变量，那么这个变量在函数退出后就会被销毁；

4).可以用一条语句定义多个变量，用逗号隔开(因为ECMAScript的变量是松散类型的，所以不同类型初始化变量的操作可以放在一条语句中来完成)。

严格地说，`var a = 1` 与 `a = 1`，这两条语句的效果不完全一样，主要体现在`delete`命令无法删除前者。不过，绝大多数情况下，这种差异是可以忽略的。

> 严格模式下，不能定义`eval`或`arguments`的变量，否则导致语法错误。

虽然省略`var`操作符可以定义全局变量，但我们不推荐这种做法。因为在局部作用域中定义的全局变量很难维护，而且如果有意的忽略`var`操作符，也会由于相应的变量不会马上就有定义而导致不必要的混乱。给未经声明的变量赋值在严格模式下会导致抛出`ReferenceRrror`错误。

#### 2.变量提升

JavaScript 引擎的工作方式是，先解析代码，获取所有被声明的变量，然后再一行一行地运行。这造成的结果，就是所有的变量的声明语句，都会被提升到代码的头部，这就叫做**变量提升（hoisting）**。

变量提升是一种机制，在作用域形成后，代码执行之前，把关键词`var`和`function`提前声明或定义；`var`只会声明，`function`定义；

> JS的变量提升是以段为处理单元的。

对引用数据类型变量提升，会把内容放在堆内存，任意类型数据都会记录（变量也会存放），若是表达式（函数也是表达式）则将结果存放；待代码执行时，才将对应的变量赋值；

变量提升注意事项：

- --

1).`if`或`while`判断语句中：不管条件判断是否成立，判断体中的内容都要进行变量提升，但是`var`和`function`都只能起声明的作用（即块级作用域之外，`function`只作声明）；若条件成立，先给`function`赋值再执行代码；

- --

2).变量提升的时候只对"`=`"等号左边的变量进行变量提升，右边代表的都是值，是不进行变量提升的；

- --

3).在全局作用域下变量提升的时候，自执行函数中的`function`是不参与的，当代码执行到对应的区域后，声明、定义、执行一起完成；

- --

4).匿名函数当做参数的时候不进行变量提升；

- --

5).虽然函数体中`return`下面的代码是不执行的，但是需要进行私有作用域下的变量提升；而`return`的代码会执行，但是不进行变量提升的；

- --

6).在变量提升的时候，如果发现名字冲突了，不需要重新声明，但是需要重新的赋值。（在JS中不管是变量还是函数，只要名字一样了，就是相互冲突，JS中一个名字就代表一个变量，只不过存储的值可以是任意数据类型的）；

- --

7).对于函数定义式，会将函数定义提前。而函数表达式，会在执行过程中才计算。

- -- 

> 变量提声时，先声明的拥有声明权，后声明的会被忽略；但是不会忽略赋值行为；

#### 3.变量赋值

变量进行连续赋值时，只有最左边才会被声明，中间变量不会被声明（但有赋值）； 

`var m = n = l = 2;`

#### 4.执行环境

**执行环境（execution context）**定义变量或函数有权访问的其他数据，决定了它们各自的行为。每个执行环境都有一个与之关联的**变量对象（variable object）**，环境中定义的所有变量和函数都保持在这个对象中。

#### 5.作用域链

当代码在一个环境中执行的时候，会创建变量对象的一个**作用域链（scope chain）**；

在私有作用域中变量调用时，先检查私有作用域是否有变量声明，若没有则往上一级查找，直至`window`，若还找不到则报错，这个过程称为作用域链；

> 私有作用域之间没有关联；

#### 6.作用域销毁机制

堆内存销毁需要手动销毁，否则待全局作用域退出才销毁 （即赋值`null`）;

- --
栈内存：
  - 全局：关闭浏览器；
  - 私有：
      - 销毁：一个函数没有返回值，或者返回值的内容没有被外界占用（返回值为引用数据类型，且被外界变量接受）；
      - 不销毁：一个函数返回值（通常是函数）被外界占用；
- --

### 4.数据类型

ECMAScript的数据类型具有动态性

#### 0.`typeof`操作符

`typeof`用来检测给定变量的数据类型。对一个值使用`typeof`操作符可能返回以下某个字符串：

- --
- `“undefined”`  该值未定义
- `“boolean”`  该值是布尔值
- `“string”`  该值是字符串
- `“number”`  该值是数值
- `“object”`  该值是对象或者`null`
- `“function”`  该值是函数
- --

```
s; // ReferenceError: v is not defined

console.log(typeof s); //undefined

s="tian";
console.log(typeof s); //string
```

上面代码中，变量s没有用`var`命令声明，直接使用就会报错。但是，放在`typeof`后面，就不报错了，而是返回`undefined`。

实际编程中，这个特点通常用在判断语句。

```
// 错误的写法
if (s) {
  // ...
}
// ReferenceError: s is not defined

// 正确的写法
if (typeof s === "undefined") {
  // ...
}
```

`typeof`操作符的操作数可以是变量，也可以是字面量。注意`typeof`是一个操作符而不是函数，因而圆括号可用可不用。

`typeof`操作符对`null`返回`"object"`，因为特殊值`"object"`被认为是一个空的对象引用。Safari及之前版本、chrome7及之前版本对正则表达式调用`typeof`操作符时会返回`"function"`，而其他浏览器在这种情况下会返回`"object"`。

> 从技术角度讲，函数在ECMAScript中是对象，不是一种数据类型。然而，函数也确实有一些特殊的属性，因此通过`typeof`操作符来区分函数和其他对象是很有必要的。

#### 1.基本数据类型（值类型）

由简单的结构组成

1).`number`  数字
2).`string`  字符串
3).`boolean`  布尔
4).`null`  空对象指针
5).`undefined` 未定义(即由于目前没有定义，所以此处暂时没有任何值)
6).`Symbol` 

> 为了避免属性名的冲突，ES6新增了`Symbol`类型，`Symbol`是一种原始数据类型，表示独一无二的值；

#### 2.引用数据类型（地址类型）

结构相对复杂

1).`object`  对象数据类型
2).`function` 函数数据类型

引用类型的值（对象）是引用类型的一个实例。在ECMAScript中，**引用类型**是一种数据结构，用于将数据和功能组织在一起。它也常被称为**类**，但这并不妥当。尽管从技术上讲，ECMAScript是一门面向对象的语言，但它不具备传统的面向对象语言所支持的类和接口等基本结构。引用类型有时也被称为**对象定义**，因为它描述的是一类对象所具有的属性和方法。

对象是某个特定引用类型的实例。新对象是使用`new`操作符后跟一个构造函数来创建的。构造函数本身就是一个函数，只不过该函数是出于用来创建新对象而定义的。

> 函数其实是处理数据的方法，JavaScript 把它当成一种数据类型，可以赋值给变量，这为编程带来了很大的灵活性，也为 JavaScript 的“函数式编程”奠定了基础。

#### 3.基本数据类型和引用数据类型的区别

原始值：存储在**栈（stack）**中的简单数据，也就是说，它们的值直接存储在变量访问的位置。这是因为这些原始类型占据的空间是固定的，所以可将他们存储在较小的内存区域 – 栈中。这样存储便于迅速查寻变量的值。
 
引用值：存储在**堆（heap）**中的对象，也就是说，存储在变量处的值是一个**指针（point）**，指向存储对象的内存地址。这是因为：引用值的大小会改变，所以不能把它放在栈中，否则会降低变量查寻的速度。相反，放在变量的栈空间中的值是该对象存储在堆中的地址。地址的大小是固定的，所以把它存储在栈中对变量性能无任何负面影响。

基本数据类型是把值直接的给变量，在接下来的操作过程中，直接拿这个值进行操作但是两者直接不会相互影响，各自独立；

而引用数据类型：

- --
- a：定义了一个变量；
- b：开辟了一个新空间，并把属性名和属性值存储在这个空间中，有相应的空间地址；
- c：把空间地址给这个变量（不是直接给值），变量并没有存储这个值，而是存储这个值的引用空间地址；
- d：接下来我们把这个地址传递给新变量，此时两个变量操作的是同一空间，当其中一个变量改变，另一个变量也会改变（因为引用空间里的值发生改变）。
- --

#### 4.`undefined`类型

`undefined`是JavaScript独有的数据和数据类型，这种类型数据只有一个值，即`undefined`，它的类型也是`undefined`。

> 一般而言，不需要显示的把变量的值设置为`undefined`。字面值`undefined`的主要目的是用来比较。ECMA-262第3版引入这个值，是为了正是区分空对象指针与未经初始化的变量。

不过，包含`undefined`值的变量与尚未定义的变量还是不一样的：

```
var mess;
console.log(mess);//undefined
console.log(me);
//Uncaught ReferenceError: me is not defined

console.log(typeof mess);//undefined
console.log(typeof me);//undefined
```

对于尚未定义的变量，只能进行一项操作，即使用`typeof`检测其数据类型（使用`delete`也不会报错，但没什么实际意义，严格模式下则会报错）。

然而对未初始化的变量和未声明的变量执行`typeof`操作，均返回`undefined`，这个结果有其逻辑上的合理性。因为虽然这两种变量从技术角度看有本质区别，但实际上无论对哪种变量也不可能执行真正的操作。

`undefined`表示“未定义”，下面是返回`undefined`的典型场景。

```
// 变量声明了，但没有赋值
var i;
i // undefined

// 调用函数时，应该提供的参数没有提供，该参数等于 undefined
function f(x) {
  return x;
}
f() // undefined

// 对象没有赋值的属性
var  o = new Object();
o.p // undefined

// 函数没有返回值时，默认返回 undefined
function f() {}
f() // undefined
```

#### 5.`null`类型

`null`是JavaScript第二个独有的数据和数据类型，这种类型数据只有一个值，即`null`，它的类型也是`null`。从逻辑角度看，`null`值表示一个空对象指针，而这也是使用`typeof`操作符检测`null`值返回`"object"`的原因。

`null`的类型是`object`，这是由于历史原因造成的。1995年的 JavaScript 语言第一版，只设计了五种数据类型（对象、整数、浮点数、字符串和布尔值），没考虑`null`，只把它当作`object`的一种特殊值。后来`null`独立出来，作为一种单独的数据类型，为了兼容以前的代码，`typeof null`返回`object`就没法改变了。

如果定义的变量准备用来保存一个对象，那么最好将该变量初始化为`null`而不是其他值。这样只要直接检测`null`值就可以知道相应的变量是否保存了一个对象的引用。

只要在意保存对象还没有真正的保存对象，就应该明确地让该变量保存`null`值。这样不仅可以体现`null`作为空对象指针的惯例，而且有助于区分`null`和`undefined`。

`null`表示空值，即该处的值现在为空。调用函数时，某个参数未设置任何值，这时就可以传入`null`，表示该参数为空。比如，某个函数接受引擎抛出的错误作为参数，如果运行过程中未出错，那么这个参数就会传入`null`，表示未发生错误。

#### 6.`null`和`undefined`

`null`和`undefined`，既然含义与用法都差不多，为什么要同时设置两个这样的值，这不是无端增加复杂度？这与历史原因有关。

1995年 JavaScript 诞生时，最初像 Java 一样，只设置了`null`表示”无”。根据 C 语言的传统，`null`可以自动转为0。

```
Number(null) // 0
5 + null // 5
```

但是，JavaScript 的设计者 Brendan Eich，觉得这样做还不够。首先，第一版的 JavaScript 里面，`null`就像在 Java 里一样，被当成一个对象，Brendan Eich 觉得表示“无”的值最好不是对象。其次，那时的 JavaScript 不包括错误处理机制，Brendan Eich 觉得，如果`null`自动转为0，很不容易发现错误。

因此，他又设计了一个`undefined`。区别是这样的：`null`是一个表示“空”的对象，转为数值时为0；`undefined`是一个表示”此处无定义”的原始值，转为数值时为`NaN`。

```
Number(undefined) // NaN
5 + undefined // NaN
```

在JavaScript里，`null`和`undefined`都表示不存在的数据，并且`undefined`也是从`null`中继承而来；主要区别如下：

- --
- 1).`null`和`undefined`都是表示没有的、不存在的值。他们在进行逻辑转换时都是false，这两个值进行比较是true；
- --
- 2).`null`表示空引用，它是`object`类型；`undefined`表示未定义，是`undefined`类型；
- --
- 3).如果一个变量的值是`null`，那么必须主动给它赋值`null`；如果这个对象以后要用，但是现在还没有值，一般情况下，会给它一个`null`值；
- --
- 4).一个变量未声明（对未声明的变量，只能进行一个操作，即使用`typeof`操作符检测其数据类型），报错：`xx is not defined`；一个变量定义了未赋值，则值是`undefined`。
- --
- 5).对属性来说，如果原来没有这个属性，根本就不存在这个属性，那么他的值就是`undefined`。（对象的属性不需要定义，如果不存在也可以直接读取，不会报错，而会给出一个`undefined`的值出来）；
- --
- 6).在函数（方法）里，如果必须返回值，但是值又计算不出来，那就返回一个`null`（这是规范，不是语法规定，js遵循）；但是没有返回值的函数，它的返回值都是`undefined`。
- --

#### 7.Boolean类型

该类型只有两个字面值：true和false(区分大小写)，这两个值与数字值不是一回事，因此true不一定等于1，而false不一定等于0； 但ECMAScript中所有数据类型都有与这两个值等价的值。

下列运算符会返回布尔值：

- --
- 两元逻辑运算符： `&& (And)，|| (Or)`
- 前置逻辑运算符： `! (Not)`
- 相等运算符：`===，!==，==，!=`
- 比较运算符：`>，>=，<，<=`
- --

##### 7.1 `Boolean()`

强制将其他类型数据转换为布尔型，对`0、NaN、null、undefined、""`(空字符串不是空格字符串)这五个为false，其余为true；

> 在流控制语句中，自动执行相应的`Boolean`转换。

##### 7.2 `！` 

取反，先将值转为布尔类型，然后取反；

##### 7.3 `！！` 

再次取反；将其他数据类型转换为布尔数据类型，相当于`Boolean（）`；

#### 8.Number类型

##### 8.1 `isNaN()`

检测一个值不是有效数字；若是有效数字返回false，不是有效数字返回true;若传入非数值会自动转换为数值。对于空数组和只有一个数组成员的数组，会返回false。因此在使用`isNaN`之前，最好先判断数据类型：

```
function myIsNaN(value) {
   return typeof value === 'number' && isNaN(value);
   //推荐用法
   return value !== value;
}
```

`isNaN()`也适用于对象。在基于对象调用`isNaN()`函数时，会首先调用对象的`valueOf()`方法，然后确定该方法返回的值是否可以装换为数值。如果不能，则基于这个返回值再调用`toString()`方法，再测试返回值，而这个过程也是ECMAScript中内置函数和操作符的一般执行流程。

##### 8.2 `isFinite()`

`isFinite`方法返回一个布尔值，表示某个值是否为正常的数值。

除了`Infinity`、`-Infinity`、`NaN`和`undefined`这几个值会返回false，`isFinite`对于其他的数值都会返回true。

##### 8.3 `Number()`

强制将其他数据类型转换为`number`类型；如果是字符串，只有全部是数字才能转换；`Number()`函数转换规则：

1).`Boolean`值，转换0或1；

2).数字值，简单传入传出（第一个数为`0`，则会被认为是八进制数，忽略剩余前导`0`，再转化为十进制输出；若前两位为`0x`，则会被认为是十六进制，同八进制处理）；

```
Number(0303)//195
Number(00303)//195
Number(0x0303)//771
```

3).空数组也会转换为0；纯数字数组转换对应的数值；非纯数值数组为`NaN`；

4).`null`，输出0; `undefined`，输出`NaN`;

5).如果是对象。则调用对象的`valueOf()`方法，然后依照前面规则转换返回值。如果转换的结果是`NaN`，则调用对象的`toString()`方法。

6).如果是字符串：

- --
- a.只包含数字（包括正负两种情况），转换十进制（忽略前导0）；
- b.带浮点格式，转换对应浮点数值（忽略前导0）；
- c.若有上述之外其他字符，输出`NaN`；
- d.注意：空字符串或者内有任意空格，转换为0；
- --

##### 8.4 `parseInt()`

非强制数据类型转换

1).主要用于将字符串转为整数；

2).忽略字符串前面的空格，直至找到第一个非空格字符；

3).如果第一个字符不是数字字符或正负号(符号后面必须有数字)，则返回`NaN`；

4).直至解析完所有数字字符或遇到非数字字符(不识别小数点)，返回转好的部分；    

> 第一个参数若不是字符串，会自动转换为字符串再读取(`String()`)；但这回导致一些意外：

```
parseInt(0x11, 36) // 43
parseInt(0x11, 2) // 1

// 等同于
parseInt(String(0x11), 36)
parseInt(String(0x11), 2)

// 等同于
parseInt('17', 36) 
parseInt('17', 2)
```

这种处理方式，对于八进制的前缀0，尤其需要注意。

```
parseInt(011, 2) // NaN

// 等同于
parseInt(String(011), 2)

// 等同于
parseInt(String(9), 2)
```

> JavaScript 不再允许将带有前缀0的数字视为八进制数，而是要求忽略这个0。但是，为了保证兼容性，大部分浏览器并没有部署这一条规定。

5).存在第二个参数，用来指定转换时的所使用的基数（进制）；如果第二个参数不是数值，会被自动转为一个整数。这个整数只有在2到36之间，才能得到有意义的结果，超出这个范围，则返回`NaN`。如果第二个参数是0、`undefined`和`null`，则直接忽略。

```
parseInt('10', 37) // NaN
parseInt('10', 1) // NaN
parseInt('10', 0) // 10
parseInt('10', null) // 10
parseInt('10', undefined) // 10
```

如果字符串包含对于指定进制无意义的字符，则从最高位开始，只返回可以转换的数值。如果最高位无法转换，则直接返回`NaN`。

```
parseInt('1546', 2) // 1
parseInt('546', 2) // NaN
```

> `parseInt()`的第二个参数默认为10；

6).对于会自动转化为科学计数法的数字，`parseInt()`会产生一些奇怪的结果；`parseFloat()`不会。

```
parseInt(1000000000000000000000.5) // 1
// 等同于
parseInt('1e+21') // 1

parseInt(0.0000008) // 8
// 等同于
parseInt('8e-7') // 8

parseFloat(1000000000000000000000.5); //1e+21
// 等同于
parseFloat('1e+21'); //1e+21

parseFloat(0.0000008); //8e-7
// 等同于
parseFloat('8e-7'); //8e-7
```

##### 8.5 `parseFloat()`

1).`parseFloat`方法用于将一个字符串转为浮点数(同`parseInt`，但多识别一个小数点)。

2).如果字符符合科学计数法，则会进行相应的转换；若有不能转换为浮点数的字符，就返回已转好的部分。

3).自动过滤前导空格；若参数不是字符串或者字符串的第一个字符不能转换为浮点数，则返回`NaN`。

`parseFloat(''); //NaN`


##### 8.6 浮点运算

JavaScript 内部，所有数字都是以64位浮点数形式储存，即使整数也是如此。所以，1与1.0是相同的，是同一个数。

`1 === 1.0 // true`

这就是说，JavaScript 语言的底层根本没有整数，所有数字都是小数（64位浮点数）。容易造成混淆的是，某些运算只有整数才能完成，此时 JavaScript 会自动把64位浮点数，转成32位整数，然后再进行运算。

根据国际标准 IEEE 754，JavaScript 浮点数的64个二进制位，从最左边开始，是这样组成的。

- --
- 第1位：符号位，0表示正数，1表示负数
- 第2位到第12位（共11位）：指数部分
- 第13位到第64位（共52位）：小数部分（即有效数字）
- --

符号位决定了一个数的正负，指数部分决定了数值的大小，小数部分决定了数值的精度。

由于浮点数不是精确的值，所以涉及小数的比较和运算要特别小心。

```
0.1 + 0.2 === 0.3
// false

0.1+0.2=
//0.300 000 000 000 000 04

0.3 / 0.1
//2.999 999 999 999 999 6

(0.3 - 0.2) === (0.2 - 0.1)
// false
```

对于那些极大或极小的数值，可以用`e`表示法表示浮点数值。默认情况下，ECMAScript会将那些小数点后面带有6个0以上的浮点数值转换为以`e`表示的数值。浮点数值最高精确度是17位小数，但在进行算数计算时精确度远远不如整数。因此，永远不要测试某个特定的浮点数值。
 
> 关于浮点数值计算产生舍入误差的问题，这是基于IEEE754数值的浮点数值计算的通病。

> 数值运算中，任何涉及`NaN`和`undefined`的运算，结果均为`NaN`;`null`会转换为0参与运算。

##### 8.7 数值范围

根据标准，64位浮点数的指数部分的长度是11个二进制位，意味着指数部分的最大值是2047（2的11次方减1）。也就是说，64位浮点数的指数部分的值最大为2047，分出一半表示负数，则 JavaScript 能够表示的数值范围为21024到2-1023（开区间），超出这个范围的数无法表示。

如果一个数大于等于2的1024次方，那么就会发生“**正向溢出（overflow）**”，即 JavaScript 无法表示这么大的数，这时就会返回`Infinity`。

```
FireFox
Math.pow(2, 1023);//8.98846567431158e+307
Math.pow(2, 1024);//Infinity
```

如果一个数小于等于2的-1075次方（指数部分最小值-1023，再加上小数部分的52位），那么就会发生为“**负向溢出（underflow）**”，即 JavaScript 无法表示这么小的数，这时会直接返回0。

```
FireFox
Math.pow(2, -1074);//5e-324  //chorme  5e-324
Math.pow(2, -1075);//5e-324  //chorme  0
Math.pow(2, -1076);//0
```

JavaScript 提供`Number`对象的`MAX_VALUE`和`MIN_VALUE`属性，返回可以表示的具体的最大值和最小值。

```
Number.MAX_VALUE // 1.7976931348623157e+308
Number.MIN_VALUE // 5e-324
```

> 如果某次计算返回了`Infinity`值，那么该值将无法继续参与下一次的计算，因为`Infinity`值不能参与数值计算。

> 要想确定一个值是不是有穷的，可以使用`isFinite()`函数，如果该值位于最小值和最大值之间，返回true。

##### 8.8 数值的表示法

JavaScript 的数值有多种表示方法，可以用字面形式直接表示，比如35（十进制）和`0xFF`（十六进制）。

数值也可以采用科学计数法表示。科学计数法允许字母e或E的后面，跟着一个整数，表示这个数值的指数部分。

以下两种情况，JavaScript 会自动将数值转为科学计数法表示，其他情况都采用字面形式直接表示。

（1）小数点前的数字多于21位。

```
1234567890123456789012
// 1.2345678901234568e+21

123456789012345678901
// 123456789012345680000
```

（2）小数点后的零多于5个。

```
// 小数点后紧跟5个以上的零，
// 就自动转为科学计数法
0.0000003 // 3e-7

// 否则，就保持原来的字面形式
0.000003 // 0.000003
```

##### 8.9 数值的进制

使用**字面量（literal）**直接表示一个数值时，JavaScript 对整数提供四种进制的表示方法：

- --
- 十进制：没有前导0的数值。
- 八进制：有前缀`0o`或`0O`的数值，或者有前导0、且只用到0-7的八个阿拉伯数字的数值。
- 十六进制：有前缀`0x`或`0X`的数值。
- 二进制：有前缀`0b`或`0B`的数值。
- --

默认情况下，JavaScript 内部会自动将八进制、十六进制、二进制转为十进制。

如果八进制、十六进制、二进制的数值里面，出现不属于该进制的数字，就会报错。

通常来说，有前导0的数值会被视为八进制，但是如果前导0后面有数字8和9，则该数值被视为十进制。

前导0表示八进制，处理时很容易造成混乱。ES5 的严格模式和 ES6，已经废除了这种表示法，但是浏览器为了兼容以前的代码，目前还继续支持这种表示法。

> 八进制字面量在严格模式下是无效的，会导致支持该模式的JavaScript引擎抛出错误。

##### 8.10 特殊数值

JavaScript 提供了几个特殊的数值。

1).正零和负零

前面说过，JavaScript 的64位浮点数之中，有一个二进制位是符号位。这意味着，任何一个数都有一个对应的负值，就连0也不例外。

JavaScript 内部实际上存在2个0：一个是`+0`，一个是`-0`，区别就是64位浮点数表示法的符号位不同。它们是等价的。

几乎所有场合，正零和负零都会被当作正常的0。唯一有区别的场合是，`+0`或`-0`当作分母，返回的值是不相等的。

`(1 / +0) === (1 / -0) // false`

上面的代码之所以出现这样结果，是因为除以正零得到`+Infinity`，除以负零得到`-Infinity`，这两者是不相等的。

2).`NaN`

`NaN`是 JavaScript 的特殊值，表示**非数字（Not a Number）**，主要出现在将字符串解析成数字出错的场合。

`5 - 'x' // NaN`

上面代码运行时，会自动将字符串x转为数值，但是由于x不是数值，所以最后得到结果为`NaN`，表示它是“非数字”（`NaN`）。

需要注意的是，`NaN`不是独立的数据类型，而是一个特殊数值，它的数据类型依然属于`Number`。

`NaN`不等于任何值，包括它本身。`NaN`与任何数（包括它自己）的运算，得到的都是`NaN`。

> 数组的`indexOf`方法内部使用的是严格相等运算符，所以该方法对`NaN`不成立。

3).`Infinity`

`Infinity`表示“无穷”，用来表示两种场景。一种是一个正的数值太大，或一个负的数值太小，无法表示；另一种是非0数值除以0，得到`Infinity`。

```
Math.pow(2, 1024)
// Infinity

0 / 0   // NaN
1 / 0   // Infinity
```

`Infinity`有正负之分，`Infinity`表示正的无穷，`-Infinity`表示负的无穷。

`Infinity === -Infinity // false`

由于数值正向溢出、负向溢出和被0除，JavaScript 都不报错，而是返回`Infinity`，所以单纯的数学运算几乎没有可能抛出错误。

`Infinity`大于一切数值（除了`NaN`），`-Infinity`小于一切数值（除了`NaN`）。

`Infinity`的四则运算，(部分)符合无穷的数学计算规则。

```
//一些特例

0 * Infinity   // NaN
0 / Infinity   // 0
Infinity / 0   // Infinity

Infinity - Infinity   // NaN
Infinity / Infinity   // NaN

null * Infinity    // NaN
null / Infinity    // 0
Infinity / null    // Infinity
```