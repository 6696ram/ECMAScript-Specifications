## ES2017 Or ES8

### **Table of Contents**
| No. | Feature |
| --- | --------- |
|1  | [Async functions](#async-functions) |
|2  | [Object values](#object-values) |
|3  | [Object entries](#object-entries) |
|4  | [Object property descriptors](#object-property-descriptors) |
|5  | [String padding](#string-padding)|
|6  | [Shared memory and atomics](#shared-memory-and-atomics)|
|7  | [Trailing commas](#trailing-commas) |


1. ### Async functions

   In ES6, Promises were introduced to solve the famous callback hell problem. When a series of nested asynchronous functions need to be executed in order, it leads to a callback hell
   ```js
   function task() {
     task1((response1) => {
       task2(response1, (response2) => {
         task3(response2, (response3) => {
           // etc...
         };
       });
     });
   }
   ```
   But the Chained Promises creates complex flow for asynchronous code.

   Async functions were introduced as a combination of promises and generators to give us the possibility of writing asynchronous in a synchronous manner. i.e, This function is going to be declared with the `async` keyword which enable asynchronous, promise-based behavior to be written in a cleaner style by avoiding promise chains.  These functions can contain zero or more `await` expressions.

   Let's take a below async function example,

     ```js
     async function logger() {

       let data = await fetch('http://someapi.com/users'); // pause until fetch returns
       console.log(data)
     }
     logger();
     ```
2. ### Object values
   Similar to Object.keys which iterate over JavaScript object’s keys, Object.values will do the same thing on values. i.e, The Object.values() method is introduced to returns an array of a given object's own enumerable property values in the same order as `for...in` loop.

   ```js
    const countries = {
      IN: 'India',
      SG: 'Singapore',
    }
    Object.values(countries) // ['India', 'Singapore']
   ```

   By the way, non-object argument will be coerced to an object

   ```js
    console.log(Object.values(['India', 'Singapore'])); // ['India', 'Singapore']
    console.log(Object.values('India')); // ['I', 'n', 'd', 'i', 'a']
   ```

3. ### Object entries
   The `Object.entries()` method is introduced to returns an array of a given object's own enumerable string-keyed property [key, value] pairsin the same order as `for...in` loop.
   ```js
       const countries = {
         IN: 'India',
         SG: 'Singapore',
       }
       Object.entries(countries) // [["IN", "India"], ["SG", "Singapore"]]
   ```
   By the way, non-object argument will be coerced to an object
   ```js
      const countriesArr = ['India', 'Singapore'];
      console.log(Object.entries(countriesArr)); // [ ['0', 'India'], ['1', 'Singapore']]

      const country = 'India';
      console.log(Object.entries(country)); // [["0", "I"], ["1", "n"], ["2", "d"], ["3", "i"], ["4", "a"]]

      console.log(Object.entries(100)); // [], an empty array for any primitive type because it won't have any own properties
   ```

4. ### Object property descriptors

   Property descriptors describe the attributes of a property. The `Object.getOwnPropertyDescriptors()` method returns all own property descriptors of a given object.

   It provides the below attributes,

   1. **value:** The value associated with the property (data descriptors only).
   2. **writable:** true if and only if the value associated with the property may be changed
   3. **get:** A function which serves as a getter for the property.
   4. **set:** A function which serves as a setter for the property.
   5. **configurable:** true if and only if the type of this property descriptor may be changed or deleted.
   6. **enumerable:** true if and only if this property shows up during enumeration of the property.

   The usage of finding property descriptors for any property seems to be as below,

   ```js
    const profile = {
      age: 42
    };

    const descriptors = Object.getOwnPropertyDescriptors(profile);
    console.log(descriptors); //  {age: {configurable: true, enumerable: true, writable: true }}
   ```

5. ### String padding
   Some strings and numbers(money, date, timers etc) need to be represented in a particular format. Both `padStart() & padEnd()` methods introduced to pad a string with another string until the resulting string reaches the supplied length.

   1. **padStart():** Using this method, padding is applied to the left or beginning side of the string.

    For example, you may want to show only the last four digits of credit card number for security reasons,

    ```js
    const cardNumber = '01234567891234';
    const lastFourDigits = cardNumber.slice(-4);
    const maskedCardNumber = lastFourDigits.padStart(cardNumber.length, '*');
    console.log(maskedCardNumber); // expected output: "**********1234"
    ```

   2. **padEnd():** Using this method, padding is applied to the right or ending side of the string.

   For example, the profile information padded for label and values as below
   ```js
   const label1 = "Name";
   const label2 = "Phone Number";
   const value1 = "John"
   const value2 = "(222)-333-3456";

   console.log((label1 + ': ').padEnd(20, ' ') + value1);
   console.log(label2 + ": " + value2); // Name:                John
                                        // Phone Number: (222)-333-3456
   ```

6. ### Shared memory and atomics
   The Atomics is a global object which provides atomic operations to be performed as static methods. They are used with SharedArrayBuffer(fixed-length binary data buffer) objects. The main use cases of these methods are,

   1. **atomic operations:** When memory is shared, multiple threads can read and write the same data in memory. So there would be a chance of loss of data. But atomic operations make sure that predictable values are written and read, that operations are finished before the next operation starts and that operations are not interrupted.

      It provides static methods such as add, or, and, xor, load, store, isLockFree etc as demonstrated below.

        ```js
        const sharedMemory = new SharedArrayBuffer(1024);
        const sharedArray = new Uint8Array(sharedMemory);
        sharedArray[0] = 10;

        Atomics.add(sharedArray, 0, 20);
        console.log(Atomics.load(sharedArray, 0)); // 30

        Atomics.sub(sharedArray, 0, 10);
        console.log(Atomics.load(sharedArray, 0)); // 20

        Atomics.and(sharedArray, 0, 5);
        console.log(Atomics.load(sharedArray, 0));  // 4

        Atomics.or(sharedArray, 0, 1);
        console.log(Atomics.load(sharedArray, 0));  // 5

        Atomics.xor(sharedArray, 0, 1);
        console.log(Atomics.load(sharedArray, 0)); // 4

        Atomics.store(sharedArray, 0, 10); // 10

        Atomics.compareExchange(sharedArray, 0, 5, 10);
        console.log(Atomics.load(sharedArray, 0)); // 10

        Atomics.exchange(sharedArray, 0, 10);
        console.log(Atomics.load(sharedArray, 0)); //10

        Atomics.isLockFree(1); // true
        ```

   2. **waiting to be notified:**
      Both `wait()` and `notify()` methods provides ways for waiting until a certain condition becomes true and are typically used as blocking constructs.

      Let's demonstrate this functionality with reading and writing threads.

      First define a shared memory and array
      ```js
      const sharedMemory = new SharedArrayBuffer(1024);
      const sharedArray = new Int32Array(sharedMemory);
      ```

      A reading thread is sleeping and waiting on location 0 which is expected to be 10. You can observe a different value after the value overwritten by a writing thread.
      ```js
      Atomics.wait(sharedArray, 0, 10);
      console.log(sharedArray[0]); // 100
      ```

      Now a writing thread stores a new value(e.g, 100) and notifies the waiting thread,
      ```js
      Atomics.store(sharedArray, 0, 100);
      Atomics.notify(sharedArray, 0, 1);
      ```

7. ### Trailing commas
   Trailing commas are  allowed in parameter definitions and function calls
   ```js
   function func(a,b,) { // declaration
     console.log(a, b);
   }
   func(1,2,); // invocation
   ```
   But if the function parameter definition or function call only contains a comma, a syntax error will be thrown
   ```js
   function func1(,) {  // SyntaxError: missing formal parameter
     console.log('no args');
   };
   func1(,); // SyntaxError: expected expression, got ','
   ```

   **Note:** Trailing commas are not allowed in Rest Parameters and JSON.