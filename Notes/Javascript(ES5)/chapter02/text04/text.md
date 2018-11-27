### 4.11 Object 类型

> 只要是引用数据类型，都可以添加自定义属性；
> 元素也是对象，DOM 对象；

#### 4.11.1 ECMAScript 中的对象

ECMAScript 中的对象其实就是一组数据和功能的集合，主要思想： `Object` 类型是所有他的的实例的基础，即 `Object` 类型所具有的任何属性和方法也同样存在于更具体的对象中。

`Object` 的每个实例都具有以下属性和方法：

属性：

1).`constructor`：保存着用于创建当前对象的函数，即**构造函数（constructor）**；

方法：

2).`hasOwnProperty(propertyName)`：用于检查对象是否拥有一个指定名字的自身定义(而不是继承)的属性。其中，作为参数的属性名(_propertyName_)必须以字符串形式指定（例如：`o.hasOwnProperty("name")`）；

3).`isPrototypeOf(object)`：用于检查传入的对象是否是当前对象的原型；

4).`propertyIsEnumerable(propertyName)`：用于检查给定的属性是否存在并且能够使用 `for-in` 语句来枚举；作为参数的属性名(propertyName)必须以字符串形式指定；

5).`toLocaleString()`：返回对象的字符串表示，该字符串执行与执行环境的地区对应；

6).`toString()`：返回对象的字符串表示；`Object` 类实现的这个方法非常宽泛，不能提供很多有效的信息。`Object` 的子类通常会通过自定义的 `toString()` 方法来将他覆盖，以便提供更多有用的输出信息。

7).`valueOf()`：返回对象的原始值(如果存在)。对于 `Object` 类来说，只是简单的返回该对象本身。`Object` 的子类(如 `Number`,`Boolean`)则重载了这个方法，以便返回与该对象相关的原始值。

由于 `object` 是所有对象的基础，因此所有对象都具有这些基本的属性和方法。

> 从技术角度讲，ECMA-262 中对象的行为不一定适用于 JavaScript 中其他对象。浏览器环境中的对象，比如 BOM 和 DOM 中的对象，都属于宿主对象，因为它们是由宿主实现提供和定义的。ECMA-262 不负责定义宿主对象，因此宿主对象可能也会也可能不会继承 `object`。

#### 4.11.2 `toString()`

在 JavaScript 程序中一般不会经常显式的调用 `toString()` 方法。当对象在一个字符串上下文中使用时，系统会调用相应的 `toString()` 方法来将该对象转换为字符串。

调用 `toString()` 方法时没有参数，返回值是一个字符串。为了便于使用，返回的字符串应当以某种形式与调用这个方法的对象的值有关。

在 JavaScript 中定义自定义类时，为这个类定义一个 `toString()` 方法是一个不错的实践。如果没有定义，则对象会从 `Object` 类继承默认的 `toString()` 方法。默认方法返回的字符串：`[object class]`，其中 `class` 是该对象的类。

有时候可以用默认的 `toString()` 方法的这个行为来判断未知对象的类型或类。不过，由于大多数对象都有自定义的 `toString()` 方法，因此一般需要在对象上显式的调用 `Object.toString()` 方法：如 `Object.prototype.toString.apply()`；注意，这个判断未知对象的技术只适用于内置对象。

自定义对象类有一个 “`Object`” 类，可以使用 `Object.constructor` 属性来获取关于这个对象更多的信息。

有时候需要手动调用 `toString()` 方法，比如某些时候，需要将某个对象显式的转换为字符串，但系统没有自动调用：

```javascript
var n=5;
console.log(n.toString());//"5"
console.log(5.toString());
//Uncaught SyntaxError: Invalid or unexpected token

var boo=true;
console.log(boo.toString());//"true"

var ary=[1,2,3];
console.log(ary.toString());//"1,2,3"

var obj={"a":"12"};
console.log(obj.toString());//[object Object]

var fn=function(){};
console.log(fn.toString());//function(){}
```

显式地使用 `toString()` 方法，有助于使代码更加清晰。

#### 4.11.3 `valueOf()`

