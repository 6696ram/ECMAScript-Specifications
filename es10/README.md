## ES2019 Or ES10

### **Table of Contents**
| No. | Feature |
| --- | --------- |
|1  | [Array flat and flatMap](#array-flat-and-flatmap) |
|2  | [Object formEntries](#object-formentries) |
|3  | [String trimStart and trimEnd](#string-trimstart-and-trimend) |
|4  | [Symbol description](#symbol-description)|
|5  | [Optional Catch Binding](#optional-catch-binding)|
|6  | [JSON Improvements](#json-improvements)|
|7  | [Array Stable Sort](#array-stable-sort)|
|8  | [Function.toString()](#function.tostring())|
|9  | [Private Class Variables](#private-class-variables)|

1. ### Array flat and flatMap

   Prior to ES2019, you need to use `reduce() or concat()` methods to get a flat array.

   ```js
   function flatten(arr) {
     const flat = [].concat(...arr);
     return flat.some(Array.isArray) ? flatten(flat) : flat;
   }
   flatten([ [1, 2, 3], ['one', 'two', 'three', [22, 33] ], ['a', 'b', 'c'] ]);
   ```

   In ES2019, the `flat()` method is introduced to 'flattens' the nested arrays into the top-level array. The functionality of this method is similar to Lodash's `_.flattenDepth()` function. This method accepts an optional argument that specifies the number of levels a nested array should be flattened and the default nested level is 1.
   **Note:** If there are any empty slots in the array, they will be discarded.

   ```js
    const numberArray = [[1, 2], [[3], 4], [5, 6]];
    const charArray = ['a', , 'b', , , ['c', 'd'], 'e'];
    const flattenedArrOneLevel = numberArray.flat(1);
    const flattenedArrTwoLevel = numberArray.flat(2);
    const flattenedCharArrOneLevel = charArray.flat(1);

    console.log(flattenedArrOneLevel);    // [1, 2, [3], 4, 5, 6]
    console.log(flattenedArrTwoLevel);    // [1, 2, 3, 4, 5, 6]
    console.log(flattenedCharArrOneLevel);    // ['a', 'b', 'c', 'd', 'e']
   ```

   Whereas, **flatMap()** method combines `map()` and `flat()` into one method.  It first creates a new array with the return value of a given function and then concatenates all sub-array elements of the array.
   ```js
   const numberArray1 = [[1], [2], [3], [4], [5]];

   console.log(numberArray1.flatMap(value => [value * 10]));  // [10, 20, 30, 40, 50]
   ```

2. ### Object fromEntries

   In JavaScript, it is very commonn to transforming data from one format. ES2017 introduced `Object.entries()` method to objects into arrays.

   **Object to Array:**

   ```js
        const obj = {'a': '1', 'b': '2', 'c': '3' };
        const arr = Object.entries(obj);
        console.log(obj); // [ ['a', '1'], ['b', '2'], ['c', '3'] ]
   ```

   But if you want to get the object back from an array then you need iterate and convert it as below,

    ```js
    const arr = [ ['a', '1'], ['b', '2'], ['c', '3'] ];
    let obj = {}
    for (let [key, val] of arr) {
        obj[key] = val;
    }
    console.log(obj);
    ```
  We need a straightforward way to avoid this iteration. In ES2019, `Object.fromEntries()` method is introduced which performs the reverse of `Object.entries()` behavior. The above loop can be avoided easily as below,

  **Iterable( e.g Array or Map) to Object**
   ```js
    const arr = [ ['a', '1'], ['b', '2'], ['c', '3'] ];
    const obj = Object.fromEntries(arr);
    console.log(obj); // { a: "1", b: "2", c: "3" }
   ```
  One of the common case of this method usage is working with query params of an URL,
   ```js
    const paramsString = 'param1=foo&param2=baz';
    const searchParams = new URLSearchParams(paramsString);

    Object.fromEntries(searchParams);    // => {param1: "foo", param2: "baz"}
   ```

3. ### String trimStart and trimEnd
   In order to make consistency with padStart/padEnd, ES2019 provided the standard functions named as `trimStart` and `trimEnd` to trim white spaces on the beginning and ending of a string. However for web compatilibity(avoid any breakage) `trimLeft` and `trimRight` will be an alias for `trimStart` and `trimEnd` respectively.

   Let's see the usage with an example,
   ```js
    //Prior ES2019
    let messageOne = "   Hello World!!    ";
    console.log(messageOne.trimLeft()); //Hello World!!
    console.log(messageOne.trimRight()); //   Hello World!!

    //With ES2019
    let messageTwo = "   Hello World!!    ";
    console.log(messageTwo.trimStart()); //Hello World!!
    console.log(messageTwo.trimEnd()); //   Hello World!!
   ```

4. ### Symbol description

 While creating symbols, you also can add a description to it for debugging purposes. But there was no method to access the description directly before ES2019. Considering this, ES2019 introduced a read-only description property to retrieve a string containing the description of the Symbol.

 This gives the possibility to access symbol description for different variations of Symbol objects

 ```js
 console.log(Symbol('one').description); // one

 console.log(Symbol.for('one').description); // "one"

 console.log(Symbol('').description); // ''

 console.log(Symbol().description); // unefined

 console.log(Symbol.iterator.description); // "Symbol.iterator"
 ```

5. ### Optional catch binding
   Prior to ES9, if you don't need `error` variable and omit the same variable then catch() clause won't be invoked. Also, the linters complain about unused variables. Inorder to avoid this problem, the optional catch binding feature is introduced to make the binding parameter optional in the catch clause. If you want to completely ignore the error or you already know the error but you just want to react to that the this feature is going to be useful.

   Let's see the below syntax difference between the versions,
   ```js
    // With binding parameter(<ES9)
    try {
        ···
    } catch (error) {
        ···
    }
    // Without binding parameter(ES9)
    try {
        ···
    } catch {
        ···
    }
   ```
   For example, the feature detection on a browser is one of the most common case
   ```js
    let isTheFeatureImplemented = false;
    try {
      if(isFeatureSupported()) {
       isTheFeatureImplemented = true;
       }
    } catch (unused) {}
   ```
6. ### JSON Improvements

    JSON is used as a lightweight format for data interchange(to read and parse). The usage of JSON has been improved as part of ECMAScript specification. Basically there are 2 important changes related to JSON.

    1. **JSON Superset**

    Prior to ES2019, ECMAScript claims JSON as a subset in JSON.parse but that is not true. Because ECMAScript string literals couldn’t contain the characters `U+2028` (LINE SEPARATOR) and `U+2029` (PARAGRAPH SEPARATOR) unlike JSON Strings. If you still use those characters then there will be a syntax error. As a workaround, you had to use an escape sequence to put them into a string.
    ```js
    eval('"\u2028"'); // SyntaxError
    ```

    Whereas JSON strings can contain both U+2028 and U+2029 without producing errors.​

    ```js
    console.log(JSON.parse('"\u2028"')); // ''
    ```

    This restriction has been removed in ES2019. This simplifies the specification without the need of separate rules for ECMAScript string literals and JSON string literals.

    2. **Well Formed JSON.Stringify():**
    Prior to ES2019, JSON.stringify method is used to return unformed Unicode strings(ill-formed Unicode strings) if there are any lone surrogates in the input.

    ```js
    console.log(JSON.stringify("\uD800")); // '"�"'
    ```

    Whereas in ES2019, JSON.stringify outputs escape sequences for lone surrogates, making its output valid Unicode and representable in UTF-8.

    ```js
    console.log(JSON.stringify("\uD800")); // '"\ud800"'
    ```

7. ### Array Stable Sort
    The sort method for arrays is stable in ES2020. i.e, If you have an array of objects and sort them on a given key, the elements in the list will retain their position relative to the other objects with the same key.​
    Now the array is using the stable `TimSort` algorithm for arrays over 10 elements instead of the unstable `QuickSort`.

    Let's see an example of users retain their original position with same age group.
    ```js
    const users = [
        { name: "Albert",  age: 30 },
        { name: "Bravo",   age: 30 },
        { name: "Colin",   age: 30 },
        { name: "Rock",    age: 50 },
        { name: "Sunny",   age: 50 },
        { name: "Talor",    age: 50 },
        { name: "John",   age: 25 },
        { name: "Kindo",  age: 25 },
        { name: "Lary",   age: 25 },
        { name: "Minjie",   age: 25 },
        { name: "Nova",    age: 25 }
    ]
    users.sort((a, b) => a.age - b.age);
    ```

8. ### Function.toString()
    Functions have an instance method called `toString()` which return a string to represent the function code. Previous versions of ECMAScript removes white spaces,new lines and comments from the function code but it has been retained with original source code in ES2020.

    ```js
    function sayHello(message) {
        let msg = message;
        //Print message
        console.log(`Hello, ${msg}`);
    }

    console.log(sayHello.toString());
    // function sayHello(message) {
    //       let msg = message;
    //       //Print message
    //       console.log(`Hello, ${msg}`);
    //   }
    ```

9. ### Private Class Variables
     In ES6, the classes are introduced to create reusable modules and variables are declared in clousure to make them private. Where as in ES2020, private class variables are introduced to allow the variables used in the class only. By just adding a simple hash symbol in front of our variable or function, you can reserve them entirely for internal to the class.
     ```js
     class User {
       #message = "Welcome to ES2020"

       login() { console.log(this.#message) }
     }

     const user = new User()

     user.login() // Welcome to ES2020
     console.log(user.#message) // Uncaught SyntaxError: Private field '#
     ```

     **Note:** As shown in the above code, If you still try to access the variable directly from the object then you will receive syntax error.
