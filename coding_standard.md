# Wiridis Javascript Coding standard

## Table of Contents
- [Preliminaries](#preliminaries)
    * [`General guide`](#general-guide)
- [Source file basics](#source-file-basics)
    * [`File name`](#file-name)
    * [`File encoding`](#file-encoding)
    * [`Special characters`](#special-characters)
      * [`Whitespace characters`](#whitespace-characters)
      * [`Special escape characters`](#special-escape-characters)
- [Formatting](#formatting)
    * [`Braces`](#braces)
      * [`Control structures`](#control-structures)
      * [`Nonempty blocks`](#nonempty-blocks)
    * [`Block indentation`](#block-indentation)
    * [`Statements`](#statements)
    * [`Column limit`](#column-limit)
    * [`Line-wrapping`](#line-wrapping)
    * [`Whitespace`](#whitespace)
      * [`Vertical whitespace`](#vertical-whitespace)
      * [`Horizontal whitespace`](#horizontal-whitespace)
      * [`Function whitespace`](#function-whitespace)    
    * [`Comments`](#comments)
      * [`Block comment style`](#block-comment-style)
      * [`Parameter name comments`](#parameter-name-comments)   
- [Language features](#language-features)
    * [`Local variable declarations`](#command-summary)
    * [`Array literals`](#command-summary)
    * [`Object literals`](#command-summary)
    * [`Classes`](#command-summary)
    * [`Functions`](#command-summary)
    * [`String literals`](#command-summary)
    * [`Number literals`](#command-summary)
    * [`Control structures`](#command-summary)
    * [`this`](#command-summary)
    * [`Equality Checks`](#command-summary)
    * [`Disallowed features`](#command-summary)
- [Naming](#naming)
    * [`Rules common to all identifiers`](#command-summary)
    * [`Rules by identifier type`](#command-summary)
    * [`Camel case: defined`](#command-summary)
- [Javascript Doc](#javascript-doc)
    * [`Rules common to all identifiers`](#command-summary)
    * [`Rules by identifier type`](#command-summary)
    * [`Camel case: defined`](#command-summary)
    * [`General form`](#abs)
    * [`Markdown`](#abs)
    * [`JSDoc tags`](#abs)
    * [`Line wrapping`](#abs)
    * [`Top/file-level comments`](#abs)
    * [`Class comments`](#abs)
    * [`Enum and typedef comments`](#abs)
    * [`Method and function comments`](#abs)
    * [`Property comments`](#abs)
    * [`Type annotations`](#abs)
    * [`Visibility annotations`](#abs)
- [Linters setup](#linters-setup)
    * [`ESlint`](#eslint-setup)
    * [`Prettier`](#prettier-setup)

## Preliminaries
This document serves as the complete definition of Wiridis’s coding standards for source code in the JavaScript programming language. 
A JavaScript source file is described as being in Wiridis style if and only if it adheres to the rules herein.

Like other programming style guides, the issues covered span not only aesthetic issues of formatting, but other types of 
conventions or coding standards as well. However, this document focuses primarily on the hard-and-fast rules 
that we follow universally, and avoids giving advice that isn't clearly enforceable (whether by human or tool).

### General guide

[insert pic of general guide on https://javascript.info/coding-style]

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
#### Nonempty blocks
Braces follow the Kernighan and Ritchie style (Egyptian brackets) for nonempty blocks and block-like constructs:
1. No line break before the opening brace.
2. Line break after the opening brace.
3. Line break before the closing brace.
4. Line break after the closing brace if that brace terminates a statement or the body of a function or class statement, or a class method. 
Specifically, there is no line break after the brace if it is followed by `else`, `catch`, `while`, or a comma, semicolon, or right-parenthesis.

**Good** example: 
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

Preferred:
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
// Arguments start on a new line, indented four spaces. Preferred when the
// arguments don't fit on the same line with the function name (or the keyword
// "function") but fit entirely on the second line. Works with very long
// function names, survives renaming without reindenting, low on space.
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
This section addresses implementation comments. JSDoc is addressed separately in [`Javascript Doc`](#javascript-doc).

#### Block comment style
Block comments are indented at the same level as the surrounding code. They may be in `/* … */` or `//`-style. For multi-line `/* … */` comments, subsequent lines must start with `*` aligned with the `*` on the previous line, to make comments obvious with no extra context.

```
/*
 * This is
 * okay.
 */

// And so
// is this.

/* This is fine, too. */
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

