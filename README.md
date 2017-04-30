# ECMAScript Proposal: Public, Private, Static

## Introduction

This depends on a type system such as: https://github.com/sirisian/ecmascript-types and supports a https://github.com/sirisian/ecmascript-static-constructor as defined there.

This uses a public as the default for class member since specifying public isn't necessary and introduces a new keyword for little benefit.

## Rationale

See other languages that support public, private, and static syntax.

One small difference is that static members act as shared members of their class.

## Syntax

```js
class Example
{
	w; // public
	private x;
	static y; // Optional since using x in the static constructor would define this also in this example. One would normally define the type here though.
	private static z;
  
	static constructor()
	{
		this.y = 0; // identical to this.constructor.y = 0;
		this.z = 0; // identical to this.constructor.z = 0;
	}
  
	constructor(w, x, y, z)
	{
		this.v = w;
		this.w = x;
		this.y = y; // identical to this.constructor.y = y;
		this.z = z; // identical to this.constructor.z = z;
	}
	
	static Example1(y, z)
	{
		// this.w = 0; // w is public error
		// this.x = 0; // x is private error
		this.y = y;
		this.z = z;
	}
}

Example.y = 5; // identical to Example.constructor.y = 5;
Example.z = 5; // identical to Example.constructor.z = 5;

let example = new Example(1, 2, 3, 4, 5);
example.w = 10;
example.x = 10;
example.y = 10;
// example.z = 10; // z is private error
```

With types:

```js
class Example
{
	w:int32;
	private x:int32;
	static y:int32;
	private static z:int32;
  
	static constructor()
	{
		this.y = 0; // identical to this.constructor.y = 0;
		this.z = 0; // identical to this.constructor.z = 0;
	}
  
	constructor(w:int32, x:int32, y:int32, z:int32)
	{
		this.v = w;
		this.w = x;
		this.y = y; // identical to this.constructor.y = y;
		this.z = z; // identical to this.constructor.z = z;
	}
	
	static Example1(y:int32, z:int32)
	{
		// this.w = 0; // w is public error
		// this.x = 0; // x is private error
		this.y = y;
		this.z = z;
	}
}
```
