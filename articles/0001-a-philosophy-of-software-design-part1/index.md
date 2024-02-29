Insights on 'Philosophy of Software Design' 1 - The Nature and Cause of Software Complexity
===
resouce：[A Philosophy of Software Design](https://book.douban.com/subject/30218046/)

## Foreword

During the development process, we sometimes feel powerless when looking at code. This is evident when:
- You have to open many files to understand the implementation of a module, jumping back and forth between them.
- An extremely long function, such as one that extends over thousands of lines, relies mostly on comments for understanding its logic.
- Making a change in one area, thinking it would succeed only to find it has cascading effects (CSS, I'm looking at you!).
- Needing to change several parts because of a single alteration.
- Often hearing colleagues complain: "Oh no, the code is too tightly coupled, it's difficult to modify." (What is coupling?)

The first part of this book mainly discusses this issue: software complexity, leading us from realizing the root cause of feeling "powerless" in programming and maintenance - high software complexity - to guiding us in reducing complexity to create maintainable code.


## Understanding High Complexity

The complexity in software development is not the time and space complexity we refer to when doing LeetCode challenges, but how**understandable and modifiable** the software design is.

'A Philosophy of Software Design' mentions that software complexity is evident when:
- A section of code cannot be easily understood - the software is too complex.
- Changing a section of code requires modifications to many other parts - it is too complex.
- Repairing a section of code leads to another bug during the process - the original code was too complex.

Conversely, if the code you write can be easily understood by others in structure and logic, and altering the code does not affect other areas, congratulations - your code design is simple! (And it's not just because the business is simple, as some simple businesses can also have complex, spaghetti-like code; or the business might be complex but still written very cleanly and refreshingly by an expert.)

A principle in software design is **loose coupling**, which means we should write code that is **highly cohesive and loosely coupled**. The degree of coupling refers to the level of dependency between different parts:

If Module A freely uses the internal details of Module B, then we can say that Module A and Module B are tightly coupled. In other words, high coupling means that one module understands the inner workings of another module in depth.

> Example: A shopping website has two modules: the order system and the inventory system. Before placing an order, one needs to know the inventory levels. Here are two implementation methods:
> - Tightly coupled: The order processing system queries the inventory management system's database directly to determine if there’s sufficient stock.
> - Loosely coupled: The inventory management system provides an API, through which the order processing system queries stock levels.

<br/>

Therefore, we need modules to abstract interfaces for external use so that the external user does not need to worry about the internal details; this is how we reduce coupling. It means we need to do:
**Modularization - breaking down modules, identifying what modules there are - which is abstraction, and what interfaces the modules provide - abstract design, API design**。

## Symptoms of High Complexity

The book 'A Philosophy of Software Design' mentions three manifestations of excessive complexity. When writing code, encountering the following three conditions is a time for caution!

### 1.Change Propagation: A Domino Effect

Simple modifications of functionality necessitate widespread changes in code across various locations.

This brings to mind Redux and its abundant boilerplate code.

For instance, in supporting theme colors, the editor's internals are saturated with color-related variables, with each business logic layer utilizing theme attributes like theme.xxxColor1, theme.xxxColor2, etc., to denote colors.

If you were tasked with adapting the editor to support theme colors, how would you proceed? The API for theme colors is: theme(color, currentTheme). It's important to note that the theme.xx reference could be used in hundreds of places!


way 1：
```tsx
(
  <Button color={theme(config.primaryColor, currentTheme)} />
  <Button color={theme(config.borderColor, currentTheme)} />
  <Button color={theme(config.textColor, currentTheme)} />
)
```

way 2：Implement a proxy on the config to manage it at the source level, rather than making scattered changes at the business layer. This way, you only need to modify one place, and the maintainability is high as well.


### 2.Cognitive Load
Cognitive load refers to "how much prerequisite knowledge is needed beforehand." An excessive cognitive burden means that if other developers want to understand a particular piece of code, the prerequisite knowledge required is too high.

For example, learning React is more cognitively demanding than Vue because React also introduces the concept of functional programming.
For instance, in the case of editor code related to graphic editing, besides programming knowledge, you also need mathematical knowledge—like understanding matrices.

### 3.Unknown Unknowns - Beyond Control

Not knowing which things need to be changed to accomplish a task, or unaware that changing one thing would cause xx to happen, are situations of "unknown unknowns."

These are generally hard to detect, and when they are discovered, it's usually through a mistake.

For example, I once encountered an "unknown unknown" pitfall: when reusing a logic component written by someone else, I intended to reuse its logic capabilities, but didn't expect that the component's CSS would pollute the global page CSS! The component had a global `overflow: hidden`. I reused it without checking and this caused the new page to be non-scrollable!

(Who would have thought that a logic component could override and modify the global styles! )


## Summary

- The essence of complexity is: "hard to understand" and "hard to change.”
- Reasons for high complexity: Dependencies and Obscurity
  - Dependency: a piece of code cannot be modified or understood independently, and other modules must be considered/modified when making changes, "dependency" causes complexity diffusion and part of the cognitive load.
  - Obscurity: important information is not conspicuous enough, "obscurity" causes unknown unknowns and another part of the cognitive load.

<br/>

Now that we understand the reasons behind "bad code," the next step is to consider: how can we reduce the complexity of software design?

See you in the next article! 👋🏻