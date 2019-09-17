---
title: Ideas to Include in a New Programming Language
categories: [programming]
tags: [fun]
---
<!-- {% raw %} -->
<!-- {% include refc-small.html text="ref commit" commit="3cad965..." %} -->
<!-- {% include ref-commit.html text="ref commit" commit="3cad965..." %} -->
<!-- {% endraw %} -->

### Idea: DAG of Programming Language Features
We model the language as a directed, acyclic graph of programming language features,
where each feature is the child of all the features it depends on, or whose behavior
it modifies. Thus, different branches of the graph must be orthogonal, by definition.

##### Application: Disabling Language Features

### Idea: Auto-Destructure Structs
Structs that are destructured into their component parts, and whose methods
get inlined automatically.

##### Application: RAII without the Overhead

### Idea: Scopes as Anonymous Structs
The namespace of a scope can be accessed as a record (struct without a type name).

##### Application: Immediately-Invoked Lambdas without the Abstraction Cost

### Idea: Contracts Over Types/Data Fields
Contracts enforced over data fields in structs, such that each contract must
hold while the struct is valid. Contracts over types would be a special case, where
the type doesn't have fields.

##### Application: Fine-Grained Safety Checks

### Idea: Associative Scopes
Scopes where the order of execution is explicitly determined at compile-time.
Mutability is not allowed, but initialization after declaration is.

##### Application: Coarse-Grained (i.e. Declarative) Control Flow

### Idea: Loop Invariant Declaration
The programmer declares loop invariants natively in the language.

##### Application: Fine-Grained Safety Checks

##### Application: Easier Optimization

### Idea: Orthogonal Pointer Annotations
Pointer annotations for concepts like mutability, ownership, block-type (single object
or array of objects), dispatch-style (dynamic or static), etc.

##### Application: Customizable Safety Checks

### Idea: Compile-Time Source Edits
The compiler is allowed to edit your source during compilation.

##### Application: Cannonicalization Work Can be Saved to Source

### Idea: Language Support for Allocators
Allocators are given the same amount of information that garbage collectors are.

##### Application: Intrusive Allocators

### Idea: Flexible Namespace Captures
Restrict access to a name in the surrounding namespace, or to all but a subsection
of names.

##### Application: Debugging Invariants
Let's say I have a global that, for some reason, doesn't take on the value I
expect it to. With namespace captures, I can simultaneously debug it while also
making my code easier to read. By putting namespace capture restrictions on scopes
in question, I can check, at compile time, which scopes are using the global.
Additionally, I can check exactly which scopes *modify* the global, which is much
harder to do with something simple like text search.

### Idea: Module Permissions
Programmatically prevent modules from doing certain kinds of tasks. For example,
prevent a module from doing anything but computation, as a feature of the language.

##### Application: Secure Black Boxes
If I import a module, I expect it to do what it says on the tin; if it doesn't,
that could be a security vulnerability. I'd like to be able to, at compile time,
check that the module I'm importing doesn't copy my entire file system, compress it,
and post it in a public repo on GitHub.

### Idea: Native Generators
Using an implicitly passed pointer into the stack, we can check whether or not we're
in a generator, and basically keep generator code as straight-line code, instead
of breaking it up into separate pieces first.

Here's how it would work:
-  Generator code is called with a pointer into the call stack where they were called.
   When you run the generator, it yields typed objects, along with a pointer into
   the binary saying which instruction it was on.
-  Generator code that doesn't yield a value or have no more values to yield will return
   a null pointer instead. Additionally, when the generator code is called with
   a null pointer, then it goes to its return value and skips any remaining yields.
-  Generators themselves are just objects on the stack that are copied in and out
   of the heap as needed. The first field is always the pointer into the callstack.
-  An advantage of this style is that any arbitrary function can be trivially turned
   into a generator by just adding read barriers in the original function and
   adding a pointer

