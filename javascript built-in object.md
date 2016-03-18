# JavaScript内置对象

### The Global Object
##### Value Properties
- NaN
- Infinity
- undefined

##### Function
- eval(x)
```
将参数按照js执行

eval('alert(1)');
```
- parseInt(string, radix)
```
将字符串转成整数。radix表示进制，2~36之间

parseInt('123'); // 123
parseInt('1.7'); // 1
parseInt('1f', 16); // 31
parseInt('12@12'); // 12
```
- parseFloat(string)
```
将字符串转成浮点数

parseFloat('12.01'); // 12.01
```
- isNaN(number)
```
是否为非数字

isNaN(12); // false
isNaN('12'); // false，进行隐式类型转换
isNaN('hello'); // true
isNaN(true); // false，进行隐式类型转换
isNaN(null); // false，进行隐式类型转换
isNaN(undefined); // true
var f = function(){};
isNaN(f); // true
var o = {};
isNaN(o); // true
```
- isFinite(number)
```
是否非无穷数

isFinite(123); // true 
isFinite(2/0); // false
isFinite(Math.PI); // true
isFinite(Number.MAX_VALUE); // true
isFinite(Number.POSITIVE_INFINITY); // false
```
##### URI Handling Function
- decodeURI(encodeURI)
```
解码URI

decodeURI('http://prose.io/#yinliguo/notes/edit/master/javascript%20built-in%20object.md')
// 输出http://prose.io/#yinliguo/notes/edit/master/javascript built-in object.md
```
- decodeURIComponent(encodeURIComponent)
```
解码URI

decodeURIComponent('http%3A%2F%2Fprose.io%2F%23yinliguo%2Fnotes%2Fedit%2Fmaster%2Fjavascript%20built-in%20object.md');
// 输出http://prose.io/#yinliguo/notes/edit/master/javascript built-in object.md
```
- encodeURI(uri)
```
将URI进行编码

encodeURI('http://prose.io/#yinliguo/notes/edit/master/javascript built-in object.md'); 
// 输出http://prose.io/#yinliguo/notes/edit/master/javascript%20built-in%20object.md
```
- encodeURIComponent(uriComponent)
```
对URI进行编码

encodeURIComponent('http://prose.io/#yinliguo/notes/edit/master/javascript%20built-in%20object.md')
// 输出http%3A%2F%2Fprose.io%2F%23yinliguo%2Fnotes%2Fedit%2Fmaster%2Fjavascript%20built-in%20object.md
```
##### Constructor
- Object()
- Function()
- Array()
- String()
- Number()
- Date()
- RegExp()
- Error()
- EvalError()
- RangeError()
- ReferenceError()
- SyntaxError()
- TypeError()
- URIError()
##### Other Properties
- Math
- JSON

