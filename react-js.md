(https://www.youtube.com/watch?v=Ke90Tje7VS0)[Watch]



# Basics of ReactJs
```
reander(){
return  (
<div>
<h1>Hellp</h1>

<p>Its a paragraph</p>
</div>

);
}
```

So you have, render function like this, bcoz you can have only one root element in render function, so to overcome that you added one extra wrapper
div, so you did like above.

But say you don't to put that extra `div` at  all, `then use React.Fragment`. like below

```
import Ract from "react";
import ReactDom from "react-dom";

reander(){
return  (
<React.Fragment>
<h1>Hellp</h1>

<p>Its a paragraph</p>
</React.Fragment>

);
}
```

And when its compile you wont have that extra element issue.

## Adding attribute


```
import Ract,{Component} from "react";
import ReactDom from "react-dom";

class Counter extends Component{


state={
counter:0,
imageUrl:'someUrl'
}

render(){
return  (
<div>
<h1>Hellp</h1>
<img src={imageurl}/>
</div>

);
}//render ends



}//class ends
```

## add class
Since React uses JSX, which is nothing but a kind of javascript, which reacts transpiles to real JS using babel.

so if you use `class` t refer to html/css class it wont work coz `class` is a reserved keyword in JS to make it wwork you should use `className` to refer to `class` attribute of an html eleme tlike below.



```
import Ract,{Component} from "react";
import ReactDom from "react-dom";

class Counter extends Component{


state={
counter:0,
imageUrl:'someUrl'
}

render(){
return  (
<div>
<h1 className="badge badge-primary">Hellp</h1>
<img src={imageurl}/>
</div>

);
}//render ends

}//class ends
```

## style attribute

to make use of style attribute of a an HTML element, you will have write it as JS oject whose properties are css property in `camelCase` i.e `fontSize` not `FontSize` okay?

```

//you calso seperately defines the style object like this


Mystyles={
fontWeight:'bold'
}


render(){
return  (
<div>
<h1 style={fontSize:'10px'} className="badge badge-primary">Hellp</h1>
<img src={imageurl}/>

<p style={this.MyStyles}>some content</p> //see the style attribute here
</div>

);
}//render ends

```


## Rendering classes dynamically

```


render(){

myClasses="badge m-2";
myclasses+=this.state.counter===0?'badge-warning':'badge-primary';


return  (
      <div>
      <h1 className={myClasses}>Hellp</h1>
     
      </div>
);
}//render ends

```


## using loops in React

you might have seen `v-for` or `*ng-for` in angular because they support templating.

But react does not support that, you will have to use JSX i.e JS array methods to do so, like this.


```

state={
counter:0,
tags:['tag1','tag2','tag3']
}

render(){

myClasses="badge m-2";
myclasses+=this.state.counter===0?'badge-warning':'badge-primary';

return  (
      <div>
      <h1 className={myClasses}>{counter}</h1>
     
     <ul>
     //now here you need to use JS array concept, for array you need to provide //key as well, that key must be unique for that particular list, it does not have //to be unique in that app or class.
     {this.state.tags.map(tag=> <li key={tag}>{tag}</li.)}
     
      </ul>
      </div>
);
}//render ends

```

using if else, there is no if else in react, you will have to go JS way.
Say if there is no tag, you want to render some text but list otherwise.




```

state={
counter:0,
tags:['tag1','tag2','tag3']
}

renderTags(){
if(this.state.tags.length===0)
return <p>There are no tags</p>

return  {this.state.tags.map(tag=> <li key={tag}>{tag}</li.)};
}

render(){

return  (
      <div>
      <h1 className={myClasses}>{counter}</h1>
     
     <ul>
     //now here you need to use JS array concept, for array you need to provide //key as well, that key must be unique for that particular list, it does not have //to be unique in that app or class.
    {this.rebderTags()}
      </ul>
      </div>
);
}//render ends

```

## Events
 React supports all Standard DOM events but only exception is `Events name are case sensitive and camelCase`
 
 so in react there is `onClick` not `onclick`. cause there is no `onclick` attribute/element in reactJs.
 
 
 
```

state={
counter:0,

}

handleClick(){
console.log("you clicked"+this.state);
}

render(){

return  (
      <div>
      <h1 className={myClasses}>{counter}</h1>
     <button onClick={this.handleClick} classname='btn btn-primary">          Increment
     </button>
      </div>
);
}//render ends

```


But if you run above code you will get error, saying `can not call state of undefined`. Meaning function `handleClick` has no access to `this` object but why?

lets udderstand that.

if you call `obj.something;` then this refers to `obj` Object and when you call `myMethodsLikeThings()` then `this` refers to global `window` object.

but since we are using `ES6` that we are making use `"use strict"` directive,  so global object is undefined here. Hence the error.



to resolve this issue there are few approaces like making use `bind()` function on  function object (Function in JS Object, so you can call function on them) in your componenent constructor. Another approach is to use `arrow function` where `this` is automatically bind to class.

__Approach one__

```
class Counter extends Component{

constructor(){
//you must call super() first

super();
this.handeClick=this.handleClick.bind(this); //see this line, now in handleClick function `this` will refer to class `this` object instead of undefined.
}
state={
counter:0,
}

handleClick(){
console.log(this.state.counter);
}

render(){

return  (
      <div>
      <h1 className={myClasses}>{counter}</h1>
     <button onClick={this.handleClick} classname='btn btn-primary">          Increment
     </button>
      </div>
);
}//render ends

}//class ends here
```

__Approach Two__


```
class Counter extends Component{
//constructor and bind() etec are removed now.

state={
counter:0,
}

handleClick=()=>{
console.log(this.state.counter);
}

render(){

return  (
      <div>
      <h1 className={myClasses}>{counter}</h1>
     <button onClick={this.handleClick} classname='btn btn-primary">          Increment
     </button>
      </div>
);
}//render ends

}//class ends here
```

## Update the component state

if you do this like `this.state.counter=1` it wont work, to make it work you will have to explicitly tell react, using one of the inherited function from Component class.


```
handleClick=()=>{

this.setState({count:this.state.counter++}); //now this will work

}
```

## passing argument to event handler function

There are two approaches:

using a wrapper  function or using using an inline arrow function.

__Approach one__


```
MyHanldewrapper(){
this.handleClick({id:1});
}

handleClick=(product)=>{
this.setState({count:this.state.counter++}); //now this will work
}

render(){

return  (
      <div>
      <h1 className={myClasses}>{counter}</h1>
     <button onClick={this.MyHanldewrapper} classname='btn btn-primary"
     //see the MyHanldewrapper onstead
     Increment
     </button>
      </div>
);
}//render ends

```

__approach two__

```


handleClick=(product)=>{
this.setState({count:this.state.counter++}); //now this will work
}

render(){

return  (
      <div>
      <h1 className={myClasses}>{counter}</h1>
     <button onClick={()=>this.handleClick({id:1})} classname='btn btn-primary"
     //see the inline arrow function here
     Increment
     </button>
      </div>
);
}//render ends

```


## Composing react components  to build UI

* Pass data to component
* Raise and Handle Events
* Multiple Components in Sync
* Functional Components
* Lifecycle Hooks



## Pass data to component

__Every React componenet has propert called `props` which has access to all the userdefined props passed to that component__

```
//counter.jsx
import React,{Component} from "react";


class Counter extends Component{

state={
value:this.props.value, //see the this.props here
}

clickhandler=()=>{
this.setState({value:this.state.value++});
}
render(){

return  (
      <div>
      <span>{this.state.value}</span>
      <button onClick={()=>this.clickhandler} className="btn btn-primary">
     Increment
     </button>
      </div>
);
}//render ends

}//class ends here

export default Counter;
```


```
//counters.jsx
import React,{Component} from "react";
import Counter "./Counter";


class Counters extends Component{

state={
counters:[
      {id:1,value:4},
      {id:2,value:0},
      {id:3,value:1},
      {id:4,value:3},
      ],
}

render(){

return  (
      <div>
      {
      this.state.counters.map(counter=>(
      <counter key={counter.id} value={counter.value} selected="true" />
      ));
      }
      </div>
);
}//render ends

}//class ends here

export default Counters;
```

__So far you passed data to other componenet as attribute, but if you want to pass something between opening and closing tags, ReactJs `props` object receives a special attribute called `children` of type `ReactElement`._ So use it if need it_

## React developer tools like Vue Dev tool

## state vs Props

In react A component has both `state` and `props`

* state refers to `private and local` copy of data only visible inside that component.

* props refers to data paased to a component from another component.

* state can be modied using setSate but props are readOnly



## Raise and Handle Events

When you need to pass some data from children to its parent, we raise an event for that. When we need to pass data from parent to children  we pass that as props.

In React pattern is:

_The component that __owns__ a piece of the state, should be the one __modifying__ it._


```
//counter.jsx
import React,{Component} from "react";


class Counter extends Component{

state={
value:this.props.value, //see the this.props here
}

clickhandler=()=>{
this.setState({value:this.state.value++});
}
render(){

return  (
      <div>
      <span>{this.state.value}</span>
      <button onClick={()=>this.clickhandler} className="btn btn-primary">
     Increment
     </button>
         <button onDelete={()=>this.props.onDelete(this.props.id)} className="btn btn-primary">
     Delete
     </button>
      </div>
);
}//render ends

}//class ends here

export default Counter;
```


```
//counters.jsx
import React,{Component} from "react";
import Counter "./Counter";


class Counters extends Component{

state={
counters:[
      {id:1,value:4},
      {id:2,value:0},
      {id:3,value:1},
      {id:4,value:3},
      ],
}

handleDelete(counterId){
let counters=this.state.counters;

counters.filter(counter=>counter.id !== counterId);
this.setState({counters});


}
render(){

return  (
      <div>
      {
      this.state.counters.map(counter=>(
      <counter onDelete={handleDelete} key={counter.id} id={counter.id} value={counter.value} selected="true" />
      ));
      }
      </div>
);
}//render ends

}//class ends here

export default Counters;
```


## Single source of truth

So, event handling is working perfectly fine but there is big issue.

if you see `counter.jsx` and `counters.jsx` , `counter.jsx component` has their own loacl state and `counters.jsx` as well.

__Here, since `Counters` component is handling `conter` component, so `Counter` component is known called as `Controlled Component` and it react, a Controlled component should not have its own state__

Since `Counter` component is having its own state, so when you add say __Reset Button__ in `Counters component` it will update the state of `Counters component` but that changes you wont see in DOM i.e on UI.

Why, Because __state on controlled Components__ are initialized only once at the time component creation, thats the reason, even if your state in Counters component is update, its not being updated on the UI i.e DOM.


To handle this issue you __should not have__ any __state__ in `Counter component` and hence the __methods__. Controlled component should only work on data provided by its parent.

So finally working refactored code is below.

NOTE: note for easy dev, instead of passing individual properties to child , you should always pass the object itself. This way even if you add attributes to the object you need to change only in place(Controlled component) not in parent component.



```
//counter.jsx
import React,{Component} from "react";


class Counter extends Component{

render(){

return  (
      <div>
      <span>{this.props.counters.value}</span>
      <button onIncrement={()=>this.props.onIncrement(this.props.counters)} className="btn btn-primary">
     Increment
     </button>
         <button onDelete={()=>this.props.onDelete(this.props.counters.id)} className="btn btn-primary">
     Delete
     </button>
      </div>
);
}//render ends

}//class ends here

export default Counter;
```


```
//counters.jsx
import React,{Component} from "react";
import Counter "./Counter";


class Counters extends Component{

state={
counters:[
      {id:1,value:4},
      {id:2,value:0},
      {id:3,value:1},
      {id:4,value:3},
      ],
}

handleDelete=(counterId)=>{
let counters=this.state.counters;
counters.filter(counter=>counter.id !== counterId);
this.setState({counters});


}

handleIncrement=(counter)=>{
//best practice is to never modify the state directly

const counters=[...this.state.counters];
const index=counters.indexOf(counter);
counters[index]={...counter};//now counter[index] is different so state wont be modified directly
counters[index].value++;
this.setState({counters})

}


handleReset=()=>{

}
render(){

return  (
      <div>
      <button className="btn btn-primary" onClick={()=>this.handleReset}>Reset</button>
      {
      this.state.counters.map(counter=>(
      <counter onDelete={handleDelete} onIncrement={this.handleIncrement} key={counter.id} counters={this.state.counter}  />
      ));
      }
      </div>
);
}//render ends

}//class ends here

export default Counters;
```

## Lifecycle Hooks

react component goes through certain lifecycle hooks.

* Mount
This is the first lifecycle hooks, where __components are created__ and __inserted into the DOM__.

There are few special methods which we can add to our component and that will get called during this lifecycle.

In __Mounting Phase__ there are three lifecycle hooks:

            ** constructor
            ** render
            ** componentDidMount

React will  call these functions in order

* Update
This __phase__ get executed when `state` of the component get changed/updated.

There are two lifecycle hooks:

            ** render
            ** componentDidUpdate

* UnMount
This phase you understand by name

There is onlone hooks;

            **componentWillUnmount
            
There are few more lifecycle hooks, but there are used rarely.


### constructor

```
class Counter extends Component{

constructor(){
//first call the super
super();
//Its is usally used to set initial component state or props

}

//otherexample with props

constructor(props){
//first call the super
super(props);
//Its is usally used to set initial component state or props
log(this.props);
}
...
}
```
* After setting the state in constructor you can not call `setState({}); becoz this function is called only after the `render()` function is called.

* you do not have direct access to `props` you need to pass that as an argument to the constructor and so to the `super` and access it using  __`this.props`__

### render()
it renders the component


### componentDidMount()

This is the best place to make all your `ajax` request to your server


```
class Counter extends Component{

constructor(){
//first call the super
super();
//Its is usally used to set initial component state or props

}

//otherexample with props

constructor(props){
//first call the super
super(props);
//Its is usally used to set initial component state or props
log(this.props);
}


componentDidMount(){
//do some ajax call
}
...
}
```













