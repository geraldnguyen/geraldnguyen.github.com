---
title: Five Minimum Viable Javascript Interview Questions
subtitle: "Interviewer: ask them if your interview is running out of time. Interviewee: practice them if your preparation is running out of time."
date: 2022-12-09T09:19:42+01:00
image: 0_ufACmK4armozbXnU.jpg
draft: false
categories: [Software Development, Career Development]
tags: [JavaScript, Interview, Practical]
---

{{< include "posts\2023\02\Five Minimum Viable Interview Questions series\series.include" >}}

It is common nowadays for companies to hire for full-stack positions. A full-stack developer is typically expected to work on both frontend and backend development. That same expectation applies to interviews.

It is thus tough to interview or be interviewed for a full-stack position. There would be too many questions to ask or to prepare for. But if you are running out of time, here are five questions I think you should know.

{{< figure src="0_ufACmK4armozbXnU.jpg" caption="Photo by [Christina @ wocintechchat.com](https://unsplash.com/@wocintechchat?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)" >}}

# #1 — What are the differences between `==` and `===` operators?

> Good Javascript practices discourage the use of `==` operator to reduce bugs due to type conversion. Modern linting tools typically flag any uses of `==` . Thus this question tests the candidate practical knowledge and experience with Javascript

Both operators compare their two operands and return a Boolean result

The strict equality operator `===` requires that its operands are of the same type and return `false` otherwise, while the `==` operator may attempt type conversion before comparing the resulting values.

The `===` considers `null` and `undefined` different while `==` considers the same

Sample codes:

```javascript
/* Equality */  
1 == "1"           // true  
1 == "00001"       // true  
1 == "  00001   "  // true  
null == undefined  // true  
  
/* Strict equality */  
1 === "1"          // false  
null == undefined  // false 
```

References:

*   [https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Strict_equality](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Strict_equality)
*   [https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Equality](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Equality)

# #2 — When do you use `const`, `let` and `var`?

> This question not only tests the candidate’s familiarity with newer constructs but also validate whether the candidate distinguishes a constant versus a normal variable.

`var` is the oldest way to declare a variable in Javascript. A variable declared with `var` within a function has that function’s scope. If declared outside any function, it has global scope. Variables declared using `var` are created _before_ any code is executed in a process known as hoisting with initial values set to `undefined`. Redeclaring a variable using `var` will not trigger an error or reset its value unless another assignment is performed

```javascript
'use strict';  
function foo() {  
  function bar() {  
    var y = 2;  
    console.log(x); // undefined (x is hoisted, carrying "undefined")  
    console.log(y); // 2 (`y` is in scope)  
  }  
  bar();  
  var x = 1;        // x is initialized  
  console.log(x);   // 1 (`x` is in scope)  
  console.log(y);   // ReferenceError in strict mode, `y` is scoped to `bar`  
}  
  
foo();
```

`let` is the newer and preferred way of declaring variables. A variable declared with `let` has block-scoped (visible to the same block and any nested scopes) and is only accessible after its declaration is _executed_. Redeclaring variables with `let` within the same block or function results in a syntax error

```javascript
/* block scope variable */  
let x = 1;  
if (x === 1) {  
  let x = 2;       // OK, redeclare in a different block  
  
  console.log(x);  // expected output: 2  
}  
console.log(x);   // expected output: 1  
let x = 10;        // syntax error  
  
  
/* Related concept: Temporal Dead Zone (TDZ) */  
{  
  // TDZ starts at beginning of scope  
  const func = () => console.log(letVar); // OK  
  
  // Within the TDZ letVar access throws `ReferenceError` : Cannot access 'letVar' before initialization
  
  let letVar = 3; // End of TDZ (for letVar)  
  func();         // Called outside TDZ!  
}
```

`const`, introduced after `var` and before `let`, is used to declare a constant whose value cannot be changed. Hence`const` declaration requires an initializer to assign a constant value to the variable. The variable’s scope can be global or block-scoped. Redeclare `const` variable results in a syntax error. A common convention is to use UPPERCASE for constant variable

