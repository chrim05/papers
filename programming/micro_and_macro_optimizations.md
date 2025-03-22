*This paper is licensed by the license file in the following path: `/LICENSE`*

------

During software development, sooner or later, we encounter the need to optimize our code in order to speed up its execution time and reduce its memory consumption.

This document focuses solely on the first aspect: reducing execution time.

There are two classes of optimizations we can apply to achieve our goal:
* Micro Optimizations
* Macro Optimizations

The classification into one of the two categories is not based on the impact of the optimization, but on its **Form**.

The Architecture of a program consists of its highest abstraction, describing the macro Steps necessary to process the Input and transform it into the expected Output. So **Architecture** = Chain of Steps, where **Step** = Individual Component/Function

Based on this, we can define Macro Optimizations as a class of simplifications of the Architecture, and Micro Optimizations as a class of implementation details of the Steps.

Some examples of Micro Optimizations:
* Reducing the number of `malloc` calls.
* Choosing whether to use null-terminated strings + `strlen` or length-specified strings.
* Using a Data-Oriented Design (DOD) approach.
* Almost all types of optimizations performed by a compiler (Call Inlining, Loop Unrolling, Constant Folding, ...).

Some examples of Macro Optimizations:
* Elimination of redundant steps.
* Removal of Indirections: Indirections in the architecture of a program lead to several problems, including:
    * Inability for the compiler to apply Micro Optimizations (e.g., Inheritance).
    * Necessity of intermediate data to contain the states necessary to "coordinate" the sides of the Indirection, for example:
        * Storing Logging messages in a temporary Buffer, from which a Loop will draw to print these messages (the Buffer is the intermediate data, the components that write to the Buffer are side A and the Loop that reads from the Buffer is side B).
        * Storing Tokens in a List during the Lexing phase in a compiler (the List is the intermediate data, the Lexer is side A and the Parser is side B).

----

Macro Optimizations, obviously, affect the Big O Notation of a program, while Micro Optimizations do not. However, both are extremely important, precisely because the classification is not based on the impact on execution speed, but on its form.

----

Which of the two is more problematic if done prematurely?

It's a rather complex topic, but we can simplify it this way:
Macro Optimizations revolve around modifying the architecture of the program, so it's likely that the program will need to be entirely restructured and most of the components, consequently, rewritten. Micro Optimizations, on the other hand, revolve around modifying implementation details of these Steps, which can undoubtedly add Friction during software maintenance (e.g., DOD).

Personally, therefore, I find it unhelpful to apply either prematurely: Micro Optimizations are absolutely useless without Macro Optimizations (it would be like comparing an `O(n) + Micro` with an `O(log n) + No Micro`, obviously the second wins), but Macro Optimizations are not well-suited to very young software (subject to continuous radical changes).

Furthermore, it's important to consider that in order to apply Macro Optimizations effectively, the developer absolutely needs to be very familiar with the structure of that kind of program.

----

Which of the two is generally more beneficial?

It's not difficult to imagine, often, the impact and complexity of an optimization, regardless of its class.
One might think that Macro Optimizations add complexity, but it's almost always the opposite, since they simplify the Architecture of the program, also often leading to a significant impact on execution speed.

Conversely, Micro Optimizations often add a lot of complexity, and it's not at all certain that the impact is sufficient to justify the added complexity, so it's possible that they are not worth implementing.