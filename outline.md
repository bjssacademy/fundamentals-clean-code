# Outline

Clean Code outline

What is it?

- Skim Readable
- Safe
- Unsurprising
- Modular
- Defined connections

Why is it important?
(Win lose as team)
Agility - slow response to change if we spend time understanding code, or being tricked by it

Defects are costly - some designs are error prone, avoid them

A Puzzle
(Code with bad names, same code with good names)

Keywords are not important, our names are

Goal of pro dev
We turn the language into a DSL

Naming data - variables

- Explain
- examples

Naming code - functions, methods, classes

- Explain
- Examples

Comment -> name

- Why?
- Example

Named constants

- MAX_DEPOSIT

Reduce Scope

- Why
- Example

Eliminate globals

- why

Use vertical space

- Mental chunking

Don’t eat the whole elephant

- Decomposition
- Easier to grasp
- Less to go wrong

Small code blocks

- Easier to skim read
- Extract code block

Design-out errors

- Prefer code where the wrong thing cannot be done
- example:
- int add ( String first, String second )
- Could pass in not-numbers as strings which cannot work. So change parameter types to design-out this error

Avoid reusing variables for different purposes

- Eg n: a surname, then a boolean flag, then a number
- Especially in dynamic languages. It is very hard to understand!
- Double especially in long ( > 1 screen) methods. See also small code blocks
- Avoid null

Avoid magic values (hybrid coupling)

- Eg customerID > 9999 gets VIP discount
- Replace with explicit system

Prefer read-only

- Immutable
- Concurrent safe
- Simpler to reason about

Create relevant Types

- What are we representing?

DRY

- Change in one place
- Same abstraction only

SRP - One reason to change

- Example

Avoid accidental complexity

- Accidental vs Essential
- Use the simplest syntax and algorithm that will work
- Show off by showing how simple and clear you can make the code

Don’t make me guess!

- Defaults, types, magic numbers

Early return

- Example each way

Prefer switch over if/else/if/else

- Why (hidden danger)
- Example

Encapsulate Conditional expression

- Pull out with name

DeMorgan - simplify (!x && !y)

- Example

Prefer foreach

- Why (less to go wrong, more direct)
- example

Information Hiding
-Parnas
-Opaque Box
-Tell Don’t Ask
-Hide Data
-Hide Algorithms
-Programming Interface
-Input, Output, Behaviour

Errors - Result object

- Example

Errors - callbacks

- Example

Errors - exceptions “cannot fulfil contract”

- Meyer, Eiffel OOSC
- Some people just won’t listen to this, so you’ve already lost the battle.

—
FURTHER reading:

Ousterhout
Clean Code, Martin
Refactoring, Fowler