```javascript

const MY_CONSTANT = 111;  
  
/** Errors:  
MY_CONSTANT= 20;   // TypeError: Assignment to constant variable.  
const MY_CONSTANT= 20;  
let MY_CONSTANT= 20;  
var MY_CONSTANT= 20;  
*/  
  
if (true) {  
  let MY_CONSTANT = 20; // OK, different block scope  
  console.log("my favorite number is " + MY_CONSTANT);  // 20  
}  
  
console.log("my favorite number is " + MY_CONSTANT);    // 111
```

References:

*   [https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/const](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/const)
*   [https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/let](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/let)
*   [https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/var](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/var)

# #3 —How do you create a new object?

> Despite being not an object-oriented language, object frequently featured in Javascript codes. Hence knowing how to create object with properties and methods is a must.

The easiest way to create an object in Javascript is by using object initializer syntax: a comma-delimited list of zero or more pairs of property names and associated values of an object, enclosed in curly braces `{}`.

```javascript
const emptyObject = {}  
  
const objectWithProperty = {   
   a: 1,   
   b: 'b',   
   c: true,   
   d: { nested: 'property'}   
}  
  
const a = 1, b = 'b', c = true, d = { nested: 'property'}  
const anotherObjectWithProperty = { a, b, c, d }  
  
// Computed property names  
const prop = 'foo';  
const computedObjectProperty = {  
  [prop]: 'hey',  
  ['b' + 'ar']: 'there',  
};
```

`new Object()` or `Object.create()` can also be used to create a new object

```javascript
const newObject = new Object();  
newObject.a = 1;  
newObject.b = 'b'  
  
const anotherObject = Object.create(null);
```

References:

*   [https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object)
*   [https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Object_initializer](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Object_initializer)

# #4 — How do you create a new 5x5 Square object with a method that computes its area?

> This is similar to question #3 where we want to test the candidate’s ability to create object. In addition, it also tests candidate’s knowledge of `this` and different ways to declare object’s function

There are 2 valid ways of declaring a method shown below in versions 1 and 2.

The 3rd version is a trap because of the way arrow function captures the `this` of the enclosing context. Binding the arrow function to the object using `.call` or `.bind` does not work.

```javascript
// version 1  
const square1 = {   
  side: 5,  
  area: function () {  
    return this.side * this.side;  
  }  
}  
square1.area();  // 25  
  
// version 2  
const square2 = {   
  side: 5,  
  area() {  
    return this.side * this.side  
  }  
}  
square2.area();  // 25  
  
  
// version 3: warning: this with arrow function  
const square3 = {   
  side: 5,  
  area: () => this.side * this.side  
}  
square3.area();  // NaN - why?  
```

References:

*   [https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/this](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/this)
*   [https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Object_initializer](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Object_initializer)

# #5 — What do you use Promise (or Observable) for?

> Asynchronous execution is quite prevalent. It is rare to program a frontend without making some network call, or setting up a timeout or interval. Knowledge of Promise (or Observable if you are using Angular or RxJs) is essential.

Promise (or Observable) is a popular way of programming asynchronous Javascript. It allows a developer to write codes to:

*   Trigger an asynchronous execution
*   Then handle the result when it finishes successfully, or in error, or regardless.

Promise (or Observable) allows chaining of calls and handlers. This enables a powerful yet intuitive way of programming multiple asynchronous executions and handling their results.

```javascript
// Promise  
const hello = fetch('https://echook.azurewebsites.net/echo')  
  .then(resp => resp.text())  
  .then(text => JSON.parse(text))   // or  resp.json() instead of text()  
  .then(console.log)  
  
// Angular's Observable  
this.httpClient.get<string\>('https://echook.azurewebsites.net/echo')  
   .subscribe(resp => {  
      console.log(resp.body)  
    });  
}
```

References:

*   [https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise)
*   [https://angular.io/guide/observables-in-angular](https://angular.io/guide/observables-in-angular)