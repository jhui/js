---
id: objects
title: Objects & prototypes
permalink: docs/objects.html
prev: introduction
---

### Objects

> Except primitive data type like number or boolean, all data including functions in JavaScript are objects.

All variables below are objects:
```javascript
var o = new Object(),
    str = 'myString',
    v = Math.random();
```

JavaScript defines an object as a collection of properties each containing a value or a function. In short, an object contains name-value pairs. JavaScript provides an _Object()_ constructor function to create an object. As a dynamic language, JavaScript can add properties to an object in runtime. Here we add 2 property values and 1 functions.
```javascript
var o = new Object();
o.name = 'Physics';
o.fee = 300;

o.m1 = function() {
    return this.fee>200;   // this refers to the object calling this function.
}

console.log(o.m1());
```

To access property:
```javascript
o.fee;
o['fee'];
```

Declare with object initializers:
```javascript
var o = {
	a: 1,
	b: 'c',
	m1: function() {
		console.log('hi');
	}
}

o.m1();
```

Method can be declared without explicitly define the key. (The key is default to the method name.) 
```javascript
var o = {
	m1() {
		console.log('hi');
	}
}

o.m1();
```

Assign a method as an object's method:
```javascript
function m1() {
	console.log('hi');
}

var o = {
}

o.m1 = m1;
o.m1();
```

Using constructor function
```javascript
function Lesson(name) {
    this.name = name;
}

l1 = new Lesson('Physics');
```

#### Getter & setter

Getter and setter methods:
```
var o = {
  a: 1,
  get b() { 
    return this.a;
  },
  set c(v) {
    this.a = v;
  }
};

console.log(o.a); // 1
console.log(o.b); // 1
o.c = 2;
console.log(o.a); // 2
```

#### Delete property
```javascript
var o = {
  a: 1,
};

delete o.a;

console.log(o);   // {}
```

### Prototype
Let's create a new object _o_ using the _Object()_ constructor:
```javascript
o = new Object();
o.lastname = 'Smith';
o.say = function () {return "hello " + this.lastname};

// Accessing an object
console.log(o.say());   // hello Smith
```

Print out _o_'s property/value pairs:
```javascript
console.log(o);
// Object {lastname: "Smith", say: function}
//    lastname: "Smith"
//    say: function ()
//    __proto__: Object   // prototype
```

In the printout above, there is a built-in property called _\_\_proto\_\__. Every object contains an additional anonymous object called **prototype** automatically. The prototype object (like other objects) contains values and functions including one pointing back to the constructor function. The following is the _Object_ function and its prototype.

<div class="imgcap">
<img src="/js/img/docs/o1.png" style="border:none;width:70%">
</div>
(Even though we use different shape and color to represent objects, constructors and prototypes, the JavaScript engine treats all as JavaScript objects.)

**Every object has a prototype object.** All non-primitive data structure in JavaScript  is an object. A constructor function is an object and has a prototype object.

The prototype object for the constructor function _Object()_ is retrieved by _.prototype_. 
```javascript
console.log(Object.prototype);
```

This is a printout of the property/value pairs inside _Object.prototype_.
<div class="imgcap">
<img src="/js/img/docs/o.png" style="border:none;width:100%">
</div>

All values and functions in the prototype object is accessible directly from _o_. 
```javascript
console.log(o.toString());    // [object Object]
```

The property _.prototype_ works with the JavaScript constructor.  To access the prototype of an object, use _Object.getPrototypeOf_:
```javascript
Object.getPrototypeOf(o);     // Find the prototype of o

o.__proto__;                  // Work but not recommended
```

### Prototype chaining

In JavaScript, a prototype provides similar purpose as a Java's class but their difference is quite significant. A JavaScript object inherits all values and functions of its prototype. With prototype chaining, it adds inheritance. To illustrate this, let's create 2 objects _l1_ and _l2_ using the _Lesson_ constructor below. When we declare a constructor function, a prototype object is automatically created and can be retrieved by Lesson.prototype.
```javascript
function Lesson(name) {
    this.name = name;
}

l1 = new Lesson('Physics');
l2 = new Lesson('Biology');

console.log(Object.getPrototypeOf(l1)===Lesson.prototype) // true
```