对象的 `valueOf()` 方法返回与该对象关联的原始值，如果存在这样的值的话。类型为 `Object` 的对象没有原始值，这个方法只是简单的返回该对象本身。

```javascript
var obj1={};
console.log(obj1.valueOf());//{}

var obj2={"a":"12"};
console.log(obj2.valueOf());//{a: "12"}

var n=123;
console.log(n.valueOf());//123
console.log(123.valueOf());
//Uncaught SyntaxError: Invalid or unexpected token

var str="nm12";
console.log(str.valueOf());//"nm12"

var boo=true;
console.log(boo.valueOf());//true

var ary=[1,3,6];
console.log(ary.valueOf());//[1,3,6]

var fn=function(){
    console.log("k");
};
console.log(fn.valueOf());//fn=function(){}
```

不过，对类型为 `Number` 的对象而言，`valueOf()` 将返回该对象表示的原始数字值。类似的，`Boolean` 对象会返回一个关联的原始布尔值，`String` 对象则返回一个关联的字符串。

`valueOf()` 方法很少需要手动调用，在需要原始值时，JavaScript 会自动调用。事实上，由于自调用，甚至很难区分原始值和他们关联的对象。例如，虽然 `typeof` 操作符能告诉你字符串和 `String` 对象之间的不同，但实际应用的 JavaScript 代码中两者完全可以等价。

`Number`、`Boolean` 以及 `String` 对象的 `valueOf()` 方法将这些包装对象转化为他们所表示的原始值。传入数字、布尔值、字符串到 `Object()` 构造函数时则进行相反的操作：他将原始值包装到一个合适的对象中。在绝大多数情况下，JavaScript 会自动处理这种原始值到对象的转换，所有很少需要这样调用 `Object()` 构造函数。

> 对象转成原始类型的值：一般来说，对象的 `valueOf` 方法总是返回对象自身，这时再自动调用对象的 `toString` 方法，将其转为字符串。如果是一个 Date 对象的实例，那么会优先执行 `toString` 方法。

#### 4.11.4 组成和特征

由多组属性名和属性值组成；属性名和属性值是用来描述对象特征的。

对象的所有键名都是字符串（ES6 又引入了 `Symbol` 值也可以作为键值），所以加不加引号都可以。如果键名是数值，会被自动转为字符串。如果键名不符合标识名的条件（比如第一个字符为数字，或者含有空格或运算符），且也不是数字，则必须加上引号，否则会报错。

对象的每一个键名又称为**“属性”（property）**，它的“键值”可以是任何数据类型。如果一个属性的值为函数，通常把这个属性称为“**方法**”，它可以像函数那样调用。对象的属性之间用逗号分隔，最后一个属性后面可以加逗号，也可以不加。属性可以动态创建，不必在对象声明时就指定。

> 如果属性的值还是一个对象，就形成了链式引用。

1).对象是无序的（若有属性名是数字的，排序时会放在前面，从小到大排序，然后再排其他属性名）；

2).属性名必须是字符串，即使不是也会默认转换为字符串类型（为了以后的严谨，建议写为字符串形式）；

#### 4.11.5 创建方式

1).实例创建方式（`new` 运算符）

```javascript
var obj = new Object();
obj.name = "Mark";
```

2).字面量（直接量）创建方式

```javascript
var obj = {}; //与new Object()相同
var obj = { name: "12", age: 6 }; //最后一对不写逗号
```

> 在通过对象字面量方式创建对象时，实际上不会调用 `Object` 构造函数

#### 4.11.6 对象的引用

如果不同的变量名指向同一个对象，那么它们都是这个对象的引用，也就是说指向同一个内存地址。修改其中一个变量，会影响到其他所有变量。此时，如果取消某一个变量对于原对象的引用，不会影响到另一个变量。

#### 4.11.7 属性值的获取/赋值

1).`对象.属性名`

获取属性名对应的属性值（属性名为数字例外,因为会被当成小数点）；

2).`对象["属性名"]`

不加引号就会被标记为变量，然后去找该变量的值，并将该值返回给该对象再进行判断选择；如果不存在该变量，则报错：`is not defined`；属性名为数字例外(因为会自动转成字符串)；此外，方括号内部还可以使用表达式；

