# Summary

Clean code is important. It enables agility and keeps our team aligned.

Let's review the main points.

## Clean code is important

Clean Code will:

- Reduce development costs
- Reduce time to understand a piece of code
- Create a shared, common understanding across a team
- Enable different developers to work on each others' code
- Reduce defects by using safer constructs
- Increase delivery velocity by avoiding unnecessary delays
- Improve predictability of delivery, by reducing surprises

All these are technical enablers of agility.

> We _cannot_ be agile when the codebase resists change

## Cleaning in the small

At the level of individual lines of code, we can create big wins for clarity.

We can use the following techniques:

- Name variables for what they hold
- Name functions for what they do
- Emphasise the problem being solved over how we are solving it

Moving out to functions, the following are helpful:

- Don't eat the whole elephant - break problems down
- Use small code blocks
- Extract loop body
- Extract conditional expression
- Extract if/else blocks
- Avoid deeply nested conditionals
- Avoid nested loops
- Avoid continue and break
- Consider better algorithms
- Prefer forEach
- Consider reduce()

Prioritise KISS: Keep It Simple.

Is there a _simpler_ way of expressing our intent, so it becomes obvious?

## Avoiding confusion

Some code is confusing. It's not clear what it is doing.

Techniques to reduce confusion:

- Replace magic numbers / constants
- Say what you mean, mean what you say
- Avoid global anything
- Use the smallest scope you can get away with
- Don't rely on side effects
- Design-out errors
- Use defensive programming (if you must)
- Avoid hybrid coupling
- Avoid null and undefined
- Avoid reusing variables
- Prefer read-only
- Prefer switch to if-else-if chains

## Cleaning in the large

Applications grow larger than we can fit in our head. We need to compartmentalise them

Useful structuring techniques include:

- Don't Repeat Yourself (DRY)
- Separation of concerns
- Single Responsibility Principle (SRP)
- Coupling and Cohesion
- Modularity (Information Hiding)
- Managing Module Dependencies
- Reducing coupling between dependencies
- Dependency Inversion Principle (DIP)
- Dependency Injection (DI)

# [Further Reading >>](furtherreading.md)