Here, a new prototype object for _Lesson_ is created. Since the prototype object is an object, it also has a \_\_proto\_\_. Here the engine will set its prototype _\_\_proto\_\__ to the prototype of _Object_.
<div class="imgcap">
<img src="/js/img/docs/o2.png" style="border:none;width:80%">
</div>

Again we print out _l1_
```javascript
console.log(l1);
// Lesson {name: "Physics""}
//   name : "Physics"
//   __proto__ : Object
```

Note that, a new property _name_ is added to _l1_ but not to the _Lesson_ even it is set in the constructor function _Lesson_. "_this_" in the constructor function below refers to the object calling/invoking the function. In this case, _this_ refers to _l1_ not the _Lesson_ object and therefore _name_ is added to _l1_ but not to Lesson.
```javascript
function Lesson(name) {
    this.name = name;
}

l1 = new Lesson('Physics');
```

As a dynamic language, JavaScript can add any properties to an object in runtime.
```javascript
l1.lab = 'Room 210';
l2.trip = 'Pacifica';
```

How about inheritance? Let's say we want to add a new property _fee_ that both _l1_ & _l2_ can inherit. We add this to the prototype of Lesson since all Lesson's objects inherit all values and functions of its prototype.
```javascript
Lesson.prototype.fee = 100;

console.log(l1.fee);    // 100
console.log(l2.fee);    // 100
```

If we print out the Lesson's prototype, we can find a new property _fee_ with value 100 is added.
```javascript
console.log(Object.getPrototypeOf(l1));

// Object {fee: 100, constructor: function}
//   fee : 100
//   constructor: function Lesson(name)
//   __proto__:
```

If _l1_ wants to override the value of _fee_, it can be done by:
```javascript
Lesson.prototype.fee = 100;

l1.fee = 200;

console.log(l1.fee);    // 200
console.log(l2.fee);    // 100
```

To access a property, JavaScript will first locate it from the object. If it is not found, it goes up the prototype chain until it is found. Setting the _fee_ for _l1_ only impacts itself but not _l2_. JavaScript creates a new property in _l1_ without changing the prototype copy.  So _fee_ for _l1_ is 200 while the prototype value remains at 100 as shown below.  To retrieve a property, JavaScript starts from the object before locate it through the prototype chain.
```javascript
console.log(l1);
// Lesson {name: "Physics", fee: 200}
//   fee: 200
//   name: "Physics"
//   lab : "Room 210"
//   __proto__: Object
//       fee: 100
//       constructor: function Lesson(name)
//       __proto__: Object
//       ...
```

### Summarize
Here we create _l1_ and _l2_ with is prototype set to the prototope of Lesson. We first set _fee_ at the prototype but then a separate copy is created in _l1_. To locate fee for _l1_, it is found in the object _l1_. For _l2_, we need to go up the chain and locate in the prototype.
<div class="imgcap">
<img src="/js/img/docs/o3.png" style="border:none;width:80%">
</div>

### Setting Prototype

#### Implicit prototype
The prototype of an object is set implicitly according to the object type. For example:
```javascript
// o ---> Object.prototype ---> null
var o = {a: 1};
o.hasOwnProperty();

// a ---> Array.prototype ---> Object.prototype ---> null
var a = ['0', '1', '2'];
a.indexOf[1];

// f ---> Function.prototype ---> Object.prototype ---> null
function f() {
  return 2;
}

f.call();
```


#### Object.setPrototypeOf
Prototype is just an object. We can set the prototype of an object explicitly using _Object.setPrototypeOf_. This creates an object hierarchy. In the code below, _o1_ inherits _d_ from _o3_ in the prototype chain.
```javascript
o1 = {a: 1, b: 2};
o2 = {b: 3, c: 4};
o3 = {d: 5};

Object.setPrototypeOf(o1, o2);
Object.setPrototypeOf(o2, o3);

console.log(o1.d);      // 5
```

#### Object.create
_Object.create_ creates a new object with an object argument as the prototype:
```javascript
var o1 = {
  name: 'stranger',
  hi: function() {
    console.log('Hi by ' + this.name);;
  }
};

var o2 = Object.create(o1);
o2.name = 'Paul';
o2.hi();    // Hi by Paul
```