JavaScript 允许属性的“**后绑定**”，也就是说，你可以在任意时刻新增属性，没必要在定义对象的时候，就定义好属性。

修改原有属性名的属性值，规定一个对象中原有的属性名不能重复；如果之前有就是修改，如果没有就是增加；

> 从功能上看，这两种访问属性的方法没有任何区别。但方括号语法的主要优点是可以通过变量来访问属性。如果属性名包含会导致语法错误的字符，或者属性名使用的是关键字或保留字，也可以通过使用方括号表示法。
> 通常，除非必须使用变量来访问属性，否则我们建议使用点表示法；

```javascript
var obj={a:"xue",2:"xi"};
console.log(obj.a);//xue

console.log(obj.2);
//Uncaught SyntaxError: missing ) after argument list

console.log(obj[2]);//xi
```

#### 4.11.8 查看所有属性

查看一个对象本身的所有属性，可以使用 `Object.keys` 方法。

```javascript
var obj = {
  key1: 1,
  key2: 2
};

Object.keys(obj);
// ['key1', 'key2']
```

#### 4.11.9 删除属性

假删除 `obj.xx = null`;
真删除 `delete obj.xx`;

> `delete` 命令用于删除对象的属性，删除成功后返回 true。注意，删除一个不存在的属性，`delete` 不报错，而且返回 true。只有一种情况，`delete` 命令会返回 false，那就是该属性存在，且不得删除。另外，需要注意的是，`delete` 命令只能删除对象本身的属性，无法删除继承的属性。

```javascript
var obj = {
  zx: "1",
  cv: "2"
};
obj.zx = null;
console.log(obj); //{ zx: null, cv: "2" }

delete obj["cv"];
console.log(obj); //{ zx: null}
```

#### 4.11.10 表达式还是语句

对象采用大括号表示，这导致了一个问题：如果行首是一个大括号，它到底是表达式还是语句？

`{ foo: 123 }`

JavaScript 引擎读到上面这行代码，会发现可能有两种含义。第一种可能是，这是一个表达式，表示一个包含 foo 属性的对象；第二种可能是，这是一个语句，表示一个代码区块，里面有一个标签 foo，指向表达式 123。

为了避免这种歧义，JavaScript 规定，如果行首是大括号，一律解释为语句（即代码块）。如果要解释为表达式（即对象），必须在大括号前加上圆括号。这种差异在 `eval` 语句（作用是对字符串求值）中反映得最明显。

#### 4.11.11 `in` 运算符

`in` 运算符用于检查对象是否包含某个属性（注意，检查的是键名，不是键值），如果包含就返回 true，否则返回 false。

```javascript
var obj = { p: 1 };
"p" in obj; // true
```

`in` 运算符的一个问题是，它不能识别哪些属性是对象自身的，哪些属性是继承的。

```javascript
var obj = {};
"toString" in obj; // true
```

上面代码中，`toString` 方法不是对象 obj 自身的属性，而是继承的属性。但是，`in` 运算符不能识别，对继承的属性也返回 true。

#### 4.11.12 `for…in` 循环

`for...in` 循环用来遍历一个对象的全部属性。

```javascript
var obj = { a: 1, b: 2, c: 3 };
for (var i in obj) {
  console.log(obj[i]);
}
// 1
// 2
// 3
```

`for...in` 循环有两个使用注意点。

- 它遍历的是对象所有**可遍历（enumerable）**的属性，会跳过不可遍历的属性。
- 它不仅遍历对象自身的属性，还遍历继承的属性。

如果继承的属性是可遍历的，那么就会被 `for...in` 循环遍历到。但是，一般情况下，都是只想遍历对象自身的属性，所以使用 `for...in` 的时候，应该结合使用 `hasOwnProperty` 方法，在循环内部判断一下，某个属性是否为对象自身的属性。

```javascript
var obj = {
  hello: "world"
};
for (var key in obj) {
  if (obj.hasOwnProperty(key)) {
    console.log(key);
  }
}

//hello
```