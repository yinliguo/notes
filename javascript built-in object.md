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
- String.prototype
- String.fromCharCode([char0[, char1[, char2...]])
```
从unicode值返回字符串

String.fromCharCode(65, 66, 67); // 'ABC'
```

##### Properties of the String Prototype Object
- String.prototype.constructor
- String.prototype.toString()
- String.prototype.valueOf()
- String.prototype.charAt(pos)
```
查询在pos位置上的字符

var str = 'abc';
str.charAt(1); // 'b'
```
- String.prototype.concat([str1[, str2[, str3...]]])
```
返回连接的字符串，不改变原字符串

var str1 = 'abc';
var str2 = 'def';
str1.concat(str2); // 'abcdef'
```
- String.prototype.indexOf(searchString, position)
```
返回查询字符串所在的第一个位置

var str = 'abcade';
str.indexOf('a'); // 0
```
- String.prototype.lastIndexOf(searchString, position)
```
返回查询字符串所在的最后一个位置

var str = 'abcade';
str.lastIndexOf('a'); // 3
```
- String.prototype.localeCompare(that)
```
以本地的排序顺序比较两个字符串，如果小于that则返回小于0的数，否则返回大于0的数

var str1 = 'abc';
var str2 = 'bcd';
str1.localeCompare(str2); // -1
```
- String.prototype.match(regexp)
```
正则表达式匹配字符串，返回数组

var str = 'abcabcabc';
str.match(/b/); // ['b']
```
- String.prototype.replace(searchValue, replaceValue)
```
替换字符串，返回替换后的字符串，原字符串不变

var str = 'abcdedf';
str.replace('d', 'e'); // 'abceedf'，如果想都替换应该用正则表达式
```
- String.prototype.search(regexp)
```
查找在字符串中的位置，只返回第一个索引，否则返回-1。

var str = 'abcdedf';
str.search('d'); // 3
```
- String.prototype.slice(start, end)
```
返回start到end-1的字符串，不改变原字符串

var str = 'abcdefg';
str.slice(1, 2); // 'b'
```
- String.prototype.split(separator, limit)
```
将字符串分割成数组，不改变原字符串

var str = '1912,1,2';
str.split('i,', 2); // ['1912', '1']
```
- String.prototype.substring(start, end)
```
获取star到end-1的子字符串，不改变原字符串

var str = 'abcdef';
str.substring(1, 2); // 'b'
```
- String.prototype.toLowerCase()
```
返回字符串的小写，不改变原字符串

var str = 'aAbB';
str.toLowerCase(); // 'aabb'
```
- String.prototype.toLocaleLowerCase()
```
按照本地语言的映射关系返回小写
```
- String.prototype.toUpperCase()
```
返回字符串的大写，不改变原字符串

var str = 'aAbB';
str.toUpperCase(); // 'AABB'
```
- String.prototype.toLocaleUpperCase()
```
按本地语言的映射关系返回大写
```
- String.prototype.trim()
```
去掉字符串两端的空格，不改变原字符串

var str = ' abc ';
str.trim(); // 'abc'
```

### Boolean Objects
##### Properties of the Boolean Constructor
- Boolean.prototype

##### Properties of the Boolean Prototype Object
- Boolean.prototype.constructor
- Boolean.prototype.toString()
```
var a = true;
a.toString(); // true
```
- Boolean.prototype.valueOf()

### Number Objects
##### Properties of the Number Constructor
- Number.prototype
- Number.MAX_VALUE
- Number.MIN_VALUE
- Number.NaN
- Number.NEGTIVE_INFINITY
- Number.POSITIVE_INFINITY

##### Properties of the Number Prototype Object
- Number.prototype.constructor
- Number.prototype.toString([radix])
```
按照进制输出字符串，默认十进制

var n = 123;
n.toString(); // '123'
n.toString(16); // '7b'
```
- Number.prototype.toLocaleString()
- Number.prototype.valueOf()
- Number.prototype.toFixed(fractionDigits)
```
限制小数位数，不改变原数

var n = 1.1234;
n.toFixed(2); // 1.12
```
- Number.prototype.toExponential(fractionDigits)
```
转换成指数形式，不改变原数

var n = 123456;
n.toExponential(2); // 1.23e+5
```
- Number.prototype.toPrecision(precision)
```
超过precision位后转换成指数形式，不改变原数

var n = 123456;
n.toPrecision(10); // 123456
n.toPrecision(2); // 1.2e+5
```

### Date Objects
##### Properties of the Date Constructor
- Date.prototype
- Date.parse(string)
```
将日期字符串转换成毫秒数

Date.parse('Jul 8, 2005'); // 1120752000000
Date.parse('2005, 7, 8'); // 1120752000000
Date.parse('2005-7-8'); // 1120752000000
```
- Date.UTC(year, month[, day[, hours[, minutes[, seconds[, ms]]]]])
```
返回日期的毫秒数，注意有时差的，跟parse返回的不一样

Data.UTC(2005, 6, 6); // 1120780800000
```
- Date.now()
```
返回现在时间的毫秒数
```

##### Properties of the Date Prototype Object
- Date.prototype.constructor
- Date.prototype.toString()
```
var t = new Date();
t.toString(); // 'Fri Mar 18 2016 17:42:33 GMT+0800 (CST)'
```
- Date.prototype.toDateString()
```
返回日期的字符串

var t = new Date();
t.toDateSting(); // 'Fri Mar 18 2016'
```
- Date.prototype.toTimeString()
```
返回时间的字符串

var t = new Date();
t.toTimeString(); // '17:42:33 GMT+0800 (CST)'
```
- Date.prototype.toLocaleString()
- Date.prototype.toLocaleDateString()
```
返回本地的日期格式

var t = new Date();
t.toLocaleDateString(); // '2016/3/18'
```
- Date.prototype.toLocaleTimeString()
```
返回本地的时间格式

var t = new Date();
t.toLocaleTimeString(); // '下午5:42:33'
```
- Date.prototype.valueOf()
```
返回毫秒数

var t = new Date();
t.valueOf(); // 1458294153410
```
- Date.prototype.getTime()
```
返回毫秒数

var t = new Date();
t.getTime(); // 1458294153410
```
- Date.prototype.getFullYear()
```
获取年份

var t = new Date();
t.getFullYear(); // 2016
```
- Date.prototype.getUTCFullYear()
```
返回UTC时间的年份
```
- Date.prototype.getMonth()
```
获取月份，注意与实际月份差1

var t = new Date();
t.getMonth(); // 2
```
- Date.prototype.getUTCMonth()
```
返回UTC时间的月份
```
- Date.prototype.getDate()
```
返回天

var t = new Date();
t.getDate(); // 18
```
- Date.prototype.getUTCDate()
```
返回UTC时间的天
```
- Date.prototype.getDay()
```
返回星期
```
- Date.prototype.getUTCDay()
```
返回UTC时间的星期
```
- Date.prototype.getHours()
```
返回小时
```
- Date.prototype.getUTCHours()
```
返回UTC时间的小时
```
- Date.prototype.getMinutes()
```
返回分钟
```
- Date.prototype.getUTCMinutes()
```
返回UTC时间的分钟
```
- Date.prototype.getSeconds()
```
返回秒
```
- Date.prototype.getUTCSeconds()
```
返回UTC时间的秒
```
- Date.prototype.getMilliseconds()
```
返回毫秒数
```
- Date.prototype.getUTCMillisecons()
```
返回UTC时间的毫秒数
```
- Date.prototype.getTimezoneOffset()
```
返回时差的分钟数
```
- Date.prototype.setTime(time)
```
设置时间，time为毫秒数
```
- Date.prototype.setMilliseconds(ms)
```
设置毫秒数
```
- Date.prototype.setUTCMilliseconds(ms)
```
设置UTC毫秒数
```
- Date.prototype.setSeconds(sec[, ms])
```
设置秒数[毫秒数]
```
- Date.prototype.setUTCSeconds(sec[, ms])
```
设置UTC秒数[毫秒数]
```
- Date.prototype.setMinutes(min[, sec[, ms]])
```
设置分钟
```
- Date.prototype.setUTCMinutes(min[, sec[, ms]])
```
设置UTC分钟
```
- Date.prototype.setHours(hour[, min[, sec[, ms]]])
```
设置小时
```
- Date.prototype.setUTCHours(hour[, min[, sec[, ms]]])
```
设置UTC小时
```
- Date.prototype.setDate(date)
```
设置天
```
- Date.prototype.setUTCDate(date)
```
设置UTC天
```
- Date.prototype.setMonth(month[, date])
```
设置月
```
- Date.prototype.setUTCMonth(month[, date])
```
设置UTC月
```
- Date.prototype.setFullYear(year[, month[, date]])
```
设置年
```
- Date.prototype.setUTCFullYear(year[, month[, date]])
```
设置UTC年
```
- Date.prototype.toUTCString()
```
返回UTC时间的字符串

var t = new Date();
t.toUTCString(); // 'Fri, 18 Mar 2016 09:42:33 GMT'
```
- Date.prototype.toISOString()
```
返回国际标准时间

var t = new Date();
t.toISOString(); // '2016-03-18T09:42:33.410Z'
```
- Date.prototype.toJSON(key)
```
返回JSON格式的时间字符串。（不懂为什么）

var t = new Date();
t.toJSON(); // '2016-03-18T09:42:33.410Z'
```

### RegExp Objects
##### Properties of the RegExp Constructor
- RegExp.prototype

##### Properties of the RegExp Prototype Object
- RegExp.prototype.constructor
- RegExp.prototype.exec(string)
- RegExp.prototype.test(string)
- RegExp.prototype.toString()

### Error Objects
##### Properties of the Error Constructor
- Error.prototype

##### Properties of the Error Prototype Object
- Error.prototype.constructor
- Error.prototype.name
- Error.prototype.message
- Error.prototype.toString()

### The Math Object
##### Value Properties of the Math Object
- E
- LN10
- LN2
- LOG2E
- LOG10E
- PI
- SQRT1_2
- SQRT2

##### Function Properties of the Math Object
- abs(x)
- acos(x)
- asin(x)
- atan(x)
- atan2(y,x)
- ceil(x)
```
Math.ceil(1.1); // 2
```
- cos(x)
- exp(x)
- floor(x)
```
Math.floor(1.8); // 1
```
- log(x)
- max([value1[, value2[, value3...]]])
- min([value1[, value2[, value3...]]])
- pow(x, y)
- random()
```
0~1之间的随机数，要注意是没有参数的
```
- round(x)
```
Math.round(1.1); // 1
Math.round(1.8); // 2
```
- sin(x)
- sqrt(x)
```
开方

Math.sqrt(4); // 2
```
- tan(x)

### The JSON Object
- parse(text[, reviver])
```
将JSON格式的字符串转成对象，注意属性是用双引号括起来的

var a = '{"a": 1}';
JSON.parse(a); // {a: 1}
```
- stingify(value[, replacer[, space]])
```
把JSON格式的对象转化成字符串

var a = {a: 1};
JSON.stringify(a); // '{"a": 1}'
```