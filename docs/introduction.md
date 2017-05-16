---
id: introduction
title: Introduction
permalink: docs/introduction.html
next: objects
---

### Comment & variables

Declare variables with different scopes:
```
// Comment
/* 
  Multiple line
*/

// Variables
var a;                              // undefined
if (a === undefined) {
     console.log('a is undefined'); // a is undefined
}

while (!a) {
    let b = 3;                       // let is block scooped varaible
    const PI = 3.14;                 // const is block scooped constant
    var c = 4;                       // var is global scopped variable
		console.log('Inside while' + b);
    break;
}

console.log(a);                       // undefined
console.log(c);                       // 4
// console.log(PI);                   // Uncaught error, not defined
// console.log(b);                    // Uncaught error, not defined

var ary = [];                         // An array 
if (!ary[0]) 
    console.log('Empyt');             // Come here
```	

> JavaScript is case sensitive.

### Type
Primitive type:
* Boolean. true and false.
* null. 
* undefined. Declare but not initialized.
* Number. (2 or 3.14159)
* String. "hi"
* Symbol: Instances are unique and immutable.

> All non-primitive data structure in JavaScript are Objects: including arrays and functions.

### Number object
```javascript
var maxv = Number.MAX_VALUE;
var minv = Number.MIN_VALUE;
var posinfinite = Number.POSITIVE_INFINITY;
var negInfinite = Number.NEGATIVE_INFINITY;
var notNum = Number.NaN;
```

Numbers:
```javascript
1e3;
0xaa;
0666;
0b0001;
```

### Math
```javascript
Math.sin(1.01);
```

### Conversion

String/number conversion:
```javascript
s = 'Your change is $' + 4   // "You change is $4"

'14' - 4;   // 10 (The string is convert to a number)
'14' + 4;   // "144" The + op is string concatenation.

// 22: 14 + 4 + 4 (+'14' convert a string to a number)
(+'14') + (+'4')  + 4;

parseInt('14') + 4; // 18: 14 + 4
```

### typeof, instanceof
```javascript
var t = typeof "hi";       // returns "string"

function Lesson() {        // A constructor function
}

l = new Lesson;
var r = l instanceof Lesson;   // true
```

### String

```javascript
// String 
'123'.length;
'123' + '45';

var s = `Add special character'\n' `;  // special character.

// Multiline strings
`Multiple line
 text.`;
 
 // String interpolation
var name = 'Paul';
var x = `Hi ${name} `;          // Hi Paul
```


### Object literal
```javascript
var o = {a:'1', b:'2'};
var o2 = {a:1, b:'2', c:o};   // Elements can have different types

console.log(o.b);       // '2'
console.log(o['b']);    // same

// Array literal is just a special case of an object
var a = ['a', , 'c']    // a[1] is undefined.
// (3) ["a", undefined × 1, "c"]
//    0: "a"
//    2: "c"
//    length: 3
//    __proto__: Array(0)
```

JavaScript object with methods and implicit key:
```javascript
var v = 22;
var obj = {
    a,    // short for a:a
    m1: function() {console.log(a);},
    toString() {  // key is default to toString
       return 'Obj: ' + super.toString(); // Call super
    },
    [ 'p_' + (() => v)() ]: v,  // Same as: p_22: 22
};

obj.m1();
```

### super
```javascript
var obj = {
    toString() { 
       return 'Obj: ' + super.toString(); // Call super
    }
};
```

### array
```javascript
// All the same
var a = new Array(3);
var a = Array(3);
var a = [];
a.length = 3

a[2];
a.length;

a = [];
a[0] = 'a';
a[1] = 'b';

a.forEach(function(item) {
  console.log(item);    // a, b
});
```

#### Array operations
```javascript
a = [];
a[0] = 'a';
a[1] = 'b';

a = a.concat('d', 'e');
var str = a.join(' - '); // list is "a - b - d - e"

a.push('f');             // ['a', 'b', 'd', 'e', 'f']
var v = a.pop();         // ['a', 'b', 'd', 'e']
v = a.shift();           // ['b', 'd', 'e']
a.unshift('a', 'b');     // ['a', 'b', 'b', 'd', 'e']
a.reverse();
a.sort();

var sortFn = function(a, b) {
  if (a.length < b.length) return -1;
  if (a.length == b.length) return 0;
  return 1;
}
a.sort(sortFn); 

a.indexOf('d');
a.lastIndexOf('d');

a = a.map(function(item) { return item.toUpperCase(); });
a = a.filter(function(item) { return typeof item === 'string'; });

function isNumber(value) {
  return typeof value === 'number';
}
a.every(isNumber);
a.some(isNumber);

a = [1, 2, 3];
var total = a.reduce(function(v1, v2) { return v1 + v2; }, 0);
```

### Map
```javascript
var m = new Map();
m.set('a', '1');
m.set('b', '2');
m.size;          // 2
m.get('c');      // undefined
m.has('c');      // false
m.delete('b');

for (var [key, value] of m) {
  console.log(key + ' = ' + value);
}

m.clear();
```

### Set
```javascript
var s = new Set();
s.add(1);
s.add('a');
s.add(1);

s.has(1); // true
s.delete('a');
s.size; 
for (let item of s) 
  console.log(item);  // 1
```

#### Array <-> set conversion
```javascript
Array.from(s);
[...s];

a = new Set([1, 2, 3, 4]);
```

### RegExp literal

```javascript
var re = /a+b/;
```

### Condition

