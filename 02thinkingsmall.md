# Thinking Small

Big Ideas are best solved in small chunks.

![Great things small things van gogh](images/great-things-van-gogh.jpeg)

Typical BJSS clients are big organisations - banks, health care, national agencies. They have many users, high availability, and need oodles of data to be shunted about. They tend to use large applications - usually fleets of microservices. These can run into millions of lines of code.

We mortals simply cannot fit all that into our head.

## Don't eat the whole elephant

As the saying goes, "How do you eat an elephant? One bite at a time".

We cannot avoid complexity entirely. Some of what our clients need doing is inherently complex. But that does not mean we have to solve all of the problems in one single function.

Instead, we _manage complexity_ by using a strategy of divide and conquer. We eat that elephant one bite at a time.

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

Going back to our fizzbuzz example, this code has a loop to run fizzbuzz over the numbers 1 to 100. The body of the loop is quite densely complex in this case. Not necessarily 'long' in length, but it packs a lot in. It's hard to skim read, certainly.

We find the same issue even with simpler code, once we hit around 30 lines or so inside the loop. Any code that does not fit on one screen gets hard to mentally follow.

The difficulty is human in nature. It's all down to our arch enemy _cognitive overload_.

We can improve matters by extracting the loop body into its own function.

> Your IDE might be able to help. Check for a _Refactor >> Extract Function_ facility

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

#### Another chance to explain what is being solved

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

Already, that is a little easier to read. The complex nested ternary staements now attempt to explain themselves. There is much more we can do here to simplify things, but extracting the conditional expression adds a lot of clarity, simply. Mostly because it _gives us words from the problem domain_ in the code.

### Extract if / else block

Another simple idea is to extract the code that lives inside the if and else blocks into their own functions.

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

The benefit - once again (...are we picking up on this, yet?) - is that we can explain what that code block does.

If we consistently do this, we create a generic building block, that looks like this (in pseudocode):

```
IF (this part of the problem happens)
THEN
    make this outcome happen
ELSE
    make the other outcome happen
```

Seeing consistent patterns in our code is another technique that aids our mental chunking.

Clean code is all about creating small abstractions that bring us closer to explaining the problem we are solving.

## Avoid deeply nested conditionals

**Public Service Announcement: Deeply nested code follows. Reader discretion is advised**

```javascript
function canBuyGlue(user) {
  if (isOver18(user)) {
    if (isAcademyStudent(user)) {
      if (isStudyingConcurrency(user)) {
        return true;
      } else if (isBewilderedByDependencyInversion(user)) {
        return true;
      }
    }
  }

  return false;
}
```

Sorry you had to see that. It's something all rookies must go through to qualify. We actually see a lot of that style of coding from applicants to the BJSS Academy, during their Codility test.

To be fair, you will see a _whole lot worse_ over your career. This is just a textbook example. Wait until you get a few pages of nested nonsense that scrolls of the end of the page!

Let's assume the above code works.

Can we simplify it?

### Option 1: Use guard clauses

_Guard clauses_ are a useful pattern where we want to guard some behaviour from being used when it is not relevant.

Guards make use of a sequence of `if` staements that check for the _opposite_ condition of what is needed. The function will immediately `return` if that opposite condition occurs.

The above would become:

```javascript
function canBuyGlue(user) {
  if (!isOver18(user)) {
    return false;
  }

  if (!isAcademyStudent(user)) {
    return false;
  }

  if (!isStudyingConcurrency(user)) {
    return isBewilderedByDependencyInversion(user);
  }

  return true;
}
```

You can see how the _if-return_ ladder 'guards' the code block at the end. You can also see in this example how the else-if has complicated the logic.

### Option 2: Combine logic into an explaining variable

Another translation is this:

```javascript
function canBuyGlue(user) {
  const isAllowed = isOver18(user) && isAcademyStudent(user);

  if (isAllowed && isStudyingConcurrency(user)) {
    return true;
  }

  if (isAllowed && isBewilderedByDependencyInversion(user)) {
    return true;
  }

  return false;
}
```

Which itself simplifies to:

```javascript
function canBuyGlue(user) {
  const isAllowed = isOver18(user) && isAcademyStudent(user);

  if (isAllowed && isStudyingConcurrency(user)) {
    return true;
  }

  return isAllowed && isBewilderedByDependencyInversion(user);
}
```

## Avoid nested loops

Here's some code to draw an ASCII-Art square:

```javascript
const length = 5;

for (let row = 0; row < length; row++) {
  for (let col = 0; col < lemgth; col++) {
    print("*");
  }

  printNewline();
}
```

We can reduce cognitive load by extracting the inner loop:

```javascript
function drawRow(length) {
  for (let col = 0; col < lemgth; col++) {
    print("*");
  }

  printNewline();
}

const length = 5;

for (let row = 0; row < length; row++) {
  drawRow(length);
}
```

Once again, we get to introduce another domain term to explain what that inner loop is doing. in this cases, it will draw a row.

As loops get more complex internally, this technique pays off well.

### Avoid continue and break

Loops generally have ways to circumvent part of their loop code:

