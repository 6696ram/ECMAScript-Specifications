## ES2018 Or ES9

### **Table of Contents**
| No. | Feature |
| --- | --------- |
|1  | [Async iterators](#async-iterators) |
|2  | [Object rest and spread operators](#object-rest-and-spread-operators) |
|3  | [Promise finally](#promise-finally) |


1. ### Async iterators

   ECMAScript 6 provides built-in support for synchronously iterating over data using iterators. Both strings and collections objects such as Set, Map, and Array come with a Symbol.iterator property which makes them iterable.

   ```js
    const arr = ['a', 'b', 'c', 'd'];
    const syncIterator = arr[Symbol.iterator]();

    console.log(syncIterator.next());    //{value: a, done: false}
    console.log(syncIterator.next());    //{value: b, done: false}
    console.log(syncIterator.next());    //{value: c, done: false}
    console.log(syncIterator.next());    //{value: d, done: false}
    console.log(syncIterator.next());    //{value: undefined, done: true}
   ```

   But these iterators are only suitable for representing synchronous data sources.

   In order to access asynchronous data sources, ES2018 introduced the AsyncIterator interface, an asynchronous iteration statement (for-await-of), and async generator functions.

2. ### Object rest and spread operators

   ES2015 or ES6 introduced both rest parameters and spread operators to convert arguments to array and vice versa using three-dot(...) notation.
   1. Rest parameters can be used to convert function arguments to an array

       ```js
        function myfunc(p1, p2, ...p3) {
          console.log(p1, p2, p3); // 1, 2, [3, 4, 5, 6]
        }
       myfunc(1, 2, 3, 4, 5, 6);
       ```

   2. The spread operator works in the opposite way by converting an array into separate arguments in order to pass to a function

       ```js
        const myArray = [10, 5, 25, -100, 200, -200];
        console.log( Math.max(...myArray) ); // 200
       ```

   ES2018 enables this rest/spread behavior for objects as well.

   1. You can pass object to a function

       ```js
        function myfunc1({ a, ...x }) {
            console.log(a, x); // 1, { b: 2, c: 3, d:4 }
        }
        myfunc1({
          a: 1,
          b: 2,
          c: 3,
          d: 4
        });
       ```

   2. The spread operator can be used within other objects

        ```js
        const myObject = { a: 1, b: 2, c: 3, d:4 };
        const myNewObject = { ...myObject, e: 5 }; // { a: 1, b: 2, c: 3, d: 4, e: 5 }
        ```

3. ### Promise finally

   Sometimes you may need to avoid duplicate code in the then() and catch() methods.

   ```js
   myPromise
       .then(result => {
           // process the result and then clean up the resources
       })
       .catch(error => {
           // handle the error and then clean up the resources
       });
   ```

   The `finally()` method is useful if you want to do some processing or resource cleanup once the promise is settled(i.e either fulfilled or rejected).

   Let's take a below example to hide the loading spinner after the data is fetched and processed.

   ```js
     let isLoading = true;
     fetch('http://somesite.com/users')
        .then(data => data.json())
        .catch(err => console.error(err))
        .finally(() => {
            isLoading = false;
            console.log('Finished loading!!');
          })
   ```
