# JavaScript Gotchas

## Equality
Equality comparison of different types is detailed [here](http://www.ecma-international.org/ecma-262/5.1/#sec-11.9.3
).

`0 == false; // true`

`"" == false;  // true`

`null == undefined;  // true`

`null === undefined;// false`

## Existence
`foo === undefined; // throws error if foo is not set yet`

Note:  Some browsers handle the above differently

`typeof foo === "undefined"; // correctly determines if foo is undefined even if unset`

Note:  All browsers handle this the same way

```javascript
foo = null;
baz = undefined;
foo == undefined; // true
baz == undefined; // true
foo == null; // true
baz == null; // true
foo === undefined; // false
baz === undefined;  // true
foo === null; // true
baz === null; // false
```

You've probably seen the form for(var index in someObject){} before, but have you seen this . . .

```javascript
person1 = {
	name: "Thumby",
	occupation: "Developer"
}

person2 = {
	name: "Blamby",
	occupation: "Developer".
	meaning: "spreading happiness"
}

function logMeaning(person){
	if("meaning" in person){
		console.log(person.name + " has " + " as their meaning in life");
	}else{
		console.log(person.name+" has no meaning in their life");
	}
}

logMeaning(person1); // logs "Thumby has no meaning in their life"
logMeaning(person2); // logs "Blamby has spreading happiness as their meaning in life"
```

You can use (someString in someObject) to test for existence of a property in your object.
NOTE:  properties set as null, undefined, false, etc. will still result in a true return using "in"
NOTE 2:  this only works for enumerable properties

## References vs. Values

### Manipulating Object Values
```javascript
obj1 = {a:1, b:2, c:3}; // Intialize an object
obj2 = {a:obj1.a, b:obj1.b, c:obj1.c} // Intialize another based on the first one
delete obj1.a; // Delete the value from obj1
obj2.a === 1;  // true, the value from obj1 was copied into obj2
obj1.b = "Foo";
obj2.b === "Foo"; // false, you've assigned a new value, but not to the same memory location.  This may run counter to what you'd expect.
obj2.c = "Baz";
obj1.c === "Baz"; // false, same as above.  You've pointed obj2.c to a new value, but not changed the original object 
```

### Manipulating Object References
```javascript
source1 = {a:1, b:2, c:3};
source2 = {a:"Foo", b:"Baz", c:"Bar"};
obj1 = source1; //Assigning a reference to an object
obj2 = source2; //Assigning a reference to an object
obj1.a = -1;
source1.a === -1; // true because I modified a reference, not a value.  I'm modifying obj1.a which is an object reference
obj2.a = "ooF";
source2.a === "ooF"; // true same reason
val1 = obj1.a;
val1 === -1; // true because we just set this, but is it a reference or a value?
obj1.a = -100;
val1 === -100; // false, we set val1 to a value, not a reference
```

## Primitives vs Objects 

**Primitives**:

- string
- number
- boolean
- undefined
- null

**Objects**:

- everything else

## Booleans

**false**:

- `false`
- `null`
- `undefined`
- `0`
- `""` (empty string)
- `NaN`

**true**:

- everything else

## Weird Values

`Infinity; // Believe it or not, this is a valid value`

`typeof Infinity === "number"; // And it is of type number`

`Infinity > -Infinity; // true, You can negate Infinity and it would be less than positive Infinity`

`(Infinity - 1) === Infinity; // true, Infinity - 1 (or any non-infinite number) is still Infinity, same with addition`

`Infinity - Infinity === 0; // false, this doesn't even make sense.  Different Infinities aren't necessarily equal.`

Don't trust math with Infinity.

`console.log(Infinity - Infinity); // output: NaN,  This brings us to NaN (Not a Number)`

`typeof NaN === "number"; // true, NaN is still a number, despite it's name.  It's just a nonsensical number`

`NaN === NaN; // false, NaN isn't equal to anything, event itself.  Testing against NaN just doesn't work`

`isNaN(NaN); // true, but . . . `

`isNaN(undefined); // true, because it isn't a number`

`isNaN(true); // false, because of coersion (I think so anyway . . .)`

`isNaN(NaN) && typeof NaN === "number"; // true, this is the only way I know to test for NaN directly`

### Testing for NaN

```javascript
temp1 = NaN;
temp2 = Infinity - Infinity;
temp3 = "String";
temp4 = {};
temp5 = undefined;

isNaN(temp1) && typeof temp1 === "number"; // true
isNaN(temp2) && typeof temp2 === "number"; // true
isNaN(temp3) && typeof temp3 === "number"; // false
isNaN(temp4) && typeof temp4 === "number"; // false
isNaN(temp5) && typeof temp5 === "number"; // false
```
NaN is weird.
