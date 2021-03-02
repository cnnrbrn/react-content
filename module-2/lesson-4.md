# Lesson 4

## React Hook Form

As a fontend developer you will need to create and validate a lot of forms.

<a href="https://react-hook-form.com/" target="_blank">React Hook Form</a> is one of the easiest form libraries to use.

---

> For the sake of brevity the example forms here use mimimal markup and exclude styling and labels or placeholders. Remember to make your forms semantically correct and user-friendly.

```
npm install react-hook-form
```

---

> The examples are in <a href="https://github.com/NoroffFEU/react-hook-form-examples" target="_blank">this repo</a>. Run the examples and watch the console for the error object and the object passed into the onSubmit function when validation passes.

## First form example

<a href="https://github.com/NoroffFEU/react-hook-form-examples/blob/master/src/components/SimpleExample.js" target="_blank">src/components/SimpleExample.js</a>

```js
import { useForm } from "react-hook-form";

function App() {
	const { register, handleSubmit, errors } = useForm();

	function onSubmit(data) {
		console.log(data);
	}

	console.log(errors);

	return (
		<form onSubmit={handleSubmit(onSubmit)}>
			<input name="firstNmame" ref={register} />

			<input name="lastName" ref={register({ required: true })} />
			{errors.lastName && <span>This field is required</span>}

			<button>Send</button>
		</form>
	);
}

export default App;
```

First we import the `useForm` hook from React Hook Form.

On line 4 we get the `register` and `handleSubmit` functions and `errors` object from calling `useForm`.

Lines 6 - 8 is the function passed to `handleSubmit` when the form is submitted. This funcion is only executed when the form passes validation. In this example the `onSubmit` will just log the `data` argument which is an object with all the form input values. This is the function you would use to make a POST, PUT or PATCH request, and the `data` object argument would be the body of the request.

On line 10 we are logging the `errors` object. If there any properties in this object the `onSubmit` function will not be executed. This object is used to display any validation errors.

On line 13 the submit event handler of the form is assigned the `handleSubmit` function.

On line 14 we pass the `register` function the input's ref. This registers the input with the library, React Hook Form is now aware of this input.

On line 16 we again pass the `register` function the input's ref, this time with a validation object passed into `register`. We are indicating here that this is a required input.

When the submit button is pressed, validation will be run. If validation fails and there are properties in the error object, the errors can be rendered like on line 17.

Line 17 is an example of conditional rendering. The code is saying "if errors.lastName exists, display this span element".

---

## Validation with Yup

<a href="https://github.com/jquense/yup" target="_blank">Yup</a> is a schema builder for value parsing and validation.

We can use it create validation rules for our form inputs.

Install the following two packages:

```
npm install yup @hookform/resolvers
```

Below we'll create a form with the following inputs and validation rules:

-   `name` - required
-   `email` - required and must be a valid email format
-   `message` - minimum 10 characters

<a href="https://github.com/NoroffFEU/react-hook-form-examples/blob/master/src/App.js" target="_blank">src/App.js</a>

```js
import { useForm } from "react-hook-form";
import * as yup from "yup";
import { yupResolver } from "@hookform/resolvers/yup";
import "./App.css";

const schema = yup.object().shape({
	name: yup.string().required("Please enter your name"),
	email: yup.string().required("Please enter an email address").email("Please enter a valid email address"),
	message: yup.string().required("Please enter your message").min(10, "The message must be at least 10 characters"),
});

function App() {
	const { register, handleSubmit, errors } = useForm({
		resolver: yupResolver(schema),
	});

	function onSubmit(data) {
		console.log(data);
	}

	console.log(errors);

	return (
		<form onSubmit={handleSubmit(onSubmit)}>
			<input name="name" ref={register} />
			{errors.name && <span>{errors.name.message}</span>}

			<input name="email" ref={register} />
			{errors.email && <span>{errors.email.message}</span>}

			<textarea name="message" ref={register}></textarea>
			{errors.message && <span>{errors.message.message}</span>}

			<button>Send</button>
		</form>
	);
}

export default App;
```

We've imported the two packages we've installed and on lines 6 - 10 the schema we're going to use for validation is created.

The keys in the shape object (name, email, message) must be the same as the name attributes on the inputs.

All three inputs are string values.

We are providing our own validation messages. If we'd left them out default validation messages would be displayed. So the schema could be written like this:

```js
const schema = yup.object().shape({
	name: yup.string().required(),
	email: yup.string().required().email(),
	message: yup.string().required().min(10),
});
```

But it's often preferable to provide your own messages.

In the first example we hard coded the error message below the required input:

```js
{
	errors.lastName && <span>This field is required</span>;
}
```

In this example we are using the `message` property from each error:

```js
{
	errors.name && <span>{errors.name.message}</span>;
}
```

This message property is the value we set in the schmema (or the default messages).

On lines 13 - 15 when call the `useForm` hook, we're passing in the schema as a resolver. This links the yup validation to the form.

You can find out how to set all the validation rules in the <a href="https://github.com/jquense/yup#api" target="_blank">official docs</a>.

--

## Integrating inputs from libraries

If you need to integrate a component from a library like <a href="https://react-select.com/" target="_blank">React Select</a> or a date picker, have a look at the <a href="https://react-hook-form.com/get-started#IntegratingControlledInputs" target="_blank">Controlled Inputs example</a> in the official React Hook Form docs.

<a href="https://codesandbox.io/s/react-hook-form-controller-079xx?file=/src/index.js" target="_blank">Here</a> is an example form using several library components like React Select, React Date Picker, Auto complete and Material UI components.

---

## Lesson Task

The lesson task is in <a href="https://github.com/NoroffFEU/lesson-task-js-frameworks-module2-lesson4" target="_blank">this repo</a>, with an example answer on the answer branch.

---

## Activities

## Watch

-   <a href="https://react-hook-form.com/" target="_blank">The official React Hook Form introduction video</a>

## Browse

-   <a href="https://github.com/react-hook-form/react-hook-form/tree/master/examples" target="_blank">The React Hook Form code examples</a>

---

[Go to the MA](ma)

---
