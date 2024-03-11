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

A counter-example would be a function that does three things:

- Opens a database connection
- reads some database data and applies some logic
- generates some html based on the data and outputs it

If we wrote all of those responsibilities into the same function, chances are any change to the database layout would end up affecting the logic and html code.

The fix is to split out the three responsibilities into three separate components:

- Database
- Logic
- HTML

Indeed this is very common, and a driver towards _three tier architecture_ and _hexagonal architecture_.

SRP is a restatement of "maximise cohesion". _Cohesion_ is a technical term with a counterpat _coupling_. Let's take a look at those importnat ideas next.

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

### Database, logic and html example

In our fictional three components, we should aim for high cohesiveness inside each component, and low coupling between components.

How might that look?

#### HTML Component

In this case, a component might be any of the following stuctural patterns:

- A set of functions inside a JS `module` that we can import
- A number of methods on a `class`
- A group of React components
- in other languages, such as Go or Java, a `package`

Whatever broader structure we choose, we aim to place all logic related to html together:

- html and css code
- react components, if used
- html templates to power a templating engine (eg handlebars)
- presentation logic, things such as date formatters (arguable this can be separate as well!)
- tests for these components in isolation

#### Logic component

This will end up fetching raw data from whichever sources it comes from. Typically a database, or web services. Perhpas reference data from JSON files.

This component _does not_ directly access those sources. Code here merely _requests_ data through a suitable abstraction. If we need some user profile data, our abstraction might be `fetchUserProfile( userId )`. We would not specify (nor restrict) where this profile data came from.

The logic is the part that implements business rules in the server-side, or complex presentation rules in the UI.

#### Data component

Our logic component requested data using an abstraction. This is where stuff gets real and the concrete code lives to actually get the data.

We might see:

- Database connections, queries (perhaps in SQL), transactions, configurations
- HTTP calls to web services
- Locally cached file data, as read-only reference data

## Modularity

The above splits are exmaples of modularity, which is also known as _Information Hiding_.

Information Hiding was first described by David L. Parnas in 1968, and was a leap forward for managing complexity.

A module should:

- Do a job
- Hide the algorithms needed to do that job
- Hide the data structures meeded to do that job
- Present a minimal programming interface to ask that the job gets done

This was a foundation of Object-Oriented Programming, and also of every module or package system that we see today.

It is "Don't eat the whole elephant" in code structure form.

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

Each time we read the number `10`, we learn to reverse-engineer the code around it, and the code that uses it, to figure out which concept it is.

This is costly, done by every reader as waste and error-prone. Combined with unclear code, we might not fully grasp what the duplicated concept represents.

### Copy, Paste, Fail

We've used a numeric constant as a typical example.

The bigger failure is when we copy-paste code around.

Each duplicate of a code block suffers the same problems. Often, we will not spot every duplicated block, and end up changing only some of them.

Now our code behaves in exciting, hard to predict ways!

Again, our tests should pick this up. But if not, look forward to such fun in debugging it.

We make this even harder once we have modified any of the blocks. Then what used to be duplication _isn't_ anymore. We have ow completely lost track of the related concept.

Future readers will spend _a very long time_ trying to assess if the two _similar_ blocks of code should be replaced by the _same_ block, or if you _intended_ them to diverge. The code doesn't say. You aren't there to help them.

### Apply DRY to solve this

The DRY principle reminds us that we cause problems by repeating information in multiple places. It doesn't matter what the information is. We need a _single source of truth_ for all information. That aids understanding as well as ensuring changes can be made safely in the future.

You can see DRY as an extension to SRP. Not only must things that change together be near to each other, but things that _are actually the same_ should be the represented by the same code.

## Dependency Inversion Principle

depend on abstractions

domain specific
