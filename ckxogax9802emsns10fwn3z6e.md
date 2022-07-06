## Hoisting in JavaScript

Hoisting is one of the most frequently asked javascript interview questions. The concepts around the hoisting are somewhat tricky and give candidates a hard time in their interviews. In the post, I will step by step explain these concepts with examples to get a better and simple understanding of hoisting in javascript.

## Declaration, Assignment, and Initialization
**Declaration** makes a variable of a given name available in its scope. <br/>
The ***Assignment*** is giving the value to the variable at a specific location in your code.

The ***Initialization*** is something that happens on the run time and gives a variable its first value. <br/>
Take a look at the code snippet


```javascript
const myVar='Hello world';
``` 
The variable is declared by the name of `myVar` in its scope and is assigned with the value of `Hello world`.<br/>
One thing to remember is that all variables are assigned with the value of `undefined` during initialization. When the javascript engine reaches the line in code on which it is given a value then they are assigned with that value.<br/> 
The **weird behavior of javascript** is that the javascript interpreter splits the variable declaration and its assignment. During run time, it takes the declaration of that variable to the top of the scope and assigns it with the `undefined` value. <br/>

Take a look at the following example,
```
console.log(myVar);
var myVar = "Hello world";
``` 
The output of the log statement will be `undefined`. This is because of the javascript splitting the declaration and assignment. It takes the declaration to the top of the scope and initializes it with the `undefined` value. Then the interpreter reaches the line of the `console.log` statement and checks the value of `myVar` which is undefined at that moment, so it prints `undefined`. At the second line, it assigns the variable with the value of `Hello world`.<br/>
Take a look at the following code

```
console.log(myVar);
var myVar = "Hello world";
console.log(myVar);
```
The first `log` will print `undefined` and the other will print `Hello world` due to the above-explained behavior.
## Hoisting by Definition
In javascript, a variable can be used before its declaration. By definition, the hoisting is moving the variable declaration to the top of the scope. It means that if a variable is declared at the bottom of a block, like at the last line of `{}` then the javascript engine will move its declaration to the first line the `{}`. Hoisting behaves a bit differently for variables and functions and we will go through all these concepts one by one. 


## Variable Hoisting 
Variables declared with the keyword `var` have different behavior than the ones with the `let` and `const` keywords.
<br/>
### `var` Hoisting
The javascript assigns `undefined` to variables declared with the `var` keyword during initialization. This means that you can access this variable with the value `undefined` before it gets assigned a value in the code.  
Take a look at the code below,
```javascript
console.log(myVar);
var myVar;
```
The output will be `undefined`.
### `let` and `const` Hoisting
The variable declared with `let` and `const` gets the declarations moved to the top of the scope but they don't get initialized with the default `undefined` value. <br/>

Take look at the following code
```javascript
console.log(myVar);
let myVar;
```
This will throw a **ReferenceError** error that the variable cannot be accessed before the initialization. The `let` and `const` keywords were introduced in ECMA 2015 to eliminate the bugs created by the hoisting of the variables declared with the `var` keyword
<br/>
```
let myVar = "Hello world";
console.log(myVar);
```
This will `log` the `Hello world`
## Function Hoisting
### Function Declarations
Functions declarations are hoisted and can be accessed before their initialization. This greatly helps hide the function implementations and clean our code. You can access them anywhere in your code, before or after the declaration, due to their hoisting.

```
a()//output Hello world
function a(){
  console.log('Hello world');
}
```
### Function expressions
Function expressions are anonymous functions the are created without the use of the function name (i.e declared as variables). The function expressions do not have hoisting in the javascript which means that you cannot access them before their declaration.
The following code will throw a **Reference error**

```
a();
let a = () => {
  console.log("Hello world");
};
```
That's it for the basics of hoisting in javascript. It's not a complicated concept just requires the breaking down of a few steps and understanding them individually.








