# Thinking Big

We have some ideas about building simplicity in the small.

But how can we tame the bigger pieces?

![thinking big](/images/thinking-big.png)

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

> Hide detail. Expose a minimal, complete abstraction

These ideas combine to allow us to eliminate unnecessary duplication of code. The guideline for this is often known as DRY.

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

Admittedly it is artificial, and probably better structures exist.

**Did you notice the _magic number_ 10 occur twice?**

That `10` represents a fixed number of things that we want add to a total and send an email about.

It is the _same concept_ repeated twice: 10 things.

This causes a few problems:

### Not changing enough

Suppose we want to now have 20 things instead of 10. We need to find every occurrence of that `10` to change and change it.

We don't always succeed.

Sometimes, our search tools don't reveal every occurrence. Sometimes, human error means we forget to change one.

the software is now **broken**.

Hopefully our unit tests will pick up that we've broken something. But if they don't, then one part of the code is working with 20 things, and the other part with 10. That ain't never gonna work.

> We are writing tests, right? ... _right?_

### Changing too much

The code may also contain occurrences of `10` that have nothing to do with our ten things. It relates to a completely different concept; perhaps the number of days a discount will apply for.

If we change the unrelated 10s, we've broken our software in a new and creative way.

Once again, our unit tests should pick this up.

But even better would be to avoid this in the first place. By having a single source of truth for the separate concepts, we will design-out this class of error.

### Confusion

Each time we read the number `10`, we must reverse-engineer the code to find out what it means.

This is wasteful of everybody's time.

### Copy, Paste, Fail

We've used a numeric constant as a typical example. The bigger failure is when we copy-paste code around.

Suppose we copy-paste a code block three times. Every change will now need to be made in those three blocks.

Worse, one of those blocks gets changed _without_ the necessary changes to the other two.

The code has diverged, and it will be broken in very difficult to spot ways.

> Tests should pick this up - **write them**

Future readers will spend _a very long time_ trying to assess if the two _similar_ blocks of code should be replaced by the _same_ block, or if you _intended_ them to diverge. The code doesn't say. You aren't there to help them.

### Apply DRY to solve this

The DRY principle solves problems caused by repeating information. It doesn't matter what the information is. We need a _single source of truth_ for all information. That aids understanding. It enables future changes to be made safely.

We can see DRY as an extension to SRP. Things that change together should be near to each other. Things that _are actually the same idea_ should be the represented by the same code. We put that code in one place and re-use it.

## Dependency Inversion Principle

depend on abstractions

domain specific
