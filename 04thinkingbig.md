# Thinking Big

We have some ideas about building simplicity in the small.

But how can we tame the bigger pieces?

![thinking big](/images/thinking-big.png)

The first tactic is to _stop repeating information_.

## Don't Repeat Yourself (DRY) Principle

Have you ever encountered code like this:

```javascript
let total = 0;

for ( let i = 0; i < 10; i ++) (
    total += getPrice(i);
)

for ( let i = 0; i < 10; i ++) (
    sendEmail( updateOrderAccepted(i) );
)
```

(_Note:_ artificial example)

**Did you notice the _magic number_ 10 occur twice?**

That `10` represents a fixed number of things that we want add to a total and send an email about.

It is the _same concept_ repeated twice: A quantity of things. We want to do something with ten things.

![Duplication is bad](/images/duplication.png)

Having this number appear more than once causes a few problems.

### Not changing enough

We want to do something to _20_ things now.

We need to change every `10` in the code to `20`.

Sometimes, we miss a few. This breaks the code in confusing ways.

> Tests detect errors like this as soon as possible

### Changing too much

We change _every_ `10` to a `20`. Now the code doesn't work properly anymore.

Some occurrences of the number `10` represent a _different_ concept. Not 10 things, but a price of 10 pounds, a Customer Number of 10 and so on.

When we swap those unrelated values to 20, we have seriously broken the code.

> Tests detect errors like this as soon as possible

### Confusion

Each time we read the number `10`, we must reverse-engineer the code. Was it the number of things? Or the price? Or the Customer Number?

Nobody knows, _because you didn't tell us in the code_.

This is a waste of everybody's time.

### Copy + Paste => Fail

The bigger failure is when we copy-paste code around.

Suppose we copy-paste a code block three times. Future changes will now need to be made in those three blocks.

However, this won;t happen. Somebody will fail to update one of the blocks.

The code has now diverged and is now broken in very difficult to spot ways.

> Tests detect errors like this as soon as possible

Future readers will spend _a very long time_ trying to figure out if you intended the code to diverge or not.

Nobody knows, _because you didn't tell us in the code_.

### Apply DRY to solve this

DRY solves this by providing a single source of truth for each idea.

![DRY is good](/images/single-source.png)

> Single source of truth **solves the root cause**

The DRY principle solves problems caused by repeating information. We need a _single source of truth_ for all information. That aids understanding. It enables future changes to be made safely.

We can see DRY as an extension to SRP:

- Things that change together should be near to each other
- Things that _are actually the same idea_ should be the represented by the same code

## Separation of concerns

The trouble with software is that its complexity grows exponentially with size, unless we limit this.

- Each line represents at least one decision
- Each line builds on the ones before it
- Every conditional adds a new set of states
- One line of code can affect another line of code that cannot be seen on the screen

The big idea is called _separation of concerns_. We divide the big problem up into smaller pieces, each one being concerned with one aspect of the problem. We keep those pieces as separated as possible.

One guideline to help us is the _Single Responsibility Principle_.

## Single Responsibility Principle (SRP)

This principle is part of five design principles known as the SOLID Principles.

