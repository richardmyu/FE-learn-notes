## 九. JSON

### 1.JSON 格式

JSON 格式（JavaScript Object Notation 的缩写）是一种用于数据交换的文本格式，2001 年由 Douglas Crockford 提出，目的是取代繁琐笨重的 XML 格式。

相比 XML 格式，JSON 格式有两个显著的优点：书写简单，一目了然；符合 JavaScript 原生语法，可以由解释引擎直接处理，不用另外添加解析代码。所以，JSON 迅速被接受，已经成为各大网站交换数据的标准格式，并被写入标准。

JSON 对值的类型和格式有严格的规定。

- 1.复合类型的值只能是数组或对象，不能是函数、正则表达式对象、日期对象。

- 2.原始类型的值只有四种：字符串、数值（必须以十进制表示）、布尔值和 `null`（不能使用 `NaN`, `Infinity`, `-Infinity` 和 `undefined`）。

- 3.字符串必须使用双引号表示，不能使用单引号。

- 4.对象的键名必须放在双引号里面。

- 5.数组或对象最后一个成员的后面，不能加逗号。

> 注意，`null`、空数组和空对象都是合法的 JSON 值。

### 2.JSON 对象

#### 1.JSON 对象

```javascript
var dataObj = [
  { name: "a", age: 2 },
  { name: "b", age: 12 },
  { name: "c", age: 22 },
  { name: "d", age: 32 }
];
```

#### 2.JSON 字符串

把 JSON 对象使用单引号包起来

```javascript
var dataStr='[
  {"name":"a","age":2},
  {"name":"b","age":12},
  {"name":"c","age":22},
  {"name":"d","age":32}
]';
```

### 3.JSON.parse()

将 JSON 字符串变成 JSON 对象(数组形式)

```javascript
JSON.parse("{}"); // {}
JSON.parse('"foo"'); // "foo"
JSON.parse('[1, 5, "false"]'); // [1, 5, "false"]
JSON.parse("null"); // null
```

如果传入的字符串不是有效的 JSON 格式，`JSON.parse`方法将报错。

```javascript
JSON.parse("'String'"); // illegal single quotes
// SyntaxError: Unexpected token ILLEGAL

console.log(JSON.parse('{"a": undefined, "b": 2}'));
// Uncaught SyntaxError: Unexpected token u in JSON at position 6

console.log(JSON.parse('{"a": function (){}, "b": 2}'));
// Uncaught SyntaxError: Unexpected token u in JSON at position 7

console.log(JSON.parse("[undefined，1，2]"));
// Uncaught SyntaxError: Unexpected token u in JSON at position 1

console.log(JSON.parse("[function (){}，1，2]"));
// Uncaught SyntaxError: Unexpected token u in JSON at position 2

console.log(JSON.parse('"/./"'));
// Uncaught SyntaxError: Unexpected token / in JSON at position 0
```

为了处理解析错误，可以将 `JSON.parse` 方法放在 `try...catch` 代码块中。

`JSON.parse` 方法可以接受一个处理函数，作为第二个参数，用法与 `JSON.stringify` 方法类似。

```javascript
function f(key, value) {
  if (key === "a") {
    return value + 10;
  }
  return value;
}

JSON.parse('{"a": 1, "b": 2}', f);
// {a: 11, b: 2}
```

### 4.JSON.stringify()

#### 1.基本用法

将 JSON 对象(数组形式)串变成 JSON 字符

> 注意，对于原始类型的字符串，转换结果会带双引号。

```javascript
JSON.stringify("foo") === "foo"; // false
JSON.stringify("foo") === '"foo"'; // true
```

上面代码中，字符串 `foo`，被转成了 `"\"foo"\"`。这是因为将来还原的时候，内层双引号可以让 JavaScript 引擎知道，这是一个字符串，而不是其他类型的值。

```javascript
// 如果对象的属性是 `undefined`、函数或 XML 对象，该属性会被 `JSON.stringify` 过滤
console.log(JSON.stringify({ a: undefined, b: 2 }));
// {"b":2}
console.log(JSON.stringify({ a: function() {}, b: 2 }));
// {"b":2}

// 如果数组的成员是 `undefined`、函数或 XML 对象，则这些值被转成 `null`
console.log(JSON.stringify([undefined, 1, 2]));
// [null,1,2]
console.log(JSON.stringify([function() {}, 1, 2]));
// [null,1,2]

// 正则对象会被转成空对象
console.log(JSON.stringify(/\./));
// {}
```

`JSON.stringify` 方法会忽略对象的不可遍历属性。

```javascript
var obj = {};
Object.defineProperties(obj, {
  foo: {
    value: 1,
    enumerable: true
  },
  bar: {
    value: 2,
    enumerable: false
  }
});

JSON.stringify(obj); // "{"foo":1}"
```

#### 2.第二个参数（数组/函数）

`JSON.stringify`方法还可以接受一个数组，作为第二个参数，指定需要转成字符串的属性。

```javascript
var obj = {
  prop1: "value1",
  prop2: "value2",
  prop3: "value3"
};

var selectedProperties = ["prop1", "prop2"];

JSON.stringify(obj, selectedProperties);
// "{"prop1":"value1","prop2":"value2"}"
```

这个类似白名单的数组，只对对象的属性有效，对数组无效。

```javascript
JSON.stringify(["a", "b"], ["0"]);
// "["a","b"]"

JSON.stringify({ 0: "a", 1: "b" }, ["0"]);
// "{"0":"a"}"
```

