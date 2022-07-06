## Shallow Copy vs Deep Copy

Let's compare shallow copy vs deep copy. Here is the definition of each:<br/>
**Shallow copy** happens when a reference variable(i.e object or array) gets copied to another variable via the assignment operator '='. <br/>
**Deep copy ** takes all the members of the old variable, saves them on separate memory locations, and then assigns them to the new variable. <br/>
Let's dive into them

 ## Primitive vs Reference type 
**Primitive** or  **Reference** are the two ways by which a javascript variable stores value. Understanding their workings is very important before we get into the main topic.<br/>
**Primitive** type values are those which are not an object and have no methods. It means they do not have any properties. Once you assign a primitive type to a variable it's completely independent and holds no reference to where it's coming from.<br/>
Take a look at the following code
```javascript
let a = 10;
let b = a;
a = 20;
console.log(a); //output 20
console.log(b); //output 10
```
You can clearly see that the value of `b` holds no reference to the value of `a` and remains the same even though we set the `a` to a new value.<br/>
**Primitive type values** include string, number, bigint, boolean, undefined, symbol, and null.
On the other hand,  **Reference** type values contain references to the value stored at the specific location in memory. They have certain properties associated with them which are identified by the `key` values.<br/> So in reality when we assign reference type to a variable then we are just assigning references to the values stored in a location. If the values at these stored locations change then all the variables holding the references will also get new values. <br/>
Take a look at the code below
```javascript
let a = { value: 10 };
let b = a;
a.value = 20;
console.log(a); //output { value: 20 }
console.log(b); //output { value: 20 }
```
You can clearly see the variable `b` getting updated once we update the value using `a.value`<br/>
**Reference type values** include objects, functions, and arrays.<br/>
## Shallow Copy
Consider the following example
```javascript
let a = { x: 10 };
let b = a;
b.x = 50;
console.log(a); //output { x: 50 }
console.log(b); //output { x: 50 }
```
Why when we change the value of `x` in the copied object `b` the `x` in the original object `a` also gets updated.<br/>
Since both variables share the reference to the values in the same memory location and if the value is changed via one variable then all the variables will output the updated value. This is called **Shallow Copying**. In shallow copy, the new and the original variable share the reference to the same values stored in memory.
## Deep Copy
In a deep copy, the original and new variables are completely disconnected and hold no reference together at all.
Consider  the following example,
```javascript
let a = { x: 10 };
let b = { ...a, y: 20 };
b.x = 50;
console.log(a); //output { x: 10 }
console.log(b); //output { x: 50, y: 20 }
```
You can clearly see that changing the value from the variable `b` has no effect on the original variable `a`.
## Nested Objects
Things get a little tricky when we have nested objects.<br/>
Take a look at the following example
```javascript
let a = {
  name: "umar",
  location: {
    city: "lahore",
  },
};
let b = { ...a };
b.name = "asif"; //disconnected
b.location.city = "quetta";//connected

console.log(a); //output { name: 'umar', location: { city: 'quetta' } }
console.log(b); //output{ name: 'asif', location: { city: 'quetta' } }
```
You can clearly see that the key `b` is completely disconnected from the original variable `a` but the nested object `location` is connected to the original variable and when we change it from variable `b` the variable `a` also outputs the changed value.<br/>
The reason is that when you deep copy an object then the main object disconnects its reference from the original object but the nested object keeps that reference. You can say that the nested object gets **shallow copied**.<br/>
If you truly want to deep copy an object then with all of the nested objects inside of it then you can use the `JSON.stringify` and `JASON.parse` methods as shown below
```javascript
let a = {
  name: "umar",
  location: {
    city: "lahore",
  },
};
let b = JSON.parse(JSON.stringify(a));
b.name = "asif"; //disconnected
b.location.city = "quetta"; //disconnected

console.log(a); //output { name: 'umar', location: { city: 'lahore' } }
console.log(b); //output{ name: 'asif', location: { city: 'quetta' } }

```
 That's the basics of how copying works in javascript. I hope this article helped you better understand the concepts of the shallow and deep copy in javascript.





