## This includes 

* Environment Set
* Variable Declaration
* Variable Types
* Functions
* Interface
* Class
* Access Modifiers


## TYpescript

Open source Programming lang from Microsoft. Types Superset of Javascript, Complies down to Javascript.


Why Ts?

* Relation to JS
* Optional static and type inference
* IDE Support(IDE Intellisense)


## Environemnt

Install typescript using `npm install typescript -g`

Now you can use `tsc -v ` to check the version

`ts` is the extension for typescript file.

Next step is to compile this `.ts`  file to `.js` file using `tsc main.ts or simply tsc main` and 
it will compile to `main.js` file and thats what you include in your `.html file or .js file`.

**NOTE** if you see error in your `.ts file` then you should make taht file as `module` by default that will be consderd as`script` and script has global scope but `module` has scope only in that particular module.
So, for time being you can export an empty curly braces like 
```
//main.ts

export {}
let msg="hello";

console.log(msg);

```
This will compile to 
```
//main.js

"use strict"
exports.__esModule=true;
var msg="Hello";
console.log(msg);

```

## Auto compile `.ts` file to `.js` file
tsc --watch main.ts

## Run Js file from terminal itself
if you have compiled your `main.ts` to `main.js` then you can run your main.js file like below.
`node main`
and thats it.

## Variable Declaration

let x;
let y=10;
const a=20;

## Variables type
```
let isApple:boolean=true; //declaration + initialization
let total:number; //only declration
let name:string='Amit'

let sentense:string=`My name is ${name}
I am a beginner in typescript
`


console.log(sentense);
```

Unlike Js type won't allow to assign some string to some boolean variable.

`null` and `undefined ` are subtyes of all types. so

```
isApple:boolean=null;
```

### Array Declaration

There are two ways to declare
```

//Arrays are homogenious in nature
let list:number[]=[1,2,3];
let list2:Array<number>=[1,2,3];

//Both is same none has advantage over other

//to handle hetrogenious data, typescript introdes tuples

```

### Tuple

Typescript introduced `tuple` as compared to `Array` to handle hetrogenious datas.

`let person:[number,string]=[121,'apple']`

Now both Array and tuple are fixed length  data type.

Below are the rules for tuple

* For above person tuple, typle type and tuple value must be in same order.
* say if you declared only 2 data types like numeric,string then you can only have two values
with string and numeric inexactly same order

### Enum datatype
```
enum Color {
Red,Green,Yellow
}


let c:Color=Color.red;
```

### `Any` data type

as name say, `any` accepts any types

```
let myvar:any=10;

myvar="abc";

//all is valid since any accepts a y datatypes

//now you call call myVar as variable or even as function cause tsc can not infer the proper type

//console.log(myVar.name);
//myVar();
//myVar.toUppercase()
// if you see tsc does not throw any error on above statement, which is not valid
// to takle this issue, tsc 3.0 introduce another type called unknown
//but if you will try to call say toUppercase() on myVar you will get error, for that to work you  need to type cast it, as below

(myVar as string).toUppercase();

```

### Unknown type
if you see tsc does not throw any error on above statement, which is not valid
to takle this issue, tsc 3.0 introduce another type called `unknown`

what this `unknown` type says is u can declare n initialize a variable with type `unknown`
but you can not call any function on that, or  you can not call that variable as function etc, if you do u will get error right away.

to make that work, you will have to use `type assertion` or also called `type casting` to convince the type system
that your are `corect and initentianlly` doing that.

```
let myvar:unknown=10;

myvar="abc";


//console.log(myVar.name);
//myVar();
//myVar.toUppercase()
// 
//but if you will try to call say toUppercase() on myVar you will get error, for that to work you  need to type cast it, as below

(myVar as string).toUppercase();

```

## Type inference

if you have just declared a variable without giving any type, in that case you assign multiple type of values 
to the variable and that will work just fine like plane JS.
```
let a;
a=20;//fine no error
a=true;//still fine

```

But if you assign any value to the variable at time of declaration, even if you did not provide the type
Tsc will automatically infer the type based on value you assigned and if try to assigned something else 
tsc will throw error.

```
let a=20;
a=true;// you will get warning saying true can not be assigned to number

```

How tsc knows that, tsc can infer type from the value you assigned to variable. Type inferecne only works if you assign value during declaration only.

## union of types for a variable
```
let a:number|boolean;
a=20;//fine
a=true;//fine
a="amit";//error

```

mostly u will use this approach when you do not have controll over the variable, such as 
variable from some third part api.


## functions