第二个参数还可以是一个函数，用来更改 `JSON.stringify` 的返回值。

```javascript
function f(key, value) {
  console.log(key);
  //
  // a
  // b
  if (typeof value === "number") {
    value = 2 * value;
  }
  return value;
}

JSON.stringify({ a: 1, b: 2 }, f);
// '{"a": 2,"b": 4}'

function f(key, value) {
  console.log(value);
  // Obj {a: 1, b: 2}
  // 1
  // 2
  if (typeof value === "number") {
    value = 2 * value;
  }
  return value;
}

console.log(JSON.stringify({ a: 1, b: 2 }, f));
// { "a": 2, "b": 4 }
```

> 注意，这个处理函数是递归处理所有的键。

```javascript
var o = { a: { b: 1 } };

function f(key, value) {
  console.log("[" + key + "]:", value);
  return value;
}

console.log(JSON.stringify(o, f));
// []:obj {a: {…}}
// [a]:obj {b: 1}
// [b]:1
// {"a":{"b":1}}
```

上面代码中，对象 `o` 一共会被 `f` 函数处理三次，最后那行是 `JSON.stringify` 的输出。第一次键名为空，键值是整个对象 `o`；第二次键名为 `a`，键值是 `{b: 1}`；第三次键名为 `b`，键值为 `1`。

递归处理中，每一次处理的对象，都是前一次返回的值。

```javascript
var o = { a: 1 };

function f(key, value) {
  if (typeof value === "object") {
    return { b: 2 };
  }
  return value * 2;
}

JSON.stringify(o, f);
// "{"b": 4}"

// key：'', value: { a: 1}
// if(true)
// return {b: 2}
// key：b, value: 2
// if(false)
// return value*2
// ----> { b: 4}
```

上面代码中，`f` 函数修改了对象 `o`，接着 `JSON.stringify` 方法就递归处理修改后的对象 `o`。

如果处理函数返回 `undefined` 或没有返回值，则该属性会被忽略。

```javascript
function f(key, value) {
  if (typeof value === "string") {
    // return undefined;
  }
  return value;
}

JSON.stringify({ a: "abc", b: 123 }, f);
// '{"b": 123}'
```

上面代码中，`a` 属性经过处理后，返回 `undefined`，于是该属性被忽略了。

#### 3.第三个参数

`JSON.stringify` 还可以接受第三个参数，用于增加返回的 JSON 字符串的可读性。如果是数字，表示每个属性前面添加的空格（最多不超过 10 个）；如果是字符串（不超过 10 个字符），则该字符串会添加在每行前面。

```javascript
JSON.stringify({ p1: 1, p2: 2 }, null, 2);
/*
"{
  "p1": 1,
  "p2": 2
}"
*/

JSON.stringify({ p1: 1, p2: 2 }, null, "|-");
/*
"{
|-"p1": 1,
|-"p2": 2
}"
*/
```

> 超出部分忽略不计。

#### 4.参数对象的 toJSON 方法

如果参数对象有自定义的 `toJSON` 方法，那么 `JSON.stringify` 会使用这个方法的返回值作为参数，而忽略原对象的其他属性。

下面是一个普通的对象。

```javascript
var user = {
  firstName: "三",
  lastName: "张",

  get fullName() {
    return this.lastName + this.firstName;
  }
};

JSON.stringify(user);
// "{"firstName":"三","lastName":"张","fullName":"张三"}"
```

现在，为这个对象加上 `toJSON` 方法。

```javascript
var user = {
  firstName: "三",
  lastName: "张",

  get fullName() {
    return this.lastName + this.firstName;
  },

  toJSON: function() {
    return {
      name: this.lastName + this.firstName
    };
  }
};

// 忽略其他属性
JSON.stringify(user);
// "{"name":"张三"}"
```

`Date` 对象就有一个自己的 `toJSON` 方法。

```javascript
var date = new Date("2015-01-01");
date.toJSON(); // 2015-01-01T00:00:00.000Z
JSON.stringify(date); // "2015-01-01T00:00:00.000Z"
```

`toJSON` 方法的一个应用是，将正则对象自动转为字符串。因为 `JSON.stringify` 默认不能转换正则对象，但是设置了 `toJSON` 方法以后，就可以转换正则对象了。

```javascript
var obj = {
  reg: /foo/
};

// 不设置 toJSON 方法时
JSON.stringify(obj); // "{"reg":{}}"

// 设置 toJSON 方法时
RegExp.prototype.toJSON = RegExp.prototype.toString;
JSON.stringify(/foo/); // ""/foo/""
```

上面代码在正则对象的原型上面部署了 `toJSON` 方法，将其指向 `toString` 方法，因此遇到转换成 JSON 时，正则对象就先调用 `toJSON` 方法转为字符串，然后再被 `JSON.stringify` 方法处理。

在低版本 IE 下没有 JSON

```javascript
function toObj(str) {
  if ("JSON" in window) {
    return JSON.parse(str);
  } else {
    return eval(str);
  }
}
```

> 1.`{}`表示对象千万不要放在行首，要置于行首可以使用一个 `()` 包起来，保证语法正确 `({a:1,b:2});`

> 2.以后 `eval` 字符串中遇到转为对象的大括号时候,一定要使用小括号 `()` 包起来 `eval("("+{}+")");`