

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





















