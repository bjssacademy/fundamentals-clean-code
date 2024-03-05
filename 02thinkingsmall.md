# Thinking Small

Big Ideas are best solved in small chunks.

![Great things small things van gogh](/images/great-things-van-gogh.jpeg)

Typical BJSS clients are big organisations - banks, health care, national agencies. They have many users, high availability, and need oodles of data to be shunted about. THey tend to be large applications - usually fleets of microservices - that can run into ,illions of lines of code.

And we mortals simply cannot fit that all into our head.

## Don't eat the whole elephant

As the saying goes, "How do you eat an elephant? One bite at a time".

We cannot avoid complexity entirely. Some of what we our clients need doing is inherently complex. But that does not mean we have to solve all of the problems in one single function.

We _manage_ complexity by using a strategy of divide and conquer. We eat that elephant one bite at a time.

To grasp a large problem, we must break it down. Our codebase becomes a collection of largely separate modules, each one built from small, highly-focussed pieces of code.

At the code level, several simple techniques help us achieve this.

## Use small code blocks

Over time, our functions and methods grow larger. New requirements, new edge cases and new error conditions to handle all increase complexity.

To implement these features, we add more code. More variables. More conditionals. More loops. The function becomes a monster, difficult to understand.

The solution is to break large functions down into smaller ones. We do this by extracting code into more functions.

Here are some ideas that can get you moving on this

### Extract loop body

```javascript
function showFizzbuzzResults() {
  for (let n = 1; n <= 100; n++) {
    const fizzbuzzResult =
      (!(n % 15) ? "FizzBu" : !(n % 3) ? "Fi" : !(n % 5) ? "Bu" : n) +
      (!!(n % 3) && !!(n % 5) ? "" : "zz");

    console.log(fizzbuzzResult);
  }
}
```

Back to our fizz buzz example, this has a loop to run fizzbuzz over the numbers 1 to 100. The body of the loop is quite densely complex in this case. Not necessarily 'long', but slows down the reading of that loop body. We find the same issue even with simpler code, once we hit around 30 lines or so inside the loop.

We can improve matters by extracting the loop body into its own function.
Our IDE might help us with a _Refactor >> Extract function_ facility.

```javascript
function toFizzbuzzResult(n) {
  return (
    (!(n % 15) ? "FizzBu" : !(n % 3) ? "Fi" : !(n % 5) ? "Bu" : n) +
    (!!(n % 3) && !!(n % 5) ? "" : "zz")
  );
}

function showFizzbuzzResults() {
  for (let number = 1; number <= 100; number++) {
    console.log(toFizzbuzzResult(number));
  }
}
```

We reduce cognitive load by splitting the problem into two smaller pieces: a loop, which calls a function to determine the fizzbuzz result.

As loop body become more complex in whatever way, this simple refactoring pays well.

Note that now the loop body is in a function, that gives us a new opportunity to _name_ that function - according to what it does.

This is how our code becomes self-documenting (or at least more so). By adding more names that _explain_ the steps and sub-processes that are happening, we communicate more of our intent to future readers.

### Extract if / else block

In a similar vein, a lot of complexity build up inside conditional statements. We can extract each ocndition into a named function.

Applying that to the fizzbuzz code above, we might get:

```javascript
function dividesBy3(n) {
  return !(n % 3);
}

function dividesBy5(n) {
  return !(n % 5);
}

function dividesBy15(n) {
  return dividesBy3(n) && dividesBy5(n);
}

function toFizzbuzzResult(n) {
  return (
    (dividesBy15(n)
      ? "FizzBu"
      : dividesBy3(n)
      ? "Fi"
      : dividesBy5(n)
      ? "Bu"
      : n) + (!dividesBy3(n) && !dividesBy5(n) ? "" : "zz")
  );
}

function showFizzbuzzResults() {
  for (let number = 1; number <= 100; number++) {
    console.log(toFizzbuzzResult(number));
  }
}
```

Already, that is a little easier to read. The complex nested ternary staements now attempts to explain itself. There is much ore we can do here to simplify things, but extracting the conditional expression adds a lot of clarity, simply. Mostly because it _gives us a domain term_ n the code.

## Avoid deeply nested code

- cyclomatic complexity
- if return
- switch
- un-nest loops at source level

basics
Comment -> name

- Why?
- Example

Named constants

- MAX_DEPOSIT

Reduce Scope

- Why
- Example

Avoid continue

Eliminate globals

- why

Use vertical space

- Mental chunking

Don’t eat the whole elephant

- Decomposition
- Easier to grasp
- Less to go wrong