> Guide to the [SOLID principles](/resources/solid-principles-javaoopdoneright.pdf) extracted from [Java OOP Done Right](https://leanpub.com/javaoopdoneright)

The **Single Responsibility Principle** says that a software component should contain things that need to change together.

All the pieces needed for a _single repsonibility_ of the code should be in a single component. When we need to change anything, we make changes in one place. We also protect the rest of the codebase from having changes scattered about.

### Too many responsibilities

A counter-example would be a function that does three things:

- Opens a database connection
- Reads some database data and applies some logic
- Generates a web page using API calls

If we wrote all of those responsibilities into the same function, it would look like this at the design level:

![Too many repsonsibilities](/images/too-many-responsibilities.png)

**Don't do this**.

Any change to any of those things would force a lot of rework and retest.

### Separation of responsibilities

The fix is to split out the three responsibilities into three separate components:

- Database
- Logic
- HTML

![Separation of responsibilities](/images/separated-responsibilities.png)

**Do this**.

A change to an SQL query will have _no effect whatsoever_ on the business rules, REST API or user interface. Those things might need to include new features, but the separation of concerns means we can make changes at some other time.

This approach is very common, and a driver towards _three tier architecture_ and _hexagonal architecture_.

### Coupling and Cohesion

The tools we use to manage that all relate to one simple idea:

> Things that belong together are grouped together
>
> Things that don't belong together are split apart

SRP is a restatement of "maximise cohesion".

There are two technical terms to know: _coupling_ and _cohesion_.

- **Coupling** Components are _coupled_ if a change in one _forces_ a change in the other
- **Cohesion** Components are _cohesive_ if they are parts of solving the same problem

![Low coupling,  high cohesion](/images/coupling-cohesion.png)

> GOAL: **LOW** coupling, **HIGH** Cohesion

That simple rule covers most of software design, object orientation, microservices and TDD.

## Modularity

> Hide as much detail as possible
>
> Expose as little interface as possible

The above splits are examples of modularity, also known as _Information Hiding_.

Information Hiding was first described by David L. Parnas in 1968, and was a leap forward for managing complexity.

A module should:

- Do a job
- Expose an interface so we can ask for that job to be done
- Hide the algorithms needed to do that job
- Hide the data structures meeded to do that job

This was a foundation of Object-Oriented Programming, and also of every module or package system that we see today.

It is "Don't eat the whole elephant" in code structure form.

> What’s the _least_ that a caller of a module needs to know about it in order to use it?

### Example: Stack module

A stack is a Last-In, First-Out (LIFO) data store.

![Stack module](/images/stack-module.png)

A stack can do two things:

- **Push** something onto the stack
- **Pop** the last thing that was pushed off the stack

A stack module exposes only the `push` and `pop` operations. Client code can use these operations.

The module _hides_ how that stack is implemented. It may be an array, a linked list or any other means of supporting last-in, first-out.

The client code cannot, does not need to and _must not_ know how the module works inside.

This is key to minimising disruption caused by changes. This enables agility.

> **Module:** Hides detail. Exposes a minimal, complete abstraction

## Module Dependencies

Applications are built from many modules. Some of them inetract with each other.

When module A uses module B, then we say _A has a dependency on B_.

Modules exist to separate different areas of code so that the code can change at different rates.

But that doesn't always happen.

### Leaky Abstractions

Sometimes, one module depends on implementation details of a dependency. It shouldn't, but that's life.

This is known as a _leaky abstraction_, and it's a mess, because:

- Changes in one module require changes in the other, even though they should be separate
- We cannot test or otherwise use the modules apart from each other

We've made modular spaghetti.

### Example: Fetching a user profile

Our app wants to fetch user profile data from a SQL database.
We attempt to separate the concerns of application logic and data storage, so we come up with two modules:

module `userdata` provides this function:

```javascript
function loadUserProfile(sqlQuery) {
  // passes the SQL query to the database
  // returns a JavaScript object of profile data
}
```

and module `application` uses that function:

```javascript
// module: application
function displayUserDescription(username) {
  const query = "SELECT * FROM USERS WHERE name='" + username + "'";
  const profile = loadUserProfile(query);

  console.log(profile.description);
}
```

Module `application` depends on `userdata`. Worse, it depends on an _implementation detail_'

Look at the function signature for `loadUserProfile`: it accepts a SQL query string. The function has accidentally exposed an implementation detail - SQL.

Now that code will compile. You could even get it to pass tests.

But ...

_Every change that affects the SQL query will cause changes to the application_. And it really should not.

> This _change preventer_ is a code smell called [Shotgun Surgery](https://refactoring.guru/smells/shotgun-surgery)

#### Developer Symptoms

- "It's broken all my tests!"
- "I hate tests. You have to change them all the time, there's more tests than code!"
- "We can't change the name of that column, sorry"
- "It would be better to change the database schema, but it would take too long"

Sound familiar?

This is the consequence of high coupling between modules caused by a leaky abstraction
![High coupling on dependency](/images/high-coupling.png)

#### Usual culprits

The usual culprits causing leaky abstractions are:

- global variables
- anything specific to one implementation instead of all
- public helper functions
- public variables - include properties and getter/setter methods

Avoiding these in our designs makes our code simpler to reason about.

### Fixing the leak

Anyway. Can we do better?

Going back to design thinking, module `application` only needs to display profile data. It does not need to know where that data came from. It certainly does not _require_ the data to come from an SQL store.

So how about we fix the code, to make that clear:

```javascript
// module: userdata
function loadUserProfile(username) {
  const query = "SELECT * FROM USERS WHERE name='" + username + "'";
  // passes the SQL query to the database
  // returns a JavaScript object of profile data
}

// module: application
function displayUserDescription(username) {
  const profile = loadUserProfile(username);

  console.log(profile.description);
}
```

It's a subtle change at the code level. But profound at the design level.

Module `application` now only depends on a _stable, true abstraction_: that calling `loadUserProfile()` with a `username` will return the profile data. From anywhere. No matter how it is implemented.

It could come from a SQL store as before, but we have more options. It could also come from a file, a web service, a local cache. A test double.

The function `loadUserProfile()` now represents a true abstraction of the data storage. Outside module `userdata`, nothing has any idea at all of how `userdata` is implemented. Just as things should be.

## Breaking the dependency

Well, almost as things should be.

We have separated concerns, but module `application` still has a hard-coded dependency on module `userdata`. It is the call to function `loadUserProfile`, which is directly wired to that one, specific function.

That means we can't easily pull those two modules apart.

That's a shame if we want to test them or reuse them. But surely, this is the best we can do? I mean we _have_ to call a function to load that data, don't we?

Well.

Turns out we don't.

### Dependency Inversion Principle

The current position is that `application` calls a hard-coded function in `userdata`, which prevents us isolating the two modules.

What if it didn't?

What if we agreed on a more flexible connection between the two modules?

Instead of `application` depending on `userdata`, how about we make neither of them know about each other. Instead, we make them both depend on a new abstraction?

![Dependency Inversion](/images/dependency-inversion.png)

To do this, we introduce the idea of an `interface` - an agreement between two modules that lives outside any implementation.

We can make two agreements:

- `application` will call a function _it is given_ with the username, and receive the data
- `userdata` will provide a suitable function

We have _inverted the dependency_. Instead of `application` depending on `userdata`, now both `application` and `userdata` depend on this new agreement.

### Dependency Injection

To complete our trick, we need two techniques:

- we must _inject_ the function we want to use to fetch data into `application`. This is \_Dependency Injection'.
- we need a new piece of separate code to _wire up_ `application` to whatever function we want to use to fetch data.

The code looks like this:

```javascript
// module: userdata
function loadUserProfile(username) {
  const query = "SELECT * FROM USERS WHERE name='" + username + "'";
  // passes the SQL query to the database
  // returns a JavaScript object of profile data
}

// module: application
function displayUserDescription(profileSource, username) {
  const profile = profileSource(username);

  console.log(profile.description);
}

// Module: main
displayUserDescription(loadUserProfile, "alan");
```

This is a Functional Programming approach to Dependency Inversion.

An equivalent Object Oriented DI technique exists that uses the keyword `interface` to specify the contract precisely. We need Typescript (or another language) to do that.

#### How does it work?

The new module `main` passes the dependency in as the first parameter. It chooses the `loadUserProfile` function from `userdata` _ in this instance_. But it could be any function that takes a username and returns user data.

This is a very powerful idea. We cover it in more depth in "DIP, DI and IoC" in the Engineering Academy. But this is enough to get the idea across.

## [Next >>](summary.md)

That completes our BJSS Academy Guide to Clean Code. In the final chapter, we'll summarise and link to Further Resources to help you with this critical topic.