#### .prototype with a constructor
```javascript
function Lesson(name) {
  this.name = name;
}

Lesson.prototype = {
  hi: function() {
    console.log('hi ' + this.name);
  }
};

var l1 = new Lesson('Paul');

l1.hi();   // hi Paul
```

#### class

ECMAScript 2015 introduced keywords related with classes. However, the implementation is through the prototype mechanism instead.
```javascript
class Lesson {
  constructor(name) {
    this.name = name;
  }
  get hi() {
  	return 'hi ' + this.name;
  }
}

class ScienceLesson extends Lesson {
  constructor(name, lab) {
    super(name);
    this.lab = lab;
  }
}

var l = new ScienceLesson('Physics', 'Room 131');
console.log(l);
// ScienceLesson {name: "Physics", lab: "Room 131"}
//    lab: "Room 131"
//    name: "Physics"
//    hi: (...)
//    __proto__: Lesson
//       constructor : class ScienceLesson
//       hi : (...)
//       __proto__: Object
//          constructor: class Lesson
//         hi: (...)
//         get hi: function hi()
//         __proto__
```

### Object hierarchy

Here we create a object hierarchy with _ScienceLesson_ inherits _Lesson_.
```javascript
function Lesson(name) {
    this.name = name;
}
Lesson.prototype.fee = 100;

function ScienceLesson(name, lab) {
	// Call the Lesson constructor with myself as the "this" object in Lesson.
    Lesson.call(this, name);
    if (typeof lab !== 'undefined') {
        this.lab = lab;
    }
}

// Replace the original prototype with a new object with prototype set to the prototype for Lesson.
ScienceLesson.prototype = Object.create(Lesson.prototype);
ScienceLesson.prototype.lab = 'Room 200';

l1 = new ScienceLesson('Physics');
l2 = new ScienceLesson('Biology');

console.log(l1.fee);   // 100
console.log(l1.lab);   // Room 200
console.log(l1);
// ScienceLesson {name: "Physics"}
//    name: "Physics"
//    __proto__: Lesson
//      lab: "Room 200"
//      __proto__: Object
//        fee: 100
//        constructor: function Lesson(name)
//        __proto__: Object
```

Replace the original prototype of _ScienceLesson_ with a new object with prototype set to the prototype for _Lesson_.
```javascript
ScienceLesson.prototype = Object.create(Lesson.prototype);
ScienceLesson.prototype.lab = 'Room 200';
```

Alternatively, we can use _new_ to create an object as the new prototype:
```javascript
ScienceLesson.prototype = new Lesson;
ScienceLesson.prototype.lab = 'Room 200';
```

Call the _Lesson_ constructor with _l1_ as the "this" object in Lesson.
```javascript
function ScienceLesson(name, lab) {
    Lesson.call(this, name);
}

l1 = new ScienceLesson('Physics');
```

### Performance
JavaScript runtime follows the chain of prototype to locate property. If the chain is long with a lot of properties in each prototype, the performance can be slow.

### Enumerate property

The code below shows how to explore properties using "for ... in" loop with _hasOwnProperty_ without going up the prototype chain. 
```
function showProps(obj) {
  var result = '';
  // Enumerate obj's property
  for (var i in obj) {
    // Only for local properties. Not from the prototype chain. 
    if (obj.hasOwnProperty(i)) {
      result += i + ' = ' + obj[i] + '\n';
    }
  }
  return result;
}

function Lesson(name) {
  this.name = name;
  this.fee = 100;
}

function ScienceLesson(name) {
  this.lab = 'Room 301';
}

var l = new ScienceLesson("Physics");
var r = showProps(l);
console.log(r);   // lab = Room 301
}
```

To explore all properties including those in the prototype chain:
```javascript
function listAllProperties(o) {
	var objectToInspect;     
	var result = [];
	
	for(o1 = o; o1 !== null; o1 = Object.getPrototypeOf(o1)) {  
      result = result.concat(Object.getOwnPropertyNames(o1));  
	}
	
	return result; 
}

var l = new ScienceLesson("Physics");
var r = listAllProperties(l);
```
