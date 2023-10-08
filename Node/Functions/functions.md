# Functions
In JavaScript, there are several types of functions, and they can be categorized into the following main types:

1. **Named Functions**:
   - These are functions with a declared name.
   - They can be defined using the `function` keyword followed by a name and a set of parentheses.
   - Example:
     ```javascript
     function add(a, b) {
       return a + b;
     }
     ```

2. **Anonymous Functions**:
   - These are functions without a name.
   - They are often defined within other functions or used as function expressions.
   - Example (as a function expression):
     ```javascript
     const multiply = function(a, b) {
       return a * b;
     };
     ```

3. **Arrow Functions (ES6)**:
   - Introduced in ECMAScript 2015 (ES6), arrow functions are a concise way to write anonymous functions.
   - They have a shorter syntax and do not bind their own `this` value.
   - Example:
     ```javascript
     const square = (x) => x * x;
     ```

4. **IIFE (Immediately Invoked Function Expression)**:
   - An IIFE is a function that is defined and executed immediately after declaration.
   - It's often used to create a private scope and prevent polluting the global scope.
   - Example:
     ```javascript
     (function() {
       // Code here
     })();
     ```

These are the main types of functions in JavaScript, each with its own characteristics and use cases. Understanding these different types of functions is essential for effective JavaScript programming.