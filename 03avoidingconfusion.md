# Avoiding Confusion

Sometimes, it is not the size, nor the complexity of the code that makes it hard to work with. It is just confusing.

This might be that the source code is confusing, making use of things that obviously meant something to the original programmer, but that are far from clear now. Or it may be that the program exhibits confusing, inconsistent behaviour (_shudders_).

Let's look at some easy tips to banish our bewilderment.

## Replace magic numbers

Magic numbers are constants in the code that have special meaning. They are usually limits or trigger points. They are often numbers, but could be any type - magic strings, magic booleans or whatever.

Here is a magic number 9 as part of a conditional:

```javascript
if (numberOfShips > 9) {
  // Do something
}
```

What does it mean?

Let's eliminate the guesswork for our future readers:

```javascript
const maximumShipsInGame = 9;

if (numberOfShips > maximumShipsInGame) {
  // Do something
}
```

## Say what you mean, mean what you say

What do you expect that this function call would do?

```javascript
const result = calculate("2 + 2");
```

Wrong! Of course you were, or this example wouldn't work.

Here's the function:

```javascript
const fs = require("fs");

function calculate(expression) {
  fs.unlink(expression, (err) => {
    if (err) {
      console.error(err);
    } else {
      console.log("File is deleted.");
    }
  });
}
```

Oooh, unlucky! The function named `calculate()` that takes a string `expression` happens to asynchronously delete a file named by that string. Oh dear.

We can probably all agree that we have never misnamed something this badly.

But what about those tiny transgressions?

Make sure that you can understand what to expect by the names and signatures of your functions and variables.

> **Names are so important. All we have are names**

## Avoid global anything

Really.

Avoid global variables in much the same way you avoid eating dog excrement, sat on a chair made of knives, at the side of a table on top of a land mine.

![Global variables are terrible](/images/global-variable.jpg)