### Object Objects
##### Properties of the Object Constructor
- Object.prototype
- Object.getPrototypeOf(O)
```
获取原型对象

var a = [];
Object.getPrototypeOf(a); // 输出[]
```
- Object.getOwnPropertyDescriptor(O, P)
```
获取自身属性描述符，而不是原型的属性

var o = {a: 123};
Object.getOwnPropertyDescriptor(o, 'a'); // 输出{value: 123, writable: true, enumberable: true, configurable: true}
```
- Object.getOwnPropertyNames(O)
```
获取自身的属性名称

var o = {a: 1, b: 2};
Object.getOwnPropertyNames(o); // ['a', 'b']
```
- Object.create(O[, Properties])
```
创建对象。O是原型对象，Properties是属性描述符

var p = {a: 1};
var d = {b: {value: 1}};
var o = Object.create(p, d); // o为{b: 1}，它的原型是p
```
- Object.defineProperty(O, P, Attributes)
```
给对象添加属性。O为需要添加属性的对象，P为属性名称，Attributes为属性描述符

var o = {};
Object.defineProperty(o, 'a', {value: 1}); // o为{a: 1}
```
- Object.defineProperties(O, Properties)
```
给对象添加属性

var o = {};
Object.defineProperties(o, {a: {value: 1}, b: {value: 2}}); // o为{a: 1, b: 2}
```
- Object.seal(O)
```
密封对象，阻止对象添加和删除属性

var o = {a: 1};
Object.seal(o);
o.a = 2; // o为{a: 2}
o.c = 2; // 不生效
delete(c); // 不生效
```
- Object.freeze(O)
```
冻结对象，在seal的基础上不能修改属性

var o = {a: 1};
Object.freeze(o);
o.a = 2; 不生效
```
- Object.preventExtensions(O)
```
阻止对象添加新属性

var o = {};
Object.preventExtensions(o);
o.a = 1; // 不生效
```
- Object.isSealed(O)
```
对象是否被密封

var o = {a: 1};
Object.seal(c);
Object.isSealed(c); // true
```
- Object.isFrozen(O)
```
对象是否被冻结

var o = {a: 1};
Object.freeze(o);
Object.isFrozen(o); // true
```
- Object.isExtensible(O)
```
对象是否可扩展属性

var o = {};
Object.preventExtensions(o);
Object.isExtensible(o); // false
```
- Object.keys(O)
```
获取对象的所有属性值

var o = {a: 1, b: 2};
Object.keys(o); // ['a', 'b']
```
##### Properties of the Object Prototype Object
- Object.prototype.constructor
- Object.prototype.toString()
```
转换成字符串

var d = 123;
Object.prototype.toString.call(d); // '[Object Number]'
```
- Object.prototype.toLocaleString()
```
转换成字符串

var d = 123;
Object.prototype.toLocaleString.call(d); // '123'
```
- Object.prototype.valueOf()
```
显示值

var t = new Date();
Object.prototype.valueOf.call(t); // Fri Mar 18 2016 14:09:25 GMT+0800 (CST)
```
- Object.prototype.hasOwnProperty(V)
```
对象是否存在属性V

var o = {a: 1};
Object.prototype.hasOwnProperty.call(o, 'a'); // ture
Object.prototype.hasOwnProperty.call(o, 'b'); // false
```
- Object.prototype.isPrototypeOf(V)
```
判断p是否是o的原型

var p = {};
var o = {a: 1};
Object.prototype.isPrototypeOf.call(p, o); // true
```
- Object.prototype.propertyIsEnumerable(V)
```
属性是否可枚举

var o = {a: 1};
Object.prototype.propertyIsEnumeralbe(o, 'a'); // true
```

### Function Objects
##### Properties of the Function Constructor
- Function.prototype
- Function.length
##### Properties of the Function Prototype Object
- Function.prototype.constructor
- Function.prototype.toString()
```
转换成字符串

var f = function(){};
Function.prototype.toString.call(f); // 'function(){}'，显然重写了toString方法
Object.prototype.toString.call(f); // '[Object Function]'
```
- Function.prototype.apply(thisArg, argArray)
```
临时改变this值，参数为数组
```
- Function.prototype.call(thisArg[, arg1[, arg2...]])
```
临时改变this值，参数为普通参数
```
- Function.prototype.bind(thisArg[, arg1[, arg2...]])
```
永久改变this值，参数为普通参数
```

