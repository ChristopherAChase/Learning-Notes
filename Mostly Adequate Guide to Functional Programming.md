# Mostly Adequate Guide to Functional Programming

## Chapter 2 - First Class Functions

- A function is considered “first class” because it can be assigned to a variable and used anywhere a variable can be used

  - Assigned to another variable value
  - Function parameters
  - Arrays
  - etc.

  ```js
  const add = (x, y) => x + y;
  
  const moreMathBad = (x, y) => add(x, y); // This just takes the same parameters as the add function, then passes them into the add function.
  
  const moreMathGood = add;
  
  moreMath(2, 4); //Returns 6
  ```

  ```js
  const add = (x, y) => x + y;
  const double = x => add(x, x);
  ```

### Why First Class Functions are Better

- First class functions remove layers of indirection and overly verbose code.
- Reduces maintenance cost when functions have to be changed. 
- If the wrapped function has to be changed, then the wrapper function must also be changed
- Simplifies naming of arguments 
  - The names are allowed/encouraged to be much more generic so that a function is not bound to the specific domain within your application.



## Chapter 3 - Pure Functions

- A pure function is a function that, given the same input, will always return the same output and does not have any observable side effects
- A pure function cannot rely on any mutable objects/values, and shouldn’t rely on any values outside of the function itself.

```js
//impure - relies on the mutable minimumAge value that is outside the scope of the function. 
let minimumAge = 21;
const checkAge = age => age >= minimumAge;

//pure - the minimumAge is now a constant that is within the scope of the function. 
const checkAge = (age) => {
  const minimumAge = 21;
  return age >= minimumAge;
}
```

### What is a Side Effect?

- An **effect** is anything that occurs in our computation other than the calculation of the results. 
- A **side effect** is a change of system state or observable interaction with the outside world that occurs during the calculation of a result.
  - Changing the file system
  - Inserting a record into a database
  - Making an HTTP call
  - Mutations
  - Printing to the Screen/Logging
  - Obtaining User Input
  - Querying the DOM
  - Accessing System State
  - ANY Interaction with the world outside of a function
- Side effects must occur in order for an application to be an application… but the goal with functional programming is to contain the side effects and only have them occur in a controlled way. 
- Side effects disqualify a function from being pure. 
  - Pure functions must always return the same output given the same input
  - This guarantee is no longer there when dealing with things outside the scope of our function.



### What is a function? 

- A function is a special relationship between values: Each of its input values gives back exactly one output value. 

- Can be described as a set of pairs with the position `(input, output)`

  - A doubling function would have the pairs of `[(1, 2), (2, 4), (3, 6), (4, 8), (5, 10)]`

- Because an input to a function will deterministically provide you the same output, the implementation details are irrelevant. 

  - You could map out a bunch of inputs with their output into an object, and retrieve the values from that object. 

    ```js
    const double = x => x * 2;
    double(2) //4
    
    
    const double = {
      1: 2, 
      2: 4, 
      3: 6,
      4: 8, 
      5: 10, 
      6: 12,
      ...
    };
      
    double[2] //4
    ```

- Pure functions *are* mathematical functions, and they are the goal of functional programming. 
  - Every input has one output. 

### Benefits of Pure Functions

- Pure functions can be cached by input - typically done by a technique called memoization.
- Pure functions are completely self contained
  - Everything a function needs in order do what it does is contained inside the function. 
    - It does not depend on anything outside of the function, so you don’t have to look outside the function to understand what it’s doing.
  - A pure function must be honest about its dependencies
  - By being self contained, it makes these functions portable - they can be used in multiple applications without changing anything but what’s into them.
- Pure functions make testing much easier - simply pass in an input and assert an output. 
- Pure functions provide **referential transparency** 
  - A spot of code is referentially transparent when it can be substituted for its evaluated value without changing the behavior of the program
    - Since pure functions (a) don’t have side effects and (b) have one input per output, I can evaluate a function and replace it’s use with the evaluated value.
- Pure functions can be ran in parallel 
  - Because they don’t rely on shared memory, and can’t have race conditions due to side effects.

## Chapter 4 - Currying