```javascript
function sayHelloToEveryoneExceptDave(users) {
  for (i = 0; i < users.length; i++) {
    const user = users[i];

    if (user == "Dave") continue;

    console.log(greetUser(user));
  }
}
```

Here, it's pretty clear what's going on. The `continue` keyword skips over the part that would write the greeting,. But as that loop body gets a little longer, it gets harder to track which lines execute when and what's in the variables. The fact that we are skipping some of the code execution makes refactoring that loop harder sometimes.

Let's combine _extracting the loop body_ with _using a guard clause_:

```javascript
function showGreetingExceptToDave(user) {
  if (user == "Dave") {
    return;
  }

  console.log(greetUser(user));
}

function sayHelloToEveryoneExceptDave(users) {
  for (i = 0; i < users.length; i++) {
    showGreetingExceptToDave(user);
  }
}
```

#### Consider a better algorithm

The code above treats the problem to solve as "greet everyone except Dave". It works by iterating over everyone, and skipping the greeting code for Dave.

But is there is a better way to think about this problem, one which will clean up the code nicely?

We want to divide our users into two groups:

- Those we want to greet
- Those we don't (the 'group' that is Dave)

We can do this using a filter operation:

```javascript
function showGreeting(user) {
  console.log(greetUser(user));
}

function sayHelloToEveryoneExceptDave(users) {
  const usersToGreet
    = users.filter(user => user != "Dave");

  usersToGreet.forEach(user => showGreeting(user););
}
```

That separates the concerns of showing a greeting from the concern of finding out who to greet. A cleaner _algorithm_ this time, not just cleaner syntax for the code.

> Prefer cleaner algorithms

Always prefer these cleaner approaches. It pays to think around the problem, rather than just throw keywords like `continue` at it.

By the way, sorry Dave. At least you got some clean code in the end.

### Prefer foreach

The most common case of using loops is to _iterate over elemnts_ in an array (or other collection).

The way we normally start out is to use a three-part counting loop, like so:

```javascript
function calculateTotal( prices ) {
  let total = 0;

  for (int i=0; i < prices.length; i++ ) {
    const price = prices[i];
    total += price;
  }

  return total;
}
```

That's pretty simple. But it does have _accidental complexity_:

- a _loop index_ variable `i`
- remember arrays start at index [0] (in this language, not all languages...)
- length check
- possible off-by-one error
- increment `i`
- Be vaguely aware of the maximum size of `i`
- If `i` was floating point, be aware of when incrementing i by one stops working due to 53 bit mantissa precision of [IEEE-754](https://en.wikipedia.org/wiki/IEEE_754) floating point numbers

Now, that sounds like a hassle to me. And we _don't even care_ about having that index variable, really. It only exists so we can iterate over array elements.

Can we be more direct about that intention?

```javascript
function calculateTotal(prices) {
  let total = 0;

  prices.forEach((price) => (total += price));

  return total;
}
```

Yes. We can. Prefer `forEach` over counting loops

#### Consider reduce()

In the spirit of getting rid of as much as we can, we could use the `reduce()` method on arrays:

```javascript
function calculateTotal(prices) {
  const addPrices = (total, price) => total + price;

  return prices.reduce(addPrices);
}
```

This gets rid of the loop entirely, by hiding it inside the `reduce()` method. There are fewer moving parts and less to go wrong.

It opens the interesting trade-off:

- less accidental complexity
- possibly unfamiliar syntax

In working groups, you'll need to agree on which way to jump.

> Me? I say always avoid accidental complexity wherever you can.

This leads - and very nicely, I thought - on to _what is accidental complexity?_

## KISS: Avoid accidental complexity

There are two kinds of complexity in any problem:

- _**Essential** complexity_. The problem itself is complex. Example: Radar signal processing
- _**Accidental** Complexity_. The problem is simple, yet our chosen implementation is hard to understand.

> **Accidental complexity is the enemy of clean code**.

This is the difference between:

```javascript
function trackAircraft(radarSamples, previousTrack) {
  // ... this is probably going to be hard
  //
  // so make sure to decompose into simpler sub-steps!
}
```

and:

```javascript
function calculateFinalPrice(shoppingBasket) {
  // This really should not be difficult!
}
```

To avoid accidental complexity:

- Use the simplest algorithm
- Be explicit about every step taken
- Use the fewest programmaing elements
- Prefer the most widely understood syntax

![Say no to accidental complexity](/images/complexity.jpg)

Note that problems with high essential complexity are just as prone to having extra _accidental_ complexity added. As we break down our complex problem into simpler sub-steps, work towards making each step as simple as possible.

This is traditionally known as the **KISS** principle - which always was _Keep It Simple, Stupid_.

We could equally say _Keep It Simple, Seriously_, should you prefer a modernised version of your final S.

> KISS: Simplify, simplify, simplify.

Always be on guard. Accidentally complex will torpedo agility like nothing else.

# Next

Now that we are busy building code blocks that are small, yet perfectly formed, let's look at another problem area: Confusing Code.

## [Avoiding Confusion >>](/03avoidingconfusion.md)