### Array Objects
##### Properties of the Array Constructor
- Array.prototype
- Array.isArray(arg)
##### Properties of the Array Prototype Object
- Array.prototype.constructor
- Array.prototype.toString()
```
转换成字符串

var a = [1, 2, 3];
a.toString(); // '1,2,3'，重写了toString方法
Object.prototype.toString.call(a); // '[Object Array]'
```
- Array.prototype.toLocalString()
- Array.ptototype.concat([item1[, item2...]])
```
连接数组

var a = [1, 2, 3], b = [4, 5];
a.concat(b); // [1, 2, 3, 4, 5]，a和b都不改变
```
- Array.prototype.join(separator)
```
将数组转换成字符串，以separator分割

var a = [1, 2, 3];
a.join('@'); // '1@2@3'，不改变a
```
- Array.prototype.pop()
```
弹出最后一个数组元素

var a = [1, 2, 3];
a.pop(); // 3，a为[1, 3]
```
- Array.prototype.push([item1[, item2...]])
```
将元素加入数组

var a = [1, 2, 3];
a.push(4); // 4，a为[1, 2, 3, 4]
```
- Array.prototype.reverse()
```
反转数组

var a = [1, 2, 3];
a.reverse(); // [3, 2, 1]，a为[3, 2, 1]
```
- Array.prototype.shift()
```
弹出第一个数组元素

var a = [1, 2, 3];
a.shift(); // 1，a为[2, 3]
```
- Array.prototype.slice(start, end)
```
获取index为start到end - 1的元素

var a = [0, 1, 2, 3, 4, 5, 6];
a.slice(1, 3); // [1, 2]，不改变a
```
- Array.prototype.sort(comparefn)
```
comparefn返回true则进行交换位置

var a = [4, 5, 2, 6, 3];
a.sort(function(prev, next) {return prev - next;}); // a变为[2, 3, 4, 5, 6]
```
- Array.prototype.splice(start, deleteCount[item1[, item2...]])
```
删除/替换元素，返回被删除的元素

var a = [1, 2, 3, 4, 5];
a.splice(2, 2); // 返回[3, 4]，a变为[1, 2, 5]
var a = [1, 2, 3, 4, 5];
a.splice(2, 2, 1, 2); // 返回[3, 4]，a变为[1, 2, 1, 2, 5]
```
- Array.prototype.unshift([item1[, item2...]])
```
向数组的开头添加元素，返回数组长度

var a = [3, 4];
a.unshift(1, 2); // 4，a变为[1, 2, 3, 4]
```
- Array.prototype.indexOf(searchElement[, fromIndex])
```
查询元素所在的第一个索引

var a = [1, 2, 3, 4, 5];
a.indexOf(3); // 4
```
- Array.prototype.lastIndex(searchElemen[, fromIndex])
```
查询元素最后一个索引

var a = [2, 3, 4, 5, 2];
a.lastIndexOf(2); // 4
```
- Array.prototype.every(callbackfn[, thisArg])
```
如果数组的每一项是否满足条件（即callbackfn返回true），则every返回true，否则返回false

var a = [1, 2, 3];
a.every(function(i) {return i > 0;}); // true
```
- Array.prototype.some(callbackfn[, thisArg])
```
如果数组中有元素满足条件（即callbackfn返回true），则some返回ture，否则返回false

var a = [1, 2, 3];
a.some(function(i) {return i > 2;}); // true
```
- Array.prototype.forEach(callbackfn[, thisArg])
```
遍历数组应用callbackfn，callbackfn的参数为element, index, array

var a = [1, 2, 3];
a.forEach(function(ele, i, arr){arr[i] = ele - 1;}); // a变为[0, 1, 2]
```
- Array.prototype.map(callbackfn[, thisArg])
```
遍历数组并应用callbackfn，参数为element, index, array

var a = [1, 2, 3];
a.forEach(function(ele, i, arr){arr[i] = ele - 1;}); // a变为[0, 1, 2]
```
- Array.prototype.filter(callbackfn[, thisArg])
```
应用callbackfn（返回true为满足条件）取出满足条件的元素并组成数组，callbackfn参数为element, index, array

var a = [1, 2, 3];
a.filter(function(e) {return e > 2;}); // [3]
```
- Array.prototype.reduce(callbackfn[, initialValue])
```
遍历数组元素并进行处理，最后返回处理结果，callbackfn的参数为result（上一个元素处理返回的结果）, element, index, array

var a = [1, 2, 3];
a.reduce(function(r, e, i, a){return r+e;}, 1); // 7
```
- Array.prototype.reduceRight(callbackfn[, initialValue])
```
从右向左reduce
```

### String Objects
##### Properties of the String Constructor
```
String.prototype
String.fromCharCode([char0[, char1[, char2...]])
```
##### Properties of the String Prototype Object
```
String.prototype.constructor
String.prototype.toString()
String.prototype.valueOf()
String.prototype.charAt(pos)
String.prototype.concat([str1[, str2[, str3...]]])
String.prototype.indexOf(searchString, position)
String.prototype.lastIndexOf(searchString, position)
String.prototype.localeCompare(that)
String.prototype.match(regexp)
String.prototype.replace(searchValue, replaceValue)
String.prototype.search(regexp)
String.prototype.slice(start, end)
String.prototype.split(separator, limit)
String.prototype.substring(start, end)
String.prototype.toLowerCase()
String.prototype.toLocaleLowerCase()
String.prototype.toUpperCase()
String.prototype.toLocaleUpperCase()
String.prototype.trim()
```

