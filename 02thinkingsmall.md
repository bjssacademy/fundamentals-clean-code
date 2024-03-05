# Thinking Small

Big Ideas are best solved in small chunks.

![Great things small things van gogh](/images/great-things-van-gogh.jpeg)

Typical BJSS clients are big organisations - banks, health care, national agencies. They have many users, high availability, and need oodles of data to be shunted about. THey tend to be large applications - usually fleets of microservices - that can run into ,illions of lines of code.

And we mortals simply cannot fit that all into our head.

## Don't eat the whole elephant

As the saying goes, "How do you eat an elephant? One bite at a time".

We cannot avoid complexity entirely. Some of what we our clients need doing is inherently complex. But that does not mean we have to solve all of the problems in one single function.

We _manage_ complexity by using a strategy of divide and conquer. We eat that elephant one bite at a time.

To grasp a large problem, we must break it down. Our codebase becomes a collection of largely separate modules, each one built from small, highly-focussed pieces of code.

At the code level, several simple techniques help us achieve this.

## Use small code blocks

Over time, our functions and methods grow larger. New requirements, new edge cases and new error conditions to handle all increase complexity.

To implement these features, we add more code. More variables. More conditionals. More loops. The function becomes a monster, difficult to understand.

The solution is to break large functions down into smaller ones. We do this by extracting code into more functions.

-- example???? struggling a bit

techniques

### Extract loop body

### Extract if / else block

- Easier to skim read
- Extract code block

## Avoid deeply nested code

- cyclomatic complexity
- if return
- switch
- un-nest loops at source level

basics
Comment -> name

- Why?
- Example

Named constants

- MAX_DEPOSIT

Reduce Scope

- Why
- Example

Avoid continue

Eliminate globals

- why

Use vertical space

- Mental chunking

Don’t eat the whole elephant

- Decomposition
- Easier to grasp
- Less to go wrong
