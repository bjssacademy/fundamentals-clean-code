# Avoiding Confusion

Sometimes, it is not the size, nor the complexity of the code that makes it hard to work with. It is just confusing.

This might be that the source code is confusing, making use of things that obviously meant something to the original programmer, but that are far from clear now. Or it may be that the program exhibits confusing, inconsistent behaviour (_shudders_).

Let's look at some easy tips to best our bewilderment.

## Replace magic numbers

Magic numbers are constants in the code that have special meaning. They are general limits or trigger points. They are often numbers, but could be any type - magic strings, magic booleans or whatever.

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
