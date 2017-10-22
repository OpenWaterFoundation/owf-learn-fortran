# Using Fortran Libraries #

A Fortran software library is a collection of code that is managed in one file that can be linked to other code.
A library is suitable when code is reused in multiple programs.
If a library is not used, then the original code must be copied into each program, which can result in maintenance issues.

## Fortran Library Concepts ##

Fortran libraries are used at compiler link time, which is the step where the compiler joins together different code modules to create the executable program.
Actually, a separate program may be used for linking (the "linker") but in many cases the linker is called by the compiler as the final step.
Library files contain binary content produced by the Fortran compiler, where in this case the compiler did not try to create an executable program,
but instead created a file contains callable routines and their associated data.  Multiple modules/routines are located in each library.
Libraries are typically created for specific purposes, such as a math library, model input/output, etc.,
to provide the most flexibility and reusability as program  building blocks.

The compiler/linker will search each library based on a list of explicitly-specified library files or by searching in folders in the link path,
such as standard library locations on the computer.  **Need to describe more where these folders are and environment variables used for the link path.**

The compiler/linker has a list of module names that were used in the software, and it looks for those module names in libraries.
Found modules are included in the created executable (if a static executable) or are marked as the dependency being found for dynamic libraries
(for dynamic executables) - see the next section for more on static and dynamic libraries.
If any modules cannot be found, the compiler will generate an error,
in which case the software developer must confirm that they are using the correct module name and are linking in the correct libraries.

The error messages printed by the linker may not exactly match expected module names because the compiler internally may prepend or append
characters such as underscores to module names to ensure uniqueness.
Module names may also have multiple versions for unicode characters or other variations in input and output.
**Need some examples of link errors**

## Static and Dynamic Libraries ##

Static libraries are intended to be linked with other code to create static executables.  In this case the code provided by the libraries
is added to other code to create the static executable and the library is no longer used in its original form.

Dynamic libraries are intended to be shared at runtime, which allows program files to be smaller.
Dynamic libraries are common on operating systems so that each program can be smaller.

The compile/link process will generally include specification of whether a static or dynamic executable is being created,
for example the `-static` compiler option.  In all cases, the software developer must specify the appropriate static or dynamic
library name, using conventions for the operating system and compiler.
Trying to link the wrong version of the library will not result in a working compiled program.

An important consideration is that distribution of static executables does not require distribution of any other files.
However, distribution of dynamic executables requires that the dynamic libraries have been installed on the computer and are accessible at runtime.
The latter requires that more effort is invested in the software installer and launcher.

**Need to include examples of Fortran compiler libraries**

For Windoews... **need to provide example**

For Linux ... **need to provide example**

The following Cygwin example illustrates static libraries, including `libc.a` (`.a` means "archive"), `libnssckbi.so` (`.so` means "shared object"),
and`cygguilereadline-v-17-17.dllcygguilereadline-v-17-17.dll` (`.dll` means dynamic link library, which is used by Windows,
important in this case because Cygwin software is binary-compatible with the Windows operating system).
**Need to figure out what libcygicons.dll.a is**:

```
$ ls /lib
apr-util-1/                          cygswrAVX.dll*      GraphicsMagick-1.3.25/  libc.a                            libintl.a             libtextmode.a      php/        tcl8/
automode.o                           cygswrAVX2.dll*     groff/                  libcygicons.dll.a*                libintl.dll.a*        libtextreadmode.a  pkcs11/     tcl8.5/
binmode.o                            default-manifest.o  gtk-2.0/                libcygwin.a                       libm.a                libutil.a          pkgconfig/  terminfo@
charset.alias                        dri/                gtk-3.0/                libdl.a                           libnssckbi.so@        libuuid.dll.a*     pstoedit/   texinfo/
cpp@                                 gawk/               httpd/                  libg.a@                           libnsssysinit.so*     lighttpd/          python2.7/  textmode.o
crt0.o                               gcc/                ImageMagick-6.9.5/      libgmon.a                         libopcodes.a          lua/               python3.4/  textreadmode.o
csih/                                gcrt0.o             lapack/                 libguilereadline-v-17.la*         libphp5.dll.a*        mysql/             qt4/        Tix8.4.3/
cygguilereadline-v-17-17.dll*        gdk-pixbuf-2.0/     libacl.a                libguile-srfi-srfi-13-14-v-3.la*  libpthread.a          octave/            R/          tk8.5/
cygguile-srfi-srfi-13-14-v-3-3.dll*  gems/               libautomode.a           libguile-srfi-srfi-1-v-3.la*      libpython2.7.dll.a@   openssl-1.0.2/     ruby/       urxvt/
cygguile-srfi-srfi-1-v-3-3.dll*      ggi/                libbash.dll.a*          libguile-srfi-srfi-4-v-3.la*      libpython3.4m.dll.a@  p11-kit/           sasl2_3/    w32api/
cygguile-srfi-srfi-4-v-3-3.dll*      gio/                libbfd.a                libguile-srfi-srfi-60-v-2.la*     libresolv.a           p7zip/             security/   X11/
cygguile-srfi-srfi-60-v-2-2.dll*     girepository-1.0/   libbinmode.a            libiberty.a                       librt.a               perl5/             sendmail@   xorg/

```

## 32-bit and 64-bit Libraries ##

Most operating systems intended to run Fortran programs are available as 32-bit and 64-bit, with 64-bit becoming the norm.
64-bit operating systems are able to address more memory locations and consequently can run programs that require more memory.
This may be important for complex models. 

64-bit operating systems are able to run 32-bit software created for the same operating system, generally by keeping 32-bit and 64-bit versions
of software and libraries.  For example, on Windows 64-bit computers the `C:\Programs Files` folder contains 64-bit software and the
`C:\Program Files (x86)` folder contains 32-bit software.
Although it is possible to install 32-bit and 64-bit versions of the same software on a computer, this may be confusing.
In general it is desirable to move to 64-bit versions compatible with the 64-bit computer, if possible.
64-bit operating systems can run software compiled for 32-bit operating systems,
but 32-bit operating systems cannot run software compiled for 64-bit operating systems.

**Need to add discussion here about how to compile and link 32-bit and 64-bit Fortran libraries**

## Linking in Libraries in Other Languages ##

At a basic level, simple Fortran functions and subroutines are the same as those in other languages.
This allows Fortran to call libraries written in other languages, typically C.
This may be appropriate if it would take a lot of effort to port the library to Fortran,
or if the library provides functionality that may not be easily implemented in Fortran,
such as graphics drivers, and database interaction libraries.
A mixed-language approach is appropriate but must utilize sufficient testing to ensure functionality.

**Need to insert an example**

## Mechanics of Maintaining  a Library with Version Control ##

Because a library contains code that can be called by other programs and libraries,
it is generally necessary to maintain the code for the library in a separate repository.
This can be accomplished easily in any repository software, including Git.
In this case, the repository will contain build processes to compile, test, and create the distributable library,
as well as creating HTML API documentation that can be used when writing other software.
Testing for the library may include unit tests for specific routines,
and perhaps one or more main programs that call library routines to test higher-level functionality.

From a practical standpoint, a library is often developed hand in hand with an application that exercises the functionality of a library.
This ensures that the library API is meeting the needs of an implementation.
This requires that the source code for the library is accessible while developing the main program.
Development environments such as Eclipse allow multiple projects to be included in a workspace.
Consequently, the workspace for a main program would include the source code projects for the library and main program.
This allows the code for both projects to be edited and tested at the same time.
The developer can then update both software projects efficiently.

The alternative is to rely on a binary version of the library similar to other libraries that may be linked to a program,
and ensure that the library has all necessary functionality
tested through unit tests.  This may or may not be practical, depending on how much the library relies on the main program for development and testing.

An important consideration is how to version libraries so that are backward compatible with old APIs
and also provide new functionality, which is why many libraries include version numbers.
Libraries must also be kept up to date to retain their value.

## Creating a Library ##

** Need to insert example **

## Using a Library ##

** Need to insert example **
