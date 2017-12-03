# Part 1 JavaScript Quick Start

## Basic JavaScript

### Syntax

- = 	A single equal sign is used to assign a value to a variable.
- === three equals sign is used to compare two values

###Statements Versus Expression

- Statement "do things"  A program is a sequence of statement .

  `var foo;`  declares a variable foo;


- Expression produce values. They are function arguments, the right side of an assignment. etc.

  `3 * 7`

### Semicolons

- Semicolons are optional in JavaScript. 

- Semicolons  terminate statements.

  `var fun = function() {};` // function expr. inside var decl.

### Comment

- ```javascript
  ++x // single line comment
  ```

- ```javascript
  /*  This is

     a  multiline

     comment.

  */

  ```

  ​

### Variables and Assignment

#### Assignment

```javascript
var foo;  //declare a variable `foo`
foo = 4;  //change variable `foo`
```

#### Compound Assingment Operator

`+=   -=`

#### Identifiers and Variable Names

- Roughly  the first character of an identifier can be any Unicode letter. `$`  `_`  etc.

- Subsequence characters can additionally any Unicode digit

  ```javascript
  arg0  _tmp  $elem  //legal identifiers
  ```


- Reserved words

  ```javascript
  arguments	break	case		catch
  class		const	continue	debugger
  default		delete	do			else
  enum		export	extends		false
  finally		for		function	if
  implements	import	 in			instanceof
  interface	let		new			null
  package		private	protected	public
  return		static	super		switch
  this		throw	true		try
  typeof		var		void		while
  ```


- Infinity    NaN  undefined

### Values

- All values in JavaScript have properties. Each property has a key (or a name) and a value.

- Use the dot (.) operator to read a property

  ```javascript
  >value.propkey  // read a property
  >var str = 'abc';
  >str.length     
  >'abc'.length   //also be written as
  ```

#### Primitive Values Versus Objects

- Boolens, numbers, strings, null  and undefined    are primitive values.

- All other values are objects.

- A major difference between the two is how they are compared. each object has a unique identity and is only (strictly) equal to itself.

  ```javascript
  >var obj1 = {} ;
  >var obj2 = {} ;
  >obj1 === obj2 ;
  false
  ```

  ```
  >var prim1 = 123 ;
  >var prim2 = 123 ;
  >prim1 === prim2; 
  true
  ```

#### Primitive Values

- Booleans: true false

- Numbers 

- Strings

- Two "nonvalues" undefined null

  Primitives have following characteristics:


- Compared by Value  
- Always immuable 























