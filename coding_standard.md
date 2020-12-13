# Wiridis Javascript Coding standard

## Table of Contents
- [Preliminaries](#preliminaries)
    * [`General guide`](#general-guide)
- [Source file basics](#source-file-basics)
    * [`File name`](#file-name)
    * [`File encoding`](#file-encoding)
    * [`Special characters`](#special-characters)
- [Formatting](#formatting)
    * [`Braces`](#braces)
    * [`Block indentation`](#block-indentation)
    * [`Statements`](#statements)
    * [`Column limit`](#column-limit)
    * [`Line-wrapping`](#line-wrapping)
    * [`Whitespace`](#whitespace)  
    * [`Comments`](#comments)  
- [Language features](#language-features)
    * [`Local variable declarations`](#local-variable-declaration)
    * [`Array literals`](#array-literals)     
    * [`Object literals`](#object-literals)
    * [`Classes`](#classes)
    * [`Functions`](#funstions)
    * [`String literals`](#string-literals)
    * [`Number literals`](#number-literals)
    * [`Control structures`](#control-structures)
    * [`this`](#this)
    * [`Equality checks`](#equality-checks)
    * [`Disallowed features`](#disallowed-features)
- [Naming](#naming)
    * [`Rules common to all identifiers`](#rules-common-to-all-identifiers)
    * [`Rules by identifier type`](#rules-by-identifier-type)
    * [`Camel case: defined`](#camel-case-defined)
- [JSDoc](#jsdoc)
    * [`General form`](#General-form)
    * [`Markdown`](#markdown)
    * [`JSDoc tags`](#jsdoc-tags)
    * [`Line wrapping`](#line-wrapping)
    * [`Top/file-level comments`](#topfile-level-comments)
    * [`Class comments`](#class-comments)
    * [`Enum and typedef comments`](#enum-and-typedef-comments)
    * [`Method and function comments`](#method-and-function-comments)
    * [`Property comments`](#property-comments)
    * [`Type annotations`](#type-annotations)
    * [`Visibility annotations`](#visibility-annotations)
- [Policies](#policies)
    * [`Issues unspecified by Wiridis Style: Be Consistent!`](#issues-unspecified-by-wiridis-style-be-consistent)
    * [`Compiler warnings`](#compiler-warnings)
    * [`Deprecation`](#deprecation)
- [Linters setup](#linters-setup)

## Preliminaries
This document serves as the complete definition of Wiridis’s coding standards for source code in the JavaScript programming language. This coding standard is based on [Google Javascript Style Guide](https://google.github.io/styleguide/jsguide.html#terminology-notes) with exclusion of irrelevant rules, such as `googl.module`, which are not applicable to Wiridis' standard. 

The following are parts in Wiridis that defer from Google Style Guide:
- [`Comments`](#comments)

This guide will be based on ES6 or ECMAScript 2015. 

A JavaScript source file is described as being in Wiridis style if and only if it adheres to the rules herein.

Like other programming style guides, the issues covered span not only aesthetic issues of formatting, but other types of 
conventions or coding standards as well. However, this document focuses primarily on the hard-and-fast rules 
that we follow universally, and avoids giving advice that isn't clearly enforceable (whether by human or tool).

[say use es6, will polyfill with Babel, taken from Google Coding Standard+ link, what is changed(why yes/not-preliminary), eslint file coding, add emojis + color]

### General guide

Quick guide to Javascript coding standard:

![JS coding standard quick summary](Javascript%20coding%20standard%20cheatsheet%20diagram.JPG)

## Source file basics

### File name
File names must be all lowercase and may include underscores (`_`) or dashes (`-`), but no additional punctuation. 
Follow the convention that your project uses. Filenames’ extension must be `.js`.

### File encoding
Source files are encoded in **UTF-8**.

### Special characters
#### Whitespace characters
Aside from the line terminator sequence, the ASCII horizontal space character (0x20) is the only whitespace character that appears anywhere in a source file. This implies that
1. All other whitespace characters in string literals are escaped, and
2. Tab characters are **not** used for indentation.
#### Special escape sequences
For any character that has a special escape sequence (`\'`, `\"`, `\\`, `\b`, `\f`, `\n`, `\r`, `\t`, `\v`),
that sequence is used rather than the corresponding numeric escape (e.g `\x0a`, `\u000a`, or `\u{a}`).
Legacy octal escapes are never used.
  
## Formatting

### Braces
#### Control structures
Braces are **required for all control structures** (i.e. `if`, `else`, `for`, `do`, `while`, as well as any others), 
even if the body contains only a single statement. The first statement of a non-empty block must begin on its own line.

**Bad** example:
```
if (someVeryLongCondition())
  doSomething();

for (let i = 0; i < foo.length; i++) bar(foo[i]);
```

**Good** example:
```
if (someVeryLongCondition())
  doSomething();

for (let i = 0; i < foo.length; i++)
  bar(foo[i]);
```
#### Nonempty blocks
Braces follow the Kernighan and Ritchie style (Egyptian brackets) for nonempty blocks and block-like constructs:
1. No line break before the opening brace.
2. Line break after the opening brace.
3. Line break before the closing brace.
4. Line break after the closing brace if that brace terminates a statement or the body of a function or class statement, or a class method. 
Specifically, there is no line break after the brace if it is followed by `else`, `catch`, `while`, or a comma, semicolon, or right-parenthesis.

**Good** example :white_check_mark: : 
```
class InnerClass {
  constructor() {}

  /** @param {number} foo */
  method(foo) {
    if (condition(foo)) {
      try {
        // Note: this might fail.
        something();
      } catch (err) {
        recover();
      }
    }
  }
}
```

### Block indentation
Each time a new block or block-like construct is opened, the indent increases by **two(2)** spaces. 
When the block ends, the indent returns to the previous indent level. The indent level applies to both code and comments throughout the block. 
(See the example in [`Nonempty blocks: K&R style`](#nonempty-blocks)).

### Statements
#### One statement per line
Each statement is followed by a line-break.

#### Semicolons are required
Every statement must be terminated with a semicolon.

### Column limit
JavaScript code has a column limit of **80 characters**. 
Except as noted below, any line that would exceed this limit must be line-wrapped, as explained in [`Line wrapping`](#line-wrapping).

**Exceptions:**
- A long URL which should be clickable in source.
- A shell command intended to be copied-and-pasted.
- A long string literal which may need to be copied or searched for wholly (e.g., a long file path).

### Line wrapping
There is no comprehensive, deterministic formula showing exactly how to line-wrap in every situation. Very often there are several valid ways to line-wrap the same piece of code.
The prime directive of line-wrapping is: prefer to break at a **higher syntactic level**.

Preferred :white_check_mark: :
```
currentEstimate =
    calc(currentEstimate + x * currentEstimate) /
        2.0;
```

Discouraged:
```
currentEstimate = calc(currentEstimate + x *
    currentEstimate) / 2.0;
```

Operators are wrapped as follows:
1. When a line is broken at an operator the break comes after the symbol.
    - This does not apply to the dot (`.`), which is not actually an operator.
2. A method or constructor name stays attached to the open parenthesis (`(`) that follows it.
3. A comma (`,`) stays attached to the token that precedes it.

When line-wrapping, each line after the first (each continuation line) is indented at least +4 from the original line, unless it falls under the rules of block indentation.

When there are multiple continuation lines, indentation may be varied beyond **+4** as appropriate. 
In general, continuation lines at a deeper syntactic level are indented by larger multiples of 4, and two lines use the same indentation level if and only if they begin with syntactically parallel elements.

### Whitespace
#### Vertical whitespace
A single blank line appears:
1. Between consecutive methods in a class or object literal
2. Within method bodies, sparingly to create **logical groupings** of statements. Blank lines at the start or end of a function body are not allowed.

#### Horizontal whitespace
Use of horizontal whitespace depends on location, and falls into three broad categories: leading (at the start of a line), trailing (at the end of a line), and internal. Leading whitespace (i.e., indentation) is addressed elsewhere. Trailing whitespace is forbidden.

Beyond where required by the language or other style rules, and apart from literals, comments, and JSDoc, a single internal ASCII space also appears in the following places **only**.
1. Separating any reserved word (such as `if`, `for`, or `catch`) except for `function` and `super`, from an open parenthesis (`(`) that follows it on that line.
2. Separating any reserved word (such as `else` or `catch`) from a closing curly brace (`}`) that precedes it on that line.
3. Before any open curly brace (`{`), with two exceptions:
    - Before an object literal that is the first argument of a function or the first element in an array literal (e.g. foo({a: [{c: d}]})).
    - In a template expansion, as it is forbidden by the language (e.g. valid: `ab${1 + 2}cd`, invalid: `xy$ {3}z`).
4. On both sides of any binary or ternary operator.
5. After a comma (`,`) or semicolon (`;`). Note that spaces are never allowed before these characters.
6. After the colon (`:`) in an object literal.
7. On both sides of the double slash (`//`) that begins an end-of-line comment. Here, multiple spaces are allowed, but not required.
8. After an open-block comment character and on both sides of close characters (e.g. for short-form type declarations, casts, and parameter name comments: 
`this.foo = /** @type {number} */ (bar);` or `function(/** string */ foo) {;` or `baz(/* buzz= */ true)`).

#### Function whitespace
Prefer to put all function arguments on the same line as the function name. If doing so would exceed the 80-column limit, the arguments must be line-wrapped in a readable way. To save space, you may wrap as close to 80 as possible, or put each argument on its own line to enhance readability. Indentation should be four spaces. Aligning to the parenthesis is allowed, but discouraged. Below are the most common patterns for argument wrapping:
```
/* 
*  Arguments start on a new line, indented four spaces. Preferred when the
*  arguments don't fit on the same line with the function name (or the keyword
*  "function") but fit entirely on the second line. Works with very long
*  function names, survives renaming without reindenting, low on space.
*/
doSomething(
    descriptiveArgumentOne, descriptiveArgumentTwo, descriptiveArgumentThree) {
  // …
}

// If the argument list is longer, wrap at 80. Uses less vertical space,
// but violates the rectangle rule and is thus not recommended.
doSomething(veryDescriptiveArgumentNumberOne, veryDescriptiveArgumentTwo,
    tableModelEventHandlerProxy, artichokeDescriptorAdapterIterator) {
  // …
}

// Four-space, one argument per line.  Works with long function names,
// survives renaming, and emphasizes each argument.
doSomething(
    veryDescriptiveArgumentNumberOne,
    veryDescriptiveArgumentTwo,
    tableModelEventHandlerProxy,
    artichokeDescriptorAdapterIterator) {
  // …
}
```

### Comments
This section addresses implementation comments. JSDoc is addressed separately in [`JSDoc`](#jsdoc).

#### Block comment style
Block comments are indented at the same level as the surrounding code. They may be in `/* … */`-style (for multipline) or `//`-style (for single line). For multi-line `/* … */` comments, subsequent lines must start with `*` aligned with the `*` on the previous line.

```
/*
 * This is
 * okay.
 */
 
 // This is okay.
```

Do not use JSDoc (`/** … */`) for implementation comments.

#### Parameter name comments
“Parameter name” comments should be used whenever the value and method name do not sufficiently convey the meaning, and refactoring the method to be clearer is infeasible . Their preferred format is before the value with =:

```
someFunction(obviousParam, /* shouldRender= */ true, /* name= */ 'hello');
```
For consistency with surrounding code you may put them after the value without =:

```
someFunction(obviousParam, true /* shouldRender */, 'hello' /* name */);
```

Do not use JSDoc (`/** … */`) for implementation comments.

## Language features
JavaScript includes many dubious (and even dangerous) features. This section delineates which features may or may not be used, and any additional constraints on their use.

### Local variables
#### Use `const` and `let`
Declare all local variables with either `const` or `let`. Use `const` by default, unless a variable needs to be reassigned. The `var` keyword must **not** be used.

#### One variable per declaration
Every local variable declaration declares only one variable: declarations such as `let a = 1, b = 2;` are not used.

#### Declared when needed, initialized as soon as possible
Local variables are not habitually declared at the start of their containing block or block-like construct. Instead, local variables are declared close to the point they are first used (within reason), to minimize their scope.

#### Declare types as needed
JSDoc type annotations may be added either on the line above the declaration, or else inline before the variable name if no other JSDoc is present.

**Good** example :white_check_mark: :
```
const /** !Array<number> */ data = [];

/**
 * Some description.
 * @type {!Array<number>}
 */
const data = [];
```

Mixing inline and JSDoc styles is not allowed: the compiler will only process the first JsDoc and the inline annotations will be lost.


**Bad** example:
```
/** Some description. */
const /** !Array<number> */ data = [];
```

### Array literals
#### Use trailing commas
Include a trailing comma whenever there is a line break between the final element and the closing bracket.

Example:
```
const values = [
  'first value',
  'second value',
];
```

#### Do not use the `Array` constructor
The constructor is error-prone if arguments are added or removed. Use a literal instead.

**Bad** example:
```
const a1 = new Array(x1, x2, x3);
const a2 = new Array(x1, x2);
const a3 = new Array(x1);
const a4 = new Array();
```

This works as expected except for the third case: if `x1` is a whole number then a3 is an array of size `x1` where all elements are undefined. If `x1` is any other number, then an exception will be thrown, and if it is anything else then it will be a single-element array.

**Good** example :white_check_mark: :
```
const a1 = [x1, x2, x3];
const a2 = [x1, x2];
const a3 = [x1];
const a4 = [];
```

Explicitly allocating an array of a given length using new Array(length) is allowed when appropriate.

#### Non-numeric properties
Do not define or use non-numeric properties on an array (other than `length`). Use a `Map` (or `Object`) instead.

#### Destructuring

Array literals may be used on the left-hand side of an assignment to perform destructuring (such as when unpacking multiple values from a single array or iterable). A final rest element may be included (with no space between the `...` and the variable name). Elements should be omitted if they are unused.

    const [a, b, c, ...rest] = generateResults();
    let [, b,, d] = someArray;
    

Destructuring may also be used for function parameters (note that a parameter name is required but ignored). Always specify `[]` as the default value if a destructured array parameter is optional, and provide default values on the left hand side:

    /** @param {!Array<number>=} param1 */
    function optionalDestructuring([a = 4, b = 2] = []) { … };
    

**Bad** example:

    function badDestructuring([a, b] = [4, 2]) { … };
    

#### Spread operator

Array literals may include the spread operator (`...`) to flatten elements out of one or more other iterables. The spread operator should be used instead of more awkward constructs with `Array.prototype`. There is no space after the `...`.

Example:

    [...foo]   // preferred over Array.prototype.slice.call(foo)
    [...foo, ...bar]   // preferred over foo.concat(bar)
    

### Object literals

#### Use trailing commas

Include a trailing comma whenever there is a line break between the final property and the closing brace.

#### Do not use the `Object` constructor

While `Object` does not have the same problems as `Array`, it is still **disallowed** for consistency. Use an object literal (`{}` or `{a: 0, b: 1, c: 2}`) instead.

#### Do not mix quoted and unquoted keys

Object literals may represent either _structs_ (with unquoted keys and/or symbols) or _dicts_ (with quoted and/or computed keys). Do not mix these key types in a single object literal.

**Disallowed:**

    {
      width: 42, // struct-style unquoted key
      'maxWidth': 43, // dict-style quoted key
    }
    

This also extends to passing the property name to functions, like `hasOwnProperty`. In particular, doing so will break in compiled code because the compiler cannot rename/obfuscate the string literal.

**Disallowed:**

    /** @type {{width: number, maxWidth: (number|undefined)}} */
    const o = {width: 42};
    if (o.hasOwnProperty('maxWidth')) {
      ...
    }
    

This is best implemented as:

    /** @type {{width: number, maxWidth: (number|undefined)}} */
    const o = {width: 42};
    if (o.maxWidth != null) {
      ...
    }
    

#### Computed property names

Computed property names (e.g., `{['key' + foo()]: 42}`) are allowed, and are considered dict-style (quoted) keys (i.e., must not be mixed with non-quoted keys) unless the computed property is a [symbol](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Symbol) (e.g., `[Symbol.iterator]`). Enum values may also be used for computed keys, but should not be mixed with non-enum keys in the same literal.

#### Method shorthand

Methods can be defined on object literals using the method shorthand (`{method() {… }}`) in place of a colon immediately followed by a `function` or arrow function literal.

Example:

    return {
      stuff: 'candy',
      method() {
        return this.stuff;  // Returns 'candy'
      },
    };
    

Note that `this` in a method shorthand or `function` refers to the object literal itself whereas `this` in an arrow function refers to the scope outside the object literal.

Example:

    class {
      getObjectLiteral() {
        this.stuff = 'fruit';
        return {
          stuff: 'candy',
          method: () => this.stuff,  // Returns 'fruit'
        };
      }
    }
    

#### Shorthand properties

Shorthand properties are allowed on object literals.

Example:

    const foo = 1;
    const bar = 2;
    const obj = {
      foo,
      bar,
      method() { return this.foo + this.bar; },
    };
    assertEquals(3, obj.method());
    

#### Destructuring

Object destructuring patterns may be used on the left-hand side of an assignment to perform destructuring and unpack multiple values from a single object.

Destructured objects may also be used as function parameters, but should be kept as simple as possible: a single level of unquoted shorthand properties. Deeper levels of nesting and computed properties may not be used in parameter destructuring. Specify any default values in the left-hand-side of the destructured parameter (`{str = 'some default'} = {}`, rather than `{str} = {str: 'some default'}`), and if a destructured object is itself optional, it must default to `{}`. The JSDoc for the destructured parameter may be given any name (the name is unused but is required by the compiler).

Example:

    /**
     * @param {string} ordinary
     * @param {{num: (number|undefined), str: (string|undefined)}=} param1
     *     num: The number of times to do something.
     *     str: A string to do stuff to.
     */
    function destructured(ordinary, {num, str = 'some default'} = {})
    

Disallowed:

    /** @param {{x: {num: (number|undefined), str: (string|undefined)}}} param1 */
    function nestedTooDeeply({x: {num, str}}) {};
    /** @param {{num: (number|undefined), str: (string|undefined)}=} param1 */
    function nonShorthandProperty({num: a, str: b} = {}) {};
    /** @param {{a: number, b: number}} param1 */
    function computedKey({a, b, [a + b]: c}) {};
    /** @param {{a: number, b: string}=} param1 */
    function nontrivialDefault({a, b} = {a: 2, b: 4}) {};
    

#### Enums

Enumerations are defined by adding the `@enum` annotation to an object literal. Additional properties may not be added to an enum after it is defined. Enums must be constant, and all enum values must be deeply immutable.

    /**
     * Supported temperature scales.
     * @enum {string}
     */
    const TemperatureScale = {
      CELSIUS: 'celsius',
      FAHRENHEIT: 'fahrenheit',
    };
    
    /**
     * An enum with two options.
     * @enum {number}
     */
    const Option = {
      /** The option used shall have been the first. */
      FIRST_OPTION: 1,
      /** The second among two options. */
      SECOND_OPTION: 2,
    };
    

### Classes

#### Constructors

Constructors are optional. Subclass constructors must call `super()` before setting any fields or otherwise accessing `this`. Interfaces should declare non-method properties in the constructor.

#### Fields

Set all of a concrete object’s fields (i.e. all properties other than methods) in the constructor. Annotate fields that are never reassigned with `@const` (these need not be deeply immutable). Annotate non-public fields with the proper visibility annotation (`@private`, `@protected`, `@package`), and end all `@private` fields' names with an underscore. Fields are never set on a concrete class' `prototype`.

Example:

    class Foo {
      constructor() {
        /** @private @const {!Bar} */
        this.bar_ = computeBar();
    
        /** @protected @const {!Baz} */
        this.baz = computeBaz();
      }
    }
    

#### Computed properties

Computed properties may only be used in classes when the property is a symbol. Dict-style properties (that is, quoted or computed non-symbol keys, as defined in [Do not mix quoted and unquoted keys](#do-not-mix-quoted-and-unquoted-keys)) are not allowed. A `[Symbol.iterator]` method should be defined for any classes that are logically iterable. Beyond this, `Symbol` should be used sparingly.

Tip: be careful of using any other built-in symbols (e.g., `Symbol.isConcatSpreadable`) as they are not polyfilled by the compiler and will therefore not work in older browsers.

#### Static methods

Where it does not interfere with readability, prefer module-local functions over private static methods.

Static methods should only be called on the base class itself. Static methods should not be called on variables containing a dynamic instance that may be either the constructor or a subclass constructor (and must be defined with `@nocollapse` if this is done), and must not be called directly on a subclass that doesn’t define the method itself.

Disallowed:

    class Base { /** @nocollapse */ static foo() {} }
    class Sub extends Base {}
    function callFoo(cls) { cls.foo(); }  // discouraged: don't call static methods dynamically
    Sub.foo();  // Disallowed: don't call static methods on subclasses that don't define it themselves
    

#### Old-style class declarations

While ES6 classes are preferred, there are cases where ES6 classes may not be feasible. For example:

1.  If there exist or will exist subclasses, including frameworks that create subclasses, that cannot be immediately changed to use ES6 class syntax. If such a class were to use ES6 syntax, all downstream subclasses not using ES6 class syntax would need to be modified.
    
2.  Frameworks that require a known `this` value before calling the superclass constructor, since constructors with ES6 super classes do not have access to the instance `this` value until the call to `super` returns.

In all other ways the style guide still applies to this code: `let`, `const`, default parameters, rest, and arrow functions should all be used when appropriate.

#### Do not manipulate `prototype`s directly

The `class` keyword allows clearer and more readable class definitions than defining `prototype` properties. Ordinary implementation code has no business manipulating these objects, though they are still useful for defining classes as defined in [Old-style class declarations](#old-style-class-declarations). Mixins and modifying the prototypes of builtin objects are explicitly forbidden.

#### Getters and Setters

Do not use [JavaScript getter and setter properties](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/get). They are potentially surprising and difficult to reason about, and have limited support in the compiler. Provide ordinary methods instead.

#### Overriding toString

The `toString` method may be overridden, but must always succeed and never have visible side effects.

Tip: Beware, in particular, of calling other methods from toString, since exceptional conditions could lead to infinite loops.

#### Interfaces

Interfaces may be declared with `@interface` or `@record`. Interfaces declared with `@record` can be explicitly (i.e. via `@implements`) or implicitly implemented by a class or object literal.

All non-static method bodies on an interface must be empty blocks. Fields must be declared as uninitialized members in the class constructor.

Example:

    /**
     * Something that can frobnicate.
     * @record
     */
    class Frobnicator {
      constructor() {
        /** @type {number} The number of attempts before giving up. */
        this.attempts;
      }
    
      /**
       * Performs the frobnication according to the given strategy.
       * @param {!FrobnicationStrategy} strategy
       */
      frobnicate(strategy) {}
    }
    
    

#### Abstract Classes

Use abstract classes when appropriate. Abstract classes and methods must be annotated with `@abstract`. See [abstract classes and methods](https://github.com/google/closure-compiler/wiki/@abstract-classes-and-methods).

### Functions

#### Top-level functions

Top-level functions may be defined directly on the `exports` object, or else declared locally and optionally exported.

Examples:

    /** @param {string} str */
    exports.processString = (str) => {
      // Process the string.
    };
    

    /** @param {string} str */
    const processString = (str) => {
      // Process the string.
    };
    
    exports = {processString};
    

#### Nested functions and closures

Functions may contain nested function definitions. If it is useful to give the function a name, it should be assigned to a local `const`.

#### Arrow functions

Arrow functions provide a concise function syntax and simplify scoping `this` for nested functions. Prefer arrow functions over the `function` keyword, particularly for nested functions (but see [Method shorthand](#method-shorthand)).

Prefer arrow functions over other `this` scoping approaches such as `f.bind(this)` and `const self = this`. Arrow functions are particularly useful for calling into callbacks as they permit explicitly specifying which parameters to pass to the callback whereas binding will blindly pass along all parameters.

The left-hand side of the arrow contains zero or more parameters. Parentheses around the parameters are optional if there is only a single non-destructured parameter. When parentheses are used, inline parameter types may be specified.

The right-hand side of the arrow contains the body of the function. By default the body is a block statement (zero or more statements surrounded by curly braces). The body may also be an implicitly returned single expression if either: the program logic requires returning a value, or the `void` operator precedes a single function or method call (using `void` ensures `undefined` is returned, prevents leaking values, and communicates intent). The single expression form is preferred if it improves readability (e.g., for short or simple expressions).

**Good** examples:

    /**
     * Arrow functions can be documented just like normal functions.
     * @param {number} numParam A number to add.
     * @param {string} strParam Another number to add that happens to be a string.
     * @return {number} The sum of the two parameters.
     */
    const moduleLocalFunc = (numParam, strParam) => numParam + Number(strParam);
    
    // Uses the single expression syntax with `void` because the program logic does
    // not require returning a value.
    getValue((result) => void alert(`Got ${result}`));
    
    class CallbackExample {
      constructor() {
        /** @private {number} */
        this.cachedValue_ = 0;
    
        // For inline callbacks, you can use inline typing for parameters.
        // Uses a block statement because the value of the single expression should
        // not be returned and the expression is not a single function call.
        getNullableValue((/** ?number */ result) => {
          this.cachedValue_ = result == null ? 0 : result;
        });
      }
    }
    

**Bad** example:

    /**
     * A function with no params and no returned value.
     * This single expression body usage is illegal because the program logic does
     * not require returning a value and we're missing the `void` operator.
     */
    const moduleLocalFunc = () => anotherFunction();
    

#### Generators

Generators enable a number of useful abstractions and may be used as needed.

When defining generator functions, attach the `*` to the `function` keyword when present, and separate it with a space from the name of the function. When using delegating yields, attach the `*` to the `yield` keyword.

Example:

    /** @return {!Iterator<number>} */
    function* gen1() {
      yield 42;
    }
    
    /** @return {!Iterator<number>} */
    const gen2 = function*() {
      yield* gen1();
    }
    
    class SomeClass {
      /** @return {!Iterator<number>} */
      * gen() {
        yield 42;
      }
    }
    

#### Parameter and return types

Function parameters and return types should usually be documented with JSDoc annotations.

##### Default parameters

Optional parameters are permitted using the equals operator in the parameter list. Optional parameters must include spaces on both sides of the equals operator, be named exactly like required parameters (i.e., not prefixed with `opt_`), use the `=` suffix in their JSDoc type, come after required parameters, and not use initializers that produce observable side effects. All optional parameters for concrete functions must have default values, even if that value is `undefined`. In contrast to concrete functions, abstract and interface methods must omit default parameter values.

Example:

    /**
     * @param {string} required This parameter is always needed.
     * @param {string=} optional This parameter can be omitted.
     * @param {!Node=} node Another optional parameter.
     */
    function maybeDoSomething(required, optional = '', node = undefined) {}
    
    /** @interface */
    class MyInterface {
      /**
       * Interface and abstract methods must omit default parameter values.
       * @param {string=} optional
       */
      someMethod(optional) {}
    }
    

Use default parameters sparingly. Prefer destructuring (as in [Destructuring](#destructuring)) to create readable APIs when there are more than a small handful of optional parameters that do not have a natural order.

Note: Unlike Python's default parameters, it is okay to use initializers that return new mutable objects (such as `{}` or `[]`) because the initializer is evaluated each time the default value is used, so a single object won't be shared across invocations.

Tip: While arbitrary expressions including function calls may be used as initializers, these should be kept as simple as possible. Avoid initializers that expose shared mutable state, as that can easily introduce unintended coupling between function calls.

##### Rest parameters

Use a _rest_ parameter instead of accessing `arguments`. Rest parameters are typed with a `...` prefix in their JSDoc. The rest parameter must be the last parameter in the list. There is no space between the `...` and the parameter name. Do not name the rest parameter `var_args`. Never name a local variable or parameter `arguments`, which confusingly shadows the built-in name.

Example:

    /**
     * @param {!Array<string>} array This is an ordinary parameter.
     * @param {...number} numbers The remainder of arguments are all numbers.
     */
    function variadic(array, ...numbers) {}
    

#### Generics

Declare generic functions and methods when necessary with `@template TYPE` in the JSDoc above the function or method definition.

#### Spread operator

Function calls may use the spread operator (`...`). Prefer the spread operator to `Function.prototype.apply` when an array or iterable is unpacked into multiple parameters of a variadic function. There is no space after the `...`.

Example:

    function myFunction(...elements) {}
    myFunction(...array, ...iterable, ...generator());
    

### String literals

#### Use single quotes

Ordinary string literals are delimited with single quotes (`'`), rather than double quotes (`"`).

#### Template literals

Use template literals (delimited with `` ` ``) over complex string concatenation, particularly if multiple string literals are involved. Template literals may span multiple lines.

If a template literal spans multiple lines, it does not need to follow the indentation of the enclosing block, though it may if the added whitespace does not matter.

Example:

    function arithmetic(a, b) {
      return `Here is a table of arithmetic operations:
    ${a} + ${b} = ${a + b}
    ${a} - ${b} = ${a - b}
    ${a} * ${b} = ${a * b}
    ${a} / ${b} = ${a / b}`;
    }
    

#### No line continuations

Do not use _line continuations_ (that is, ending a line inside a string literal with a backslash) in either ordinary or template string literals. Even though ES5 allows this, it can lead to tricky errors if any trailing whitespace comes after the slash, and is less obvious to readers.

**Bad** example:

    const longString = 'This is a very long string that far exceeds the 80 \
        column limit. It unfortunately contains long stretches of spaces due \
        to how the continued lines are indented.';
    

Instead, write

    const longString = 'This is a very long string that far exceeds the 80 ' +
        'column limit. It does not contain long stretches of spaces since ' +
        'the concatenated strings are cleaner.';
    

### Number literals

Numbers may be specified in decimal, hex, octal, or binary. Use exactly `0x`, `0o`, and `0b` prefixes, with lowercase letters, for hex, octal, and binary, respectively. Never include a leading zero unless it is immediately followed by `x`, `o`, or `b`.

### Control structures

#### For loops

With ES6, the language now has three different kinds of `for` loops. All may be used, though `for`\-`of` loops should be preferred when possible.

`for`\-`in` loops may only be used on dict-style objects (see [Do not mix quoted and unquoted keys](#do-not-mix-quoted-and-unquoted-keys)), and should not be used to iterate over an array. `Object.prototype.hasOwnProperty` should be used in `for`\-`in` loops to exclude unwanted prototype properties. Prefer `for`\-`of` and `Object.keys` over `for`\-`in` when possible.

#### Exceptions

Exceptions are an important part of the language and should be used whenever exceptional cases occur. Always throw `Error`s or subclasses of `Error`: never throw string literals or other objects. Always use `new` when constructing an `Error`.

This treatment extends to `Promise` rejection values as `Promise.reject(obj)` is equivalent to `throw obj;` in async functions.

Custom exceptions provide a great way to convey additional error information from functions. They should be defined and used wherever the native `Error` type is insufficient.

Prefer throwing exceptions over ad-hoc error-handling approaches (such as passing an error container reference type, or returning an object with an error property).

##### Empty catch blocks

It is very rarely correct to do nothing in response to a caught exception. When it truly is appropriate to take no action whatsoever in a catch block, the reason this is justified is explained in a comment.

    try {
      return handleNumericResponse(response);
    } catch (ok) {
      // it's not numeric; that's fine, just continue
    }
    return handleTextResponse(response);
    

Disallowed:

      try {
        shouldFail();
        fail('expected an error');
      } catch (expected) {
      }
    

#### Switch statements

Terminology Note: Inside the braces of a switch block are one or more statement groups. Each statement group consists of one or more switch labels (either `case FOO:` or `default:`), followed by one or more statements.

##### Fall-through: commented

Within a switch block, each statement group either terminates abruptly (with a `break`, `return` or `throw`n exception), or is marked with a comment to indicate that execution will or might continue into the next statement group. Any comment that communicates the idea of fall-through is sufficient (typically `// fall through`). This special comment is not required in the last statement group of the switch block.

Example:

    switch (input) {
      case 1:
      case 2:
        prepareOneOrTwo();
      // fall through
      case 3:
        handleOneTwoOrThree();
        break;
      default:
        handleLargeNumber(input);
    }
    

##### The `default` case is present

Each switch statement includes a `default` statement group, even if it contains no code. The `default` statement group must be last.

### this

Only use `this` in class constructors and methods, in arrow functions defined within class constructors and methods, or in functions that have an explicit `@this` declared in the immediately-enclosing function’s JSDoc.

Never use `this` to refer to the global object, the context of an `eval`, the target of an event, or unnecessarily `call()`ed or `apply()`ed functions.

### Equality Checks

Use identity operators (`===`/`!==`) except in the cases documented below.

#### Exceptions Where Coercion is Desirable

Catching both `null` and `undefined` values:

    if (someObjectOrPrimitive == null) {
      // Checking for null catches both null and undefined for objects and
      // primitives, but does not catch other falsy values like 0 or the empty
      // string.
    }
    

### Disallowed features

#### with

Do not use the `with` keyword. It makes your code harder to understand and has been banned in strict mode since ES5.

#### Dynamic code evaluation

Do not use `eval` or the `Function(...string)` constructor (except for code loaders). These features are potentially dangerous and simply do not work in CSP environments.

#### Automatic semicolon insertion

Always terminate statements with semicolons (except function and class declarations, as noted above).

#### Non-standard features

Do not use non-standard features. This includes old features that have been removed (e.g., `WeakMap.clear`), new features that are not yet standardized (e.g., the current TC39 working draft, proposals at any stage, or proposed but not-yet-complete web standards), or proprietary features that are only implemented in some browsers. Use only features defined in the current ECMA-262 or WHATWG standards. (Note that projects writing against specific APIs, such as Chrome extensions or Node.js, can obviously use those APIs). Non-standard language “extensions” (such as those provided by some external transpilers) are forbidden.

#### Wrapper objects for primitive types

Never use `new` on the primitive object wrappers (`Boolean`, `Number`, `String`, `Symbol`), nor include them in type annotations.

Disallowed:

    const /** Boolean */ x = new Boolean(false);
    if (x) alert(typeof x);  // alerts 'object' - WAT?
    

The wrappers may be called as functions for coercing (which is preferred over using `+` or concatenating the empty string) or creating symbols.

Example:

    const /** boolean */ x = Boolean(0);
    if (!x) alert(typeof x);  // alerts 'boolean', as expected
    

#### Modifying builtin objects

Never modify builtin types, either by adding methods to their constructors or to their prototypes. Avoid depending on libraries that do this. Note that the JSCompiler’s runtime library will provide standards-compliant polyfills where possible; nothing else may modify builtin objects.

Do not add symbols to the global object unless absolutely necessary (e.g. required by a third-party API).

#### Omitting `()` when invoking a constructor

Never invoke a constructor in a `new` statement without using parentheses `()`.

Disallowed:

    new Foo;
    

Use instead:

    new Foo();
    

Omitting parentheses can lead to subtle mistakes. These two lines are not equivalent:

    new Foo().Bar();
    new Foo.Bar();
    

## Naming

### Rules common to all identifiers

Identifiers use only ASCII letters and digits, and, in a small number of cases noted below, underscores and very rarely (when required by frameworks like Angular) dollar signs.

Give as descriptive a name as possible, within reason. Do not worry about saving horizontal space as it is far more important to make your code immediately understandable by a new reader. Do not use abbreviations that are ambiguous or unfamiliar to readers outside your project, and do not abbreviate by deleting letters within a word.

    errorCount          // No abbreviation.
    dnsConnectionIndex  // Most people know what "DNS" stands for.
    referrerUrl         // Ditto for "URL".
    customerId          // "Id" is both ubiquitous and unlikely to be misunderstood.
    

Disallowed:

    n                   // Meaningless.
    nErr                // Ambiguous abbreviation.
    nCompConns          // Ambiguous abbreviation.
    wgcConnections      // Only your group knows what this stands for.
    pcReader            // Lots of things can be abbreviated "pc".
    cstmrId             // Deletes internal letters.
    kSecondsPerDay      // Do not use Hungarian notation.
    

### Rules by identifier type

#### Package names

Package names are all `lowerCamelCase`. For example, `my.exampleCode.deepSpace`, but not `my.examplecode.deepspace` or `my.example_code.deep_space`.

#### Class names

Class, interface, record, and typedef names are written in `UpperCamelCase`. Unexported classes are simply locals: they are not marked `@private` and therefore are not named with a trailing underscore.

Type names are typically nouns or noun phrases. For example, `Request`, `ImmutableList`, or `VisibilityMode`. Additionally, interface names may sometimes be adjectives or adjective phrases instead (for example, `Readable`).

#### Method names

Method names are written in `lowerCamelCase`. Names for `@private` methods must end with a trailing underscore.

Method names are typically verbs or verb phrases. For example, `sendMessage` or `stop_`. Getter and setter methods for properties are never required, but if they are used they should be named `getFoo` (or optionally `isFoo` or `hasFoo` for booleans), or `setFoo(value)` for setters.

Underscores may also appear in JsUnit test method names to separate logical components of the name. One typical pattern is `test<MethodUnderTest>_<state>_<expectedOutcome>`, for example `testPop_emptyStack_throws`. There is no One Correct Way to name test methods.

#### Enum names

Enum names are written in `UpperCamelCase`, similar to classes, and should generally be singular nouns. Individual items within the enum are named in `CONSTANT_CASE`.

#### Constant names

Constant names use `CONSTANT_CASE`: all uppercase letters, with words separated by underscores. There is no reason for a constant to be named with a trailing underscore, since private static properties can be replaced by (implicitly private) module locals.

##### Definition of “constant”

Every constant is a `@const` static property or a module-local `const` declaration, but not all `@const` static properties and module-local `const`s are constants. Before choosing constant case, consider whether the field really feels like a _deeply immutable_ constant. For example, if any of that instance's observable state can change, it is almost certainly not a constant. Merely intending to never mutate the object is generally not enough.

Examples:

    // Constants
    const NUMBER = 5;
    /** @const */ exports.NAMES = ImmutableList.of('Ed', 'Ann');
    /** @enum */ exports.SomeEnum = { ENUM_CONSTANT: 'value' };
    
    // Not constants
    let letVariable = 'non-const';
    class MyClass { constructor() { /** @const {string} */ this.nonStatic = 'non-static'; } };
    /** @type {string} */ MyClass.staticButMutable = 'not @const, can be reassigned';
    const /** Set<string> */ mutableCollection = new Set();
    const /** ImmutableSet<SomeMutableType> */ mutableElements = ImmutableSet.of(mutable);
    const Foo = goog.require('my.Foo');  // mirrors imported name
    const logger = log.getLogger('loggers.are.not.immutable');
    

Constants’ names are typically nouns or noun phrases.

##### Local aliases

Local aliases should be used whenever they improve readability over fully-qualified names.

Examples:

    const staticHelper = importedNamespace.staticHelper;
    const CONSTANT_NAME = ImportedClass.CONSTANT_NAME;
    const {assert, assertInstanceof} = asserts;
    

#### Non-constant field names

Non-constant field names (static or otherwise) are written in `lowerCamelCase`, with a trailing underscore for private fields.

These names are typically nouns or noun phrases. For example, `computedValues` or `index_`.

#### Parameter names

Parameter names are written in `lowerCamelCase`. Note that this applies even if the parameter expects a constructor.

One-character parameter names should not be used in public methods.

**Exception**: When required by a third-party framework, parameter names may begin with a `$`. This exception does not apply to any other identifiers (e.g. local variables or properties).

#### Local variable names

Local variable names are written in `lowerCamelCase`, except for module-local (top-level) constants, as described above. Constants in function scopes are still named in `lowerCamelCase`. Note that `lowerCamelCase` is used even if the variable holds a constructor.

#### Template parameter names

Template parameter names should be concise, single-word or single-letter identifiers, and must be all-caps, such as `TYPE` or `THIS`.

#### Module-local names

Module-local names that are not exported are implicitly private. They are not marked `@private` and do not end in an underscore. This applies to classes, functions, variables, constants, enums, and other module-local identifiers.

### Camel case: defined

Sometimes there is more than one reasonable way to convert an English phrase into camel case, such as when acronyms or unusual constructs like IPv6 or iOS are present. To improve predictability, the following (nearly) deterministic scheme.

Beginning with the prose form of the name:

1.  Convert the phrase to plain ASCII and remove any apostrophes. For example, Müller's algorithm might become Muellers algorithm.
2.  Divide this result into words, splitting on spaces and any remaining punctuation (typically hyphens).
    1.  Recommended: if any word already has a conventional camel case appearance in common usage, split this into its constituent parts (e.g., AdWords becomes ad words). Note that a word such as iOS is not really in camel case per se; it defies any convention, so this recommendation does not apply.
3.  Now lowercase everything (including acronyms), then uppercase only the first character of:
    1.  … each word, to yield upper camel case, or
    2.  … each word except the first, to yield lower camel case
4.  Finally, join all the words into a single identifier.

Note that the casing of the original words is almost entirely disregarded.

Examples:

Prose form

Correct

Incorrect

XML HTTP request

XmlHttpRequest

XMLHTTPRequest

new customer ID

newCustomerId

newCustomerID

inner stopwatch

innerStopwatch

innerStopWatch

supports IPv6 on iOS?

supportsIpv6OnIos

supportsIPv6OnIOS

YouTube importer

YouTubeImporter

Note: Some words are ambiguously hyphenated in the English language: for example nonempty and non-empty are both correct, so the method names checkNonempty and checkNonEmpty are likewise both correct.

## JSDoc

[JSDoc](https://developers.google.com/closure/compiler/docs/js-for-compiler) is used on all classes, fields, and methods.

### General form

The basic formatting of JSDoc blocks is as seen in this example:

    /**
     * Multiple lines of JSDoc text are written here,
     * wrapped normally.
     * @param {number} arg A number to do something to.
     */
    function doSomething(arg) { … }
    

or in this single-line example:

    /** @const @private {!Foo} A short bit of JSDoc. */
    this.foo_ = foo;
    

If a single-line comment overflows into multiple lines, it must use the multi-line style with `/**` and `*/` on their own lines.

Many tools extract metadata from JSDoc comments to perform code validation and optimization. As such, these comments **must** be well-formed.

### Markdown

JSDoc is written in Markdown, though it may include HTML when necessary.

Note that tools that automatically extract JSDoc (e.g. [JsDossier](https://github.com/jleyba/js-dossier)) will often ignore plain text formatting, so if you did this:

    /**
     * Computes weight based on three factors:
     *   items sent
     *   items received
     *   last timestamp
     */
    

it would come out like this:

    Computes weight based on three factors: items sent items received last timestamp
    

Instead, write a Markdown list:

    /**
     * Computes weight based on three factors:
     *
     *  - items sent
     *  - items received
     *  - last timestamp
     */
    

### JSDoc tags

This style allows a subset of JSDoc tags. See [JSDoc tag reference](#jsdoc-tag-reference) for the complete list. Most tags must occupy their own line, with the tag at the beginning of the line.

Disallowed:

    /**
     * The "param" tag must occupy its own line and may not be combined.
     * @param {number} left @param {number} right
     */
    function add(left, right) { ... }
    

Simple tags that do not require any additional data (such as `@private`, `@const`, `@final`, `@export`) may be combined onto the same line, along with an optional type when appropriate.

    /**
     * Place more complex annotations (like "implements" and "template")
     * on their own lines.  Multiple simple tags (like "export" and "final")
     * may be combined in one line.
     * @export @final
     * @implements {Iterable<TYPE>}
     * @template TYPE
     */
    class MyClass {
      /**
       * @param {!ObjType} obj Some object.
       * @param {number=} num An optional number.
       */
      constructor(obj, num = 42) {
        /** @private @const {!Array<!ObjType|number>} */
        this.data_ = [obj, num];
      }
    }
    

There is no hard rule for when to combine tags, or in which order, but be consistent.

For general information about annotating types in JavaScript see [Annotating JavaScript for the Closure Compiler](https://github.com/google/closure-compiler/wiki/Annotating-JavaScript-for-the-Closure-Compiler) and [Types in the Closure Type System](https://github.com/google/closure-compiler/wiki/Types-in-the-Closure-Type-System).

### Line wrapping

Line-wrapped block tags are indented four spaces. Wrapped description text may be lined up with the description on previous lines, but this horizontal alignment is discouraged.

    /**
     * Illustrates line wrapping for long param/return descriptions.
     * @param {string} foo This is a param with a description too long to fit in
     *     one line.
     * @return {number} This returns something that has a description too long to
     *     fit in one line.
     */
    exports.method = function(foo) {
      return 5;
    };
    

Do not indent when wrapping a `@desc` or `@fileoverview` description.

### Top/file-level comments

A file may have a top-level file overview. A copyright notice , author information, and default [visibility level](#jsdoc-visibility-annotations) are optional. File overviews are generally recommended whenever a file consists of more than a single class definition. The top level comment is designed to orient readers unfamiliar with the code to what is in this file. If present, it may provide a description of the file's contents and any dependencies or compatibility information. Wrapped lines are not indented.

Example:

    /**
     * @fileoverview Description of file, its uses and information
     * about its dependencies.
     * @package
     */
    

### Class comments

Classes, interfaces and records must be documented with a description and any template parameters, implemented interfaces, visibility, or other appropriate tags. The class description should provide the reader with enough information to know how and when to use the class, as well as any additional considerations necessary to correctly use the class. Textual descriptions may be omitted on the constructor. `@constructor` and `@extends` annotations are not used with the `class` keyword unless the class is being used to declare an `@interface` or it extends a generic class.

    /**
     * A fancier event target that does cool things.
     * @implements {Iterable<string>}
     */
    class MyFancyTarget extends EventTarget {
      /**
       * @param {string} arg1 An argument that makes this more interesting.
       * @param {!Array<number>} arg2 List of numbers to be processed.
       */
      constructor(arg1, arg2) {
        // ...
      }
    };
    
    /**
     * Records are also helpful.
     * @extends {Iterator<TYPE>}
     * @record
     * @template TYPE
     */
    class Listable {
      /** @return {TYPE} The next item in line to be returned. */
      next() {}
    }
    

### Enum and typedef comments

All enums and typedefs must be documented with appropriate JSDoc tags (`@typedef` or `@enum`) on the preceding line. Public enums and typedefs must also have a description. Individual enum items may be documented with a JSDoc comment on the preceding line.

    /**
     * A useful type union, which is reused often.
     * @typedef {!Bandersnatch|!BandersnatchType}
     */
    let CoolUnionType;
    
    
    /**
     * Types of bandersnatches.
     * @enum {string}
     */
    const BandersnatchType = {
      /** This kind is really frumious. */
      FRUMIOUS: 'frumious',
      /** The less-frumious kind. */
      MANXOME: 'manxome',
    };
    

Typedefs are useful for defining short record types, or aliases for unions, complex functions, or generic types. Typedefs should be avoided for record types with many fields, since they do not allow documenting individual fields, nor using templates or recursive references. For large record types, prefer `@record`.

### Method and function comments

In methods and named functions, parameter and return types must be documented, except in the case of same-signature `@override`s, where all types are omitted. The `this` type should be documented when necessary. Return type may be omitted if the function has no non-empty `return` statements.

Method, parameter, and return descriptions (but not types) may be omitted if they are obvious from the rest of the method’s JSDoc or from its signature.

Method descriptions begin with a verb phrase that describes what the method does. This phrase is not an imperative sentence, but instead is written in the third person, as if there is an implied This method ... before it.

If a method overrides a superclass method, it must include an `@override` annotation. Overridden methods inherit all JSDoc annotations from the super class method (including visibility annotations) and they should be omitted in the overridden method. However, if any type is refined in type annotations, all `@param` and `@return` annotations must be specified explicitly.

    /** A class that does something. */
    class SomeClass extends SomeBaseClass {
      /**
       * Operates on an instance of MyClass and returns something.
       * @param {!MyClass} obj An object that for some reason needs detailed
       *     explanation that spans multiple lines.
       * @param {!OtherClass} obviousOtherClass
       * @return {boolean} Whether something occurred.
       */
      someMethod(obj, obviousOtherClass) { ... }
    
      /** @override */
      overriddenMethod(param) { ... }
    }
    
    /**
     * Demonstrates how top-level functions follow the same rules.  This one
     * makes an array.
     * @param {TYPE} arg
     * @return {!Array<TYPE>}
     * @template TYPE
     */
    function makeArray(arg) { ... }
    

If you only need to document the param and return types of a function, you may optionally use inline JSDocs in the function's signature. These inline JSDocs specify the return and param types without tags.

    function /** string */ foo(/** number */ arg) {...}
    

If you need descriptions or tags, use a single JSDoc comment above the method. For example, methods which return values need a `@return` tag.

    class MyClass {
      /**
       * @param {number} arg
       * @return {string}
       */
      bar(arg) {...}
    }
    

    // Illegal inline JSDocs.
    
    class MyClass {
      /** @return {string} */ foo() {...}
    }
    
    /** Function description. */ bar() {...}
    

In anonymous functions annotations are generally optional. If the automatic type inference is insufficient or explicit annotation improves readability, then annotate param and return types like this:

    promise.then(
        /** @return {string} */
        (/** !Array<string> */ items) => {
          doSomethingWith(items);
          return items[0];
        });
    

For function type expressions, see [Function Type Expressions](#function-type-expressions).

### Property comments

Property types must be documented. The description may be omitted for private properties, if name and type provide enough documentation for understanding the code.

Publicly exported constants are commented the same way as properties.

    /** My class. */
    class MyClass {
      /** @param {string=} someString */
      constructor(someString = 'default string') {
        /** @private @const {string} */
        this.someString_ = someString;
    
        /** @private @const {!OtherType} */
        this.someOtherThing_ = functionThatReturnsAThing();
    
        /**
         * Maximum number of things per pane.
         * @type {number}
         */
        this.someProperty = 4;
      }
    }
    
    /**
     * The number of times we'll try before giving up.
     * @const {number}
     */
    MyClass.RETRY_COUNT = 33;
    

### Type annotations

Type annotations are found on `@param`, `@return`, `@this`, and `@type` tags, and optionally on `@const`, `@export`, and any visibility tags. Type annotations attached to JSDoc tags must always be enclosed in braces.

#### Nullability

The type system defines modifiers `!` and `?` for non-null and nullable, respectively. These modifiers must precede the type.

Nullability modifiers have different requirements for different types, which fall into two broad categories:

1.  Type annotations for primitives (`string`, `number`, `boolean`, `symbol`, `undefined`, `null`) and literals (`{function(...): ...}` and `{{foo: string...}}`) are always non-nullable by default. Use the `?` modifier to make it nullable, but omit the redundant `!`.
2.  Reference types (generally, anything in `UpperCamelCase`, including `some.namespace.ReferenceType`) refer to a class, enum, record, or typedef defined elsewhere. Since these types may or may not be nullable, it is impossible to tell from the name alone whether it is nullable or not. Always use explicit `?` and `!` modifiers for these types to prevent ambiguity at use sites.

Bad:

    const /** MyObject */ myObject = null; // Non-primitive types must be annotated.
    const /** !number */ someNum = 5; // Primitives are non-nullable by default.
    const /** number? */ someNullableNum = null; // ? should precede the type.
    const /** !{foo: string, bar: number} */ record = ...; // Already non-nullable.
    const /** MyTypeDef */ def = ...; // Not sure if MyTypeDef is nullable.
    
    // Not sure if object (nullable), enum (non-nullable, unless otherwise
    // specified), or typedef (depends on definition).
    const /** SomeCamelCaseName */ n = ...;
    

Good:

    const /** ?MyObject */ myObject = null;
    const /** number */ someNum = 5;
    const /** ?number */ someNullableNum = null;
    const /** {foo: string, bar: number} */ record = ...;
    const /** !MyTypeDef */ def = ...;
    const /** ?SomeCamelCaseName */ n = ...;
    

#### Type Casts

In cases where the compiler doesn't accurately infer the type of an expression, and the assertion functions in [goog.asserts](https://google.github.io/closure-library/api/goog.asserts.html) cannot remedy it , it is possible to tighten the type by adding a type annotation comment and enclosing the expression in parentheses. Note that the parentheses are required.

    /** @type {number} */ (x)
    

#### Template Parameter Types

Always specify template parameters. This way compiler can do a better job and it makes it easier for readers to understand what code does.

Bad:

    const /** !Object */ users = {};
    const /** !Array */ books = [];
    const /** !Promise */ response = ...;
    

Good:

    const /** !Object<string, !User> */ users = {};
    const /** !Array<string> */ books = [];
    const /** !Promise<!Response> */ response = ...;
    
    const /** !Promise<undefined> */ thisPromiseReturnsNothingButParameterIsStillUseful = ...;
    const /** !Object<string, *> */ mapOfEverything = {};
    

Cases when template parameters should not be used:

*   `Object` is used for type hierarchy and not as map-like structure.

#### Function type expressions

**Terminology Note**: _function type expression_ refers to a type annotation for function types with the keyword `function` in the annotation (see examples below).

Where the function definition is given, do not use a function type expression. Specify parameter and return types with `@param` and `@return`, or with inline annotations (see [Method and function comments](#method-and-function-comments)). This includes anonymous functions and functions defined and assigned to a const (where the function jsdoc appears above the whole assignment expression).

Function type expressions are needed, for example, inside `@typedef`, `@param` or `@return`. Use it also for variables or properties of function type, if they are not immediately initialized with the function definition.

      /** @private {function(string): string} */
      this.idGenerator_ = googFunctions.identity;
    

When using a function type expression, always specify the return type explicitly. Otherwise the default return type is unknown (`?`), which leads to strange and unexpected behavior, and is rarely what is actually desired.

Bad - type error, but no warning given:

    /** @param {function()} generateNumber */
    function foo(generateNumber) {
      const /** number */ x = generateNumber();  // No compile-time type error here.
    }
    
    foo(() => 'clearly not a number');
    

Good:

    /**
     * @param {function(): *} inputFunction1 Can return any type.
     * @param {function(): undefined} inputFunction2 Definitely doesn't return
     *      anything.
     * NOTE: the return type of `foo` itself is safely implied to be {undefined}.
     */
    function foo(inputFunction1, inputFunction2) {...}
    

#### Whitespace

Within a type annotation, a single space or line break is required after each comma or colon. Additional line breaks may be inserted to improve readability or avoid exceeding the column limit. These breaks should be chosen and indented following the applicable guidelines (e.g. [Line Wrapping](#line-wrapping) and [Block Indentation](#block-indentation)). No other whitespace is allowed in type annotations.

Good:

    /** @type {function(string): number} */
    
    /** @type {{foo: number, bar: number}} */
    
    /** @type {number|string} */
    
    /** @type {!Object<string, string>} */
    
    /** @type {function(this: Object<string, string>, number): string} */
    
    /**
     * @type {function(
     *     !SuperDuperReallyReallyLongTypedefThatForcesTheLineBreak,
     *     !OtherVeryLongTypedef): string}
     */
    
    /**
     * @type {!SuperDuperReallyReallyLongTypedefThatForcesTheLineBreak|
     *     !OtherVeryLongTypedef}
     */
    

Bad:

    // Only put a space after the colon
    /** @type {function(string) : number} */
    
    // Put spaces after colons and commas
    /** @type {{foo:number,bar:number}} */
    
    // No space in union types
    /** @type {number | string} */
    

### Visibility annotations

Visibility annotations (`@private`, `@package`, `@protected`) may be specified in a `@fileoverview` block, or on any exported symbol or property. Do not specify visibility for local variables, whether within a function or at the top level of a module. All `@private` names must end with an underscore.

## Policies
### Issues unspecified by Wiridis Style: Be Consistent!

For any style question that isn't settled definitively by this specification, prefer to do what the other code in the same file is already doing. If that doesn't resolve the question, consider emulating the other files in the same package.

### Compiler warnings

#### Use a standard warning set

As far as possible projects should use `--warning_level=VERBOSE`.

#### How to handle a warning

Before doing anything, make sure you understand exactly what the warning is telling you. If you're not positive why a warning is appearing, ask for help .

Once you understand the warning, attempt the following solutions in order:

1.  **First, fix it or work around it.** Make a strong attempt to actually address the warning, or find another way to accomplish the task that avoids the situation entirely.
2.  **Otherwise, determine if it's a false alarm.** If you are convinced that the warning is invalid and that the code is actually safe and correct, add a comment to convince the reader of this fact and apply the `@suppress` annotation.
3.  **Otherwise, leave a TODO comment.** This is a **last resort**. If you do this, **do not suppress the warning.** The warning should be visible until it can be taken care of properly.

#### Suppress a warning at the narrowest reasonable scope

Warnings are suppressed at the narrowest reasonable scope, usually that of a single local variable or very small method. Often a variable or method is extracted for that reason alone.

Example

    /** @suppress {uselessCode} Unrecognized 'use asm' declaration */
    function fn() {
      'use asm';
      return 0;
    }
    

Even a large number of suppressions in a class is still better than blinding the entire class to this type of warning.

### Deprecation

Mark deprecated methods, classes or interfaces with `@deprecated` annotations. A deprecation comment must include simple, clear directions for people to fix their call sites.

## Linters setup
These are some tools that can make our life easier regarding the coding standards :smiley:.

To fit the Wiridis coding standard, we can iinstall linters and configure VSCode as shown below:
1. Install required dependencies as DevDependencies

   ```
   $ npm install --save-dev eslint prettier eslint-config-google eslint-config-prettier eslint-plugin-prettier 
   ```

ESLint: Linters that can help us spot coding standard mistake in our program on the `Problems` tab.

Prettier: Plugin to autoformat our code to adhere to the required coding standard

Remaining modules are for configuration so that all our linters works as expected.

:warn: Tips: You also need to install module for your own Javascript framework for ESLint to function properly. For VueJS, we can add `eslint-plugin-vue` as shown below:

   ```
   $ npm install --save-dev eslint prettier eslint-config-google eslint-config-prettier eslint-plugin-prettier eslint-plugin-vue
   ```

2. Create a `.eslintrc.json` configuration file
Create a configuration file for ESLint with the following contents:
   ```
   {
     "env": {
       "browser": true,
       "commonjs": true,
       "es6": true,
       "node": true
     },
     "extends": ["google", "plugin:prettier/recommended", "eslint:recommended"],
     "plugins": ["prettier"],
     "parserOptions": {
       "ecmaVersion": 6,
       "sourceType": "module"
     },
     "rules": {
       "indent": ["warn", 2],
       "prettier/prettier": "warn"
     }
   }
   ```

3. Create a `.prettierrc.json` configuration file
Create a configuration file for ESLint with the following contents:
   ```
   {
     "trailingComma": "es5",
     "useTabs": false,
     "tabWidth": 2,
     "semi": true,
     "singleQuote": true,
     "bracketSpacing": true,
     "printWidth": 80,
     "endOfLine": "auto"
   }
   ```
4. Set Prettier to format on Save (optional)
You can set Prettier to format your file on each save. Open the Settings pane (`Ctrl + ,` or `Cmd + ,`), search for "Format" in the search field and check the box that says “Editor: Format on Save”.
![VSCode settings image](format%20on%20save%20pic.PNG)

We are good to go :smiley:. However, note that these Linters can only warn and format our code to a certain extend. We need to remain concious in ensuring our code adhere to the coding standards explained in this document.

