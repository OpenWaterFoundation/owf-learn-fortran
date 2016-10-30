# Fortran Development Environment / Overview

Fortran has traditionally been used to develop computationally-intensive programs,
and many legacy water resources models are still written in Fortran.
Fortran has also traditionally been used to develop command line programs, rather than graphical user interfaces (GUIs).
GUIs may be implemented in other languages as wrappers that display Fortran model input and results.

This documentation assumes that the Fortran program being written is run from the command-line rather than a GUI.
Consequently, the developer environment focuses on command-line execution of the Fortran program.
However, an interactive development environment (IDE) may be used for development, based on developer tool selection.

Rather that focusing on one environment, this documentation describes several developer environments
so that software developers can work in the environment that they feel comfortable.
The following sections provide an overview of the major developer environment components.
This information is biased by OWF's need for and experience with specific tools.
However, an attempt has been made to structure documentation so that multiple options can be included as first-hand experience is gained.

This documentation focuses first on compiling static executables, which are larger, but ensure complete packaging of the executable program,
without run-time shared library dependencies.  
Information about compiling libraries and executables that use shared libraries is included where appropriate.
Similarly, discussion of 32-bit and 64-bit executables is included where appropriate.

# Operating System

The following operating systems are described by this documentation:

* Windows 7 64-bit
* Windows 10 64-bit
* Cygwin 64-bit (compatible with Windows 7 or 10)
* Linux (**need to determine whether to reference generically or recommend specific versions - probably focus on Debian, Ubuntu, Linux Mint because all use apt-get**)

# Compiler

The compiler converts Fortran source code (\*.for, \*.f files) into executable programs and libraries.
Legacy Fortran programs may assume a certain compiler consistent with the development team tool choices.
Although some mention of commertial compilers is included in this documentation,
the focus is on freely-available open source compilers, in particular gfortran.

The Fortran language specification version is assumed to be at least 77, with some discussion of 90 and 95.
This documentation focuses more on core features of Fortran and avoids specific extensions.

# Interactive Developer Environment (IDE)

An IDE can greatly increase the efficiency of development.
For example, IDEs can provide:

* code completion
* access to documentation for variables and routines
* edit-time error-checking (prior to full compile/run)
* refactoring tools (renaming code components)
* interactive debugger and performance analysis

Multiple IDE options for Fortran exist.
However, because Fortran is not as popular a language as other languages, IDEs may require additional setup.
In addition to full IDEs, it is also possible to use high-end text editors that provide software project support (for example [Sublime Text](https://www.sublimetext.com)).

Rather than get into discussions about which tools are the best,
this documentation focuses on the popular Eclipse/Photran IDE.
Additional options may be documented in the future.

Additional details of the development environment flow from other tool choices, including use of the `make` program.

# Revision Control

This documentation focuses on using Git for revision control due to its popularity and OWF experience on various projects.
Specific Git workflow protocols are not discussed but should be determined for each specific Fortran software project.

# Code Documentation

This documentation focuses on using Doxygen for code documentation.
Other technologies may be available; however, OWF has had success with Doxygen.
