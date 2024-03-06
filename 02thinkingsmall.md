# Thinking Small

Big Ideas are best solved in small chunks.

![Great things small things van gogh](images/great-things-van-gogh.jpeg)

Typical BJSS clients are big organisations - banks, health care, national agencies. They have many users, high availability, and need oodles of data to be shunted about. They tend to use large applications - usually fleets of microservices. These can run into millions of lines of code.

We mortals simply cannot fit that that into our head.

## Don't eat the whole elephant

As the saying goes, "How do you eat an elephant? One bite at a time".

We cannot avoid complexity entirely. Some of what our clients need doing is inherently complex. But that does not mean we have to solve all of the problems in one single function.

We _manage_ complexity by using a strategy of divide and conquer. We eat that elephant one bite at a time.

To grasp a large problem, we break it down. Our codebase becomes a collection of largely separate modules, each one built from small, highly-focussed pieces of code.

At the code level, several simple techniques help us achieve this. The following sections describe those most commonly used.

## Use small code blocks

New requirements, new edge cases and new error conditions all tend to increase the size of our code blocks.

To implement these features, we add more logic. More variables. More conditionals. More loops. Small functions grow into monsters of complexity, difficult to understand.

We can easily fix this. Break large functions down into smaller ones. We do this by extracting code into more -smaller- functions. We trade concise, terse code for more lines of explanation.

Here are some ideas that can get you moving on this.

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

Back to our fizz buzz example, this code has a loop to run fizzbuzz over the numbers 1 to 100. The body of the loop is quite densely complex in this case. Not necessarily 'long', but it slows down the reading of that loop body. We find the same issue even with simpler code, once we hit around 30 lines or so inside the loop. Any code that goes off the edge of one screen gets hard to mentally follow.

It's all down to our arch enemy _cognitive overload_.

We can improve matters by extracting the loop body into its own function.
Our IDE might even help us with a _Refactor >> Extract function_ facility - do check if that is available on your system.

```javascript
function toFizzbuzzResult(n) {
  return (
    (!(n % 15) ? "FizzBu" : !(n % 3) ? "Fi" : !(n % 5) ? "Bu" : n) +
    (!!(n % 3) && !!(n % 5) ? "" : "zz")
  );
}

function showFizzbuzzResults() {
  for (let number = 1; number <= 100; number++) {
    const text = toFizzbuzzResult(number);
    console.log(text);
  }
}
```

We reduce cognitive load by splitting the problem into two smaller pieces: a loop, which calls a function to determine the fizzbuzz result. It is now easy to see that we have two big ideas:

- A loop, that does a thing multiple times
- A thing that gets done

As the loop body becomes more complex in whatever way, this simple refactoring pays well.

### Another chance to explain what is being solved

Now we have the loop body in its own function, that gives us a new opportunity to _name_ that function - according to what it does.

That thing we are doing multiple times - _why_ are we doing it? What will the outcome be?

This is how our code becomes self-documenting (or at least more so). By adding more names that _explain_ the steps and sub-processes that are happening, we communicate more of our intent to future readers.

### Extract conditional expression

In a similar vein, a lot of complexity can build up inside conditional statements. We can extract each condition into a named function. That name can explain what part of the problem that condition is checking for.

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
    const text = toFizzbuzzResult(number);
    console.log(text);
  }
}
```

> Note: that JS as written might not compile. That's due to the multi-line formatting, and JS' legendary insistence on adding semicolons when it feels like it

Already, that is a little easier to read. The complex nested ternary staements now attempt to explain themselves. There is much ore we can do here to simplify things, but extracting the conditional expression adds a lot of clarity, simply. Mostly because it _gives us a domain term_ in the code.

### Extract if / else block

Another simple idea is to extract the code into functions that lives inside the if and else blocks.

We can move from this:

```javascript
if (age < 18) {
  const message =
    "Unfortunately, you cannot buy Academy Glue if you are under 18";

  ui.showAlertBox(message);
}
```

To this:

```javascript
function showAgeRestrictionForGluePurchase(ui) {}
 const message =
    "Unfortunately, you cannot buy Academy Glue if you are under 18";

    ui.showAlertBox( message );
}

if ( age < 18 ) {
    showAgeRestrictionForGluePurchase(ui);
}
```

And if we extract the age check condition, we further explain like this:

```javascript
function isUnderAgeForGlue(age) {
   return age < 18;
}

function showAgeRestrictionForGluePurchase(ui) {}
 const message =
    "Unfortunately, you cannot buy Academy Glue if you are under 18";

    ui.showAlertBox( message );
}

if ( isUnderAgeForGlue(age) ) {
    showAgeRestrictionForGluePurchase(ui);
}
```

> Trade-offs: We must find the balance between under-explaining and over-explaining

The benefit - once again (...are we picking up on this, yet?) - is that we can explain what that code block does. The condtional block now becomes a generic

```
IF (this part of the problem happens)
THEN
    make this outcome happen
ELSE
    make the other outcome happen
```

Once again, it is creating small abstractions that bring us one step closer to explaining the problem we are solving by names in our code.

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
