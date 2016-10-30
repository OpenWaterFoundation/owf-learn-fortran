# Fortran Language Overview

The Fortran language is a generally considered a procedural language,
meaning that logic is executed as a sequency of function and subroutine calls that process data.
Although later versions of the language (90, 95, etc.) and compiler extensions introduced concepts of objects (see derived data types/structures),
many legacy Fortran programs continue to utilize basic data types such as arrays of numbers.

## Fortran 77 and 90 Differences

Fortran code has traditionally been a bit cryptic due to early limitations on the length of variable names (6 characters in Fortran 77) and implicit typing.
For example, many old programs have variables with very short names that follow implicit typing (`i1` for integer, `a` for string, `x` for floating-point, etc.)

Differences between Fortran versions are discussed in the following resources:

* [Fortran 90 vs Fortran 77](http://www.owlnet.rice.edu/~ceng303/manuals/fortran/FOR1_5.html)
* [Fortran: what is it and what's the diffference between F90 and F77](http://www.astro.ex.ac.uk/people/saunders/computing_tutorials/fortran.pdf)

For this documentation, the term "Fortran 77" means explicit Fortran 77 language features and the term "Fortran 77-style" means Fortran 77 style,
but perhaps within a Fortran 90 program.  The goal over time is to migrate away from Fortran 77-style coding to Fortran 90 coding.

## Langage Reference Resources

The following are useful Fortran language references:

* [Tutorials Point - Learn Fortran](https://www.tutorialspoint.com/fortran/fortran_basic_syntax.htm)
* [Fortan 90 Tutorial (Stanford University)](https://web.stanford.edu/class/me200c/tutorial_90/)
* [CHBE 303 Fotran 90 Manual](http://www.owlnet.rice.edu/~ceng303/manuals/fortran/)

## Case-insensitive and Implicit Variable Typing]

Fortran is case-insensitive other than string literals.  "i" and "I" are considered the same variable.
Code can be written in all lowercase, all uppercase, or mixed, although older code is often all uppercase.

Fortran 77 will use implicit typing where variables with names starting with `i` through `n` are considered integers and others are considered
real (floating precision).  Modern typed languages require types to be explicitly declared and it is recommended that the following
statement is included at the top of the main program to require types to be declared:

```
implicit none
```

This will cause the compiler to generate errors if variables are used without declaring the type.

As to where to include the above statement, see the following:  ["Where to put `implicit none` in Fortran](http://stackoverflow.com/questions/24337413/where-to-put-implicit-none-in-fortran).
To be safe, include the statement at the top of each `PROGRAM`, module, and stand-alone subroutine and function.

## Fortran Passes Variables by Reference and Value

* Fortran passes primitive variables by value (meaning that calling routines do not see changes in the value).
However, it passes arrays by reference (meaning that calling routines do see changes in the values in the arrays).

Fortran 90 provides the `intent(in)`, `intent(out)`, `intent(inout)` syntax to provide a hint to the compiler as to whether routine parameters
are input, output, or both.

## Array Concepts

Arrays in Fortran can be declared in any module and arrays can be passed to called modules.
Legacy code may declare arrays with number sizes in each subroutine, for example:

```
PROGRAM ABC
REAL X(100)
CALL XYZ(X)
END

SUBROUTINE XYZ(X)
REAL X(100)
END
```

The above array declarations indicated that variable `X` will consist of 100 single-precision floating-point numbers.
The individual values can be accessed using an array index with value 1 to 100 (Fortran arrays by default are indexed 1+).

The above examples works until the first array size is changed.
If the dimension in the main program is reduced, then memory may be wasted.
If the dimension in the main program is increased, the array that is passed to the subroutine may overrun its bounds and cause an error.
One alternative is to declare the array dimension as dynamic in the subroutine, for example:

```
SUBROUTINE XYZ(X)
REAL X(*)
END
```

This indicates that the subroutine is receiving an array but it does not know the size.

## Sharing Global Data - Common Blocks vs. Modules

Need to discuss here transitioning from Fortran 77 common blocks to Fortran 90 modules and `use` syntax.
Useful resources:

* [Modules (Emory Univerisity)](http://www.mathcs.emory.edu/~cheung/Courses/561/Syllabus/6-Fortran/modules.html)

Common block background:

* Common blocks are essentially shared static data that can be accessed from multiple Fortran 77-style program/subroutine/function blocks
* Common blocks are typically included in multiple modules using `include common.inc` syntax.

Module background:

* Modules are essentially object constructs similar to objects in other languages.
* Do not use `include` statement, but are linked and therefore must be compiled before reference to them is used.
* Can put in library like intrinsic functions.
* includes a specifications section where data are declared and a `contains` section where subroutines and functions can be declared.
* Question:  do modules only contain global data or are object instances supported?

## Object-oriented Fortran

It is possible to create object-oriented Fortran using Fortran 90 features.
The behavior ("routines") are provided using a `module` and instance data are maintained in a structure.
The mudule routines take as a parameter an instance of the structure.
Consequently, the object data are maintained separate from the behavior.
See the following resources:

* [Pseudo Object Oriented Style in Fortran 90](http://fortranwiki.org/fortran/show/Object-oriented+programming)
