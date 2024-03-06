# Avoiding Confusion

Sometimes, it is not the size, nor the complexity of the code that makes it hard to work with. It is just confusing.

This might be that the source code is confusing, making use of things that obviously meant something to the original programmer, but that are far from clear now. Or it may be that the program exhibits confusing, inconsistent behaviour (_shudders_).

Let's look at some easy tips to banish our bewilderment.

## Replace magic numbers

Magic numbers are constants in the code that have special meaning. They are generalusually limits or trigger points. They are often numbers, but could be any type - magic strings, magic booleans or whatever.

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

Oooh, unlucky! The function named calculate() that takes a string happens to asynchronously delete a file named by that string. Oh dear.

We can probably all agree that we have never misnamed something this badly.

But what about those tiny transgressions?

Make sure that you can understand what to expect by the names and signatures of your functions and variables.

> **Names are so important. All we have are names**
