# JS
##### Compatibility table : http://kangax.github.io/compat-table/es6/
ES2015 is currently not well supported, a transpiler (from source to source) is needed : [Babel](http://babeljs.io/)

to install:
```
npm install -g babel
```
```
babel app.js > es5_app.js
```
## let

ES5:
```
for (var i = 0; i < 5; i++) {
  // bla
}
console.log(i); // 5
```

ES2015:
```
for (let i = 0; i < 5; i++) {
  // bla
}
console.log(i); // Uncaught ReferenceError : i is not defined
```

ES5:
```
var x = 3;
    function func(randomize) {
        if (randomize) {
            var x = Math.random(); // (A) scope: whole function
            return x;
        }
        return x; // accesses the x from line A
    }
    func(false); // undefined
```
or
```
var x = 3;
    function func(randomize) {
        var x;
        if (randomize) {
            x = Math.random();
            return x;
        }
        return x;
    }
    func(false); // undefined
```

ES2015:
```
let x = 3;
    function func(randomize) {
        if (randomize) {
            let x = Math.random();
            return x;
        }
        return x;
    }
    func(false); // 3
```

ES5:
```
(function () {  // open IIFE to keep variable local
        var tmp = ···;
        ···
    }());  // close IIFE

    console.log(tmp); // ReferenceError
```

ES2015:
```
{  // open block and use let
        let tmp = ···;
        ···
    }  // close block

    console.log(tmp); // ReferenceError
```

pros :
* ```let``` scope only exists in the block where it has been created

## const
```
const TEST = 1;
TEST = 2; // Error : TEST is read-only
```

pros :
* ```const``` variable can't be changed

## fat arrow function
```
var numbers = [1,2,3];

numbers.forEach(function(element, index) {
  console.log('numbers[' + index + '] = ' + element);
});
```
```
let numbers = [1,2,3];

numbers.forEach((element, index) => { console.log('numbers[' + index + '] = ' + element) });
```

pros :
* improve visibility and clear code
* it comes from the coffeescript syntax

```
function Person(){
  this.age = 0;

  setInterval(() => {
    this.age++;
  }, 1000);
}
```

pros :
* possibility to change context : it can reach the parent scope with flat arrow function (like ```bind(this)```)

## Templating system for string

ES5:
```
var name = "DCK";
console.log("Hello " + name + " !"); // "Hello DCK"
```

ES2015:
```
let name = "DCK";
console.log(`Hello ${name} !`); // "Hello DCK"
```

ES5:
```
var HTML5_SKELETON =
        '<!doctype html>\n' +
        '<html>\n' +
        '<head>\n' +
        '    <meta charset="UTF-8">\n' +
        '    <title></title>\n' +
        '</head>\n' +
        '<body>\n' +
        '</body>\n' +
        '</html>\n';
```

ES5 improved:
```
var HTML5_SKELETON = '\
        <!doctype html>\n\
        <html>\n\
        <head>\n\
            <meta charset="UTF-8">\n\
            <title></title>\n\
        </head>\n\
        <body>\n\
        </body>\n\
        </html>';
```

ES2015:
```
const HTML5_SKELETON = `
        <!doctype html>
        <html>
        <head>
            <meta charset="UTF-8">
            <title></title>
        </head>
        <body>
        </body>
        </html>`;
```

pros :
* improve visibility and clear code

cons :
* new syntax with  ```${name}``` and ``` ` ```

## Iterators

ES3 => ES5 => ES2015

Array:
for => forEach => for-of

Object:
for-in => Object.keys => previous ones or turn into iterable object

### ES3 (Most used : for and for-in (while, do-while)):

#### for (for looping over an array)

```
var arr = [ 'a', 'b', 'c' ];

for (var i=0; i<arr.length; i++){
    console.log(i+' : '+arr[i]);
}
// 0 : a
// 1 : b
// 2 : c
```

#### for-in (for looping over an object)

##### Using with array (wrong !)

```
var a = [];
a[5] = 'hello'; // Perfectly legal JavaScript that resizes the array.

for (var i=0; i<a.length; i++) {
    console.log(i); // Iterates over numeric indexes from 0 to 5, as everyone expects.
}
// 0
// 1
// 2
// 3
// 4
// 5
```

```
var a = [];
a[5] = 'hello';
for (var x in a) {
    console.log(x); // Shows only the explicitly set index of "5", and ignores 0-4
}
// 5
```

```
// Somewhere deep in your JavaScript library...
Array.prototype.foo = 1;

// Now you have no idea what the below code will do.
var a = [1,2,3,4,5];
for (var x in a){
    console.log(x);
    // Now foo is a part of EVERY array and
    // will show up here as a value of 'x'.
}
// 0
// 1
// 2
// 3
// 4
// foo
```

cons :
* no guarantee of order
* inherited properties from prototype or from the array

##### Using with object (good !)

```
var person = {fname:'John', lname:'Doe', age:25};

for (var prop in person){
    if (Object.prototype.hasOwnProperty.call(person, prop)){
        console.log(prop+' : '+person[prop]);
    }
}
// fname : John
// lname : Doe
// age : 25
```
### ES5:

* [1,2,3].every()
* [1,2,3].filter()
* [1,2,3].forEach()
* [1,2,3].map()
* [1,2,3].reduce()
* [1,2,3].some()

#### forEach

```
var arr = [ 'a', 'b', 'c' ];

arr.forEach(function(elem,i){
    console.log(i+' : '+elem);
});
// 0 : a
// 1 : b
// 2 : c
```
pros:
* replace ES3 for

#### Object.keys

```
var person = {fname:'John', lname:'Doe', age:25};

Object.keys(person).forEach(function(prop){
    console.log(prop+' : '+person[prop]);
});
// fname : John
// lname : Doe
// age : 25
//Note a "for in" loop is still a viable alternative, you just have
//to deal with inherited properties like we did in ES3
```
pros:
* Replace ES3 for-in
* Inherited properties are not being accessed (for arrays)

### ES2015:

#### New in ES2015 : iterable protocol and iterator protocol (TODO)

This JS values are iterable (means that each of this listed values can be looped by way of the new for-of statement) by default because their prototype objects all provide access to an iterator method :

* Array
* String
* Map (new in ES6)
* Set (new in ES6)
* NodeLists and HTMLCollections from the DOM
* Arguments
* Generator functions (Note: A generator is both an iterator and iterable)

#### for-of

```
for (var key of table) {
  console.log(key + ' = ' + table[key]);
}
```

pros :
* guarantee the order of key
* implement the iterable protocol (the object must have a property with a Symbol.iterator key, it would be : ```table [Symbol.iterator]```). It defines special keys that will never conflict with regular object keys. (TODO : add content from https://strongloop.com/strongblog/introduction-to-es6-iterators/)

##### array with for-of
```
var arr = [ 'a', 'b', 'c' ];

for (let elem of arr) {
   console.log(elem);
}
// a
// b
// c
```
##### string with for-of
```
for (let item of 'hello') {
   console.log(item);
}
// h
// e
// l
// l
// o
```
##### map with for-of
```
//construct map with an iterable i.e. an array
let myMap = new Map([[1,'h'],[2,'e'],[3,'l'],[4,'l'],[5,'o']]);

//loop over both key and value
for (let val of myMap){
   console.log(val);
}

//loop over just values
for (let val of myMap.values()){
   console.log(val);
}

//loop over just keys
for (let key of myMap.keys()){
   console.log(key);
}
// [1,"h"]
// [2,"e"]
// [3,"l"]
// [4,"l"]
// [5,"o"]
// h
// e
// l
// l
// o
// 1
// 2
// 3
// 4
// 5
```
Note : only Array, Map and Set values have the ```entries()```, ```keys()``` and ```values()``` methods.

```
let myArray = ['a', 'b', 'c'];

for(let item of myArray.entries()){
  console.log(item);
}

for(let item of myArray.keys()){
  console.log(item);
}

for(let item of myArray.values()){
  console.log(item);
}
// [0, "a"]
// [1, "b"]
// [2, "c"]
// 0
// 1
// 2
// "a"
// "b"
// "c"
```

##### set with for-of

```
//construct set with an iterable i.e. an array
let mySet = new Set([1,2,2,3,4,5,6,6]); //

//loop values in set, note I did not use a key, but you can
for (let val of mySet){
   console.log(val);
}
// 1
// 2
// 3
// 4
// 5
// 6
```

##### DOM Nodes with for-of

```
// HTML

<!DOCTYPE html>
<html>
<head>
<script src="https://cdn.rawgit.com/zloirock/core-js/master/client/shim.min.js"></script>
  <meta charset="utf-8">
  <title>JS Bin</title>
</head>
  <script src=""></script>
<body>
  <div id="foo"></div>
  <div id="bar"></div>
</body>
</html>

// JS

//loop over NodeList
for (let node of document.querySelectorAll('div')) {
  console.log(node.id);
}

//loop over HTMLCollection
for (let node of document.getElementsByTagName('div')) {
  console.log(node.id);
}
// "foo"
// "bar"
// "foo"
// "bar"
```

##### Arguments with for-of

```
let logArguments = function(){
  for (let items of arguments) {
    console.log(items);
  }
}

logArguments('a', 'b', 'c');
// "a"
// "b"
// "c"
```

##### Looping generators

```
var generator1 = function*(){
    yield 1;
    yield 2;
    yield 3;
}();

for(let y of generator1){
    //iterable, use "for-of" to yield values
    console.log(y); //logs 1,2,3
};

var generator2 = function*(){//iterator, use next() to yield
    yield 1;
    yield 2;
    yield 3;
}();

console.log(generator2.next()); //{"value":1,"done":false}
console.log(generator2.next()); //{"value":2,"done":false}
console.log(generator2.next()); //{"value":3,"done":false}
console.log(generator2.next()); //{"done":true}

//Note: once a generator has been "looped" it can't be looped again, so to speak
```

##### Looping an iterable with an iterator

```
let myArray = ['a', 'b', 'c'];

//create iterator instance, by invoking Symbol.iterator
let iter = myArray[Symbol.iterator](); //Symbol.iterator is an inherited function

//now use next() to step through array

console.log(iter.next());
console.log(iter.next());
console.log(iter.next());
console.log(iter.next());
// [object Object] {done: false, value: "a"}
// [object Object] {done: false, value: "b"}
// [object Object] {done: false, value: "c"}
// [object Object] {done: true, value: undefined}
```

##### Creating own iterable Objects

Object objects are not designed to be iterable.

To iterate over an Object object, one can still use the looping mechanisms found in ES3 or ES5 (i.e. for-in loop or Object.keys and forEach()). Or, as of ES6, you do have the option of turning a plain Object object into an iterable object.

```
let sentence = {
  _str : 'Try Kendo UI Today'
};

sentence[Symbol.iterator] = function() {
    var re = /\S+/g;
    var str = this._str;

    return {
        next: function() {
            var match = re.exec(str);
            if (match) {
                return {value: match[0], done: false};
            }
            return {value: undefined, done: true};
        }
    };
};

for (let words of sentence) {
    console.log(words);
}
// "Try"
// "Kendo"
// "UI"
// "Today"
```

## Objects

In JS, methods are properties whose values are functions.

ES5:
```
var obj = {
  foo: function () {
    ···
  },
  bar: function () {
    this.foo();
  }, // trailing comma is legal in ES5
}
```

ES2015:
```
let obj = {
  foo() {
      ···
  },
  bar() {
    this.foo();
  },
}
```

## Class

ES5:
```
function Player(name) {
  this.name = name;
  this.life = 100;
}
Player.prototype.getLife = function () {
    return this.life;
};

var Tom = new Player("Tom");
Tom.getLife(); // 100
```

ES2015:
```
class Player {
  static bouyah() {
    alert('Bouyah !');
  }

  constructor(name) {
    this.name = name;
    this.life = 100;
  }

  getLife() {
    return this.life;
  }
};

var Tom = new Player("Tom");

Tom.getLife(); // 100
Player.bouyah(); // alert('Bouyah');
```

pros :
* new syntax with ```class```, ```static``` (static method called with the ```Player``` object), ```constructor```(called for each instantiation).

## Inheritance

ES5:
```
function Player(name) {
  this.name = name;
  this.life = 100;
}
Player.prototype.getLife = function () {
    return this.life;
};

function Mage(name) {
  Player.call(this, name, life); // super(name, life)
}

Mage.prototype = Object.create(Player.prototype);
Mage.prototype.constructor = Mage;
Mage.prototype.describe = function () {
    return Player.prototype.getLife.call(this); // super.getLife()
};

var TomTheWizard = new Mage('Tom');
TomTheWizard.getLife(); // 100
```

ES2015:
```
class Player {
  constructor(name) {
    this.name = name;
    this.life = 100;
  }

  get name() {
    return this.name;
  }

  getLife() {
    return this.life;
  }
}

class Mage extends Player {
  constructor(name) {
    super(name);
  }

  fireball() {
    alert('Fireball !!!!');
  }
}

var TomTheWizard = new Mage('Tom');
TomTheWizard.getLife(); // 100
TomTheWizard.fireball(); // alert
console.log(TomTheWizard.name); // 'Tom'
```
pros:
* new syntax with ```extends``` (inheritance of methods from parent to child), ```super()``` (call the constructor of parent), ```get name()``` (simplify getter and setter).

ES5:
```
function MyError() {
// Use Error as a function
var superInstance = Error.apply(null, arguments);
copyOwnPropertiesFrom(this, superInstance);
}
MyError.prototype = Object.create(Error.prototype);
MyError.prototype.constructor = MyError;
```

ES2015:
```
class MyError extends Error {
}
```
pros:
* In ES2015, all built-in constructors can be subclassed

## Modules
```
// addition.js
export function(a, b) {
  return a + b;
}

// app.js
import { addition } from './addition.js';
```
pros:
* To avoid using tools like Browserify or RequireJS

```
// calc.js
export default function (a, b) {
  return a + b;
};

export function substract(a, b) {
  return a - b;
};

// app.js
import add, { substract } from './calc.js';
```
pros:
* Default methods to avoid destructuring assignement

```
// addition.js
export function add(a, b) {
  return a + b;
};

// app.js
import { add as addition } from './addition.js';
```
pros:
* Creating alias to rename if 2 modules have the same name

## Destructuring assignement

ES5:
```
var email = req.body.email;
var message = req.body.message;

var unTableau = [1,2,3];
var one = unTableau[0];
var two = unTableau[1];
```
ES2015:

```
var { email, message } = req.body;
//create an email and message variable from the req.body object

var unTableau = [1,2,3];
var [one, two] = unTableau;
//create a one and two variable with the value in the index (0 and 1)
```

ES5:
```
var obj = { foo: 123 };

    var propDesc = Object.getOwnPropertyDescriptor(obj, 'foo');
    var writable = propDesc.writable;
    var configurable = propDesc.configurable;

    console.log(writable, configurable); // true true
```

ES2015:
```
let obj = { foo: 123 };

    let {writable, configurable} = // = { writable: writable, configurable: configurable }
        Object.getOwnPropertyDescriptor(obj, 'foo');

    console.log(writable, configurable); // true true
```

## Promise
```
function findUser(id) {
  return new Promise(function(resolve, reject) {
    xhr(`/users/${id}`, function(err, user) {
      if (err) {
        reject(err);
      }

      resolve(user);
    })
  });
}

findUser(10)
  .then(user => {
    // deal with user
  })
  .catch(err => {
    //deal with error
  });
```
The ```findUser``` function return a new instance of the new class ```Promise``` (with a callback with 2 arguments (which are functions)): ```resolve```(called when it's allright) and ```reject```(do a trigger on the ```catch``` method))

pros:
* No more callback hell
* Clear code

## Default parameter

ES5:
```
function hello(message) {
  message = message || 'Hello World';
  return message;
}
hello(); // 'Hello World'
hello('toto'); // 'toto'
```

ES2015:
```
function hello(message='Hello World') {
  return message;
}

hello(); // 'Hello World'
hello('toto'); // 'toto'
```

## Naming parameters

ES5 via object literals (options object pattern):
```
function selectEntries(options) {
        options = options || {}; // make parameter option optional
        var start = options.start || 0;
        var end = options.end || -1;
        var step = options.step || 1;
        ···
    }
```

ES2015:
```
function selectEntries({ start=0, end=-1, step=1 } = {}) {
        ···
    }
```

pros:
* No need to add a condition in function

## Spread operator

ES5:
```
function add(a, b) {
  return a + b;
}

var someNumbers = [1, 2];
add(someNumbers[0], someNumbers[1]); //3
```

ES2015:
```
function add(a, b) {
  return a + b;
}
var someNumbers = [1, 2];
add(...someNumbers); //3
```

ES5:
```
var array1 = [1, 2];
var array2 = [3, 4];

array1.push.apply(array1, array2); //[1, 2, 3, 4]
```

ES2015:
```
var array1 = [1, 2];
var array2 = [3, 4];

array1.push(...array2); //[1, 2, 3, 4]
```

ES2015:
```
var articulations = ['épaules', 'genoux'];
var corps = ['têtes', ...articulations, 'bras', 'pieds'];
//['têtes', 'épaules', 'genoux', 'bras', 'pieds']
```

## Glossary

* Arity : number of arguments expected by a function (arity property deprecated since length property)

* Anonymous function : function not identified by a name

```
// IIFE
(function (){
  //body
})();

// Anonymous too
var foo = function() {
  //body
};
```

* Closure : an inner function, that is accessible outside of its outer function’s scope, with its connection to the outer function’s variables still intact

```
function order() {
    var food;   
    function waiter(order) {
        chef(order);
        return food;
    }
    function chef(order) {
        if (order === 'pasta') {
            food = ['pasta', 'gravy', 'seasoning'];
            cook();
        }
    }
    function cook() { food.push('cooked'); }
    return waiter;
}
var myOrder = order();
console.log(myOrder('pasta'));
// Array [ "pasta", "gravy", "seasoning", "cooked" ]
```

* Currying : using multiple functions with single arguments

```
function addx(x){
    function addy(y){
        return x+y;
    }
    return addy
}

function add(x,y){
    return(x+y);
}

console.log(addx(3)(4)); \\7
console.log(add(3,4)); \\7
```
```
var add4 = addx(4);
console.log(add4(8)); //12
console.log(add4(6)); //10
console.log(add4(-74)); //-70
```
Fixing a step in a sequence of operations like the addition of 4 is helpful when one of the variables used in the operation is always the same.

* Mutation : changes of DOM elements (MutationObserver API)

* Pragma : code that consist of useful information on how a compiler or interpreter or assembler should process the program

```
"use strict"; // means no bad syntax, no hoisting, silent errors shown ...
```
* Sentinels (or flags) : values that are used to indicate the end of a loop or process

```
function getPos(ary, val) {
    var i=0, len=ary.length;    
    for(;i<len;i++){
        if(ary[i]===val) return i+1;
    }    
    return -1;
}
console.log(getPos(['r','y','w'],'y')); //2
console.log(getPos(['r','y','w'],'g')); //-1
```

* Variadic function : function that has variable number of arguments

```
function test(...a){
  console.log(a);
}
test('a','b','c',8,[56,-89]);
//output is Array [ "a", "b", "c", 8, Array[2] ]
```

next :

* https://blog.risingstack.com/asynchronous-javascript/
* https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Iteration_protocols#iterator
* https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/function*
* http://www.2ality.com/2015/08/es6-map-json.html
* https://github.com/mattdesl/promise-cookbook
* http://blogs.msdn.com/b/eternalcoding/archive/2015/09/30/javascript-goes-to-asynchronous-city.aspx
* https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions
* https://developer.mozilla.org/fr/docs/Web/JavaScript/Reference/Instructions/try...catch
* https://developer.uber.com/v1/sandbox/
* http://www.2ality.com/2015/08/getting-started-es6.html (TODO: 16 and 17)
* https://medium.com/@housecor/12-rules-for-professional-javascript-in-2015-f158e7d3f0fc
* https://medium.com/javascript-scene/10-interview-questions-every-javascript-developer-should-know-6fa6bdf5ad95
* http://engineering.widen.com/blog/future-of-the-web-react-falcor/

##### Markdown Cheatsheet : https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet
##### :question: Sources
* https://developer.mozilla.org/fr/docs/Web/JavaScript
* https://fivejs.codeschool.com/
* http://maxlab.fr/
* http://www.lilleweb.fr/
* https://strongloop.com/strongblog/
* http://www.2ality.com/
* http://developer.telerik.com/featured/six-steps-for-approaching-the-next-javascript/
* https://github.com/airbnb/javascript

##### :books: Books
* http://exploringjs.com/es6/
* http://speakingjs.com/es5/
* https://github.com/getify/You-Dont-Know-JS
* http://netcraft.co.il/fedia/books/SecretsoftheJavaScriptNinja.pdf
* http://addyosmani.com/resources/essentialjsdesignpatterns/book/
* https://github.com/jasonzhuang/tech_books/tree/master/js

##### :+1: JS Stuff
* https://github.com/mgechev/javascript-algorithms
* https://github.com/sorrycc/awesome-javascript
* http://projects.formidablelabs.com/es6-interactive-guide/#/

##### :eyes: Not JS Stuff
* http://www.hongkiat.com/blog/social-media-imaging-tools/
* http://www.bootply.com/
* http://blog.bitovi.com/style-guide-driven-development/
