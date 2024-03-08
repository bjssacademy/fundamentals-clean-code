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

The big idea is called _separattion of concerns_. We divide the big problem up into smaller pieces, each one being concerned with one aspect of the problem. We keep those pieces as separated as possible.

### Coupling and Cohesion

The tools we use to manage that all relate to one simple idea:

> Things that belong together are grouped together

There are two technical terms to know: _coupling_ and _cohesion_.

- **Coupling** Components are _coupled_ if a change in one mandates a change in the other
- **Cohesion** Components are _cohesive_ if they are parts of solving the same problem

![Low coupling,  high cohesion](/images/coupling-cohesion.png)

A, B and C are all small software components that solve part of the same problem. They communicate with each other in well-defined ways.

D and E are small components that are part of solving a _different_ problem. D and E communicate with each other.

ABC and DE are contained in larger self-contained modules.

The two larger modules talk to each other infrequently at a higher level.

> Aim for Low coupling, High Cohesion

### Say again, in English?

- Things that change together belong together
- Things that don't belong apart
- Things apart should know nothing about how the others work inside
- Things apart communicate the bare minimum to get the job done

That's most of software design, object orientation, microservice deisgn and TDD summed up.

But the industry can't normally make things this simple, otherwise no-one would get paid ...

## Single Responsibility Principle (SRP)

This principle is part of five design principles known as the SOLID Principles.

> Guide to the [SOLID principles](/resources/solid-principles-javaoopdoneright.pdf)

The **Single Responsibility Principle** says that a software component should contain things that need to change together.

All the pieces needed for a single repsonibility of the code should be in that component. When we need to change anything, we make changes in one place. We also protect the rest of the codebase from having changes scattered about.

An exmaple would be a function that does three things:

- Opens a database connection
- reads some database data and applies some logic
- generates some html based on the data and outputs it

If we wrote all of those responsibilities into the same function, chances are any change to the database layout would end up affecting the logic and html code.

The fix is to split out the three responsibilities into three separate components:

- Database
- Logic
- HTML

Indeed this is very common, and a driver towards _three tier architecture_ and _hexagonal architecture_.

## Don't Repeat Yoursefl (DRY) Principle

DRY

- Change in one place
- Same abstraction only

## Modularity: Hide as much as you can

information hiding
encapsulation
private/public
packages - export as little as possible

## Dependency Inversion Principle

depend on abstractions

domain specific

## Published APIs

limit to naming and limited side effects
