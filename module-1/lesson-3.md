# Lesson 3 - Function return types, void, type aliases and classes

## Function return types

We can type the return value of a function using a colon `:`.

This will set the return type of the function (as well as both its arguments) to be a number:

```ts
function multiply(number1: number, number2: number): number {
	return number1 * number2;
}
```

If you tried to return a value type other than a number you would see a warning.

## Void

`void` has no type at all.

If a function doesn't return a value, you can set its return type to void:

```ts
function logMessage(message: string): void {
	console.log(`The message is: ${message}`);
}
```

---

## Types aliases

Types aliases create new names for type. They are similar to interfaces.

Let's see how we can make this function code more readable:

```ts
function LogError(errorCategory: string | number, errorDetails: { code: string | number; warning: string }) {}
```

The function has two parameters:

`errorCategory` is union type and accepts either a string or a number
`errorDetails` is an object with two properties, the first also holds a union type accepting either a number or string and the second holds a string value.

We'll create a type alias for the string | number union:

```ts
type StrNum = string | number;
```

We can use this type in our function code:

```ts
function LogError(errorCategory: StrNum, errorDetails: { code: StrNum; warning: string }) {}
```

We can also create a type for the `errorDetails` value:

```ts
type Details = { code: StrNum; warning: string };
```

And then use that type as the argument type:

```ts
function LogError(errorCategory: StrNum, errorDetails: Details) {}
```

Full code:

```ts
type StrNum = string | number;
type Details = { code: StrNum; warning: string };

function LogError(errorCategory: StrNum, errorDetails: Details) {}
```

## Classes

Classes were added to JavaScript with ES6 and provide a object-oriented way to handle inheritance and reusability.

Let's at this example:

```ts
class Product {
	id: number;

	constructor(idNumber: number) {
		this.id = idNumber;
	}
}
```

This class is called `Product` and has one property, `id` and a constructor.

The constructor is a special function in a class that is called when creating a new instance of a class.

```ts
const newProduct = new Product(12345);
```

Above we've created a new instance of the Product class and assigned it to the variable `newProduct`.

When creating the instance we passed the value `12345` to the constructor which assigned the value to its `id` property, using `this`.

`this` inside a function refers to the object the function belongs to, in this case it refers to the instance of the class created by the `new` keyword.

We can access the `id` property using dot notation:

```ts
console.log(newProduct.id);
```

### Class methods

Methods are functions within a class. We have already looked at the constructor method, which is invoked every time a new instance of the class is created.

Methods are created without the `function` keyword.

Let's add a method to the Product class that returns the id along with a message.

```ts
class Product {
	id: number;

	constructor(idNumber: number) {
		this.id = idNumber;
	}

	displayId() {
		return "This product's id is " + this.id;
	}
}
```

After creating a new instance of the class with the new keyword, we can call the method like this:

```ts
const newProduct = new Product(12345);
const productId = newProduct.displayId();
```

This will assign the return value of the method call to the `productId` variable which we could use elsewhere in the code.

```ts
console.log(productId);
```

<!-- ## Lesson Task

There are practice questions in the master branch of <a href="https://github.com/NoroffFEU/lesson-task-pf-module1-lesson3" target="_blank">this repo</a>.

There are example answers in the <a href="https://github.com/NoroffFEU/lesson-task-pf-module1-lesson3/tree/answers" target="_blank">answers branch</a>.

Try the exercises before checking the solutions. -->

---

[Go to lesson 4](4)

---
