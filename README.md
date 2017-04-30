# ECMAScript Proposal: Public, Private, Static

## Introduction

This depends on a type system such as: https://github.com/sirisian/ecmascript-types and supports a https://github.com/sirisian/ecmascript-static-constructor as defined there.

This uses a public as the default for class members since specifying public isn't necessary and introduces a new keyword for little benefit.

## Rationale

See other languages that support public, private, and static syntax.

One difference is that static members and methods act as shared members of their class instances meaning they can be accessed from any instance variable. They can also be accessed from the class itself.

## Syntax

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
		return x;
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
		example.x = 5; // Without types it's impossible to know if x is private so this is allowed.
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

This brings up future features like friends and other concepts, but this is designed to be simple. It also wouldn't conflict with any proposed suggestions like that if they were added at a future time.
