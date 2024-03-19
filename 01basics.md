# Clean Code Basics

There is some low-hanging fruit to making code readable.

One easy thing to do is improve names.

Programming languages are general purpose. Our code is specific to our needs. Therefore the programming language keywords don't add much by way of explaining our code. That must happen by the names we choose for things.

![Naming things](/images/naming-things.png)

## Naming data - variables

The most common ingredient in code is the humble variable:

```javascript
let t = 0.0;
```

We see a variable declared and initialised to zero.

_Why is `t` there? What does `t` represent?_

The meaning of `t` is unclear. It could be anything. Not good when we are in ahurry to understand the code!

The simplest way to help our readers is to name the variable for what it holds.

Imagine in this case our variable `t` holds the total price of an invoice.

Let's call it that:

```javascript
let totalPrice = 0.0;
```

No surprises there.

That now helps us understand what that variable is _intended_ to be used for.

> **Name variables by what they store**

It's important that our variable names are accurate. If we have `totalPrice` make sure it is _only_ ever used to hold a total price - nothing else.

### Common trap: naming by data type

A common mistake is to name variables according to a data type:

```javascript
let myString = "BJSS Academy";
const number = 36;
```

_What does myString mean? Why would I use your string?_

Remember to name variables according to what they hold.

Let's _rename_ those variables to do just that:

```javascript
let departmentName = "BJSS Academy";
const maximumNumberOfStudents = 36;
```

> Your IDE often has a _Refactor > Rename_ option that does this automatically

This is much more useful to future readers.

### Common trap: useless names

A smorgasbord for your delight and delectation:

```javascript
let tmp = "dogbanana"; // temporary what?

const number = 6; // Yes, I can see that is indeed a number, cheers

let length = array.length // length? Like in miles? How many *of what*?

function process(input) {
   // I got a two-for-one name fail right there
}

// Words that mean many things to many people, and yet nothing...

const controller = ...

let manager = ...

const service = ...

const handler = ...

function handle( event ) {
  // Well, that cleared things up nicely, cheers
}
```

## Naming code blocks - functions and methods

All programs feature blocks of code.

In all but the simplest of scripts, we aim to divide code up into smaller blocks. We then assemble these blocks to make a bigger application. This is [decomposition](https://www.bbc.co.uk/bitesize/topics/zkcqn39/articles/z8ngr82#zg73r2p), where we take a big problem and split it up.

Most languages provide tools to give names to these code blocks and organise them:

- Function
- Procedure
- Module export/import
- Package
- Class and Method in object-oriented languages

Let's start with the most common code block, the function.

It's important to be able to tell what a function does _from its name_, not by having to read the code block.

What does this function do?

```javascript
function z(a) {
  return "Hello, " + a;
}
```

We have to look inside the function and _reverse engineer_ what it does. In this case it seems to return the word Hello, and then add on whatever thing we passed in to it.

_This takes time_. It is time wasted by everybody _every time_ they have to read this code.

Let's improve both the function name and the variable name to be more descriptive:

```javascript
function greetUser(name) {
  return "Hello, " + name;
}
```

That's better! The function will greet a user by name. Got that. Now it's clear what we intended this function to be used for.

### This is abstraction - getting closer to the problem

Our improved function now forms an _abstraction_. We have a higher-level building block called `greetUser` that we can use to, well, greet our users.

Looking at the call site - where the function is called - this becomes even more obvious:

```javascript
const greeting = greetUser("alan");
```

This is now more than a function. We have introduced terms from our problem domain into the code. The code is beginning to speak the language of the _problem_ more strongly than that of the _implementation_.

#### Abstractions - commit to what, defer how

We are describing **what** is being solved over **how**.

Ultimately, this leads to us creating a _Domain Specific Language (DSL)_. The more domain specific words we get in our code, the better our code tells the story of the problem being solved.

A further benefit of such abstractions is it frees us from reading the inside of the function.
If we can trust the code we read, we don't need to understand the details of how it works. We can simply use it.

This has two further benefits:

- easier to change the implementation without changing everywhere that calls it
- we can create a _library_ of such functions. There, we don't see the source code. The function usage must be clear.

### Naming rule for functions

This leads to our naming rule for functions:

> **Name functions for what they do**

A function name explains why we would call that function.

#### Common trap: naming by implementation details

Thinking up good names is hard (...that again...).

It's tempting to give up on thinking about _why_ we would want to call that function, and instead just describe _how_ it works:

```javascript
returnWordHelloAppendParameter("alan");
```

Now, this does form a pretty good description of how our function works. But obfuscates what our function _is there for_.

For callers of the function, we get more useful information from

```javascript
const greeting = greetUser("alan");
```

Focus on what functions do, in the context of why we would want to call them.

> A function presents a _behaviour_ that our application can make use of

There is a further benefit of this abstracted approach. We can change how the function works internally, without having to _ripple out_ that change to every call site:

```javascript
// Change the wording of the greeting. Enforce proper capitalisation of the name
function greetUser(name) {
  const capitalisedName = name[0].toUpperCase() + name.slice(1);
  const greeting = "Welcome, most glorious colleague " + capitalisedName;

  return greeting;
}

// NOTE: No change to the rest of the codebase - MASSIVE WIN
greetUser("alan");
```

A large part of clean coding is finding the useful abstractions, then naming them well.

### Clarity is a cost saving

Think about a large codebase, where everything is badly named (_shudders_).

That reverse engineering step must be done for _every_ line of code. By _every_ programmer who touches it. _Every_ time it is touched. For the _entire lifetime_ of that code - which might be 25 years, plus.

That is waste. Waste costs money - as we are paid time and materials. It slows down delivery. It also has a very demotivating and wearying effect on you as a programmer.

> Clean code is **key** to being agile - it enables responding quickly to change

# Next steps

Applying these two naming rules consistently will hugely improve your code.

Let's move on to some ideas that will help us tame complexity of bigger functions.

## [Thinking Small >>](/02thinkingsmall.md)