**false** in JavaScript means:
* false
* undefined
* null
* 0
* NaN
* the empty string ("")


```javascript
var x = 0;

if (x==5) {
  console.log('Equal to 5');
} else if (x==6) {
  console.log('Equal to 6');
} else {
  console.log('else');
} 

if ((x==0) || (x==1)) {
    console.log('0 or 1');  	
}

switch (x) {
  case 5:
    console.log('Equal to 5'); // auto break 
  case 0:
    console.log('Equal to 0');
  default:
    console.log('else');
}
```

Value comparison:
```javascript
// Comparison
v1 = '1'
v2 = 1
console.log(v1==v2);   // true
console.log(v1===v2);  // false since they are not the same type
console.log(v1!==v2);  // true
```

#### if ... in & delete
```javascript
a = ['a', 'b', 'c']
if (2 in a)            // check if the key 2 is present in the object
  delete a[2];         // undefined a[2]
```

### try ... catch ... finally

Exception handling:
```javascript
try { 
  throw 'Some error';
}
catch (e) {
  console.log(e);   // Some error
} finally {
  console.log('finally');   // Always execute
}
```

Throw error objects:
```
throw (new Error('Some error'));
````

### Promise

Return a promise object so the caller can handle whether the call is resolved or rejected.
```javascript
  function imgLoad(url) {
    return new Promise(function(resolve, reject) {
      var request = new XMLHttpRequest();
      request.open('GET', url);
      request.responseType = 'blob';
      request.onload = function() {
        if (request.status === 200) {
          resolve(request.response);      
        } else {
          reject(Error('error:' + request.statusText));
        }
      };
      request.onerror = function() {
          reject(Error('There was a network error.'));
      };
      request.send();
    });
  }

  var body = document.querySelector('body');
  var myImage = new Image();
  imgLoad('img.jpg').then(function(response) {   // Get a response object.
    var imageURL = window.URL.createObjectURL(response);
    myImage.src = imageURL;
    body.appendChild(myImage);
  }, function(Error) {
    console.log(Error);
  });
  ```
  
### Loop
```javascript
  var x = 0;

  do
    x++;
  while (x<5);

  while (x < 5) {
    x++;
  }

  outerLoop:
  while (x > 1) {
    while (x > 2) {
      break outerLoop;     // Break out of the loop with label: outerLoop
	  break;
      continue outerLoop;  // Continue to the loop.
    }
  }
```
  
### for loop, for ... in. for ... of
```javascript
  var step;
  for (step = 0; step < 2; step++) {
    console.log(step);
  }

  var o = {a:'1', b:'2'};

  for (key in o) {
     console.log(o[key]);  // 1, 2
  }

  var a = ['a', 'b']
  for (elm of a) {
     console.log(elm);  // 'a', 'b'
  }
```
  
### Function
  
```javascript
  function f1() {
  }
  f1();

  // Function expression
  var square = function(number) { return number * number; };
  var v = square(3);

  // With a name so it can be called recursively
  var factorial = function fact(n) { return n < 2 ? 1 : n * fact(n - 1); };

  // Take a function parameter
  function map(f, a) {
    for (item of a) {
  	  console.log(f(item))   // 1, 4
    }
  }
  map(square, [1, 2])
```

#### Nested functions and closure
```javascript
function outside(x) {
  function inside(y) {
    return x + y;
  }
  return inside;
}

fn_inside = outside(2);   // A function with a closure of x set to 2.
r1 = fn_inside(3); 

r2 = outside(2)(3);       // 5
```

Closure
```javascript
var c = function(name) {   
  var getName = function() {
    return name;
  }
  return getName;         
}
cl = c('Paul');
   
cl();                     // 'Paul'
```

#### Default and rest parameters
```javascript
function m1(a, b = 1) {  // default parameters
}
m1(3);

function m2(m, ...params) {  // rest parameters
  return params.map(x => m * x);
}

var res = m2(2, 1, 2, 3);
console.log(res);            // 2, 4, 6
```

### Arrow function
```javascript
var a = ['a', 'bc'];

var a2 = a.map(function(s) { return s.length; });
console.log(a2); // 1, 2

var a3 = a.map(s => s.length);  // arrow function
console.log(a3); // Same
```

### Operators
```javascript
var r = 2 ** 3 // 8
r>>>1          //unsigned shift

1 / 2;         // Division is done in float: 0.5

// Destructuring assignment
var a = [1, 2];
var [one, two] = a   // one = 1, two = 2
console.log(two);

var r = (r >= 0) ? r++ : r--;
```

#### Spread ...
```javascript
function f(x, y, z) { 
}
var params = [0, 1, 2];
f(...args);

```

### Iterator
```javascript
function next(a) {
    var index = 0;   
    return {
       next: function() {
           return index < a.length ?
               {value: a[index++], done: false} :
               {done: true};
       }
    };
}

var it = next(['a', 'b']);
console.log(it.next().value); // 'a'
console.log(it.next().value); // 'b'
console.log(it.next().done);  // true
```

### Iterables
```javascript
var iter = {};

iter[Symbol.iterator] = function* () {
    yield 1;
    yield 2;
    yield 3;
};

for (let v of iter) { 
    console.log(v); 
}
// 1
// 2
// 3

[...iter]; // [1, 2, 3]
```
### Generator
```javascript
function* generator() {   // * indicates it is a generator
  var index = 0;
  while(true)
    yield index++;
}

var gen = generator();

console.log(gen.next().value); // 0
console.log(gen.next().value); // 1
console.log(gen.next().value); // 2
```
