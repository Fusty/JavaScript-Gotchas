# JavaScript Gotchas

## Equality
`0 == false; // true`

`"" == false;  // true`

`null == undefined;  // true`

`null === undefined;// false`



## Existence
`foo === undefined; // throws error if foo is not set yet`
`typeof foo === "undefined"; // correctly determines if foo is undefined even if unset`

```javascript
foo = null;
baz = undefined;
foo == undefined; // true
baz == undefined; true;
foo == null; // true
baz == null; // true
foo === undefined; // false
baz === undefined;  // true
foo === null; // true
baz === null; // false
```

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