(Maybe be even more vigilant, I don't know)

Global variables have the advantage that any piece of code, anywhere in the codebase can read and modify those variables. That means you don't have to think about which pieces of code can access them. They all can.

Global variables have the _significant disadvantage_ that any piece of code, anywhere in the codebase can read and modify those variables. That means you _cannot reason_ about which pieces of code can access them. They all can.

Global anything will connect multiple pieces of code together with an implicit connection. It is not generally possible to skim read the code and understand which pieces are connected.

Globals obscure the fact that an action in code block A will affect code block B, transmitted via the hidden global variable. They introduce hideous synchronisation problems in concurrent systems.

This is a recipe for disaster.

> **Uncle Al's War Story**
> My worst global variable tale happened in embedded code for an industrial machine. This variable controlled how quickly a status LED flashed on and off. Somebody later re-used that global variable to control how quickly data was written to non-volatile memory. At the time, it had 'just the right speed' to control the write timings.
>
> It was not obvious from the source code that this had been done. There was nothing explicit to indicate the shared access to the global. (There never is).
>
> The day came when we needed a faster status LED flash.
>
> The result was (a) a faster LED flash, combined with (b) losing all the stored data on the next update, as the write timings were now wrong.
>
> _Oops_

Globals. Just say no.

> In JavaScript, keyword **var** creates a global variable
>
> Avoid **var**

## Use the smallest scope you can get away with

We can take avoiding globals one step further. Make everything work in as small a scope as possible. Make things relevant for just one module, file, function or even line.

Here's some code that doesn't do this:

```javascript
var total = 0;
var prices = [50, 250, 395, 9995, 10];
var itemsCounted = 0;
var i;

function calculateTotal() {
  for (i = 0; i < prices.length; i++) {
    total += prices[i];
  }

  itemsCounted = i;
}
```

It's a short piece of code, but it's tricky to analyse:

- the function is locked-in to reading from the global `prices` array. That's a shame in itself. The algorithm should be reusable.
- global variable `total` will magically change once we call `calculateTotal()`
- the loop index `i` used to pull prices out of the array is defined quite far away from where it is used. In larger code, we would have to mentally track what state `i` was in.
- We assign the value of `i` after the loop ends to a global `itemsCounted`. That will work (sort of) as a way of counting how many items we included, but is fragile. If we added another loop in the future, and copy-pasted the loop in this code block, `i` would mean something else. Would we spot that?

Imagine that, once you hit even a couple of hundred lines. This stuff gets exponentially harder to juggle in your head, as a reader of unfamiliar code.

The better approach is to reduce scope to the shortest possible for all the things:

```javascript
function calculateTotal(prices) {
  let total = 0;

  for (let i = 0; i < prices.length; i++) {
    total += prices[i];
  }

  return total;
}
```

This is much safer to change in future and is much more direct to understand.

A further improvement here would be to `use prices.forEach()`. That completely eliminates the variable `i`, which is by far the best way of reducing scope.

## Don't rely on side effects

Side effects: things which happen as an indirect result of something else.

This time, I really am looking at you, JavaScript:

```javascript
let a = [];
let b = {};

if (a == b) {
  // NO LET ME STOP YOU RIGHT THERE!!!!
}
```

Shoot me now.

More generally, code is easiest to read when it is direct. The comparison above relies on weird side-effect rules of the language type coercion system. Avoid that.

Anything where you are relying on something that coincidentally happens to be useful to you, that's fragile code right there.

The coinncidence might be stopped in future, somewhere else. Even if it remains, it's very hard to reason about because it is _hidden_.

## Avoid hybrid coupling

Hybrid coupling applies to data, and it is where one value - or a range of values - has a special meaning, unrelated to the data itself.

An example might be:

```javascript
function calculateDiscount(customerId) {
  if (customerId <= 9999) {
    return standardDiscountRate(customerId);
  }

  // VIP customers only - they have a customerID of 10,000 or more
  return vipDiscountRate(customerId);
}
```

The code above treats all customers with a `customerId` of 10000 or more as VIP customers, and gives them a VIP discount. This concept should not be tied in anyway to customerId, which should serve only to uniquely identify the customer - _not encode information about them_. That should be done separately.

> One real system did this and grew past 10,000 cutomers. New customers were delighted to receive VIP discounts!
>
> The CFO of the company was rather less pleased by this.

It is better to pass in a stronger data structure, that explicitly mentions if the customer is a VIP or not:

```javascript
function calculateDiscount(customer) {
  if (customer.isVIP) {
    return vipDiscountRate(customer.id);
  }

  return standardDiscountRate(customer.id);
}
```

This code is more explicit about what it is doing, and is therefore easier to understand.

## Avoid null and undefined

`null` and `undefined` are a special case of hybrid coupling.

"Here be dragons" for you:

```javascript
function getUserProfileById( userId ) {
    sql = "SELECT profile FROM users WHERE id = ?";
    results = executePreparedSql(sql, userId);

    if results.length > 0 {
        return results;
    }

    return null ; // OUCH
}
```

The idea is we're going to pull some user profile blurb from a SQL database (very approximate coding here to illustrate). If we don't have anything for that `userId`, we'll get nothing back from the database. We will report to the caller of the function that nothing is available by returning `null`.

That's hybrid coupling, with a false beard. One piece of information can mean two things - either 'here are your results' or 'there were no results'.

It's always better to avoid this.

There are many available techniques, but at a minimum, return a simple results object:

```javascript
function getUserProfileById(userId) {
  sql = "SELECT profile FROM users WHERE id = ?";
  results = executePreparedSql(sql, userId);

  const isValid = results.length > 0;

  return { isValid, results };
}
```

The calling code is then told explicitly that there were - or were not - results available.

> Prefer explicit code that clearly describes what it does

## Avoid reusing variables

Many languages make it easy to re-use a variable for different purposes.

> Just because we _can_ does not mean we _should_

This adds to our cognitive load, as we must mentally track what the variable represents _at every line it occurs_.

```javascript
function getUserDescriptionById(i) {
  const i = fetchUserProfileById(i);

  i = i.description.json;

  i = "Describes themselves as " + JSON.parse(i).short;

  return i;
}
```

Good luck working with such code.

It would be just as easy to write this:

```javascript
function getUserDescriptionById(userId) {
  const profile = fetchUserProfileById(userId);

  const descriptionJson = profile.description.json;

  const shortDescriptionText = JSON.parse(descriptionJson).short;

  return "Describes themselves as " + shortDescriptionText;
}
```

Which avoids that mental juggling, making the code simpler and safer to work with. The translation steps are now clear.

This problem gets worse as:

- The scope of that reused variable gets larger: global being the worst
- Different data types get mixed in
- Subtle type conversion side-effects are relied on
- The variable name becomes more disconnected from what it is representing at that time

## Prefer read-only

What's the difference between these two lines as a reader of code:

```
let maximumSize = 500;
```

and

```
const maximumSize = 500;
```

The first definition might be changed elsewhere. Maybe the programmer intended it to change; maybe not. We cannot tell from this line alone what they were thinking.

The second definition explicitly is a read-only variable. The `const` keyword means that the value cannot change. We can tell that from only this line of code.

Explicit constraints like this greatly simplifies reading code. Whenever we see that variable name, we know what its value will be.

> Use read-only wherever possible.

This has an even bigger effect in concurrent programs. They are outside the scope of this guide.

## Prefer switch to if-else-if chains

```javascript
if (operation == "add") {
  add();
} else if (operation == "subtract") {
  subtract();
} else if (operation == "multiply") {
  multiply();
}
```

Can be simplified to:

```javascript
switch (operation) {
  case "add":
    add();
    break;

  case "subtract":
    subtract();
    break;

  case "multiply":
    multiply();
    break;
}
```

This has a more straightforward flow.

# Moving on

For the final part of this guide, let's look at the bigger picture. How do we grow a larger application out of small pieces?

## [Thinking Big >>](/04thinkingbig.md)
