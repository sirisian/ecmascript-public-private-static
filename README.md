# ECMAScript Proposal: Public, Private, Static

## Introduction

This uses a public as the default for class members since specifying public isn't necessary and would introduce a new keyword for little benefit.

## Rationale

See other languages that support public, private, and static syntax.

## Syntax

One important difference in this proposal is that static members and methods can be accessed from any instance of the class and also using "this" inside of the class. What this means is whether a variable, say 'x', is public, private, or static inside of the class you access it like this.x. The only case where that isn't the case would be attempting to access public and private variables in a static method since those members are per instance and thus don't exist in that context.

(There's some parts mentioning types. They refer to this proposal: https://github.com/sirisian/ecmascript-types and are mostly notes on behavior with a type system).

```js
class Example
{
	w; // public
	private x;
	static y; // Optional in this example since assigning this.y in the static constructor would define a static member y
	private static z;
  
	static constructor()
	{
		this.y = 0; // identical to this.constructor.y = 0;
		this.z = 0; // identical to this.constructor.z = 0;
	}
  
	constructor(w, x, y, z)
	{
		this.w = w;
		this.x = x;
		this.y = y; // identical to this.constructor.y = y;
		this.z = z; // identical to this.constructor.z = z;
	}
	
	private static Example1(y, z)
	{
		// this.w = 0; // error w is not static
		// this.x = 0; // error x is not static
		this.y = y;
		this.z = z;
	}
	
	static Example2(y, z)
	{
		Example1(y, z);
	}
	
	get X()
	{
		return this.x;
	}
	
	set X(x)
	{
		this.x = x;
	}
	
	private Example3()
	{
		return this.w + this.x + this.y + this.z;
	}

  	Example4()
	{
		return this.Example3();
	}
	
	Example5(example)
	{
		example.x = 5; // Without types is it possible to know if x is private?
	}
	
	Example6(example:Example)
	{
		example.x = 5; // With types this is the same class and in general languages allow this.
	}
	
	Example7(example:Example2)
	{
		example.x = 5; // error x is private
	}
}

class Example2
{
	private x;
}

Example.y = 0; // identical to Example.constructor.y = 0;
Example.z = 0; // identical to Example.constructor.z = 0;
// Example.Example1(0, 0); // error Example1 is private
Example.Example2(0, 0);

let example = new Example(0, 0, 0, 0);
example.w = 0;
example.x = 0;
example.y = 0;
// example.z = 0; // error z is private
// example.Example1(0, 0); // error Example1 is private
example.Example2(0, 0);
let x = example.X;
example.X = 0;
// example.Example3(); // error Example3 is private
example.Example4();
example.Example5(example);
example.Example6(example);
let example2 = new Example2();
example.Example5(example2);
example.Example6(example2);
// example.Example7(example2); // error x is private (in Example2)
```

This brings up future features like friends and other concepts, but this is designed to be simple.
