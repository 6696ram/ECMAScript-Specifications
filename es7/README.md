## ES2016 Or ES7

### **Table of Contents**
| No. | Feature |
| --- | --------- |
|1  | [Array includes](#array-includes) |
|2  | [Exponentiation Operator](#exponentiation-operator) |

ES2015/ES6 introduced a huge set of new features. But ECMAScript 2016 Or ES7 introduced only two new features:

   1. Array.prototype.includes()
   2. Exponentiation operator

1. ### Array Includes
   Prior to ES7, you have to use `indexOf` method and compare the result with '-1' to check whether an array element contains particular element or not.
   ```js
    const array = [1,2,3,4,5,6];
    if(array.indexOf(5) > -1 ){
    console.log("Found an element");
    }
   ```
   Whereas in ES7, `array.prototype.includes()` method is introduced as a direct approach to determine whether an array includes a certain value among its entries or not.
   ```js
   const array = [1,2,3,4,5,6];
   if(array.includes(5)){
   console.log("Found an element");
   }
   ```
  In addition to this, `Array.prototype.includes()` handles NaN and Undefined values better than `Array.prototype.indexOf()` methods. i.e, If the array contains NaN and Undefined values then `indexOf()` does not return correct index while searching for NaN and Undefined.
  ```js
  let numbers = [1, 2, 3, 4, NaN, ,];
  console.log(numbers.indexOf(NaN)); // -1
  console.log(numbers.indexOf(undefined)); // -1
  ```
  On the otherhand, `includes` method is able to find these elements
  ```js
    let numbers = [1, 2, 3, 4, NaN, ,];
    console.log(numbers.includes(NaN)); // true
    console.log(numbers.includes(undefined)); // true
  ```
2. ### Exponentiation Operator

   The older versions of javascript uses `Math.pow` function to find the exponentiation of given numbers. ECMAScript 2016 introduced the exponentiation operator, **(similar to other languages such as Python or F#) to calculate the power computation in a clear representation using infix notation.
   ```js
   //Prior ES7
   const cube = x => Math.pow(x, 3);
   console.log(cube(3)); // 27

   //Using ES7
   const cube1 = x => x ** 3;
   console.log(cube1(3)); // 27
   ```