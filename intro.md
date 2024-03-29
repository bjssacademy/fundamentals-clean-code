# What is Clean Code?

You've doubtless heard about Clean Code, if only because it has become an industry buzzword. We certainly talk about it _a lot_ (Ed: ad nauseam, actually) in Academy.

But what is it? Do we need it? How do we write it?

## Take some 'quick and dirty' code ...

With code, there are many ways of implementing a feature. You've probably done this yourself: taken a feature, and written some code that works. Perhaps like this function:

```javascript
function fb(n) {
  return (
    (!(n % 15) ? "FizzBu" : !(n % 3) ? "Fi" : !(n % 5) ? "Bu" : n) +
    (!!(n % 3) && !!(n % 5) ? "" : "zz")
  );
}
```

Hurray! We did a thing! It works! Look at its elegant optimisations! _Aren't we a clever cat_

Ah, yes. The unbridled optimism of that first cut of code.

### Some time later ...

> _Wavy lines ... time passes ..._

Only next week, we need to implement a new feature from Mx. Customer.

We need to make the function return the word "dog" for number values divisible by 10.

**And it is an utter, utter nightmare to add**

Try it ;) That code will fight back.

![we can fix the code later, right?](/images/fix-it-later.png)

And that's just one line of code. In one function. In a codebase on an unloved planet, in the unfashionable Western spiral arm of the galaxy.

Imagine doing hundreds of similar things each week - but with millions of lines of code you've never seen before.

## Tell us it doesn't have to be this way!

The fact is that **code that reads easy writes hard**. Most of the readability of code is set when we write it. If we needlessly complicate our code, we will bewilder future readers.

So we must _decide_ to be part of the solution.

![BJSS Needs clean code!](/images/needs-you-clean-coder.jpg)

Humans are not magic at reading code. It's not like we are a failure because we struggle to read hard-to-read code. Some code is easier to read than others.

The trouble is that readable code does not write itself. It needs care taken. By every one of us.

At each of the trade-off points where we decide to go with approach A or approach B, we must choose wisely.

We must first know that alternatives A and B are even possible.

That's what this BJSS Academy guide is all about.

### Cleaning our example code

Here's one possibility for our original function `fb()`:

```javascript
function dividesBy15(n) {
  return n % 15 == 0;
}

function dividesBy5(n) {
  return n % 5 == 0;
}

function dividesBy3(n) {
  return n % 3 == 0;
}

function toFizzBuzzText(n) {
  if (dividesBy15(n)) {
    return "FizzBuzz";
  }

  if (dividesBy3(n)) {
    return "Fizz";
  }

  if (dividesBy5(n)) {
    return "Buzz";
  }

  return n; // number as supplied
}
```

Now, this is longer. It's more wordy. It has more lines of code. It takes up more space in the source file. It is also recognisably [FizzBuzz](https://en.wikipedia.org/wiki/Fizz_buzz)

But it is simpler.

### The value of simple code

Simple code has great value. We can see some of the qualities of simple code in the previous example:

- It uses _domain terms_ - words that explain the problem being solved
- It uses simpler syntax. One example: replace nested ternary expressions with if/return
- It removes the unnecessary 'optimisation' (ahem) surrounding the "zz"
- Each line does less. This reduces _cognitive load_
- The flow of exeuction is straightforward
- Helper functions give names that _explain_ the steps of the solution

In many ways, this second code block is _better_. Yes, that is subjective. Yes, against some measures, the first block wins out.

But in terms of keeping code easy to work with across a whole team, the second block is a good choice.

This second block is simpler in nature. It makes your brain melt less.

This is what clean code aims to do - improve communication between programmers.

![Tony Hoare on clean code](/images/quote-tony-hoare.jpg)

### Defining clean code

Clean Code is shorthand for code with these properties:

- Easy to read
- Uses the simplest constructs possible
- Safe to make changes to
- No hidden surprises
- Is made of small, independent components
- Idiomatic for the language used

It is easier to write 'dirty' code that violates these goals than it is to write clean code. Clean code never writes itself. It is a result of intentional code writing.

> Clean code is story telling. It explains to other humans what the computer is doing

## Why do we need clean code?

Writing code that meets the above goals has important benefits.

Beyond trivial projects, programming is a team sport. We work in teams, growing a program piece by piece.

_Most of the code we work with will be written by others._

### Shared understanding

When working in a team, it is essential that every member can understand the codebase.

As novices, we tend to work alone. We are - justifiably - proud when some code simply works.

Professionally, working code is merely table stakes. Of course it must do _at least_ that. But we need more. We need code that the whole team can work with.

The first task in writing new code is to understand the code that is already there. Then we can decide how our new code fits together with it. We never write code in isolation; it will always have to integrate with what's there.

Being able to understand what's already there is critical.

> Our code must be understood by others
>
> Their code must be understood by us

### Fewer defects

Defects - or bugs, as we call them - cost us. They slow down delivery. They can cause financial and reputational loss.

Software defects have been known to [kill people](http://sunnyday.mit.edu/papers/therac.pdf) or result in their [false imprisonment](https://www.bbc.co.uk/news/business-56718036).

**We don't like defects**.

Clean code aims to use syntax and logic that is straightforward and clear. It does what it says. You don't need to factor in weird edge cases of the language to understand the code.

Doing this reduces defects. When defects happen, they are easier to spot. Clean code makes it easier to understand what the code is - and is not - doing.

### Faster delivery

With fewer defects and shorter times to understand the code, Clean code gives us faster delivery times. It gives us _smoother_ delivery times as well, with added confidence that our code is likely to be correct.

## Benefits of clean code

- Code explains the problem being solved in human language
- Enables agility - ability to change code safely and rapidly
- Shared understanding across team
- Quicker to understand
- Cheaper to work with
- Defects are more obvious to spot
- Changes are less likely to break unrelated areas

# Getting started

Let's look at the low-hanging fruit of how to write clean code

## [Clean Code Basics >>](/01basics.md)
