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

## Dependency Inversion Principle

What happens when you have one module use another module?

We have a _dependency_.

When module A uses operations provided by module B, we say module A _depends on_ module B.

This is all well and good until module B changes the operations it provides.

All changes to the programming interface of module B will require changes in every other module that uses them.

Oh dear:

- Rework slows delivery and costs money
- Retest of all affected modules is required

Let's walk through an example of this ckind of change and how we might avoid it.

### Example: Fetching a user profile

Our app wants to fetch some user profile data from a SQL database.

We follow the previous advice and create two modules: `application` and `userdata`.

The modules look like this:

```javascript
// module: userdata
function loadUserProfile(sqlQuery) {
  // passes the SQL query to the database
  // returns a JavaScript object of profile data
}

// module: application
function displayUserDescription(username) {
  const query = "SELECT * FROM USERS WHERE name='" + username + "'";
  const profile = loadUserProfile(query);

  console.log(profile.description);
}
```

Note:

- function loadUserProfile() accepts a raw SQL query string
- displayUserDescription() creates the SQL query string
- There is a SQL injection vulnerability, which we won't talk about

Problem: Changes in module userdata _ripple out_ to module application. Specifically, a change to the table structure of the database must be reflected in the displayUserDescription() function.

#### The problem of leaky abstractions

We said [reviously that a module should _hide_ implementation details and _expose_ an abstraction.

This abstraction is known as _leaky_. It is not really abstract. It directly exposes detailed knowledge about SQL and the database table structure.

Any changes in these details _leak out_ to every other module.

To avoid the required rework, it would be great if we could fix this.

Thankfully, we can. We need to fix that leak. And we do this using _Dependency Inversion_.

### Dependency Inversion

The problem above is that our `application` module depends on 'userdata', causing changes to ripple out.

Instead, we can make _both_ modules `application` and `userdata` depend not on each other, but on a third abstraction.

Revised code might be as simple as this:

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

It's a subtle change:

![Dependency Inversion](/images/dependency-inversion.png)

- `displayUserDescription` still depends on `loadUserProfile`
- `loadUserProfile` no longer accepts SQL. It takes a username instead
- This means no SQL or database details leak out
- module `userdata` now _also_ depends on this new abstraction: it must implement `loadUserProfile( username )`

> High level modules depend on abstractions, not on details

The old code had the `application` module depend on the low-level details of SQL, database tables and column names.

After Dependency Inversion, both modules depend on the high-level abstraction of `loadUserProfile( username )`.

This means that module `application` has no knowledge of how the data is stored. Any changes to those details _do not affect_ the module.

Module `userdata` is now the only place (DRY) that contains knowledge of how to work with the database. It _adapts_ the data returned from that into our high level abstraction.

> Dependency Inversion is a powerful technique for splitting up an application.
