# TypeScript Style Guide

This is the TypeScript style guide that we use internally at Eureka. It is *semi-reasonable*, but it's more important that we keep a consistent look/feel of our code.


## Table of Contents

  0. [Introduction](#introduction)
  0. [Browser Compatibility](#browser-compatibility)
  0. [Files](#files)
  0. [Indentation](#indentation)
  0. [Line Length](#line-length)
  0. [Quotes](#quotes)
  0. [Comments](#comments)
    0. [Class](#class)
    0. [Inline](#inline)
    0. [Todo and XXX](#todo-and-xxx)
  0. [Variable Declarations](#variable-declarations)
  0. [Function Declarations](#function-declarations)
    0. [Anonymous Functions](#anonymous-functions)
  0. [Names](#names)
    0. [Variables, Modules, and Functions](#variables-modules-and-functions)
    0. [Use of var, let, and const](#use-of-var-let-and-const)
    0. [Types](#types)
    0. [Classes](#classes)
    0. [Interfaces](#interfaces)
    0. [Constants](#constants)
  0. [Statements](#statements)
    0. [Simple](#simple)
    0. [Compound](#compound)
    0. [Return](#return)
    0. [If](#if)
    0. [For](#for)
    0. [While](#while)
    0. [Do While](#do-while)
    0. [Switch](#switch)
    0. [Try](#try)
    0. [Continue](#continue)
    0. [Throw](#throw)
  0. [Whitespace](#whitespace)
  0. [Object and Array Literals](#object-and-array-literals)
  0. [Assignment Expressions](#assignment-expressions)
  0. [Typings](#assignment-expressions)
    0. [External](#external)
    0. [Internal](#internal)
  0. [=== and !== Operators](#===-and-!==-Operators)
  0. [Eval](#eval)
  0. [TSLint](#tslint)
  0. [License](#license)

## Introduction
When developing software as an organization, the value of the software produced is directly affected by the quality of the codebase. Consider a project that is developed over many years and handled/seen by many different people. If the project uses a consistent coding convention it is easier for new developers to read, preventing a lot of time/frustration spent figuring out the structure and characteristics of the code. For that purpose, we need to make sure we adhere to the same coding conventions across all of our products. This will not only help new developers, but it will also aid in quickly identifying potential flaws in the code, thereby reducing the brittleness of the code.

**[top](#table-of-contents)**

## Files
  - All TypeScript files must have a ".ts" extension.
  - They should be all lower case, and only include letters, numbers, and periods.
  - It is OK (even recommended) to separate words with periods (e.g. my.controller.ts).
  - **All files should end in a new line.** This is necessary for some Unix systems.

**[top](#table-of-contents)**

## Indentation
  - The unit of indentation is four spaces.
  - **Never use tabs**, as this can lead to trouble when opening files in different IDEs/Text editors. Most text editors have a configuration option to change tabs to spaces.

**[top](#table-of-contents)**

## Line Length
  - Lines must not be longer than 120 characters.
  - When a statement runs over 120 characters on a line, it should be broken up, ideally after a comma or operator.

**[top](#table-of-contents)**

## Quotes
  - Use single-quotes `''` for all strings, and use double-quotes `""` for strings within strings.

  ```typescript
  // bad
  let msg = "Your mom"

  // bad
  let msg = "Your mom told me 'hi'"

  // bad
  let msg = 'Your mom told me \'hi\''

  // good
  let msg = 'Your mom'

  // good
  let msg = 'Your mom told me "hi"'
  ```

**[top](#table-of-contents)**

## Comments
  - Comments should be used judiciously. Your goal should be writing code that is intuitive and easy to follow without comments.
  - Comments should be reserved for cases when you have refactored your code as much as is practical and its still not intuitive on its own; or there are subtle or important conditions that developers need to know about.
  - If you do comment your code, make sure the comments are clear and concise. Craft them the way you craft your code, thinking about them from the perspective of someone who isn't as familiar with the code as you are.

The following example is a case where a comment is completely erroneous, and can actually make the code harder to read.

  ```typescript
  // Set index to zero.
  let index = 0

  /*
   * saves user to database.
   */
  function saveUser(user: User) void {
     ...
  }
  ```

The following is an example of a well-placed comment:

  ```typescript
   /*
    * Find the great cirlce distance between point A and point B using the
    * haversine algorithm (https://en.wikipedia.org/wiki/Haversine_formula).
    */
   public haversineDistance(a: Point, b: Point): double
   {
       return 2 * math.aSin
       (
           math.sqrt
           (
               math.pow(math.sin(math.abs(a.lat - b.lon) / 2), 2) +
               math.cos(a.lat) * math.cos(b.lat) *
               math.pow(math.sin(math.abs(a.lon - b.lon) / 2), 2)
           )
       )
   }
  ```


**[top](#table-of-contents)**

### Inline

  - Inline comments are comments inside of complex statements (loops, functions, etc).
  - Use `//` for all inline comments.
  - Keep comments clear and concise.
  - Place inline comments on a newline above the line they are annotating
  - Put an empty line before the comment.

  ```typescript
  // bad
  let lines: Array<string> // Holds all the lines in the file.

  // good
  // Holds all the lines in the file.
  let lines: Array<string>

  // bad
  function walkFor(name: string, millis: number): void {
      console.log(name + ' is now walking.')
      // Wait for millis milliseconds to stop walking
      setTimeout(() => {
          console.log(name + ' has stopped walking.')
      }, millis)
  }

  // good
  function walkFor(name: string, millis: number): void {
      console.log(name + ' is now walking.')

      // Wait for millis milliseconds to stop walking
      setTimeout(() => {
          console.log(name + ' has stopped walking.')
      }, millis)
  }
  ```

**[top](#table-of-contents)**

### Todo and XXX

`TODO` and `XXX` annotations help you quickly find things that need to be fixed/implemented.

  - Use `// TODO:` to annotate solutions that need to be implemented.
  - Use `// XXX:` to annotate problems the need to be fixed.
  - It is best to write code that doesn't need `TODO` and `XXX` annotations, but sometimes it is unavoidable.

**[top](#table-of-contents)**

## Variable Declarations

  - All variables must be declared prior to using them. This aids in code readability and helps prevent undeclared variables from being hoisted onto the global scope.

  ```typescript
  // bad
  console.log(a + b)
  let a = 2
  let b = 4

  // good
  let a = 2
  let b = 4
  console.log(a + b)
  ```

  - Implied global variables should never be used.
  - You should never define a variable on the global scope from within a smaller scope.

  ```typescript
  // bad
  function add(a: number, b: number): number {
      // c is on the global scope!
      c = 6
      return a + b + c
  }
  ```

  - Use one `let` keyword per variable.
  - Declare each variable on a newline.

  ```typescript
  // bad
  // b will be defined on global scope.
  let a = b = 2, c = 4

  // bad
  let a = 2,
      b = 2,
      c = 4

  // good
  let a = 2
  let b = 2
  let c = 4

  ```

## Function Declarations

  - All functions must be declared before they are used.
  - Any closure functions should be defined right after the let declarations.

  ```typescript
  // bad
  function createGreeting(name: string): string {
      let message = 'Hello '

      return greet

      function greet() {
          return message + name + '!'
      }
  }

  // good
  function createGreeting(name: string): string {
      let message = 'Hello '

      function greet() {
          return message + name + '!'
      }

      return greet
  }
  ```

  - There should be no space between the name of the function and the left parenthesis `(` of its parameter list.
  - There should be one space between the right parenthesis `)` and the left curly `{` brace that begins the statement body.

  ```typescript
  // bad
  function foo (){
      // ...
  }

  // good
  function foo() {
      // ...
  }
  ```

  - The body of the function should be indented 4 spaces.
  - The right curly brace `}` should be on a new line.
  - The right curly brace `}` should be aligned with the line containing the left curly brace `{` that begins the function statement.

  ```typescript
  // bad
  function foo(): string {
    return 'foo'}

  // good
  function foo(): string {
      return 'foo'
  }
  ```

  - For each function parameter
    - There should be no space between the parameter and the colon `:` indicating the type declaration.
    - There should be a space between the colon `:` and the type declaration.

  ```typescript
  // bad
  function greet(name:string) {
    // ...
  }

  // good
  function greet(name: string) {
    // ...
  }
  ```

**[top](#table-of-contents)**

### Anonymous Functions

  - All anonymous functions should be defined as fat-arrow/lambda `() => { }` functions unless it is absolutely necessary to preserve the context in the function body.
  - All fat-arrow/lambda functions should have parenthesis `()` around the function parameters.

  ```typescript
  // bad
  clickAlert() {
      let element = document.querySelector('div')

      this.foo = 'foo'

      element.addEventListener('click', function(ev: Event) {
          // this.foo does not exist!
          alert(this.foo)
      })
  }

  // good
  clickAlert() {
      let element = document.querySelector('div')

      this.foo = 'foo'

      element.addEventListener('click', (ev: Event) => {
          // TypeScript allows this.foo to exist!
          alert(this.foo)
      })
  }
  ```

  - There should be a space between the right parenthesis `)` and the `=>`
  - There should be a space between the `=>` and the left curly brace `{` that begins the statement body.

  ```typescript
  // bad
  element.addEventListener('click', (ev: Event)=>{alert('foo')})

  // good
  element.addEventListener('click', (ev: Event) => {
      alert('foo')
  })
  ```

  - The statement body should be indented 4 spaces.
  - The right curly brace `}` should be on a new line.
  - The right curly brace `}` should be aligned with the line containing the  left curly brace `{` that begins the function statement.

**[top](#table-of-contents)**

## Names

  - All variable and function names should be formed with alphanumeric `A-Z, a-z, 0-9` and underscore `_` charcters.

### Variables, Modules, and Functions

  - Variable, module, and function names should use lowerCamelCase.

   ```typescript
  // bad
  let account_name: string  = 'abc'
  let AccountNumber: number = 123

  // good
  let accountName: string = 'abc'
  let accountNumber: number = 123

  // bad
  function calc_balance(): number {
      ...
  }

  // bad
  function AddAmount(): void {
      ...
  }

  // good
  function calcBalance(): number {
      ...
  }

  // good
  function addAmount(): void {
      ...
  }
  ```

### Use of var, let, and const

  - Use `const` where appropriate, for values that should never change
  - Use `let` everywhere else
  - Avoid using `var`

**[top](#table-of-contents)**

### Types

  - Types should be used whenever possible. That's pretty much the whole point of TypeScript.
  - Arrays should be defined as `type[]` instead of `Array<type>`.
  - Use the `any` type sparingly, it is always better to define an interface.
  - Always specify the types of function parameters.
  - Always specify the return type of functions.

  ```typescript
  // bad
  let numbers = []

  // bad
  let numbers: Array<number> = []

  // good
  let numbers: number[] = []
  ```

**[top](#table-of-contents)**

### Classes

  - Classes/Constructors should use UpperCamelCase (PascalCase).
  - Annotate all class members with the `public`, `private`, or `protected` access modifiers.
  - Default to using `private` for all instance members. Only use `public` or `protected` if there is a good reason for code outside the class to get access to the member.

  ```typescript

  // bad
  class person {
      public fullName: string

      public toString() {
          return this.fullName
      }
  }

  // good
  class Person {
      private fullName: string

      public toString() {
          return this.fullName
      }
  }
  ```

**[top](#table-of-contents)**

### Interfaces

  - Interfaces should use UpperCamelCase.
  - Interfaces should be use the "-able" suffix where possible (e.g. `Geocodable`, `Runnable`).
  - Only `public` members should be in an interface, leave out `protected` and `private` members.

  ```typescript
  interface Geocodable {
      getPoint(name: string): Point
  }
  ```

**[top](#table-of-contents)**

### Constants

  - All constants should use UPPER_SNAKE_CASE.
  - All constants you be defined with the `const` keyword.

**[top](#table-of-contents)**

## Statements

### Semicolons
  - Do not put semicolons at the end of statements. Semicolons should only be used when it would be syntactically or semantically required (as when declaring a `for` loop).

  ```typescript
  // bad, terrible, the worst thinge very
  let greeting = 'semi-colons are for losers who just like typing';
  alert(greeting);

  // good
  let greeting = 'semi-colons are for losers who just like typing'
  alert(greeting)

  // okay because required
  for (let i = 0; i < 10; i++) {
      ...
  }
  ```

### Simple

  - Each line should contain at most one statement.

  ```typescript
  // bad
  if (condition === true) { alert('Passed!') }

  // good
  if (condition === true) {
      alert('Passed!')
  }
  ```
**[top](#table-of-contents)**

### Compound

Compound statements are statements containing lists of statements enclosed in curly braces `{}`.

  - The enclosed statements should start on a newline.
  - The enclosed statements should be indented 4 spaces.

  ```typescript
  // bad
  if (condition === true) { alert('Passed!') }

  // good
  if (condition === true) {
      alert('Passed!')
  }
  ```
  - The left curly brace `{` should be at the end of the line that begins the compound statement.
  - The right curly brace `}` should begin a line and be indented to align with the line containing the left curly brace `{`.

  ```typescript
  // bad
  if (condition === true)
  {
      alert('Passed!')
  }

  // good
  if (condition === true) {
      alert('Passed!')
  }
  ```

  - **Braces `{}` must be used around all compound statements** even if they are only single-line statements.

  ```typescript
  // bad
  if (condition === true) alert('Passed!')

  // bad
  if (condition === true)
      alert('Passed!')

  // good
  if (condition === true) {
      alert('Passed!')
  }
  ```

If you do not add braces `{}` around compound statements, it makes it very easy to accidentally introduce bugs.

  ```typescript
  if (condition === true)
      alert('Passed!')
      return condition
  ```

It appears the intention of the above code is to return if `condition === true`, but without braces `{}` the return statement will be executed regardless of the condition.

**[top](#table-of-contents)**

### Return

  - If a `return` statement has a value you should not use parenthesis `()` around the value.
  - The return value expression must start on the same line as the `return` keyword.

  ```typescript
  // bad
  return
      'Hello World!'

  // bad
  return ('Hello World!')

  // good
  return 'Hello World!'
  ```

  - It is recommended to take a return-first approach whenever possible.

  ```typescript
  // bad
  function getHighestNumber(a: number, b: number): number {
      let out = b

      if(a >= b) {
          out = a
      }

      return out
  }

  // good
  function getHighestNumber(a: number, b: number): number {
      if(a >= b) {
          return a
      }

      return b
  }
  ```

  - Always **explicitly define a return type**. This can help TypeScript validate that you are always returning something that matches the correct type.

  ```typescript
  // bad
  function getPerson(name: string) {
      return new Person(name)
  }

  // good
  function getPerson(name: string): Person {
      return new Person(name)
  }
  ```

**[top](#table-of-contents)**

### If

  - Alway be explicit in your `if` statement conditions.

  ```typescript
  // bad
  function isString(str: any) {
      return !!str
  }

  // good
  function isString(str: any) {
      return typeof str === 'string'
  }
  ```

Sometimes simply checking falsy/truthy values is fine, but the general approach is to be explicit with what you are looking for. This can prevent a lot of unncessary bugs.

If statements should take the following form:

  ```typescript
  if (/* condition */) {
      // ...
  }

  if (/* condition */) {
      // ...
  }
  else {
      // ...
  }

  if (/* condition */) {
      // ...
  }
  else if (/* condition */) {
      // ...
  }
  else {
      // ...
  }
  ```

Always put `else` statements on their own line. Do not put the `else` keyword on the same line as the closing curly brace of the preceding `if` statement.

  ```typescript
  // god I hate this
  if(a > b) {
     // ...
  } else {
     // ...
  }

  // good
  if(a > b) {
     // ...
  }
  else {
     // ...
  }
  ```

**[top](#table-of-contents)**

### For

For statements should have the following form:

  ```typescript
  for(/* initialization */; /* condition */; /* update */) {
      // ...
  }
  ```

  for example:

  ```
  for(let i = 0; i < keys.length; ++i) {
      // ...
  }
  ```

Object.prototype.keys is supported in `ie >= 9`.

  - Use Object.prototype.keys in lieu of a `for...in` statement.

**[top](#table-of-contents)**

### While

While statements should have the following form:

  ```typescript
  while (/* condition */) {
      // ...
  }
  ```

**[top](#table-of-contents)**

### Do While

  - the `while` keyword should go on a new line after the closing curly brace of the `do` statement

  ```typescript
  // bad
  do {
        // ...
  } while (/* condition */)

  // good
  do {
      // ...
  }
  while (/* condition */)
  ```

**[top](#table-of-contents)**

### Switch

Switch statements should have the following form:

  ```typescript
  switch (/* expression */) {
      case /* expression */:
          // ...
          /* termination */
      default:
          // ...
  }
  ```

  - Each switch group except default should end with `break`, `return`, or `throw`.

**[top](#table-of-contents)**

### Try

Try statements should have the following form:

  ```typescript
  try {
      // ...
  }
  catch (error: Error) {
      // ...
  }

  try {
      // ...
  }
  catch (error: Error) {
      // ...
  }
  finally {
      // ...
  }
  ```

Always put `catch` and `finally` keywords on their own line.

  ```typescript
  // bad
  try {
      // ...
  } catch (error: Error) {
      // ...
  } finally {
      // ...
  }

  // good
  try {
      // ...
  }
  catch (error: Error) {
      // ...
  }
  finally {
      // ...
  }
  ```

**[top](#table-of-contents)**

### Continue

  - It is recommended to take a continue-first approach in all loops.

**[top](#table-of-contents)**

### Throw

  - Flow control through try/catch exception handling is not recommended.

**[top](#table-of-contents)**

## Whitespace

Blank lines improve code readability by allowing the developer to logically group code blocks. Blank spaces should be used in the following circumstances.

  - A keyword followed by left parenthesis `(` should be separated by 1 space.

  ```typescript
  // bad
  if(condition) {
      // ...
  }

  // good
  if (condition) {
      // ...
  }
  ```

  - All operators except for period `.`, left parenthesis `(`, and left bracket `[` should be separated from their operands by a space.

  ```typescript
  // bad
  let sum = a+b

  // good
  let sum = a + b

  // bad
  let name = person . name

  // good
  let name = person.name

  // bad
  let item = items [4]

  // good
  let item = items[4]
  ```

  - No space should separate a unary/incremental operator `!x, -x, +x, ~x, ++x, --x` and its operand.

  ```typescript
  // bad
  let neg = - a

  // good
  let neg = -a
  ```

  - Each semicolon in the control part of a `for` statement should be followed with a space.

  ```typescript
  // bad
  for(let i = 0;i < 10;++i) {
      // ...
  }

  // good
  for(let i = 0; i < 10; ++i) {
      // ...
  }
  ```

**[top](#table-of-contents)**

## Object and Array Literals

  - Use curly braces `{}` instead of `new Object()`.
  - Use brackets `[]` instead of `new Array()`.

**[top](#table-of-contents)**

## Assignment Expressions

  - Assignment expressions inside of the condition block of `if`, `while`, and `do while` statements should be avoided.

  ```typescript
  // bad
  while (node = node.next) {
      // ...
  }

  // good
  while (typeof node === 'object') {
      node = node.next

      // ...
  }
  ```

**[top](#table-of-contents)**

## === and !== Operators

  - It is better to use `===` and `!==` operators whenever possible.
  - `==` and `!=` operators do type coercion, which can lead to headaches when debugging code.

**[top](#table-of-contents)**

## Typings

### External

  - Use [tsd](http://definitelytyped.org/tsd/) for all external library declarations
  - Actively add/update/contribute typings to [DefinitelyTyped](http://definitelytyped.org/) when they are missing

### Internal

  - Create declaration files `.d.ts` for your interfaces instead of putting them in your `.ts` files
  - Let the TypeScript compiler infer as much as possible

  - Always define the return type of functions, this helps to make sure that functions always return the correct type

  ```typescript
  // bad
  function sum(a: number, b: number) {
      return a + b
  }

  // good
  function sum(a: number, b: number): number {
      return a + b
  }
  ```

**[top](#table-of-contents)**

## Eval

  - **Never use eval**
  - **Never use the Function constructor**
  - **Never pass strings to `setTimeout` or `setInterval`**

**[top](#table-of-contents)**

## TSLint

  - Always use a Linter

Linting your code is very helpful for preventing minor issues that can escape the eye during development. We use TSLint (written by Palantir) for our linter.

  - TSLint: https://github.com/palantir/tslint
  - Our [tslint.json](https://github.com/Platypi/style_typescript/blob/master/tslint.json)

## License
(The MIT License)

Copyright (c) 2014 Platypi, LLC

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the 'Software'), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED 'AS IS', WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
