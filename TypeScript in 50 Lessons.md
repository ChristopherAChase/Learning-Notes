# TypeScript in 50 Lessons

## Lesson 11 - Typing Objects

- Composite types are a combination of property names and other types
  - These property types can be both primitive, and even more composite types
- ==Objects are composite types.==

```typescript
//Objects are types.
const book = {
  title: 'Lets Talk',
  price: 28.99,
  vat: 0.19,
  stock: 1000, 
  description: 'A good book on providing good feedback in various forms'
};

type Article = {
  title: string, 
  price: number, 
  vat: number, 
  stock: number, 
  description: string
};

```

- Type annotating a variable as a composite type ensures we will have all of the required properties needed to function
- Type annotating an object and having too many properties throws an error and is called **excess property check**. 
  - Creating an object with all of the properties of a type and with an extra property, and then later assigning that object to a constant that is type annotated as a type with all the same, but fewer properties does not throw an error. 
    - You just won’t be able to access those properties in development
- Type annotating an object and having too few properties also throws an error. 
  - Creating an object with too few properties of a type, and then assigning that object to a type annotated object will throw an error because it doesn’t meet the minimum requirements.
- You can type annotate the parameters of a function to require a composite type. 
  - You can call this function with any object that meets the structural requirement of the expected object. 
- You can also specify the list of required properties an object must have to be passed into the function.

```typescript
type Coordinate = {
  xPos: number, 
  yPos: number
}

const moveUp(coordinate: Coordinate){
  return {
    xPos: coordinate.xPos, 
    yPos: coordinate.yPos + 1
  }
}

const moveUp(coordinate: {xPos: number, yPos: number}){
  return {
    xPos: coordinate.xPos, 
    yPos: coordinate.yPos + 1
  }
}

const currentLocation: Coordinate = {
  xPos: 12, 
  yPos: 3
};

const threeDimensionalLocation: {
  xPos: 12, 
  yPos: 3, 
  zPos: 42
};

const newCurrentLocation = moveUp(currentLocation);

const newThreeDimentionalLocation = moveUp(threeDimensionalLocation);
```



## Lesson 12: Object Type Tool Belt

- Types can get quite complicated fairly quickly

- The `typeof` operator takes any object, function, or constant, and extracts the *shape* of it.

  - ```typescript
    const threeDimensionalCoordinate = {
      xPos: 12, 
      yPos: 3, 
      zPos: 42
    };
    
    type ThreeDimensionCoordinate = typeof threeDimensionalCoordinate;
    // ThreeDimensionCoordinate now has the same properties as the object's declaration.
    ```

  - So now, when the `threeDimensionalCoordinate` object changes, the type does too!
    - ==So when the object definition changes, so does the type structure? So if I were to add a property to the object, would the type have that new property available? Is this a good thing?==
      - ==Should the type determine the object, or the object determine the type?==

- We can create types with optional properties in the event that the properties are a part of the object, but aren’t always present. 

  - Place a question mark after the property name

  - ```typescript
    type Coordinate = {
      xPos: number, 
      yPos: number, 
      zPos?: number
    };
    
    const isThreeDimensional = (coordinate: Coordinate): boolean => coordinate.zPos;
    
    ```

- Exporting a type is as simple as placing the keyword `export` before the `type` keyword
- Importing is done at the top of any file that wants to use a type that has been exported.

## Lesson 13: Typing Classes

- Normally, our declarations do one of two things - either create types, or create values
- Using classes is where those two meet. 
  - By creating a class, it’s available in the type space as well. 
    - This allows for the classes methods, method definitions, and properties are all type safe and available to intellisence.



## Lesson 15: A search Function

Just like we can type objects, we can also type functions. 

```typescript
type Result = {
  title: string, 
  url: string, 
  abstract: string
};

declare function search(query: string, tags?: string[]) => Result[];
```

- Just like making object properties optional, we can make function parameters optional. 
- Generic types look like `{Object}<{type}>` with the angle brackets denoting which type the object must have the shape of.



## Lesson 16: Callbacks

- Functions have types too! `type SearchFunction = typeof search;` will make `SearchFunction` a function with the same type structure as the search function above.

- With functions as a type, you can assign a function to a type’s property value: 

  ```typescript
  type AssembleFunction = (includeTags: boolean) => string;
  type Query = {
    query: string, 
    tags?: string[], 
    assemble: AssembleFunction
  }
  ```

  - ❓ Is assigning functions to a types property something that is recommended? Whenever I think of the implementation of doing this, I imagine you’d have to use the `this` keyword, and I know that that keyword is kind of taboo in JavaScript because it’s meaning isn’t always what you think it is, so just stay away from it. Is it recommended to keep your functions separate from your objects, and just be able to apply your functions to objects instead of have your functions be apart of your objects?

- ❓ Is what makes a callback function the fact that we are passing the function as a parameter as opposed to the results of the function? Or is it a callback function because of how the function parameter is being used? 
- ❓ Typescript Unrelated - What is the purpose of `Promise.resolve`? MDN says it returns a promise object that is resolved with a given value. 
  - My understanding is a promise is JavaScript’s implementation of an asynchronous function that is guaranteed to get you a result… eventually. By using `Promise.resolve`, are we cutting the “eventually” down to “immediately” and short-circuiting the waiting by saying “this is the value that promise returned”? Or am I misunderstanding this? What is the point/benefit of doing this? 
- TypeScript has a structural type system - this also applying to functions.
  - This means if a function is declared/typed as expecting two strings with parameter names `firstName` and `lastName`, but when you use it, you create the function with parameters `fName` and `lName`, but they’re both strings, then TypeScript doesn’t care. 
  - This could help with providing the current use of the function some context as to what the values mean if the declared function used very generic terms.

## Lesson 17: Substitutability

- Not only can we leave out parameters that are optional… but we can also leave out parameters of a function if they’re not going to be used!?!
- So we can have less parameters required than the function type used, but not more. 
- But when using the function that was declared with less parameters than the function type it was annotated with, we still can’t pass zero parameters in if the annotated function type requires at least one parameter - even if we’re not going to do anything with that parameter…
  - This makes sense in the aspect that typescript is making sure you hold true to the contract of the annotated function type… but seems odd that I can annotate a function with the function type, with declare it with less parameters than the function type in the first place… and then to not be able to use the amount of parameters I declared the new function with because the annotated function type requires more parameters… ⁉⁉⁉

- `void` is TypeScript’s version of undefined?
- If a function type is expecting a `void` return type (nothing being returned), and your implementation returns something, that something will be treated as void/undefined, so you can’t do anything with it. 
  - If a function type is expecting an `undefined` return type, and your implementation returns something other than undefined (including void), typescript will throw an error.

## Lesson 18: This and That

- `this` is always bound to an object. 
- `getElementById` returns an element of `HTMLElement` which is the supertype to all HTML Elements. 
- in order to use `this` in our callback, we have to check which type of an HTML element we’re working with
  - `if(this instanceof HTMLInputElement)` makes sure that the object referenced by `this` is the correct subtype you’re wanting to work with. 
- You can also use `this` as a parameter name, specifying the type of object that `this` represents when extracting the callback method.



## Lesson 19: The function Type Tool Belt

