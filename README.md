
Front End Style Guide
==========

Front-End Style Guide

-   [Front-End Style Guide](#front_end_style_guide)
    -   [1. JavaScript Style Guide](#javascript_style_guide)
        -   [1.1. References](#references)
        -   [1.2. Objects](#objects)
        -   [1.3. Arrays](#arrays)
        -   [1.4. Destructuring](#destructuring)
        -   [1.5. Strings](#strings)
        -   [1.6. Functions](#functions)
        -   [1.7. Arrow Functions](#arrow_functions)
        -   [1.8. Classes & Constructors](#classes_constructors)
        -   [1.9. Modules](#modules)
        -   [1.10. Iterators and Generators](#iterators_and_generators)
        -   [1.11. Properties](#properties)
        -   [1.12. Variables](#variables)
        -   [1.13. Hoisting](#hoisting)
        -   [1.14. Comparison Operators & Equality](#comparison_operators_equality)
        -   [1.15. Blocks](#blocks)
        -   [1.16. Comments](#comments)
        -   [1.17. Whitespace](#whitespace)
        -   [1.18. Commas](#commas)
        -   [1.19. Semicolons](#semicolons)
        -   [1.20. Type Casting & Coercion](#type_casting_coercion)
        -   [1.21. Naming Conventions](#naming_conventions)
        -   [1.22. Accessors](#accessors)
        -   [1.23. Events](#events)
        -   [1.24. jQuery](#jquery)
    -   [2. Ember Style Guide](#ember_style_guide)
        -   [2.1. Organizing modules](#organizing_modules)

-   ** JavaScript

-   ** EmberJS

-   ** Core Module

-   ** Criteria

-   ** TypeScript

<a href="#front_end_style_guide" class="anchor"></a>Front-End Style Guide
=========================================================================

<a href="#javascript_style_guide" class="anchor"></a>1. JavaScript Style Guide
------------------------------------------------------------------------------

Mostly stolen from [Airbnb Style Guide](https://github.com/airbnb/javascript)

### <a href="#references" class="anchor"></a>1.1. References

Use `const` for all of your references; avoid using `var`.

|     |                                                                                                                     |
|-----|---------------------------------------------------------------------------------------------------------------------|
| **  | Why? This ensures that you can’t reassign your references, which can lead to bugs and difficult to comprehend code. |

``` highlight
// bad
var a = 1;
var b = 2;

// good
const a = 1;
const b = 2;
```

If you must reassign references, use `let` instead of `var`.

|     |                                                                    |
|-----|--------------------------------------------------------------------|
| **  | Why? `let` is block-scoped rather than function-scoped like `var`. |

``` highlight
// bad
var count = 1;
if (true) {
  count += 1;
}

// good, use the let.
let count = 1;
if (true) {
  count += 1;
}
```

Note that both `let` and `const` are block-scoped.

``` highlight
// const and let only exist in the blocks they are defined in.
{
  let a = 1;
  const b = 1;
}
console.log(a); // ReferenceError
console.log(b); // ReferenceError
```

### <a href="#objects" class="anchor"></a>1.2. Objects

Use the literal syntax for object creation.

``` highlight
// bad
const item = new Object();

// good
const item = {};
```

Use computed property names when creating objects with dynamic property names.

|     |                                                                             |
|-----|-----------------------------------------------------------------------------|
| **  | Why? They allow you to define all the properties of an object in one place. |

``` highlight
function getKey(k) {
  return `a key named ${k}`;
}

// bad
const obj = {
  id: 5,
  name: 'San Francisco',
};
obj[getKey('enabled')] = true;

// good
const obj = {
  id: 5,
  name: 'San Francisco',
  [getKey('enabled')]: true,
};
```

Use object method shorthand.

``` highlight
// bad
const atom = {
  value: 1,

  addValue: function (value) {
    return atom.value + value;
  },
};

// good
const atom = {
  value: 1,

  addValue(value) {
    return atom.value + value;
  },
};
```

Use property value shorthand.

|     |                                              |
|-----|----------------------------------------------|
| **  | Why? It is shorter to write and descriptive. |

``` highlight
const lukeSkywalker = 'Luke Skywalker';

// bad
const obj = {
  lukeSkywalker: lukeSkywalker,
};

// good
const obj = {
  lukeSkywalker,
};
```

Group your shorthand properties at the beginning of your object declaration.

|     |                                                                    |
|-----|--------------------------------------------------------------------|
| **  | Why? It’s easier to tell which properties are using the shorthand. |

``` highlight
const anakinSkywalker = 'Anakin Skywalker';
const lukeSkywalker = 'Luke Skywalker';

// bad
const obj = {
  episodeOne: 1,
  twoJediWalkIntoACantina: 2,
  lukeSkywalker,
  episodeThree: 3,
  mayTheFourth: 4,
  anakinSkywalker,
};

// good
const obj = {
  lukeSkywalker,
  anakinSkywalker,
  episodeOne: 1,
  twoJediWalkIntoACantina: 2,
  episodeThree: 3,
  mayTheFourth: 4,
};
```

Only quote properties that are invalid identifiers.

|     |                                                                                                                                                    |
|-----|----------------------------------------------------------------------------------------------------------------------------------------------------|
| **  | Why? In general we consider it subjectively easier to read. It improves syntax highlighting, and is also more easily optimized by many JS engines. |

``` highlight
// bad
const bad = {
  'foo': 3,
  'bar': 4,
  'data-blah': 5,
};

// good
const good = {
  foo: 3,
  bar: 4,
  'data-blah': 5,
};
```

Do not call `Object.prototype` methods directly, such as `hasOwnProperty`, `propertyIsEnumerable`, and `isPrototypeOf`.

|     |                                                                                                                                                                                  |
|-----|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **  | Why? These methods may be shadowed by properties on the object in question - consider `{ hasOwnProperty: false }` - or, the object may be a null object (`Object.create(null)`). |

``` highlight
// bad
console.log(object.hasOwnProperty(key));

// good
console.log(Object.prototype.hasOwnProperty.call(object, key));

// best
const has = Object.prototype.hasOwnProperty; // cache the lookup once, in module scope.
/* or */
import has from 'has';
…
console.log(has.call(object, key));
```

Prefer the object spread operator over Object.assign to shallow-copy objects. Use the object rest operator to get a new object with certain properties omitted.

``` highlight
// very bad
const original = { a: 1, b: 2 };
const copy = Object.assign(original, { c: 3 }); // this mutates `original` ಠ_ಠ
delete copy.a; // so does this

// bad
const original = { a: 1, b: 2 };
const copy = Object.assign({}, original, { c: 3 }); // copy => { a: 1, b: 2, c: 3 }

// good
const original = { a: 1, b: 2 };
const copy = { ...original, c: 3 }; // copy => { a: 1, b: 2, c: 3 }

const { a, ...noA } = copy; // noA => { b: 2, c: 3 }
```

### <a href="#arrays" class="anchor"></a>1.3. Arrays

Use the literal syntax for array creation.

``` highlight
// bad
const items = new Array();

// good
const items = [];
```

Use `Array.push` instead of direct assignment to add items to an array.

``` highlight
const someStack = [];

// bad
someStack[someStack.length] = 'abracadabra';

// good
someStack.push('abracadabra');
```

Use array spreads `…​` to copy arrays.

``` highlight
// bad
const len = items.length;
const itemsCopy = [];
let i;

for (i = 0; i < len; i++) {
  itemsCopy[i] = items[i];
}

// good
const itemsCopy = [...items];
```

To convert an array-like object to an array, use `Array.from`.

``` highlight
const foo = document.querySelectorAll('.foo');
const nodes = Array.from(foo);
```

Use return statements in array method callbacks. It’s ok to omit the return if the function body consists of a single statement following 8.2.

``` highlight
// good
[1, 2, 3].map((x) => {
  const y = x + 1;
  return x * y;
});

// good
[1, 2, 3].map(x => x + 1);

// bad
const flat = {};
[[0, 1], [2, 3], [4, 5]].reduce((memo, item, index) => {
  const flatten = memo.concat(item);
  flat[index] = flatten;
});

// good
const flat = {};
[[0, 1], [2, 3], [4, 5]].reduce((memo, item, index) => {
  const flatten = memo.concat(item);
  flat[index] = flatten;
  return flatten;
});

// bad
inbox.filter((msg) => {
  const { subject, author } = msg;
  if (subject === 'Mockingbird') {
    return author === 'Harper Lee';
  } else {
    return false;
  }
});

// good
inbox.filter((msg) => {
  const { subject, author } = msg;
  if (subject === 'Mockingbird') {
    return author === 'Harper Lee';
  }

  return false;
});
```

### <a href="#destructuring" class="anchor"></a>1.4. Destructuring

Use object destructuring when accessing and using multiple properties of an object.

|     |                                                                                       |
|-----|---------------------------------------------------------------------------------------|
| **  | Why? Destructuring saves you from creating temporary references for those properties. |

``` highlight
// bad
function getFullName(user) {
  const firstName = user.firstName;
  const lastName = user.lastName;

  return `${firstName} ${lastName}`;
}

// good
function getFullName(user) {
  const { firstName, lastName } = user;
  return `${firstName} ${lastName}`;
}

// best
function getFullName({ firstName, lastName }) {
  return `${firstName} ${lastName}`;
}
```

Use array destructuring.

``` highlight
const arr = [1, 2, 3, 4];

// bad
const first = arr[0];
const second = arr[1];

// good
const [first, second] = arr;
```

Use object destructuring for multiple return values, not array destructuring.

|     |                                                                                                      |
|-----|------------------------------------------------------------------------------------------------------|
| **  | Why? You can add new properties over time or change the order of things without breaking call sites. |

``` highlight
// bad
function processInput(input) {
  // then a miracle occurs
  return [left, right, top, bottom];
}

// the caller needs to think about the order of return data
const [left, __, top] = processInput(input);

// good
function processInput(input) {
  // then a miracle occurs
  return { left, right, top, bottom };
}

// the caller selects only the data they need
const { left, top } = processInput(input);
```

### <a href="#strings" class="anchor"></a>1.5. Strings

Use single quotes `''` for strings.

``` highlight
// bad
const name = "Capt. Janeway";

// bad - template literals should contain interpolation or newlines
const name = `Capt. Janeway`;

// good
const name = 'Capt. Janeway';
```

Strings that cause the line to go over 100 characters should not be written across multiple lines using string concatenation.

|     |                                                                             |
|-----|-----------------------------------------------------------------------------|
| **  | Why? Broken strings are painful to work with and make code less searchable. |

|     |                                                                |
|-----|----------------------------------------------------------------|
| **  | But still…​ maybe you should not use such a long lines at all? |

``` highlight
// bad
const errorMessage = 'This is a super long error that was thrown because \
of Batman. When you stop to think about how Batman had anything to do \
with this, you would get nowhere \
fast.';

// bad
const errorMessage = 'This is a super long error that was thrown because ' +
  'of Batman. When you stop to think about how Batman had anything to do ' +
  'with this, you would get nowhere fast.';

// good
const errorMessage = 'This is a super long error that was thrown because of Batman. When you stop to think about how Batman had anything to do with this, you would get nowhere fast.';
```

When programmatically building up strings, use template strings instead of concatenation.

|     |                                                                                                                   |
|-----|-------------------------------------------------------------------------------------------------------------------|
| **  | Why? Template strings give you a readable, concise syntax with proper newlines and string interpolation features. |

``` highlight
// bad
function sayHi(name) {
  return 'How are you, ' + name + '?';
}

// bad
function sayHi(name) {
  return ['How are you, ', name, '?'].join();
}

// bad
function sayHi(name) {
  return `How are you, ${ name }?`;
}

// good
function sayHi(name) {
  return `How are you, ${name}?`;
}
```

Never use `eval()` on a string, it opens too many vulnerabilities.

|     |              |
|-----|--------------|
| **  | NEVER! EVER! |

Do not unnecessarily escape characters in strings.

|     |                                                                                     |
|-----|-------------------------------------------------------------------------------------|
| **  | Why? Backslashes harm readability, thus they should only be present when necessary. |

``` highlight
// bad
const foo = '\'this\' \i\s \"quoted\"';

// good
const foo = '\'this\' is "quoted"';
const foo = `'this' is "quoted"`;
```

### <a href="#functions" class="anchor"></a>1.6. Functions

Use named function expressions instead of function declarations.

|     |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
|-----|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **  | Why? Function declarations are hoisted, which means that it’s easy - too easy - to reference the function before it is defined in the file. This harms readability and maintainability. If you find that a function’s definition is large or complex enough that it is interfering with understanding the rest of the file, then perhaps it’s time to extract it to its own module! Don’t forget to name the expression - anonymous functions can make it harder to locate the problem in an Error’s call stack. |

``` highlight
// bad
const foo = function () {
};

// bad
function foo() {
}

// good
const foo = function foo() {
};
```

Name your reference to function like name of function itself.

|     |                                                                                       |
|-----|---------------------------------------------------------------------------------------|
| **  | Why? It will be easier to find function by name that returned from Error Stack Trace. |

``` highlight
// bad
const foo = function bar() {
};

// good
const foo = function foo() {
};
```

Wrap immediately invoked function expressions in parentheses.

|     |                                                                                                                                                                                                                                 |
|-----|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **  | Why? An immediately invoked function expression is a single unit - wrapping both it, and its invocation parens, in parens, cleanly expresses this. Note that in a world with modules everywhere, you almost never need an IIFE. |

``` highlight
// immediately-invoked function expression (IIFE)
(function () {
  console.log('Welcome to the Internet. Please follow me.');
}());
```

Never declare a function in a non-function block (if, while, etc). Assign the function to a variable instead. Browsers will allow you to do it, but they all interpret it differently, which is bad news bears.

ECMA-262 defines a block as a list of statements. A function declaration is not a statement.

``` highlight
// bad
if (currentUser) {
  function test() {
    console.log('Nope.');
  }
}

// good
let test;
if (currentUser) {
  test = () => {
    console.log('Yup.');
  };
}
```

Never name a parameter `arguments`. This will take precedence over the arguments object that is given to every function scope.

``` highlight
// bad
function nope(name, options, arguments) {
  // ...stuff...
}

// good
function yup(name, options, args) {
  // ...stuff...
}
```

Never use `arguments`, opt to use rest syntax `…​` instead.

|     |                                                                                                                                                 |
|-----|-------------------------------------------------------------------------------------------------------------------------------------------------|
| **  | Why? `…​` is explicit about which arguments you want pulled. Plus, rest arguments are a real Array, and not merely Array-like like `arguments`. |

``` highlight
// bad
function concatenateAll() {
  const args = Array.prototype.slice.call(arguments);
  return args.join('');
}

// good
function concatenateAll(...args) {
  return args.join('');
}
```

Use default parameter syntax rather than mutating function arguments.

``` highlight
// really bad
function handleThings(opts) {
  // No! We shouldn't mutate function arguments.
  // Double bad: if opts is falsy it'll be set to an object which may
  // be what you want but it can introduce subtle bugs.
  opts = opts || {};
  // ...
}

// still bad
function handleThings(opts) {
  if (opts === void 0) {
    opts = {};
  }
  // ...
}

// good
function handleThings(opts = {}) {
  // ...
}
```

Avoid side effects with default parameters.

|     |                                          |
|-----|------------------------------------------|
| **  | Why? They are confusing to reason about. |

``` highlight
var b = 1;
// bad
function count(a = b++) {
  console.log(a);
}
count();  // 1
count();  // 2
count(3); // 3
count();  // 3
```

Always put default parameters last.

``` highlight
// bad
function handleThings(opts = {}, name) {
  // ...
}

// good
function handleThings(name, opts = {}) {
  // ...
}
```

Never use the Function constructor to create a new function.

|     |                                                                                                             |
|-----|-------------------------------------------------------------------------------------------------------------|
| **  | Why? Creating a function in this way evaluates a string similarly to `eval()`, which opens vulnerabilities. |

``` highlight
// bad
var add = new Function('a', 'b', 'return a + b');

// still bad
var subtract = Function('a', 'b', 'return a - b');
```

Spacing in a function signature.

|     |                                                                                                           |
|-----|-----------------------------------------------------------------------------------------------------------|
| **  | Why? Consistency is good, and you shouldn’t have to add or remove a space when adding or removing a name. |

``` highlight
// bad
const f = function(){};
const g = function (){};
const h = function() {};

// good
const x = function () {};
const a = function a() {};
```

Never mutate parameters.

|     |                                                                                                                    |
|-----|--------------------------------------------------------------------------------------------------------------------|
| **  | Why? Manipulating objects passed in as parameters can cause unwanted variable side effects in the original caller. |

``` highlight
// bad
function f1(obj) {
  obj.key = 1;
};

// good
function f2(obj) {
  const key = Object.prototype.hasOwnProperty.call(obj, 'key') ? obj.key : 1;
};
```

Never reassign parameters.

|     |                                                                                                                                                                       |
|-----|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **  | Why? Reassigning parameters can lead to unexpected behavior, especially when accessing the arguments object. It can also cause optimization issues, especially in V8. |

``` highlight
// bad
function f1(a) {
  a = 1;
}

function f2(a) {
  if (!a) { a = 1; }
}

// good
function f3(a) {
  const b = a || 1;
}

// best
function f4(a = 1) {
}
```

Prefer the use of the spread operator `…​` to call variadic functions.

|     |                                                                                                           |
|-----|-----------------------------------------------------------------------------------------------------------|
| **  | Why? It’s cleaner, you don’t need to supply a context, and you can not easily compose `new` with `apply`. |

``` highlight
// bad
const x = [1, 2, 3, 4, 5];
console.log.apply(console, x);

// good
const x = [1, 2, 3, 4, 5];
console.log(...x);

// bad
new (Function.prototype.bind.apply(Date, [null, 2016, 08, 05]));

// good
new Date(...[2016, 08, 05]);
```

Functions with multiline signatures, or invocations, should be indented just like every other multiline list in this guide: with each item on a line by itself, without a trailing comma on the last item.

``` highlight
// bad
function foo(bar,
             baz,
             quux) {
  // body
}

// good
function foo(
  bar,
  baz,
  quux
) {
  // body
}

// bad
console.log(foo,
  bar,
  baz);

// good
console.log(
  foo,
  bar,
  baz
);
```

Prefer use one hash rather than list of parameters as arguments.

|     |                                                                                                                                              |
|-----|----------------------------------------------------------------------------------------------------------------------------------------------|
| **  | Why? It will be easier to maintain function without refactoring any usages of that function. Also you should not worry about argument order. |

|     |                                                                                                                                                                                                         |
|-----|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **  | Why not? Default value is assigned only if passed argument is `undefined`. So be careful about passing `null` instead of hash, since it is literal this: `const {foo} = null`; which lead to TypeError; |

``` highlight
// good
function foo(a, b, c) {
}

// better
function foo({a, b, c}) {
}

// even better
function foo({a = 1, b = 2, c = 3} = {}) {
}
```

### <a href="#arrow_functions" class="anchor"></a>1.7. Arrow Functions

When you must use function expressions (as when passing an anonymous function), use arrow function notation.

|     |                                                                                                                                               |
|-----|-----------------------------------------------------------------------------------------------------------------------------------------------|
| **  | Why? It creates a version of the function that executes in the context of this, which is usually what you want, and is a more concise syntax. |

|     |                                                                                                                      |
|-----|----------------------------------------------------------------------------------------------------------------------|
| **  | Why not? If you have a fairly complicated function, you might move that logic out into its own function declaration. |

``` highlight
// bad
[1, 2, 3].map(function (x) {
  const y = x + 1;
  return x * y;
});

// good
[1, 2, 3].map((x) => {
  const y = x + 1;
  return x * y;
});
```

If the function body consists of a single expression, omit the braces and use the implicit return. Otherwise, keep the braces and use a `return` statement.

|     |                                                                                   |
|-----|-----------------------------------------------------------------------------------|
| **  | Why? Syntactic sugar. It reads well when multiple functions are chained together. |

``` highlight
// bad
[1, 2, 3].map(number => {
  const nextNumber = number + 1;
  `A string containing the ${nextNumber}.`;
});

// good
[1, 2, 3].map(number => `A string containing the ${number}.`);

// good
[1, 2, 3].map((number) => {
  const nextNumber = number + 1;
  return `A string containing the ${nextNumber}.`;
});

// good
[1, 2, 3].map((number, index) => ({
  [index]: number
}));
```

In case the expression spans over multiple lines, wrap it in parentheses for better readability.

|     |                                                           |
|-----|-----------------------------------------------------------|
| **  | Why? It shows clearly where the function starts and ends. |

``` highlight
// bad
['get', 'post', 'put'].map(httpMethod => Object.prototype.hasOwnProperty.call(
    httpMagicObjectWithAVeryLongName,
    httpMethod
  )
);

// good
['get', 'post', 'put'].map(httpMethod => (
  Object.prototype.hasOwnProperty.call(
    httpMagicObjectWithAVeryLongName,
    httpMethod
  )
));
```

If your function takes a single argument and doesn’t use braces, omit the parentheses. Otherwise, always include parentheses around arguments.

|     |                           |
|-----|---------------------------|
| **  | Why? Less visual clutter. |

``` highlight
// bad
[1, 2, 3].map((x) => x * x);

// good
[1, 2, 3].map(x => x * x);

// good
[1, 2, 3].map(number => (
  `A long string with the ${number}. It’s so long that we don’t want it to take up space on the .map line!`
));

// bad
[1, 2, 3].map(x => {
  const y = x + 1;
  return x * y;
});

// good
[1, 2, 3].map((x) => {
  const y = x + 1;
  return x * y;
});
```

Avoid confusing arrow function syntax (`⇒`) with comparison operators (`⇐`, `>=`).

``` highlight
// bad
const itemHeight = item => item.height > 256 ? item.largeSize : item.smallSize;

// bad
const itemHeight = (item) => item.height > 256 ? item.largeSize : item.smallSize;

// good
const itemHeight = item => (item.height > 256 ? item.largeSize : item.smallSize);

// good
const itemHeight = (item) => {
  const { height, largeSize, smallSize } = item;
  return height > 256 ? largeSize : smallSize;
};
```

### <a href="#classes_constructors" class="anchor"></a>1.8. Classes & Constructors

Always use `class`. Avoid manipulating `prototype` directly.

|     |                                                                 |
|-----|-----------------------------------------------------------------|
| **  | Why? `class` syntax is more concise and easier to reason about. |

``` highlight
// bad
function Queue(contents = []) {
  this.queue = [...contents];
}
Queue.prototype.pop = function () {
  const value = this.queue[0];
  this.queue.splice(0, 1);
  return value;
};


// good
class Queue {
  constructor(contents = []) {
    this.queue = [...contents];
  }
  pop() {
    const value = this.queue[0];
    this.queue.splice(0, 1);
    return value;
  }
}
```

Use extends for inheritance.

|     |                                                                                             |
|-----|---------------------------------------------------------------------------------------------|
| **  | Why? It is a built-in way to inherit prototype functionality without breaking `instanceof`. |

``` highlight
// bad
const inherits = require('inherits');
function PeekableQueue(contents) {
  Queue.apply(this, contents);
}
inherits(PeekableQueue, Queue);
PeekableQueue.prototype.peek = function () {
  return this._queue[0];
}

// good
class PeekableQueue extends Queue {
  peek() {
    return this._queue[0];
  }
}
```

Methods can return `this` to help with method chaining.

``` highlight
// bad
Jedi.prototype.jump = function () {
  this.jumping = true;
  return true;
};

Jedi.prototype.setHeight = function (height) {
  this.height = height;
};

const luke = new Jedi();
luke.jump(); // => true
luke.setHeight(20); // => undefined

// good
class Jedi {
  jump() {
    this.jumping = true;
    return this;
  }

  setHeight(height) {
    this.height = height;
    return this;
  }
}

const luke = new Jedi();

luke.jump()
    .setHeight(20);
```

It’s okay to write a custom `toString()` method, just make sure it works successfully and causes no side effects.

``` highlight
class Jedi {
  constructor(options = {}) {
    this.name = options.name || 'no name';
  }

  getName() {
    return this.name;
  }

  toString() {
    return `Jedi - ${this.getName()}`;
  }
}
```

Classes have a default constructor if one is not specified. An empty constructor function or one that just delegates to a parent class is unnecessary.

``` highlight
// bad
class Jedi {
  constructor() {}

  getName() {
    return this.name;
  }
}

// bad
class Rey extends Jedi {
  constructor(...args) {
    super(...args);
  }
}

// good
class Rey extends Jedi {
  constructor(...args) {
    super(...args);
    this.name = 'Rey';
  }
}
```

Avoid duplicate class members.

|     |                                                                                                                           |
|-----|---------------------------------------------------------------------------------------------------------------------------|
| **  | Why? Duplicate class member declarations will silently prefer the last one - having duplicates is almost certainly a bug. |

``` highlight
// bad
class Foo {
  bar() { return 1; }
  bar() { return 2; }
}

// good
class Foo {
  bar() { return 1; }
}

// good
class Foo {
  bar() { return 2; }
}
```

Avoid usage of `this` in static methods since it’s points to current class (read: `prototype`), not instance of class.

|     |                                                                             |
|-----|-----------------------------------------------------------------------------|
| **  | Why not? You can use `this` if you clearly understand what you want to get. |

``` highlight
// bad
class Foo {
  static bar() {
    return this.name;
  }
}

// good
class Foo {
  static bar(model) {
    return model.name;
  }
}

// still good
class Foo {
  static foo() {
    return 'baz';
  }
  static bar() {
    return this.foo();
  }
}

class Foo2 extends Foo {
  static foo() {
    return 'baz2';
  }
}

Foo2.bar(); // will output 'baz2'
```

If you want to call another static method of current class, points it by class name is preferable. Most of the time you need to specify which behavior (`class`) you want. And `this` is very confusing.

|     |                            |
|-----|----------------------------|
| **  | Why not? Abstract classes. |

``` highlight
// bad
class Foo {
  static foo() {
    return 'some';
  }

  static bar() {
    return this.foo();
  }
}

// good
class Foo {
  static foo() {
    return 'some';
  }

  static bar() {
    return Foo.foo();
  }
}
```

### <a href="#modules" class="anchor"></a>1.9. Modules

Always use modules (`import`/`export`) over a non-standard module system. You can always transpile to your preferred module system.

|     |                                                                |
|-----|----------------------------------------------------------------|
| **  | Why? Modules are the future, let’s start using the future now. |

``` highlight
// bad
const FancyModule = require('./fancy-module');
module.exports = FancyModule.es6;

// ok
import FancyModule from './fancy-module';
export default FancyModule.es6;

// best
import { es6 } from './fancy-module';
export default es6;
```

Do not use wildcard imports.

|     |                                                        |
|-----|--------------------------------------------------------|
| **  | Why? This makes sure you have a single default export. |

``` highlight
// bad
import * as FancyModule from './fancy-module';

// good
import FancyModule from './fancy-module';
```

And do not export directly from an import.

|     |                                                                                                                             |
|-----|-----------------------------------------------------------------------------------------------------------------------------|
| **  | Why? Although the one-liner is concise, having one clear way to import and one clear way to export makes things consistent. |

``` highlight
// bad
// filename es6.js
export { es6 as default } from './fancy-module';

// good
// filename es6.js
import { es6 } from './fancy-module';
export default es6;
```

Only import from a path in one place.

|     |                                                                                             |
|-----|---------------------------------------------------------------------------------------------|
| **  | Why? Having multiple lines that import from the same path can make code harder to maintain. |

``` highlight
// bad
import foo from 'foo';
// … some other imports … //
import { named1, named2 } from 'foo';

// good
import foo, { named1, named2 } from 'foo';

// good
import foo, {
named1,
named2,
} from 'foo';
```

Do not export mutable bindings.

|     |                                                                                                                                                                                                                    |
|-----|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **  | Why? Mutation should be avoided in general, but in particular when exporting mutable bindings. While this technique may be needed for some special cases, in general, only constant references should be exported. |

``` highlight
// bad
let foo = 3;
export { foo }

// good
const foo = 3;
export { foo }
```

In modules with a single export, prefer default export over named export.

``` highlight
// bad
export function foo() {}

// good
export default function foo() {}
```

Put all imports above non-import statements.

|     |                                                                                           |
|-----|-------------------------------------------------------------------------------------------|
| **  | Why? Since imports are hoisted, keeping them all at the top prevents surprising behavior. |

``` highlight
// bad
import foo from 'foo';
foo.init();

import bar from 'bar';

// good
import foo from 'foo';
import bar from 'bar';

foo.init();
```

Multiline imports should be indented just like multiline array and object literals.

|     |                                                                                                                                                             |
|-----|-------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **  | Why? The curly braces follow the same indentation rules as every other curly brace block in the style guide, as do the trailing commas (no trailing comma). |

``` highlight
// bad
import {longNameA, longNameB, longNameC, longNameD, longNameE} from 'path';

// good
import {
  longNameA,
  longNameB,
  longNameC,
  longNameD,
  longNameE
} from 'path';
```

### <a href="#iterators_and_generators" class="anchor"></a>1.10. Iterators and Generators

Don’t use iterators. Prefer JavaScript’s higher-order functions instead of loops like for-in or for-of.

|     |                                                                                                                                    |
|-----|------------------------------------------------------------------------------------------------------------------------------------|
| **  | Why? This enforces our immutable rule. Dealing with pure functions that return values is easier to reason about than side effects. |

|     |                                                                                                                                                                                                                                      |
|-----|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **  | Use `map()` / `every()` / `filter()` / `find()` / `findIndex()` / `reduce()` / `some()` / …​ to iterate over arrays, and `Object.keys()` / `Object.values()` / `Object.entries()` to produce arrays so you can iterate over objects. |

``` highlight
const numbers = [1, 2, 3, 4, 5];

// bad
let sum = 0;
for (let num of numbers) {
  sum += num;
}

sum === 15;

// good
let sum = 0;
numbers.forEach(num => sum += num);
sum === 15;

// best (use the functional force - we have cookies)
const sum = numbers.reduce((total, num) => total + num, 0);
sum === 15;
```

Don’t use generators for now.

|     |                                        |
|-----|----------------------------------------|
| **  | Why? They don’t transpile well to ES5. |

If you must use generators, or if you disregard our advice, make sure their function signature is spaced properly.

|     |                                                                                                                                                                       |
|-----|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **  | Why? `function` and `*` are part of the same conceptual keyword - `*` is not a modifier for `function`, `function*` is a unique construct, different from `function`. |

``` highlight
// bad
function * foo() {
}

const bar = function * () {
}

const baz = function *() {
}

const quux = function*() {
}

function*foo() {
}

function *foo() {
}

// very bad
function
*
foo() {
}

const wat = function
*
() {
}

// good
function* foo() {
}

const foo = function* () {
}
```

### <a href="#properties" class="anchor"></a>1.11. Properties

Use dot notation when accessing properties.

|     |                                       |
|-----|---------------------------------------|
| **  | Why not? TypeScript in several cases. |

``` highlight
const luke = {
  jedi: true,
  age: 28,
};

// bad
const isJedi = luke['jedi'];

// good
const isJedi = luke.jedi;
```

Use bracket notation `[]` when accessing properties with a variable.

``` highlight
const luke = {
  jedi: true,
  age: 28,
};

function getProp(prop) {
  return luke[prop];
}

const isJedi = getProp('jedi');
```

### <a href="#variables" class="anchor"></a>1.12. Variables

Always use `const` to declare variables. Not doing so will result in global variables. We want to avoid polluting the global namespace. Captain Planet warned us of that.

``` highlight
// bad
superPower = new SuperPower();

// good
const superPower = new SuperPower();
```

Use one `const` declaration per variable.

|     |                                                                                                                                                                                                                                                                                 |
|-----|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **  | Why? It’s easier to add new variable declarations this way, and you never have to worry about swapping out a `;` for a `,` or introducing punctuation-only diffs. You can also step through each declaration with the debugger, instead of jumping through all of them at once. |

``` highlight
// bad
const items = getItems(),
    goSportsTeam = true,
    dragonball = 'z';

// bad
// (compare to above, and try to spot the mistake)
const items = getItems(),
    goSportsTeam = true;
    dragonball = 'z';

// good
const items = getItems();
const goSportsTeam = true;
const dragonball = 'z';
```

Group all your `const` s and then group all your `let` s.

|     |                                                                                                                             |
|-----|-----------------------------------------------------------------------------------------------------------------------------|
| **  | Why? This is helpful when later on you might need to assign a variable depending on one of the previous assigned variables. |

``` highlight
// bad
let i, len, dragonball,
    items = getItems(),
    goSportsTeam = true;

// bad
let i;
const items = getItems();
let dragonball;
const goSportsTeam = true;
let len;

// good
const goSportsTeam = true;
const items = getItems();
let dragonball;
let i;
let length;
```

Assign variables where you need them, but place them in a reasonable place.

|     |                                                                  |
|-----|------------------------------------------------------------------|
| **  | Why? `let` and `const` are block scoped and not function scoped. |

``` highlight
// bad - unnecessary function call
function checkName(hasName) {
  const name = getName();

  if (hasName === 'test') {
    return false;
  }

  if (name === 'test') {
    this.setName('');
    return false;
  }

  return name;
}

// good
function checkName(hasName) {
  if (hasName === 'test') {
    return false;
  }

  const name = getName();

  if (name === 'test') {
    this.setName('');
    return false;
  }

  return name;
}
```

Don’t chain variable assignments.

|     |                                                                       |
|-----|-----------------------------------------------------------------------|
| **  | Why? Chaining variable assignments creates implicit global variables. |

``` highlight
// bad
(function example() {
  // JavaScript interprets this as
  // let a = ( b = ( c = 1 ) );
  // The let keyword only applies to variable a; variables b and c become
  // global variables.
  let a = b = c = 1;
}());

console.log(a); // undefined
console.log(b); // 1
console.log(c); // 1

// good
(function example() {
  let a = 1;
  let b = a;
  let c = a;
}());

console.log(a); // undefined
console.log(b); // undefined
console.log(c); // undefined

// the same applies for `const`
```

13.6 Avoid using unary increments and decrements (++, --).

|     |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
|-----|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **  | Why? Per the eslint documentation, unary increment and decrement statements are subject to automatic semicolon insertion and can cause silent errors with incrementing or decrementing values within an application. It is also more expressive to mutate your values with statements like `num += 1` instead of `num++` or `num ++`. Disallowing unary increment and decrement statements also prevents you from pre-incrementing/pre-decrementing values unintentionally which can also cause unexpected behavior in your programs. |

``` highlight
// bad

let array = [1, 2, 3];
let num = 1;
num++;
--num;

let sum = 0;
let truthyCount = 0;
for(let i = 0; i < array.length; i++){
let value = array[i];
sum += value;
if (value) {
  truthyCount++;
}
}

// good

let array = [1, 2, 3];
let num = 1;
num += 1;
num -= 1;

const sum = array.reduce((a, b) => a + b, 0);
const truthyCount = array.filter(Boolean).length;
```

### <a href="#hoisting" class="anchor"></a>1.13. Hoisting

`var` declarations get hoisted to the top of their scope, their assignment does not. `const` and `let` declarations are blessed with a new concept called Temporal Dead Zones (TDZ). It’s important to know why typeof is no longer safe.

``` highlight
// we know this wouldn't work (assuming there
// is no notDefined global variable)
function example() {
  console.log(notDefined); // => throws a ReferenceError
}

// creating a variable declaration after you
// reference the variable will work due to
// variable hoisting. Note: the assignment
// value of `true` is not hoisted.
function example() {
  console.log(declaredButNotAssigned); // => undefined
  var declaredButNotAssigned = true;
}

// the interpreter is hoisting the variable
// declaration to the top of the scope,
// which means our example could be rewritten as:
function example() {
  let declaredButNotAssigned;
  console.log(declaredButNotAssigned); // => undefined
  declaredButNotAssigned = true;
}

// using const and let
function example() {
  console.log(declaredButNotAssigned); // => throws a ReferenceError
  console.log(typeof declaredButNotAssigned); // => throws a ReferenceError
  const declaredButNotAssigned = true;
}
```

Anonymous function expressions hoist their variable name, but not the function assignment.

``` highlight
function example() {
  console.log(anonymous); // => undefined

  anonymous(); // => TypeError anonymous is not a function

  var anonymous = function () {
    console.log('anonymous function expression');
  };
}
```

Named function expressions hoist the variable name, not the function name or the function body.

``` highlight
function example() {
  console.log(named); // => undefined

  named(); // => TypeError named is not a function

  superPower(); // => ReferenceError superPower is not defined

  var named = function superPower() {
    console.log('Flying');
  };
}

// the same is true when the function name
// is the same as the variable name.
function example() {
  console.log(named); // => undefined

  named(); // => TypeError named is not a function

  var named = function named() {
    console.log('named');
  }
}
```

Function declarations hoist their name and the function body.

``` highlight
function example() {
  superPower(); // => Flying

  function superPower() {
    console.log('Flying');
  }
}
```

### <a href="#comparison_operators_equality" class="anchor"></a>1.14. Comparison Operators & Equality

Use `===` and `!==` over `==` and `!=`.

Conditional statements such as the if statement evaluate their expression using coercion with the ToBoolean abstract method and always follow these simple rules:

-   **Objects** evaluate to **true**

-   **Undefined** evaluates to **false**

-   **Null** evaluates to **false**

-   **Booleans** evaluate to **the value of the boolean**

-   **Numbers** evaluate to **false** if **0**, **+0**, **-0**, or **NaN**, otherwise **true**

-   **Strings** evaluate to **false** if an empty string `''`, otherwise **true**

``` highlight
if ([0] && []) {
  // true
  // an array (even an empty one) is an object, objects will evaluate to true
}
```

Use shortcuts for booleans, but explicit comparisons for strings and numbers.

``` highlight
// bad
if (isValid === true) {
  // ...stuff...
}

// good
if (isValid) {
  // ...stuff...
}

// bad
if (name) {
  // ...stuff...
}

// good
if (name !== '') {
  // ...stuff...
}

// bad
if (collection.length) {
  // ...stuff...
}

// good
if (collection.length > 0) {
  // ...stuff...
}
```

Always use braces blocks in `case` and `default` clauses.

|     |                                                                                                                                                                                                                                         |
|-----|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **  | Why? Lexical declarations are visible in the entire switch block but only get initialized when assigned, which only happens when its case is reached. This causes problems when multiple case clauses attempt to define the same thing. |

``` highlight
// bad
switch (foo) {
 case 1:
   let x = 1;
   break;
 case 2:
   const y = 2;
   break;
 case 3:
   function f() {}
   break;
 default:
   class C {}
}

// good
switch (foo) {
  case 1: {
    let x = 1;
    break;
  }
  case 2: {
    const y = 2;
    break;
  }
  case 3: {
    function f() {}
    break;
  }
  case 4: {
    bar();
    break;
  }
  default: {
    class C {}
  }
}
```

Ternaries should not be nested and generally be single line expressions.

``` highlight
// bad
const foo = maybe1 > maybe2
  ? "bar"
  : value1 > value2 ? "baz" : null;

// better
const maybeNull = value1 > value2 ? 'baz' : null;

const foo = maybe1 > maybe2
  ? 'bar'
  : maybeNull;

// best
const maybeNull = value1 > value2 ? 'baz' : null;

const foo = maybe1 > maybe2 ? 'bar' : maybeNull;
```

Avoid unneeded ternary statements.

``` highlight
// bad
const foo = a ? a : b;
const bar = c ? true : false;
const baz = c ? false : true;

// good
const foo = a || b;
const bar = !!c;
const baz = !c;
```

### <a href="#blocks" class="anchor"></a>1.15. Blocks

Use braces with all blocks.

``` highlight
// bad
if (test)
  return false;

// bad
if (test) return false;

// good
if (test) {
  return false;
}

// bad
function foo() { return false; }

// good
function bar() {
  return false;
}
```

If you’re using multi-line blocks with `if` and `else`, put `else` on the same line as your `if` block’s closing brace.

``` highlight
// bad
if (test) {
  thing1();
  thing2();
}
else {
  thing3();
}

// good
if (test) {
  thing1();
  thing2();
} else {
  thing3();
}
```

### <a href="#comments" class="anchor"></a>1.16. Comments

Use `/** …​ */` for multi-line comments.

``` highlight
// bad
// make() returns a new element
// based on the passed in tag name
//
// @param {String} tag
// @return {Element} element
function make(tag) {

  // ...stuff...

  return element;
}

// good
/**
 * make() returns a new element
 * based on the passed-in tag name
 */
function make(tag) {

  // ...stuff...

  return element;
}
```

Use `//` for single line comments. Place single line comments on a newline above the subject of the comment. Put an empty line before the comment unless it’s on the first line of a block.

``` highlight
// bad
const active = true;  // is current tab

// good
// is current tab
const active = true;

// bad
function getType() {
  console.log('fetching type...');
  // set the default type to 'no type'
  const type = this._type || 'no type';

  return type;
}

// good
function getType() {
  console.log('fetching type...');

  // set the default type to 'no type'
  const type = this._type || 'no type';

  return type;
}

// also good
function getType() {
  // set the default type to 'no type'
  const type = this._type || 'no type';

  return type;
}
```

Start all comments with a space to make it easier to read.

``` highlight
// bad
//is current tab
const active = true;

// good
// is current tab
const active = true;

// bad
/**
 *make() returns a new element
 *based on the passed-in tag name
 */
function make(tag) {

  // ...stuff...

  return element;
}

// good
/**
 * make() returns a new element
 * based on the passed-in tag name
 */
function make(tag) {

  // ...stuff...

  return element;
}
```

Prefixing your comments with `FIXME` or `TODO` helps other developers quickly understand if you’re pointing out a problem that needs to be revisited, or if you’re suggesting a solution to the problem that needs to be implemented. These are different than regular comments because they are actionable. The actions are `FIXME: — need to figure this out` or `TODO: — need to implement`.

Use `// FIXME:` to annotate problems.

``` highlight
class Calculator extends Abacus {
  constructor() {
    super();

    // FIXME: shouldn't use a global here
    total = 0;
  }
}
```

Use `// TODO:` to annotate solutions to problems.

``` highlight
class Calculator extends Abacus {
  constructor() {
    super();

    // TODO: total should be configurable by an options param
    this.total = 0;
  }
}
```

### <a href="#whitespace" class="anchor"></a>1.17. Whitespace

Use soft tabs set to 2 spaces.

``` highlight
// bad
function foo() {
∙∙∙∙const name;
}

// bad
function bar() {
∙const name;
}

// good
function baz() {
∙∙const name;
}
```

Place 1 space before the leading brace.

``` highlight
// bad
function test(){
  console.log('test');
}

// good
function test() {
  console.log('test');
}

// bad
dog.set('attr',{
  age: '1 year',
  breed: 'Bernese Mountain Dog',
});

// good
dog.set('attr', {
  age: '1 year',
  breed: 'Bernese Mountain Dog',
});
```

Place 1 space before the opening parenthesis in control statements (`if`, `while` etc.). Place no space between the argument list and the function name in function calls and declarations.

``` highlight
// bad
if(isJedi) {
  fight ();
}

// good
if (isJedi) {
  fight();
}

// bad
function fight () {
  console.log ('Swooosh!');
}

// good
function fight() {
  console.log('Swooosh!');
}
```

Set off operators with spaces.

``` highlight
// bad
const x=y+5;

// good
const x = y + 5;
```

End files with a single newline character.

``` highlight
// bad
import { es6 } from './AirbnbStyleGuide';
  // ...
export default es6;
```

``` highlight
// bad
import { es6 } from './AirbnbStyleGuide';
  // ...
export default es6;↵
↵
```

``` highlight
// good
import { es6 } from './AirbnbStyleGuide';
  // ...
export default es6;↵
```

Use indentation when making method chains. Use a leading dot, which emphasizes that the line is a method call, not a new statement.

``` highlight
// bad
$('#items').find('.selected').highlight().end().find('.open').updateCount();

// bad
const leds = stage.selectAll('.led').data(data);

// bad
$('#items').
  find('.selected').
    highlight().
    end().
  find('.open').
    updateCount();

// bad
$('#items')
  .find('.selected')
    .highlight()
    .end()
  .find('.open')
    .updateCount();

// bad
const leds = stage.selectAll('.led').data(data).enter().append('svg:svg').classed('led', true)
    .attr('width', (radius + margin) * 2).append('svg:g')
    .attr('transform', 'translate(' + (radius + margin) + ',' + (radius + margin) + ')')
    .call(tron.led);

// bad
const leds = stage.selectAll('.led')
    .data(data)
  .enter().append('svg:svg')
    .classed('led', true)
    .attr('width', (radius + margin) * 2)
  .append('svg:g')
    .attr('transform', 'translate(' + (radius + margin) + ',' + (radius + margin) + ')')
    .call(tron.led);

// good
$('#items')
  .find('.selected')
  .highlight()
  .end()
  .find('.open')
  .updateCount();

// good
const leds = stage.selectAll('.led')
                  .data(data)
                  .enter()
                  .append('svg:svg')
                  .classed('led', true)
                  .attr('width', (radius + margin) * 2)
                  .append('svg:g')
                  .attr('transform', 'translate(' + (radius + margin) + ',' + (radius + margin) + ')')
                  .call(tron.led);
```

Leave a blank line after blocks and before the next statement.

``` highlight
// bad
if (foo) {
  return bar;
}
return baz;

// good
if (foo) {
  return bar;
}

return baz;

// bad
const obj = {
  foo() {
  },
  bar() {
  },
};
return obj;

// good
const obj = {
  foo() {
  },

  bar() {
  },
};

return obj;

// bad
const arr = [
  function foo() {
  },
  function bar() {
  },
];
return arr;

// good
const arr = [
  function foo() {
  },

  function bar() {
  },
];

return arr;
```

Do not pad your blocks with blank lines.

``` highlight
// bad
function bar() {

  console.log(foo);

}

// also bad
if (baz) {

  console.log(qux);
} else {
  console.log(foo);

}

// good
function bar() {
  console.log(foo);
}

// good
if (baz) {
  console.log(qux);
} else {
  console.log(foo);
}
```

Do not add spaces inside parentheses.

``` highlight
// bad
function bar( foo ) {
  return foo;
}

// good
function bar(foo) {
  return foo;
}

// bad
if ( foo ) {
  console.log(foo);
}

// good
if (foo) {
  console.log(foo);
}
```

Do not add spaces inside brackets.

``` highlight
// bad
const foo = [ 1, 2, 3 ];
console.log(foo[ 0 ]);

// good
const foo = [1, 2, 3];
console.log(foo[0]);
```

Add spaces inside curly braces.

``` highlight
// bad
const foo = {clark: 'kent'};

// good
const foo = { clark: 'kent' };
```

Avoid having lines of code that are longer than 100 characters (including whitespace).

|     |                                                                                          |
|-----|------------------------------------------------------------------------------------------|
| **  | Why not? Per above, long strings are exempt from this rule, and should not be broken up. |

``` highlight
// bad
const foo = jsonData && jsonData.foo && jsonData.foo.bar && jsonData.foo.bar.baz && jsonData.foo.bar.baz.quux && jsonData.foo.bar.baz.quux.xyzzy;

// bad
$.ajax({ method: 'POST', url: 'https://some.com/', data: { name: 'John' } }).done(() => console.log('Congratulations!')).fail(() => console.log('You have failed this city.'));

// good
const foo = jsonData
  && jsonData.foo
  && jsonData.foo.bar
  && jsonData.foo.bar.baz
  && jsonData.foo.bar.baz.quux
  && jsonData.foo.bar.baz.quux.xyzzy;

// good
$.ajax({
  method: 'POST',
  url: 'https://some.com/',
  data: { name: 'John' },
})
  .done(() => console.log('Congratulations!'))
  .fail(() => console.log('You have failed this city.'));
```

### <a href="#commas" class="anchor"></a>1.18. Commas

Leading commas: Nope.

``` highlight
// bad
const story = [
    once
  , upon
  , aTime
];

// good
const story = [
  once,
  upon,
  aTime
];

// bad
const hero = {
    firstName: 'Ada'
  , lastName: 'Lovelace'
  , birthYear: 1815
  , superPower: 'computers'
};

// good
const hero = {
  firstName: 'Ada',
  lastName: 'Lovelace',
  birthYear: 1815,
  superPower: 'computers'
};
```

Additional trailing comma: Also nope.

``` highlight
// bad
const hero = {
  firstName: 'Dana',
  lastName: 'Scully',
};

// good
const hero = {
  firstName: 'Dana',
  lastName: 'Scully'
};

// bad
function createHero(
  firstName,
  lastName,
  inventorOf,
) {
  // does nothing
}

// good
function createHero(
  firstName,
  lastName,
  inventorOf
) {
  // does nothing
}

// good (note that a comma must not appear after a "rest" element)
function createHero(
  firstName,
  lastName,
  inventorOf,
  ...heroArgs
) {
  // does nothing
}

// bad
createHero(
  firstName,
  lastName,
  inventorOf,
);

// good
createHero(
  firstName,
  lastName,
  inventorOf
);

// good (note that a comma must not appear after a "rest" element)
createHero(
  firstName,
  lastName,
  inventorOf,
  ...heroArgs
)
```

### <a href="#semicolons" class="anchor"></a>1.19. Semicolons

Yup.

``` highlight
// bad
(function () {
  const name = 'Skywalker'
  return name
})()

// better, but legacy (guards against the function becoming an argument when two files with IIFEs are concatenated)
;(() => {
  const name = 'Skywalker';
  return name;
}());

// good
(function () {
  const name = 'Skywalker';
  return name;
}());
```

### <a href="#type_casting_coercion" class="anchor"></a>1.20. Type Casting & Coercion

Perform type coercion at the beginning of the statement.

Strings:

``` highlight
// => this.reviewScore = 9;

// bad
const totalScore = this.reviewScore + ''; // invokes this.reviewScore.valueOf()

// bad
const totalScore = this.reviewScore.toString(); // isn't guaranteed to return a string

// good
const totalScore = String(this.reviewScore);
```

Numbers: Use Number for type casting and parseInt always with a radix for parsing strings.

``` highlight
const inputValue = '4';

// bad
const val = new Number(inputValue);

// bad
const val = +inputValue;

// bad
const val = inputValue >> 0;

// bad
const val = parseInt(inputValue);

// good
const val = Number(inputValue);

// good
const val = parseInt(inputValue, 10);
```

If for whatever reason you are doing something wild and `parseInt` is your bottleneck and need to use Bitshift for performance reasons, leave a comment explaining why and what you’re doing.

``` highlight
// good
/**
 * parseInt was the reason my code was slow.
 * Bitshifting the String to coerce it to a
 * Number made it a lot faster.
 */
const val = inputValue >> 0;
```

**Note**: Be careful when using bitshift operations. Numbers are represented as 64-bit values, but bitshift operations always return a 32-bit integer. Bitshift can lead to unexpected behavior for integer values larger than 32 bits. Largest signed 32-bit Int is 2,147,483,647:

``` highlight
2147483647 >> 0 //=> 2147483647
2147483648 >> 0 //=> -2147483648
2147483649 >> 0 //=> -2147483647
```

Booleans:

``` highlight
const age = 0;

// bad
const hasAge = new Boolean(age);

// good
const hasAge = Boolean(age);

// best
const hasAge = !!age;
```

### <a href="#naming_conventions" class="anchor"></a>1.21. Naming Conventions

Avoid single letter names. Be descriptive with your naming.

``` highlight
// bad
function q() {
  // ...stuff...
}

// good
function query() {
  // ..stuff..
}
```

Use camelCase when naming objects, functions, and instances.

``` highlight
// bad
const OBJEcttsssss = {};
const this_is_my_object = {};
function c() {}

// good
const thisIsMyObject = {};
function thisIsMyFunction() {}
```

Use PascalCase only when naming constructors or classes.

``` highlight
// bad
function user(options) {
  this.name = options.name;
}

const bad = new user({
  name: 'nope',
});

// good
class User {
  constructor(options) {
    this.name = options.name;
  }
}

const good = new User({
  name: 'yup',
});
```

Do not use trailing or leading underscores.

|     |                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
|-----|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **  | Why? JavaScript does not have the concept of privacy in terms of properties or methods. Although a leading underscore is a common convention to mean “private”, in fact, these properties are fully public, and as such, are part of your public API contract. This convention might lead developers to wrongly think that a change won’t count as breaking, or that tests aren’t needed. tl;dr: if you want something to be “private”, it must not be observably present. |

``` highlight
// bad
this.__firstName__ = 'Panda';
this.firstName_ = 'Panda';
this._firstName = 'Panda';

// good
this.firstName = 'Panda';
```

Don’t save references to this. Use arrow functions or Function\#bind (less preferable).

``` highlight
// bad
function foo() {
  const self = this;
  return function () {
    console.log(self);
  };
}

// bad
function foo() {
  const that = this;
  return function () {
    console.log(that);
  };
}

// good
function foo() {
  return () => {
    console.log(this);
  };
}
```

A base filename should be dasherized name of its default export.

``` highlight
// file 1 contents
class CheckBox {
  // ...
}
export default CheckBox;

// file 2 contents
export default function fortyTwo() { return 42; }

// file 3 contents
export default function insideDirectory() {}

// in some other file
// bad
import CheckBox from './checkBox'; // PascalCase import/export, camelCase filename
import FortyTwo from './FortyTwo'; // PascalCase import/filename, camelCase export
import InsideDirectory from './InsideDirectory'; // PascalCase import/filename, camelCase export

// bad
import CheckBox from './check_box'; // PascalCase import/export, snake_case filename
import forty_two from './forty_two'; // snake_case import/filename, camelCase export
import inside_directory from './inside_directory'; // snake_case import, camelCase export
import index from './inside_directory/index'; // requiring the index file explicitly
import insideDirectory from './insideDirectory/index'; // requiring the index file explicitly

// good
import CheckBox from './check-box'; // PascalCase export/import/filename
import fortyTwo from './forty-two'; // camelCase export/import/filename
import insideDirectory from './inside-directory'; // camelCase export/import/directory name/implicit "index"
// ^ supports both inside-directory.js and inside-directory/index.js
```

Use camelCase when you export-default a function. Your filename should be dasherized function’s name.

``` highlight
function makeStyleGuide() {
}

export default makeStyleGuide;
```

Use PascalCase when you export a constructor / class / singleton / function library / bare object.

``` highlight
const StyleGuide = {
  es6: {
  }
};

export default StyleGuide;
```

Acronyms and initialisms should always be all capitalized, or all lowercased.

|     |                                                                      |
|-----|----------------------------------------------------------------------|
| **  | Why? Names are for readability, not to appease a computer algorithm. |

``` highlight
// bad
import SmsContainer from './containers/SmsContainer';

// bad
const HttpRequests = [
  // ...
];

// good
import SMSContainer from './containers/SMSContainer';

// good
const HTTPRequests = [
  // ...
];

// best
import TextMessageContainer from './containers/TextMessageContainer';

// best
const Requests = [
  // ...
];
```

### <a href="#accessors" class="anchor"></a>1.22. Accessors

Accessor functions for properties are not required.

Do not use JavaScript getters/setters as they cause unexpected side effects and are harder to test, maintain, and reason about. Instead, if you do make accessor functions, use `getVal()` and `setVal('hello')`.

``` highlight
// bad
class Dragon {
  get age() {
    // ...
  }

  set age(value) {
    // ...
  }
}

// good
class Dragon {
  getAge() {
    // ...
  }

  setAge(value) {
    // ...
  }
}
```

If the property/method is a `boolean`, use `isVal()` or `hasVal()`.

``` highlight
// bad
if (!dragon.age()) {
  return false;
}

// good
if (!dragon.hasAge()) {
  return false;
}
```

It’s okay to create `get()` and `set()` functions, but be consistent.

``` highlight
class Jedi {
  constructor(options = {}) {
    const lightsaber = options.lightsaber || 'blue';
    this.set('lightsaber', lightsaber);
  }

  set(key, val) {
    this[key] = val;
  }

  get(key) {
    return this[key];
  }
}
```

### <a href="#events" class="anchor"></a>1.23. Events

When attaching data payloads to events (whether DOM events or something more proprietary like Backbone events), pass a hash instead of a raw value. This allows a subsequent contributor to add more data to the event payload without finding and updating every handler for the event. For example, instead of:

``` highlight
// bad
$(this).trigger('listingUpdated', listing.id);

...

$(this).on('listingUpdated', (e, listingId) => {
  // do something with listingId
});
```

prefer:

``` highlight
// good
$(this).trigger('listingUpdated', { listingId: listing.id });

...

$(this).on('listingUpdated', (e, data) => {
  // do something with data.listingId
});
```

### <a href="#jquery" class="anchor"></a>1.24. jQuery

Prefix jQuery object variables with a `$`.

``` highlight
// bad
const sidebar = $('.sidebar');

// good
const $sidebar = $('.sidebar');

// good
const $sidebarBtn = $('.sidebar-btn');
```

Cache jQuery lookups.

``` highlight
// bad
function setSidebar() {
  $('.sidebar').hide();

  // ...stuff...

  $('.sidebar').css({
    'background-color': 'pink'
  });
}

// good
function setSidebar() {
  const $sidebar = $('.sidebar');
  $sidebar.hide();

  // ...stuff...

  $sidebar.css({
    'background-color': 'pink'
  });
}
```

For DOM queries use Cascading `$('.sidebar ul')` or parent &gt; child `$('.sidebar > ul')`.

Use `find` with scoped jQuery object queries.

``` highlight
// bad
$('ul', '.sidebar').hide();

// bad
$('.sidebar').find('ul').hide();

// good
$('.sidebar ul').hide();

// good
$('.sidebar > ul').hide();

// good
$sidebar.find('ul').hide();
```

<a href="#ember_style_guide" class="anchor"></a>2. Ember Style Guide
--------------------------------------------------------------------

### <a href="#organizing_modules" class="anchor"></a>2.1. Organizing modules

1.  Injections

2.  Properties

3.  Single line CP

4.  Multiline CP

5.  Lifecycle hooks

6.  Methods

7.  Actions

``` highlight
export default Component.extend({

  // Injections
  user: inject.service(),

  // Defaults
  tagName: 'span',

  // Single line CP
  post: alias('myPost'),

  // Multiline CP
  authorName: computed('author.{firstName,lastName}', {
    get() {
      // Code
    },

    set() {
      // Code
    }
  }),

  // Lifecycle hooks
  didReceiveAttrs() {
    this._super(...arguments);
    // code
  },

  // Methods
  someMethod() {
    // code
  },

  // Actions
  actions: {
    someAction() {
      // Code
    }
  }
});
```

<span>Collapse/Expand</span>

Last updated 2018-03-22 14:50:02 +06