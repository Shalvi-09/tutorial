# Javascript

- [Javascript](#javascript)
  - [Part - 1](#part---1)
    - [Javascript in html](#javascript-in-html)
      - [Summary](#summary)
  - [&quot;use strict&quot; directive for modern mode](#quotuse-strictquot-directive-for-modern-mode)
        - [Example: &quot;use strict&quot; at script level](#example-quotuse-strictquot-at-script-level)
        - [Example: Incorrect use of &quot;Use strict&quot; at functiona level](#example-incorrect-use-of-quotuse-strictquot-at-functiona-level)
  - [Data types](#data-types)
    - [number](#number)
    - [bigint](#bigint)
    - [string](#string)
    - [boolean](#boolean)
    - [null](#null)
    - [undefined](#undefined)
    - [object and symbol](#object-and-symbol)
    - [typeof operator](#typeof-operator)
        - [Example:](#example)
  - [data type conversion and the Yellow magic](#data-type-conversion-and-the-yellow-magic)
    - [String conversion](#string-conversion)
    - [Number conversion](#number-conversion)
      - [Example](#example)
    - [boolean conversion](#boolean-conversion)
  - [Logical ||,&amp;&amp; and !](#logical-ampamp-and)
    - [short circuit evaluation](#short-circuit-evaluation)
      - [Example](#example-1)
    - [Using !! for converting type to boolean](#using--for-converting-type-to-boolean)

## Part - 1

### Javascript in html

> The benefit of a __separate file__ is that the __browser__ will download it and **store it in its cache**.
**Other pages** that reference the same script will **take it from the cache instead** of downloading it, so the file is actually downloaded only once.
That **reduces traffic and makes pages faster.**

> If **src** is set, the script content is ignored. A single `<script> tag` **can’t have both** the src attribute and code inside.

#### Summary

* We can use a `<script>` tag to add JavaScript code to a page.
  
* The type and language attributes are not required.
  
* A script in an external file can be inserted with` <script src="path/to/script.js"></script>`.

## `"use strict"` directive for modern mode

`"use strict"` `directive` tells the modern browsers to use and evaluates the javascript with Javascript syntax.

The directive looks like a string: `"use strict"` or `'use strict'`

*There’s no way to cancel use strict*
**There is no directive** like `"no use strict"` that reverts the engine to old behavior.

Once we enter strict mode, there’s no going back.
##### Example: `"use strict"` at script level
```
"use strict";

// this code works the modern way
...

```

##### Example: Incorrect use of `"Use strict"` at functiona level

```
alert("some code");
// "use strict" below is ignored--it must be at the top

"use strict";

// strict mode is not activated
```

## Data types

JavaScript has 8 data types namely `number`,`bigint`,`string`,`boolean`,`null`,`undefined`,`object`,and `symbol`

> Looks all data types names are in **lowercase**

A varibale in JavaScript is not bound to any specific data type, meaning a JS variable can store string value at time and number value another time.

That is JS is `dynamically typed` language and is not `statically typed` like languages like `java`,`C++` etc.

Example

```
"use strict";

let a="My Str value"; // type of variable `a` is String

a=123; // now type of variable a is number not string

```

### number

 The `number` data types stores both integer and floating point numbers.

 `number` data type has few special values apart from normal integer and floating values. These are

 **`Infinity`** , **`-Infinity`** and **`NaN`**

 **`Infinity`** and **`-Infinity`** represents the normal matematical values you know. 
 
 But **`Nan`** represnts a **computational error**.  It is a result of an incorrect or an undefined mathematical operation, for instance:
 ```
alert( "not a number" / 2 ); // NaN, such division is erroneous

 ```

 `NaN` is **sticky**. `*Any further operation on NaN returns NaN*`: So, if there’s a NaN somewhere in a mathematical expression, it propagates to the whole result.

 ```
alert( "not a number" / 2 + 5 ); // NaN
 ```
> **Mathematical operations are safe**
Doing maths is “safe” in JavaScript. We can do anything: divide by zero, treat non-numeric strings as numbers, etc.
The script will never stop with a fatal error (“die”). At worst, we’ll get NaN as the result.

### bigint

`bigint` represents number in range of **2<sup>53</sup>** and **-2<sup>53</sup>**

> **Compatability issues**
Right now BigInt is supported in Firefox and Chrome, but not in Safari/IE/Edge.

### string

as you know.

There are three types of quotes, you can use.

```
let str = "Hello";
let str2 = 'Single quotes are ok too';
let phrase = `can embed another ${str}`;
```

### boolean

have only two values `true` and `false`

### null

`null` in javascript represents **nothing** , **empty value** or **value unknown**

### undefined

`undefined` in JS represents **Value is not assigned**

```
let x;
alert(x); //undefined
```

Technically its possible to assign, `undefined` to any variable but its is **not recommended**. `undefined` is usally used to check if avariable is actually assigned.

To assign empty value or no value, you should use `null`

### object and symbol

Object represents complex data types, and `symbol` data type is used create **unique identifier** for object.

To create `symbol` use `Symbol` constroctor.

### typeof operator

There are 8 data types in JS, out of which Except `object` reset are called as primitive.

Now to check the data type of variable `typeof` operator can be used.

`typeof` operator is used in two form.

`typeof <variablename>`  and `typeof(<variablename>)`

You can use either of the form.

##### Example:

```
typeof undefined // "undefined"

typeof 0 // "number"

typeof 10n // "bigint"

typeof true // "boolean"

typeof "foo" // "string"

typeof Symbol("id") // "symbol"

typeof Math // "object"  (1)

typeof null // "object"  (2)

typeof alert // "function"  (3)
```

> **NOTE:** The result of typeof null is "object". That’s wrong. It is an officially recognized error in typeof, kept for compatibility. Of course, null is not an object. It is a special value with a separate type of its own. So, again, this is an error in the language.

## data type conversion and the Yellow magic

Most of the time, operators and functions automatically convert the values given to them to the right type.

For example, alert automatically converts any value to a string to show it. Mathematical operations convert values to numbers.

There are also cases when we need to explicitly convert a value to the expected type.


### String conversion

String conversion is mostly obvious. A `false` becomes `"false"`, `null` becomes `"null"`, etc.

### Number conversion

Numeric conversion happens in mathematical functions and expressions automatically.

For example, when division / is applied to non-numbers:

```
alert( "6" / "2" ); // 3, strings are converted to numbers
```

> We can use the Number(value) function to explicitly convert a value to a number:

```
let str = "123";
alert(typeof str); // string

let num = Number(str); // becomes a number 123

alert(typeof num); // number
```

> If the string is not a valid number, the result of such a conversion is NaN. For instance:

```
let age = Number("an arbitrary string instead of a number");

alert(age); // NaN, conversion failed
```

| Value	| Becomes… |
| ----- | :------- |
| undefined | 	NaN |
| null |	0       |
| true and false |	1 and 0 |
| string |	Whitespaces from the start and end are removed. If the remaining string is empty, the result is 0. Otherwise, the number is “read” from the string. An error gives NaN. |

#### Example

```
alert( Number("   123   ") ); // 123
alert( Number("123z") );      // NaN (error reading a number at "z")
alert( Number(true) );        // 1
alert( Number(false) );       // 0
```

### boolean conversion

* Values that are intuitively “empty”, like 0, an empty string, null, undefined, and NaN, become false.
* Other values become true.

## Logical  `||`,`&&` and `!` 

OR represented by `||`
AND represented by  `&&`
NOT represented by `!`

### short circuit evaluation

Operands can be not only values, but arbitrary expressions. OR evaluates and tests them from left to right. The evaluation stops when a truthy value is reached, and the value is returned. This process is called “a short-circuit evaluation” because it goes as short as possible from left to right.

This is clearly seen when the expression given as the second argument has a side effect like a variable assignment.

In the example below, x does not get assigned:

#### Example
```
let x;

true || (x = 1);

alert(x); // undefined, because (x = 1) not evaluated

```
If, instead, the first argument is false, || evaluates the second one, thus running the assignment:

```

let x;

false || (x = 1);

alert(x); // 1
```


> Precedence of AND && is higher than OR ||

### Using !! for converting type to boolean

A double NOT !! is sometimes used for converting a value to boolean type:

```
alert( !!"non-empty string" ); // true
alert( !!null ); // false
```

@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@


## fetch api

[watch](https://youtu.be/cuEtnrL9-H0)

you might have used `axios` library or `$http` from angular or something similar but do you know, there is an out of the box support making ap call/network call.

yup, guessed it right!, now you can use `fetch` api to achieve the same.


### syntax
```
fetch("<url>",{//this object is optional})

```

Now few important point to note is:

1. `fetch` api returns promise
2. if you are `then().catch()`, then you may expect that it will execute `catch(..)` for any kind http failure like `404` etc but in case of `fetch` api its true. It simple logic is _no matter happens, it is always going to succeed except some hard failure net network issue_
3. to determine success or failure of the call, you need to `ok` attribute of the response object return by `fecth` call.
4. if you want to make a `post`, `put` etc kind of request you need to pass those data as object in second argument
5. to get the response data yo you need to call `responseObject.json()` function which as `promise` in it self.


### Example

```

let p=fetch("https://reqres.in/api/users");
p.then(res=>{
// do your stuff
})

//format of res object is like
body:()
bodyUsed:false,
headers:{},
ok:true,
redirected:false,
status:200,
statusText:"",
type:"cors",
url:""

```


now to get the data, you need to call `res.json()` which will return another `promise`


```
let p=fetch("https://reqres.in/api/users");
p.then(res=>{
// do your stuff
res.json();
}).then(data => {
//Now you received the actual data
console.log(data);
});

```

Now to check for error and all , you can do like this

```
let p=fetch("https://reqres.in/api/users");
p.then(res=>{
// do your stuff

if(res.ok){
//request successful
//do something
res.json();
}
else{
//request failed
//do something
}

}).then(data => {
//Now you received the actual data
console.log(data);
});

```



## Promise
[watch](https://youtu.be/DHvZLI7Db8E)

`Promise` in JS is similar to real life promise r=or commitment which either success or fails. Real use case for promise is for any thing which will take longer time to finish and you are not sure, when this finish or fail.

Also Promise is asynchronous which means it is going to run in background and return something once it succed or fails.

`Promise` accepts one function, which in turn accepts two arguements **resolve** and other one is **reject**. as name suggests they work as per their name. so when your `promise` succed you will be `resolving` it but when it fails you will be `rejecting` it.

```
let p=new Promise((resolve,reject)=>{
// now here you do stuff
//once success you resolve it
//if it does not satisfy the condition you rejects it
});
```
if your promise is successfull and you resolve it then it go inside `then((....)=>{})` block if it was rejected in will go inside `reject((....)=>{})` block.

### Example
```

      let p=new Promise((resolve,reject)=>{
      let sum=1+1;

      if(sum === 2){
          //you can pass anything and any number of arguments to resolve(...)
           resolve("Success");
      }
      else{
          //you can pass anything and any number of arguments to reject(...)
          reject("Failed");
      }
});


//now you can use this promise to do your stuff
//for successfull then(res) block/method will be called 
// for unsuccessfull catch(err) method will be called
p.then(message=>{
console.log("Successful "+message);
}).catch((message)=>{
console.log("Successful "+message);
});

```

## async await

So you akready know how promise works and how you can create one. In last section you used `then(...).catch(...)` to handle the promise but there is a neater way to achieve the same without loosing the benefits of clean code and thats the `async and await`.

as you already know _if you resolve the query, it will call the then block and if it fails then it will catch() block_ in similar fashion, __if you resolve the promise, it will call the await statement and if it fails it go inside catch block of `try catch`__.

isn't it simple, 

### Example

```
function myP(){

      let p=new Promise((resolve,reject)=>{
      let sum=1+1;

      if(sum === 2){
          //you can pass anything and any number of arguments to resolve(...)
           resolve("Success");
      }
      else{
          //you can pass anything and any number of arguments to reject(...)
          reject("Failed");
      }
});

}

```
Now to handle this, you you can use async await like below:

1. create a function either named, anonymous or arrow function any use __async__ in function signature
2. use __await__ before calling the function

```

async myf(){

let d = await myP(); // notice the await keyword here
console.log(d)

}

//finally call this

myf();


//to handle the error use try-catch

async myf(){ //notice the async in the function signature
try{

let d = await myP(); // notice the await keyword here
console.log(d)

}
catch(err){
console.log("your promise was rejected");
}
}
//finally call it
myf();
```

