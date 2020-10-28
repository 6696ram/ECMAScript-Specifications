## ES2020 Or ES11

### **Table of Contents**
| No. | Feature |
| --- | --------- |
|1  | [BigInt](#bigint) |
|2  | [Dynamic Import](#dynamic-import) |
|3  | [Nullish Coalescing Operator](#nullish-coalescing-operator)|
|4  | [Optional chaining](#optional-chaining)|
|5  | [Promise allSettled](#promise-allsettled)|
|6  | [String matchAll](#string-matchall)|
|7  | [globalThis](#globalthis)|
|8  | [import.meta](#import.meta)|
|9  | [for..in order](#for..in-order)|


ES2020 is the current newer version of ECMAScript corresponding to the year 2020. This is the eleventh edition of the ECMAScript Language Specification. Even though this release doesn't bring as many features as ES6, it included some really useful features.

Most of these features already supported by some browsers and try out with babel parser support for unsupported features. This edition is set for final approval by the ECMA general assembly in June, 2020. The [ECMAScript 2020 (ES2020) language specification](https://tc39.es/ecma262/2020/) is ready now.

1. ### BigInt
    In earlier JavaScript version, there is a limitation of using the Number type. i.e, You cannot safely represent integer values(`Number` primitive) larger than pow(2, 53). In ES2020,

    `BigInt` is introduced as the 7th primitive type to represent whole numbers(integers with arbitrary precision) larger than pow(2, 53) - 1(or 9007199254740991 or Number.MAX_SAFE_INTEGER). This is been created by appending `n` to the end of an integer literal or by calling the function BigInt().

    ```js
    // 1. Current number system
    const max = Number.MAX_SAFE_INTEGER;
    console.log(max + 1) // 9007199254740992
    console.log(max + 2) // 9007199254740992

    // 2. BigInt representation
    const bigInt = 9007199254740991n;
    const bigIntConstructorRep = BigInt(9007199254740991); // 9007199254740991n
    const bigIntStringRep = BigInt("9007199254740991"); // 9007199254740991n

    // 3. Typeof usage

    console.log(typeof 1)// number
    console.log(typeof 1n)// bigint
    console.log(typeof BigInt('1'))// bigint

    // 4. Operators

    const previousMaxNum = BigInt(Number.MAX_SAFE_INTEGER);
    console.log(previousMaxNum + 2n); //9007199254740993n (this was not possible before)
    console.log(previousMaxNum -2n); //9007199254740990n
    console.log(previousMaxNum * 2n); //18014398509481982n
    console.log(previousMaxNum % 2n); //1n
    console.log(previousMaxNum / 2n); // 4503599627370495n

    // 5. comparison
    console.log(1n === 1); // false
    console.log(1n === BigInt(1)); // true
    console.log(1n == 1); // true
    ```

2. ### Dynamic Import
    Static imports supports some of the important use cases such as static analysis, bundling tools, and tree shaking, it is also it's desirable to be able to dynamically load parts of a JavaScript application at runtime.

    The new feature `dynamic import` is introduced to load a module conditionally or on demand. Since it returns a promise for the module namespace object of the requested module, the module can be resolved or import can now be assigned to a variable using async/await as below
    ```js
    <script>
    const moduleSpecifier = './message.js';
    import(moduleSpecifier)
        .then((module) => {
            module.default(); // Hello, default export
            module.sayGoodBye(); //Bye, named export
        })
        .catch( err => console.log('loading error'));
    </script>
    ```
    ```js
    <script>
    (async function() {
        const moduleSpecifier = './message.js';
        const messageModule = await import(moduleSpecifier);
        messageModule.default(); // Hello, default export
        messageModule.sayGoodBye(); //Bye, named export
    })();
    </script>
    ```
    and the imported module appears with both default and named exports
    ```js
    export default () => {
       return "Hello, default export";
    }
    export const sayGoodBye = () => {
       return "Bye, named export"
    }
    ```

    **Note:** Dynamic import does not require scripts of `type="module"`

3. ### Nullish Coalescing Operator
    The nullish coalescing operator (??) is a logical operator that returns its right-hand side operand when its left-hand side operand is `null` or `undefined`, and otherwise returns its left-hand side operand. This operator replaces `||` operator to provide default values if you treat empty value or '', 0 and NaN as valid values. This is because the logical OR(||) operator treats(empty value or '', 0 and NaN) as falsy values and returns the right operand value which is wrong in this case. Hence, this operator truely checks for `nullish` values instead `falsy` values.
    ```js
    let vehicle = {
        car: {
            name: "",
            speed: 0
        }
    };

    console.log(vehicle.car.name || "Unknown"); // Unknown
    console.log(vehicle.car.speed || 90); // 90

    console.log(vehicle.car.name ?? "Unknown"); // ""(empty is valid case for name)
    console.log(vehicle.car.speed ?? 90); // 0(zero is valid case for speed)
    ```
    In a short note, nullish operator returns a non-nullish value and || operator returns truthy values.

4. ### String matchAll

    There is `String#match` method to get all the matches of a string against a regular expression by iterating for each match. However this method gives you the substrings that match.

    The `String#matchAll()` is a new method added to String prototype, which returns an iterator of all results matching a string against a regular expression.
    ```js
    const regex = /t(e)(st(\d?))/g;
    const string = 'test1test2';
    const matchesIterator = string.matchAll(regex);
    Array.from(matchesIterator, result => console.log(result));
    ```
    When you this code in browser console, the matches iterator produces an array for each match including the capturing groups with a few extras.
    ```cmd
    ["test1", "e", "st1", "1", index: 0, input: "test1test2", groups: undefined]
    ["test2", "e", "st2", "2", index: 5, input: "test1test2", groups: undefined]
    ```

5. ### Optional chaining

    In JavaScript, Long chains of property accesses is quite error-prone if any of them evaluates to `null` or `undefined` value. Also, it is not a good idea to check property existence on each item which in turn leads to a deeply-nested structured `if` statements.

    Optional chaining is a new feature that can make your JavaScript code look cleaner and robust by appending(?.) operator to stop the evaluation and return undefined if the item is undefined or null.
    By the way, this operator can be used together with nullish coalescing operator to provide default values
    ```js
    let vehicle = {
    };

    let vehicle1 = {
        car: {
            name: 'ABC',
            speed: 90
        }
    };

    console.log(vehicle.car?.name); // TypeError: Cannot read property 'name' of undefined

    console.log(vehicle.car?.name); // Undefined
    console.log(vehicle.car?.speed); // Undefined

    console.log(vehicle1.car?.name); // ABC
    console.log(vehicle1.car?.speed); // 90

    console.log(vehicle.car?.name ?? "Unknown"); // Unknown
    console.log(vehicle.car?.speed ?? 90); // 90
    ```

5. ### Promise.allSettled

    It is really helpful to log(especially to debug errors) about each promise when you are handling multiple promises. The  `Promise.allSettled()` method returns a new promise that resolves after all of the given promises have either fulfilled or rejected, with an array of objects describing the outcome of each promise.
    ```js
    const promise1 = new Promise((resolve, reject) => setTimeout(() => resolve(100), 1000));

    const promise2 = new Promise((resolve, reject) => setTimeout(reject, 1000));

    Promise.allSettled([promise1, promise2]).then(data => console.log(data)); // [
                                                                                 Object { status: "fulfilled", value: 100},
                                                                                 Object { status: "rejected", reason: undefined}
                                                                                 ]
    ```
    As per the output, each outcome object returns `status` field which denotes either "fulfilled"(value present) or "rejected"(reason present)

6. ### globalThis
    Prior to ES2020, you need to write different syntax in different JavaScript environments(cross-platforms) just to access the global object. It is really a hard time for developers because you need to use `window, self, or frames` on the browser side, `global` on the nodejs, `self` on the web workers side.

    On the other hand, `this` keyword can be used inside functions for non-strict mode but it gives undefined in strict mode. If you think about `Function('return this')()` as a solution for above environments, it will fail for CSP enabled environments(where eval() is disabled).

    In the older versions, you can use es6-shim as below,
    ```js
    var getGlobal = function () {
      if (typeof self !== 'undefined') { return self; }
      if (typeof window !== 'undefined') { return window; }
      if (typeof global !== 'undefined') { return global; }
      throw new Error('unable to locate global object');
    };

    var globals = getGlobal();

    if (typeof globals.setTimeout !== 'function') {
      console.log('no setTimeout in this environment or runtime');
    }
    ```
    In ES2020, `globalThis` property is introduced to provide a standard way of accessing the global this value across environments.
    ```js
    if (typeof globalThis.setTimeout !== 'function') {
      console.log('no setTimeout in this environment or runtime');
    }
    ```

8. ### import.meta

    The `import.meta` object was created by the ECMAScript implementation with a null prototype to get context-specific metadata about a JavaScript module.
    Let's say you are trying to load `my-module` from a script,
    ```js
    <script type="module" src="my-module.js"></script>
    ```
    Now you can access meta information(base URL of the module) about the module using the import.meta object
    ```js
    console.log(import.meta); // { url: "file:///home/user/my-module.js" }
    ```
    The above URL can be either URL from which the script was obtained (for external scripts), or the document base URL of the containing document (for inline scripts).

    **Note:** Remember `import` is not really an object but `import.meta` is provided as an object which is extensible, and its properties are writable, configurable, and enumerable.

9. ### for..in order

    Prior to ES2020, the specifications did not specify in which order for (a in b)  should run. Even though most of the javascript engines/browsers loop over the properties of an object in the order in which they were defined, it is not the case with all scenarios. This has been officially standardized in ES2020.
    ```js
    var object = {
      'a': 2,
      'b': 3,
      'c': 4
    }


    for(let key in object) {
      console.log(key); // a b c
    }
    ```