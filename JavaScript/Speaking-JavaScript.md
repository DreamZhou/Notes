# Part 1 JavaScript Quick Start

## 1. Basic JavaScript


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


## 17. Objects and Inheritance 

### Layer 1: Single Objects

- JavaScripts 的所有对象(objects) 都是 maps(dictionaries)  from strings to values.
- A (key, value) entry in an object is called a property.
- Methods are properties whoes values are functions

#### Kinds of Properties

- Properties (named data properties)
- Accessors (named accessor properties)
- Internal properties 

#### Object Literals

- `object literals` allow you to create `plain object`(direct instance of Object)

  ```javascript
  var jane = {
      name: 'Jane',
    describe: function(){
        retrun 'Person named ' + this.name;    //use this to refer to the current object
    },  				//
  };
  ```

#### Dot Operator (.): Accessing Properties via Fixed Keyss

- the property keys must be identifiers 

- arbitrary names  use the bracket operator ( [] )

- getting properties 

  ```javascript
  jane.name
  jane.describe
  jane.unknownProperty   // undefined
  ```

- calling methods

  ```javascript
  jane.describe()
  ```

- setting properties 

  ```javascript
  jane.name = John; // if a property doesn't exist yet ,setting it automatically creates it
  ```

#### Deleting properties 

- the `delete	`  operator 

  ```javascript
  var obj={hello: 'world'};
  delete obj.hello;	// true
  obj.hello;			// undefined
  ```

- set a property to undefined ,the property still exists 

  ```javascript
  var obj1 = {foo:'a',bar:'b'};
  obj1.foo= undefined;
  Object.keys(obj1);
  ```

- `delete`affect only the direct('"own", nonherited) properties of an object.

- the return value of `delete` : `fales` if the property is an own property ,but cannot be delete.

-  `true` othes

   ```javascript
   var obj2 ={};
   Object.defineProperty(obj2,'canBeDeleted',{
       value: 123,
     	configurable: true,
   });
   Object.defineProperty(obj2,'cannotBeDeleted',{
       value: 456,
     configurable: false,
   });
   console.log(delete obj2.cannotBeDeleted);	//false
   console.log(delete obj2.canBeDeleted); 		//true
   ```

####Unusual Property Keys

- cannot use reserved words (`var function`) as variable names. but can use them as property keys.

  ```javascript
  var obj3 = { var: 'a', function: 'b'};
  console.log(obj3.var);
  console.log(obj3.function);
  ```

  ​

- Numbers can be used as property keys in objects literals .but they are interpreted as string.assess  `[]` 

  ```javascript
  var obj4 = {0.7: 'abc'};
  console.log(Object.keys(obj4));
  console.log(obj4[0.7]);
  ```

- Arbitrary strings : must quete them need the bracket operator to access the property values.

  ```javascript
  var obj5 = {'not an identifier': 123};
  console.log(Object.keys(obj5));
  console.log(obj5['not an identifier']);
  ```

#### Bracket Operator ( [] ) : Accessing Properties via Computed Keys

- Getting properties via []

  ```java
  var obj6 = {someProperty: 'abc'};
  console.log(obj6['some' + 'Property']);
  var propKey = 'someProperty';
  console.log(obj6[propKey]);

  var obj7 = {'not an identifier': 123};
  console.log(obj7['not an identifier']);	

  var obj8 = {'6': 'bar'};
  console.log(obj8[3+3]);
  ```

- Calling methods via [] 

  ```javascript
  var obj9 ={myMethod: function(){return true;}};
  console.log(obj9['myMethod']());                           
  ```

- Setting properties via []

  ```javascript
  var obj10 = {};
  obj['anotherProperty'] = 'def'
  console.log(obj.anotherProperty);
  ```

- Delete properties via [] 

  ```javascript
  var obj11 = {'not an identifier': 123, prop: 2};
  console.log(Object.keys(obj11));
  delete obj11['not an identifier'];
  console.log(Object.keys(obj11));
  ```

#### Converting Any Value to an Object

- convert an arbitrary value to an object  `Object()`

| Value                       | Result            |
| :-------------------------- | ----------------- |
| (Called with no parameters) | {}                |
| undefined                   | {}                |
| null                        | {}                |
| a boolean bool              | new Boolean(bool) |
| a number num                | new Number(num)   |
| a string str                | new String(str)   |
| An Object obj               | obj (unchanged)   |

```javascript
console.log(Object(null) instanceof Object);
console.log(Object(false) instanceof Boolean);
var obj12 = {};
console.log(Object(obj12) === obj12 );

//check whecher value is an object
function(){
    return value === Object(value);
}
```

