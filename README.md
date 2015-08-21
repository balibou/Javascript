# ES2015
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

```
for (var i = 0; i < 5; i++) {
  // bla
}
console.log(i); // 5
```
```
for (let i = 0; i < 5; i++) {
  // bla
}
console.log(i); // Uncaught ReferenceError : i is not defined
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

```
var name = "DCK";
console.log("Hello " + name + " !"); // "Hello DCK"
```
```
let name = "DCK";
console.log(`Hello ${name} !`); // "Hello DCK"
```

pros :
* improve visibility and clear code

cons :
* new syntax with  ```${name}``` and ``` ` ```

## Iterators

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

#### for-of

```
for (var key of table) {
  console.log(key + ' = ' + table[key]);
}```

pros :
* guarantee the order of key
* implement the iterable protocol (the object must have a property with a Symbol.iterator key, it would be : ```table [Symbol.iterator]```). It defines special keys that will never conflict with regular object keys. (TODO : add content from https://strongloop.com/strongblog/introduction-to-es6-iterators/)

## Class
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

```
var email = req.body.email;
var message = req.body.message;

var unTableau = [1,2,3];
var one = unTableau[0];
var two = unTableau[1];
```
New assignement method :

```
var { email, message } = req.body; //create an email and message variable from the req.body object

var unTableau = [1,2,3];
var [one, two] = unTableau; //create a one and two variable with the value in the index (0 and 1)
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

```
function hello(message='Hello World') {
  return message;
}

hello(); // 'Hello World'
hello('toto'); // 'toto'
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
```
var array1 = [1, 2];
var array2 = [3, 4];

array1.push(...array2); //[1, 2, 3, 4]

var articulations = ['épaules', 'genoux'];
var corps = ['têtes', ...articulations, 'bras', 'pieds']; //['têtes', 'épaules', 'genoux', 'bras', 'pieds']
```

next :

* http://developer.telerik.com/featured/the-javascript-looping-evolution/
* https://strongloop.com/strongblog/
* https://developer.mozilla.org/fr/docs/Web/JavaScript/Reference/Instructions/for...of
* http://www.2ality.com/2015/08/getting-started-es6.html
* http://www.hongkiat.com/blog/javascript-jargon/
* https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Iteration_protocols#iterator

##### Markdown Cheatsheet : https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet
##### Sources :
* https://developer.mozilla.org/fr/docs/Web/JavaScript
* https://fivejs.codeschool.com/
* http://maxlab.fr/
* http://www.lilleweb.fr/
* https://strongloop.com/strongblog/
* http://www.2ality.com/
* http://developer.telerik.com/featured/six-steps-for-approaching-the-next-javascript/
* https://github.com/airbnb/javascript

##### Books :
* http://exploringjs.com/es6/
* https://github.com/getify/You-Dont-Know-JS
* http://netcraft.co.il/fedia/books/SecretsoftheJavaScriptNinja.pdf
