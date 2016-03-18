# JavaScript内置对象

### The Global Object
- Value Properties
> - NaN
> - Infinity
> - undefined

- Function
> - eval(x)
```
// 将参数按照js执行
eval('alert(1)');
```
> - parseInt(string, radix)
```
// 将字符串转成整数。radix表示进制，2~36之间
parseInt('123'); // 123
parseInt('1.7'); // 1
parseInt('1f', 16); // 31
parseInt('12@12'); // 12
```

> - parseFloat(string)
```
// 将字符串转成浮点数
parseFloat('12.01'); // 12.01
```

> - isNaN(number)
```
// 是否为非数字
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
> - isFinite(number)

```
// 是否非无穷数
isFinite(123); // true 
isFinite(2/0); // false
isFinite(Math.PI); // true
isFinite(Number.MAX_VALUE); // true
isFinite(Number.POSITIVE_INFINITY); // false
```

- URI Handling Function
```
decodeURI(encodeURI)
decodeURIComponent(encodeURIComponent)
encodeURI(uri)
encodeURIComponent(uriComponent)
```
- Constructor
```
Object()
Function()
Array()
String()
Number()
Date()
RegExp()
Error()
EvalError()
RangeError()
ReferenceError()
SyntaxError()
TypeError()
URIError()
```
- Other Properties
```
Math
JSON
```

### Object Objects
- Properties of the Object Constructor
```
Object.prototype
Object.getPrototypeOf(O)
Object.getOwnPropertyDescriptor(O, P)
Object.getOwnPropertyNames(O)
Object.create(O[, Properties])
Object.defineProperty(O, P, Attributes)
Object.defineProperties(O, Properties)
Object.seal(O)
Object.freeze(O)
Object.preventExtensions(O)
Object.isSealed(O)
Object.isFrozen(O)
Object.isExtensible(O)
Object.keys(O)
```
- Properties of the Object Prototype Object
```
Object.prototype.constructor
Object.prototype.toString()
Object.prototype.toLocaleString()
Object.prototype.valueOf()
Object.prototype.hasOwnProperty(V)
Object.prototype.isPrototypeOf(V)
Object.prototype.propertyIsEnumerable(V)
```

### Function Objects
- Properties of the Function Constructor
```
Function.prototype
Function.length
```
- Properties of the Function Prototype Object
```
Function.prototype.constructor
Function.prototype.toString()
Function.prototype.apply(thisArg, argArray)
Function.prototype.call(thisArg[, arg1[, arg2...]])
Function.prototype.bind(thisArg[, arg1[, arg2...]])
```

### Array Objects
- Properties of the Array Constructor
```
Array.prototype
Array.isArray(arg)
```
- Properties of the Array Prototype Object
```
Array.prototype.constructor
Array.prototype.toString()
Array.prototype.toLocalString()
Array.ptototype.concat([item1[, item2...]])
Array.prototype.join(separator)
Array.prototype.pop()
Array.prototype.push([item1[, item2...]])
Array.prototype.reverse()
Array.prototype.shift()
Array.prototype.slice(start, end)
Array.prototype.sort(comparefn)
Array.prototype.splice(start, deleteCount[item1[, item2...]])
Array.prototype.unshift([item1[, item2...]])
Array.prototype.indexOf(searchElement[, fromIndex])
Array.prototype.lastIndex(searchElemen[, fromIndex])
Array.prototype.every(callbackfn[, thisArg])
Array.prototype.some(callbackfn[, thisArg])
Array.prototype.forEach(callbackfn[, thisArg])
Array.prototype.map(callbackfn[, thisArg])
Array.prototype.filter(callbackfn[, thisArg])
Array.prototype.reduce(callbackfn[, initialValue])
Array.prototype.reduceRight(callbackfn[, initialValue])
```

### String Objects
- Properties of the String Constructor
```
String.prototype
String.fromCharCode([char0[, char1[, char2...]])
```
- Properties of the String Prototype Object
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
- Properties of the Boolean Constructor
```
Boolean.prototype
```
- Properties of the Boolean Prototype Object
```
Boolean.prototype.constructor
Boolean.prototype.toString()
Boolean.prototype.valueOf()
```

### Number Objects
- Properties of the Number Constructor
```
Number.prototype
Number.MAX_VALUE
Number.MIN_VALUE
Number.NaN
Number.NEGTIVE_INFINITY
Number.POSITIVE_INFINITY
```
- Properties of the Number Prototype Object
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
- Properties of the Date Constructor
```
Date.prototype
Date.parse(string)
Date.UTC(year, month[, day[, hours[, minutes[, seconds[, ms]]]]])
Date.now()
```
- Properties of the Date Prototype Object
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
- Properties of the RegExp Constructor
```
RegExp.prototype
```
- Properties of the RegExp Prototype Object
```
RegExp.prototype.constructor
RegExp.prototype.exec(string)
RegExp.prototype.test(string)
RegExp.prototype.toString()
```

### Error Objects
- Properties of the Error Constructor
```
Error.prototype
```
- Properties of the Error Prototype Object
```
Error.prototype.constructor
Error.prototype.name
Error.prototype.message
Error.prototype.toString()
```

### The Math Object
- Value Properties of the Math Object
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
- Function Properties of the Math Object
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