### Boolean Objects
##### Properties of the Boolean Constructor
```
Boolean.prototype
```
##### Properties of the Boolean Prototype Object
```
Boolean.prototype.constructor
Boolean.prototype.toString()
Boolean.prototype.valueOf()
```

### Number Objects
##### Properties of the Number Constructor
```
Number.prototype
Number.MAX_VALUE
Number.MIN_VALUE
Number.NaN
Number.NEGTIVE_INFINITY
Number.POSITIVE_INFINITY
```
##### Properties of the Number Prototype Object
```
Number.prototype.constructor
Number.prototype.toString([radix])
Number.prototype.toLocaleString()
Number.prototype.valueOf()
Number.prototype.toFixed(fractionDigits)
Number.prototype.toExponential(fractionDigits)
Number.prototype.toPrecision(precision)
```

### Date Objects
##### Properties of the Date Constructor
```
Date.prototype
Date.parse(string)
Date.UTC(year, month[, day[, hours[, minutes[, seconds[, ms]]]]])
Date.now()
```
##### Properties of the Date Prototype Object
```
Date.prototype.constructor
Date.prototype.toString()
Date.prototype.toDateString()
Date.prototype.toTimeString()
Date.prototype.toLocaleString()
Date.prototype.toLocaleDateString()
Date.prototype.toLocaleTimeString()
Date.prototype.valueOf()
Date.prototype.getTime()
Date.prototype.getFullYear()
Date.prototype.getUTCFullYear()
Date.prototype.getMonth()
Date.prototype.getUTCMonth()
Date.prototype.getDate()
Date.prototype.getUTCDate()
Date.prototype.getDay()
Date.prototype.getUTCDay()
Date.prototype.getHours()
Date.prototype.getUTCHours()
Date.prototype.getMinutes()
Date.prototype.getUTCMinutes()
Date.prototype.getSeconds()
Date.prototype.getUTCSeconds()
Date.prototype.getMilliseconds()
Date.prototype.getUTCMillisecons()
Date.prototype.getTimezoneOffset()
Date.prototype.setTime(time)
Date.prototype.setMilliseconds(ms)
Date.prototype.setUTCMilliseconds(ms)
Date.prototype.setSeconds(sec[, ms])
Date.prototype.setUTCSeconds(sec[, ms])
Date.prototype.setMinutes(min[, sec[, ms]])
Date.prototype.setUTCMinutes(min[, sec[, ms]])
Date.prototype.setHours(hour[, min[, sec[, ms]]])
Date.prototype.setUTCHours(hour[, min[, sec[, ms]]])
Date.prototype.setDate(date)
Date.prototype.setUTCDate(date)
Date.prototype.setMonth(month[, date])
Date.prototype.setUTCMonth(month[, date])
Date.prototype.setFullYear(year[, month[, date]])
Date.prototype.setUTCFullYear(year[, month[, date]])
Date.prototype.toUTCString()
Date.prototype.toISOString()
Date.prototype.toJSON(key)
```

### RegExp Objects
##### Properties of the RegExp Constructor
```
RegExp.prototype
```
##### Properties of the RegExp Prototype Object
```
RegExp.prototype.constructor
RegExp.prototype.exec(string)
RegExp.prototype.test(string)
RegExp.prototype.toString()
```

### Error Objects
##### Properties of the Error Constructor
```
Error.prototype
```
##### Properties of the Error Prototype Object
```
Error.prototype.constructor
Error.prototype.name
Error.prototype.message
Error.prototype.toString()
```

### The Math Object
##### Value Properties of the Math Object
```
E
LN10
LN2
LOG2E
LOG10E
PI
SQRT1_2
SQRT2
```
##### Function Properties of the Math Object
```
abs(x)
acos(x)
asin(x)
atan(x)
atan2(y,x)
ceil(x)
cos(x)
exp(x)
floor(x)
log(x)
max([value1[, value2[, value3...]]])
min([value1[, value2[, value3...]]])
pow(x, y)
random()
round(x)
sin(x)
sqrt(x)
tan(x)
```

### The JSON Object
```
parse(text[, reviver])
stingify(value[, replacer[, space]])
```
