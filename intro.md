# Introduction to Clean Code

You've doubtless heard about Clean Code, if only because it has become an industry buzzword.

But what is it? Do we need it? How do we write it?

## What is clean code?

With code, there are many ways of implementing a feature. You've probably done this yourself: taken a feature, and written some code that works. Perhaps like this function:

```javascript
function fb(n) {
  return !(n % 15) ? "FizzBuzz" : !(n % 3) ? "Fizz" : !(n % 5) ? "Buzz" : n;
}
```

> Can you guess what it is yet?

But you come back to the code later, and have no clue what that code is doing. You rip it all out and replace with some new code. This code does the same thing, but in a way you and your team can understand. Maybe like this:

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

The second code block is longer. It has more words. Specifically, it has more _domain terms_ - words that are part of the problem domain. This code uses the function name `toFizzBuzzText` as that describes its _purpose_ for our problem. Helper functions such as `dividesBy15` are there to _explain_ steps of the solution.

This second block is simpler in nature. There are no nested ternary statements to make your brain melt.

That is pretty much how we change _dirty code_ into _clean code_.

### Defining clean code

Clean Code is shorthand for code with these properties:

- Easy to read
- Uses the simplest constructs possible
- Safe to make changes to
- No hidden surprises
- Is made of small, independent components
- Idiomatic for the language used

It is easier to write 'dirty' code that violates these goals than it is to write clean code. Clean code never writes itself. It is intentional.

> Clean code is story telling. It explains to other humans what the computer is doing

## Why do we need clean code?

Writing code that meets the above goals has important benefits.

Beyond trivial projects, programming is a team sport. We work in teams, growing a program piece by piece.

_Most of the code we work with will be written by others._

### Code the team can understand

As novices, we tend to work alone. We are - justifiably - proud when some code simply works. Professionally, that is table stakes. We need code that the whole team can work with.

The first task in writing new code is to understand the code that is already there. Then we can decide how our new code fits together with it. We never write code in isolation; it will always have to integrate with what's there.

Being able to understand what's there is critical.

That means that our code must be understood by others and their code must be understandable to us.

### Defect prevention

Defects - bugs - cost us. They slow down delivery. They can cause financial and reputational loss. SOftware defects have been known to kill people and result in their false imprisonment.

We don't like defects.

Clean code aims to use syntax and logic that is straightforward and clear. It does what it says. You don;t need to factor in weird edge cases of the language to understand the code.

Doing this reduces defects. When defects happen, they are easier to spot, as we find it easier to understand what the code is - and is not - doing.

### Rapid delivery

With fewer defects and shorter times to understand the code, Clean code gives us faster delivery times. It gives us _smoother_ delivery times as well, with added confidence that our code is likely to be correct.

## Benefits of clean code

- Code explains the problem being solved in human language
- Quicker to understand
- Cheaper to work with
- Defects are more obvious to spot
- Changes are less likely to break unrelated areas

# Get Started

Let's look at the low-hanging fruit of how to write clean code
[Basics >>](/01basics.md)
