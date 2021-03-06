<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [OCaml](#ocaml)
  - [Setup](#setup)
  - [Toolset](#toolset)
      - [OCAML Interactive Shell](#ocaml-interactive-shell)
      - [UTOP Interactive Shell](#utop-interactive-shell)
      - [OCaml Browser](#ocaml-browser)
      - [Troubleshooting](#troubleshooting)
      - [Opam Package Manager](#opam-package-manager)
      - [Misc](#misc)
  - [Basic Syntax](#basic-syntax)
        - [Primitive Types](#primitive-types)
        - [Operators](#operators)
        - [Variable Declaration](#variable-declaration)
        - [Polymorphic Functions](#polymorphic-functions)
        - [Number Formats](#number-formats)
        - [Math / Float Functions](#math--float-functions)
        - [Function Declaration](#function-declaration)
        - [Function Composition](#function-composition)
        - [Lambda Functions/ Anonymous Functions](#lambda-functions-anonymous-functions)
        - [Control Structures](#control-structures)
  - [Standard Library Modules](#standard-library-modules)
    - [List](#list)
    - [Array](#array)
    - [String](#string)
    - [Sys](#sys)
    - [Unix](#unix)
  - [IO - Input / Output](#io---input--output)
    - [Type Casting](#type-casting)
  - [Algebraic Data Types and Pattern Matching](#algebraic-data-types-and-pattern-matching)
    - [Algebraic Data Types](#algebraic-data-types)
      - [Record Types](#record-types)
      - [Disjoint Union](#disjoint-union)
      - [Agreggated Data types](#agreggated-data-types)
      - [Pattern Matching](#pattern-matching)
        - [Basic Pattern Matching](#basic-pattern-matching)
        - [Tuple Pattern Matching](#tuple-pattern-matching)
        - [List Recursive Functions](#list-recursive-functions)
      - [Recursive Data Structures](#recursive-data-structures)
        - [Alternative List Implementation](#alternative-list-implementation)
        - [File Tree Recursive Directory Walk](#file-tree-recursive-directory-walk)
  - [Lazy Evaluation](#lazy-evaluation)
        - [Infinite Lazy Lists](#infinite-lazy-lists)
  - [Foreign Function Interface FFI](#foreign-function-interface-ffi)
    - [Calling C Standard Library and Unix System Calls](#calling-c-standard-library-and-unix-system-calls)
    - [Calling Shared Libraries](#calling-shared-libraries)
      - [Calling GNU Scientific Library](#calling-gnu-scientific-library)
      - [Calling Python](#calling-python)
    - [Finding Shared Libraries](#finding-shared-libraries)
    - [References](#references)
  - [Creating Libraries, Modules and Compiling to Bytecode or Machine Code](#creating-libraries-modules-and-compiling-to-bytecode-or-machine-code)
    - [Loading Files in Interactive Shell](#loading-files-in-interactive-shell)
    - [Compile Module to Bytecode](#compile-module-to-bytecode)
  - [Batteries Standard Library](#batteries-standard-library)
    - [Getting Started](#getting-started)
  - [References](#references-1)
    - [Articles](#articles)
    - [Links](#links)
    - [Books](#books)
    - [Community](#community)
      - [Online Resources](#online-resources)
      - [Blogs](#blogs)
      - [Hacker News Threads](#hacker-news-threads)
      - [Stack Overflow Highlighted Questions](#stack-overflow-highlighted-questions)
      - [See Also](#see-also)
      - [Quick Reference Sheets](#quick-reference-sheets)
    - [References By Subject](#references-by-subject)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

<!--
The recursion for foldr f x ys where ys = [y1,y2,...,yk] looks like

f y1 (f y2 (... (f yk x) ...))

whereas the recursion for foldl f x ys looks like

f (... (f (f x y1) y2) ...) yk

foldl is:

    Left associative: f ( ... (f (f (f (f z x1) x2) x3) x4) ...) xn
    Tail recursive: It iterates through the list, producing the value afterwards
    Lazy: Nothing is evaluated until the result is needed
    Backwards: foldl (flip (:)) [] reverses a list.

foldr is:

    Right associative: f x1 (f x2 (f x3 (f x4 ... (f xn z) ... )))
    Recursive into an argument: Each iteration applies f to the next value and the result of folding the rest of the list.
    Lazy: Nothing is evaluated until the result is needed
    Forwards: foldr (:) [] returns a list unchanged.

http://stackoverflow.com/questions/3082324/foldl-versus-foldr-behavior-with-infinite-lists
-->

# OCaml

OCaml (Objective Categorical Abstract Machine Language) (formerly known as Objective Caml) is the main implementation of the Caml programming language, created by Xavier Leroy, Jérôme Vouillon, Damien Doligez, Didier Rémy and others in 1996. OCaml is an open source project managed and principally maintained by the French institute INRIA. The Caml's toolset includes an interactive toplevel interpreter, a bytecode compiler, and an optimizing native code compiler.

**Features**

* Strong Static Type  - Type Safety
* Type Inference      - The compiler infers the types for you, you don't need to write all the types;
* Curried Functions   - Allows to create and functions on the fly.
* Strict Evaluation by default
* Optional Lazy Evaluation
* Multi Paradigm      - Functional, Imperative and Object Orientated
* Compiled and Interpreted - It allows iteractive development and debugging.
    * Compiles to natve code and bytecode
* Algebraic Data Types
* Pattern Matching

**History**

* ML: Meta Language
    * 1973, University of Edinburg
    * Used to program search tactics in LCF theorem prover

* SML: Standard ML
    * 1990, Princeton University

* OCaml
    * 1996, INRIA

**Ocaml Shell Online**

You Can try OCaml online in:

* [OCsigen - JS of Ocaml Toploop](http://ocsigen.org/js_of_ocaml/dev/files/toplevel/index.html)

<!--
@TODO: Optional data Type description and examples / Maybe Monads
@TODO: Show how to create libraries, project structure, compile
@TODO: Add more examples about Lazy Evaluation
@TODO: Add more examples about Read Input and Output
-->

## Setup

Ocaml installer, packages and releases:
* http://caml.inria.fr/ocaml/release.en.html

* [OCamlPro's version of OCaml on Windows](https://github.com/OCamlPro/ocpwin-distrib)

**Linux**

Ubuntu:

Show all OCaml packages available.
```bash
$ apt-cache search ocaml
```

```bash
sudo apt-get install curl build-essential
sudo apt-get install ocaml opam ocaml-native-compilers m4
```

Arch Linux:
```
$ yaourt -s ocaml
```

Gentoo:
```
$ emerge ocaml
```

Fedora
```
$ su - -c "yum install ocaml"
```

Install Utop, Core library and Batteries library

```
opam update
opam install utop core batteries
```

## Toolset

See also: https://github.com/OCamlPro/ocaml-cheat-sheets

| Program      | Description                                |
|--------------|--------------------------------------------|
| ocaml        | toplevel loop  / Interactive Shell         |
| ocamlrun     | bytecode interpreter                       |
| ocamlc       | bytecode batch compiler                    |
| ocamlopt     | native code batch compiler                 |
| ocamlc.opt   | optimized bytecode batch compiler          |
| ocamlopt.opt | optimized native code batch compiler       |
| ocamlmktop   | new toplevel constructor                   |
| ocamldoc     | Generate documentation for source files in HTML/ LaTex / man pages |
| ocamlfind    | Find OCaml packages installed              |
| opam         | Package manager for OCaml                  |
| utop[optional] | Improved interactive shell              |


```
Purpose             C       Bytecode    Native code
Source code         *.c     *.ml        *.ml
Header files1       *.h     *.mli       *.mli
Object files        *.o     *.cmo       *.cmx2
Library files       *.a     *.cma       *.cmxa3
Binary programs     prog    prog        prog.opt4
```

Standard Libraries:

* [Native Library](http://caml.inria.fr/pub/docs/manual-ocaml/libref/)

* [Core](https://ocaml.janestreet.com/ocaml-core/111.17.00/doc/core/) - Jane Street Capital's Standard
* https://github.com/janestreet/core_kernel

Library
* [Batteries](http://batteries.forge.ocamlcore.org/) - Maintained by community

Linux:
```
$ apropos ocaml
Callback (3o)        - Registering OCaml values with the C runtime.
Lexing (3o)          - The run-time library for lexers generated by ocamllex.
ocaml (1)            - The OCaml interactive toplevel
ocaml.m4 (1)         - Autoconf macros for OCaml
ocamlbuild (1)       - The OCaml project compilation tool
ocamlbuild.byte (1)  - The OCaml project compilation tool
ocamlbuild.native (1) - The OCaml project compilation tool
ocamlc (1)           - The OCaml bytecode compiler
ocamlcp (1)          - The OCaml profiling compilers
ocamldebug (1)       - the OCaml source-level replay debugger.
ocamldep (1)         - Dependency generator for OCaml
ocamldoc (1)         - The OCaml documentation generator
ocamldot (1)         - generate dependency graphs of ocaml programs
ocamldumpobj (1)     - disassembler for OCaml executable and .cmo object files
ocamllex (1)         - The OCaml lexer generator
ocamlmklib (1)       - generate libraries with mixed C / Caml code.
ocamlmktop (1)       - Building custom toplevel systems
ocamlobjinfo (1)     - dump information about OCaml compiled objects
ocamlopt (1)         - The OCaml native-code compiler
ocamloptp (1)        - The OCaml profiling compilers
ocamlprof (1)        - The OCaml profiler
ocamlrun (1)         - The OCaml bytecode interpreter
ocamlyacc (1)        - The OCaml parser generator
opam (1)             - source-based OCaml package management
Parsing (3o)         - The run-time library for parsers generated by ocamlyacc.

$ whereis ocaml
ocaml: /usr/bin/ocaml /usr/lib/ocaml /usr/bin/X11/ocaml
/usr/local/lib/ocaml /usr/share/man/man1/ocaml.1.gz

```

Extension Files:

| Extension  |  Meaning                         |
|------------|----------------------------------|
| .ml        | source file                      |
| .mli       | interface file                   |
| .cmo       | object file (bytecode)           |
| .cma       | library object file (bytecode)   |
| .cmi       | compiled interface file          |
| .cmx       | object file (native)             |
| .cmxa      | library object file (native)     |
| .c         | C source file                    |
| .o         | C object file (native)           |
| .a         | C library object file (native)   |


From:
* http://caml.inria.fr/pub/docs/oreilly-book/html/book-ora066.html



#### OCAML Interactive Shell

```
$ rlwrap -m -c -r -H ../history.ml -f ../completion.txt ocaml
```

OCAML Directives

Read, compile and execute source phrases from the given file. This is textual inclusion: phrases are processed just as if they were typed on standard input. The reading of the file stops at the first error encountered.

```
    #use "whatever.ml";;
```

Load in memory a bytecode object file (.cmo file) or library file (.cma file) produced by the batch compiler ocamlc.

```
    #load "file-name";;
```

Add the given directory to the list of directories searched for source and compiled files.

```
    #directory "dir-name";;
```

Change the current working directory.

```
    #cd "dir-name";;
```

Exit the toplevel loop and terminate the ocaml command.

```
    #quit;;
```

Source:

[OCaml Manual](http://caml.inria.fr/pub/docs/manual-ocaml-4.00/manual023.html#toc91)


#### UTOP Interactive Shell

![](images/utopshell.png)

Show UTOP help

```ocaml
    utop # #utop_help;;
    If you can't see the prompt properly try: #utop_prompt_simple                                                                                                                               utop defines the following directives:

    #utop_bindings   : list all the current key bindings
    #utop_macro      : display the currently recorded macro
    #topfind_log     : display messages recorded from findlib since the beginning of the session
    #topfind_verbose : enable/disable topfind verbosity
    For a complete description of utop, look at the utop(1) manual page.
```

Load a library

```ocaml
    utop # #use "topfind";;
    - : unit = ()                                                                                 Findlib has been successfully loaded. Additional directives:                                    #require "package";;      to load a package
      #list;;                   to list the available packages
      #camlp4o;;                to load camlp4 (standard syntax)
      #camlp4r;;                to load camlp4 (revised syntax)
      #predicates "p,q,...";;   to set these predicates
      Topfind.reset();;         to force that packages will be reloaded
      #thread;;                 to enable threads

    - : unit = ()
```

List Installed Packages

```ocaml
    utop # #list ;;
    archimedes          (version: 0.4.17) archimedes.graphics (version: 0.4.17)                                  archimedes.internals (version: 0.4.17)
    archimedes.top      (version: 0.4.17)
    batteries           (version: 2.3)
    bigarray            (version: [distributed with Ocaml])
    bin_prot            (version: 111.03.00)
    ...
```


Load Installed OCaml Packages
```
    utop # #require "batteries";;
    utop # #require "gnuplot";;
```

Show Key Bindings
```
utop # #utop_bindings;;
enter       : accept                        -> accept the current input.                                  escape      : cancel-search                 -> cancel search mode.                                        tab         : complete                      -> complete current input.
up          : history-prev                  -> go to the previous entry of the history.
down        : history-next                  -> go to the next entry of the history.
...
```

#### OCaml Browser

Browser OCaml Type Signatures.

```
$ ocamlfind browser -all
$ ocamlfind ocamlbrowser -package core
$ ocamlfind browser -package batterie
```

![](images/ocamlbrowser.png)

* http://caml.inria.fr/pub/docs/manual-ocaml/browser.html

#### Troubleshooting

Finding the version of Ocaml and Opam

```bash
$ ocaml -version
The OCaml toplevel, version 4.01.0

$ opam --version
1.2.2

$ ocamlc -v
The OCaml compiler, version 4.01.0
Standard library directory: /usr/lib/ocaml

$ ocamlopt -v
The OCaml native-code compiler, version 4.01.0
Standard library directory: /usr/lib/ocaml

$ utop -version
The universal toplevel for OCaml, version 1.17, compiled for OCaml version 4.01.0
```

Get information about compiled files:

```
$ ocamlobjinfo seq.cmo
File seq.cmo
Unit name: Seq
Interfaces imported:
    54ba2685e6ed154753718e9c8becb28b    String
    6e0efdddf4b33e30be4cc8ff056b56ff    Seq
    4836c254f0eacad92fbf67abc525fdda    Pervasives
Uses unsafe features: no
Force link: no


$ ocamlobjinfo seq.cma
File seq.cma
Force custom: no
Extra C object files:
Extra C options:
Extra dynamically-loaded libraries:
Unit name: Seq
Interfaces imported:
    54ba2685e6ed154753718e9c8becb28b    String
    4387952f7aad2695faf187cd3feeb5e5    Seq
    4836c254f0eacad92fbf67abc525fdda    Pervasives
Uses unsafe features: no
Force link: no


$ ocamlobjinfo seq.cmi
File seq.cmi
Unit name: Seq
Interfaces imported:
    4387952f7aad2695faf187cd3feeb5e5    Seq
    4836c254f0eacad92fbf67abc525fdda    Pervasives

```

#### Opam Package Manager

Opam Version

```
$ opam --version
1.2.2
```

List Installed Versions of OCaml compilers:

```
$ opam switch list
--     -- 3.11.2             Official 3.11.2 release
--     -- 3.12.1             Official 3.12.1 release
--     -- 4.00.0             Official 4.00.0 release
--     -- 4.00.1             Official 4.00.1 release
4.01.0  C 4.01.0             Official 4.01.0 release
--     -- 4.02.0             Official 4.02.0 release
4.02.1  I 4.02.1             Official 4.02.1 release
--     -- ocamljava-preview  
doc     I system             System compiler (4.00.1)
 # 111 more patched or experimental compilers, use '--all' to show
```

Switch to a ocaml version 4.02.1:

```
$ opam switch 4.02.1
# To setup the new switch in the current shell, you need to run:
eval `opam config env`

```

Search Packages

```
$ opam search core
# Existing packages for 4.01.0:
async               --  Monadic concurrency library
async_core          --  Monadic concurrency library
async_extended      --  Additional utilities for async
...
```

Install Packages

```
opam install lablgtk ocamlfind
```

Upgrade Packages

```
$ opam upgrade
```

Show all Installed Packages

```
$  ocamlfind list
archimedes          (version: 0.4.17)
archimedes.graphics (version: 0.4.17)
archimedes.internals (version: 0.4.17)
archimedes.top      (version: 0.4.17)
batteries           (version: 2.3)
bigarray            (version: [distributed with Ocaml])
bin_prot            (version: 111.03.00)
bin_prot.syntax     (version: 111.03.00)
bytes               (version: [OCaml strictly before 4.02])
camlp4              (version: [distributed with Ocaml])
camlp4.exceptiontracer (version: [distributed with Ocaml])
camlp4.extend       (version: [distributed with Ocaml])
camlp4.foldgenerator (version: [distributed with Ocaml])
..
```

Finding Opam Settings

```bash
$ opam config list
 # Global OPAM configuration variables

user                 tux
group                tux
make                 make
os                   linux
root                 /home/tux/.opam
prefix               /home/tux/.opam/system
lib                  /home/tux/.opam/system/lib
bin                  /home/tux/.opam/system/bin
sbin                 /home/tux/.opam/system/sbin
doc                  /home/tux/.opam/system/doc
stublibs             /home/tux/.opam/system/lib/stublibs
toplevel             /home/tux/.opam/system/lib/toplevel
man                  /home/tux/.opam/system/man
share                /home/tux/.opam/system/share
etc                  /home/tux/.opam/system/etc

 # Global variables from the environment

ocaml-version        4.01.0     # The version of the currently used OCaml compiler
opam-version         1.2.2      # The currently running OPAM version
compiler             system     # The name of the current OCaml compiler (may be more specific than the version, eg: "4.01.0+fp", or "system")
preinstalled         true       # Whether the compiler was preinstalled on the system, or installed by OPAM
switch               system     # The local name (alias) of the current switch
jobs                 1          # The number of parallel jobs set up in OPAM configuration
ocaml-native         true       # Whether the OCaml native compilers are available
ocaml-native-tools   false      # Whether the native ".opt" version of the OCaml toolchain is available
ocaml-native-dynlink true       # Whether native dynlink is available on this installation
arch                 i686       # The current arch, as returned by "uname -m"

 # Package variables ('opam config list PKG' to show)

PKG:name         # Name of the package
PKG:version      # Version of the package
PKG:depends      # Resolved direct dependencies of the package
PKG:installed    # Whether the package is installed
PKG:enable       # Takes the value "enable" or "disable" depending on whether the package is installed
PKG:pinned       # Whether the package is pinned
PKG:bin          # Binary directory for this package
PKG:sbin         # System binary directory for this package
PKG:lib          # Library directory for this package
PKG:man          # Man directory for this package
PKG:doc          # Doc directory for this package
PKG:share        # Share directory for this package
PKG:etc          # Etc directory for this package
PKG:build        # Directory where the package was built
PKG:hash         # Hash of the package archive

```

Filter Installed Packages

```
$ ocamlfind list | grep -i core
cohttp.lwt-core     (version: 0.17.1)
core                (version: 111.28.01)
core.syntax         (version: 109.32.00)
core.top            (version: 111.28.01)
core_kernel         (version: 111.28.00)
core_kernel.check_caml_modify (version: 111.28.00)
core_kernel.raise_without_backtrace (version: 111.28.00)
netmulticore        (version: 4.0.2)
num.core            (version: [internal])

$ ocamlfind list | grep -i batteries
batteries           (version: 2.3
```

#### Misc

Create .mli file from .ml files. The command bellow creates context.mli from context.ml
```
$ ocamlc -i -c context.ml > context.mli
```

Custom Top Level Interpreter with preload libraries:

```
$ ocamlmktop -custom -o mytoplevel graphics.cma -cclib -lX11
./mytoplevel
```

<!--
    ---------------------------------------------------------------
-->

## Basic Syntax

This section describes the OCaml native library and [Pervasives module](http://caml.inria.fr/pub/docs/manual-ocaml/libref/Pervasives.html).

Escape Characters:

```
Sequence  ASCII     Name
\\        \         Backlash
\"        "         Double Quote
\'        '         Double Quote
\n        LF        Line Feed
\r        CR        Carriage Return
\t        TAB       Horizontal tabulation
\b        BS        Backspace
'\ '      SPC       Space
\nnnn     nnn       Decimal Code of Acii Character
\xhhh     xhh       Hexadecimal Code of Ascii Character
```

```ocaml
    # '\\' ;;
    - : char = '\\'
    # '\ ' ;;
    - : char = ' '
    # '\"' ;;
    - : char = '"'
    # '\r' ;;
    - : char = '\r'
    # '\n' ;;
    - : char = '\n'
    # '\t' ;;
    - : char = '\t'
    # '\ ' ;;
    - : char = ' '
    # '\098' ;;
    - : char = 'b'
    # '\x21' ;;
    - : char = '!'
```


##### Primitive Types

```ocaml
$ ocaml
        OCaml version 4.01.0

    (* Boolean          *)
    (*------------------*)

    # true ;;
    - : bool = true
    # false ;;
    - : bool = false
    #

    (* Int              *)
    (*--------------------
     *      32 bits  signed int in 32 bits machines
     *      64 bits  signed int in 64 bits machines
     *)
    # 1000 ;;
    - : int = 1000

    (* Binary  int *)
    # 0b1000111011 ;;
    - : int = 571

    (* Hexadecimal int *)
    # 0xf0057a3 ;;
    - : int = 251680675

    (* octal int *)
    # 0o73456 ;;
    - : int = 30510

    (* Floats                   *)
    (* ------------------------ *)
    # 3.23e3 ;;
    - : float = 3230.
    # 34.2E-5 ;;
    - : float = 0.000342
    # 232. ;;
    - : float = 232.
    #

    (* Char                     *)
    (* ------------------------ *)

    # 'a' ;;
    - : char = 'a'
    # '\n' ;;
    - : char = '\n'
    # '\232' ;;
    - : char = '\232'


    (* String                         *)
    (* ------------------------------- *)

    # "Hello world OCaml" ;;
    - : string = "Hello world OCaml"
    #

    (* Tuples                            *)
    (* ----------------------------------*)
    # "Hello world OCaml" ;;
    - : string = "Hello world OCaml"
    # (10, 203.2322) ;;
    - : int * float = (10, 203.2322)
    # ("hello", 'w', 10 ) ;;
    - : string * char * int = ("hello", 'w', 10)
    #

    (* Lists / Are immutable data structure   *)
    (*----------------------------------------*)

    # [10; 20; 30; 40; 1; 3] ;;
    - : int list = [10; 20; 30; 40; 1; 3]

    # ["hello" ; "world" ; "ocaml" ; "F#" ; "Haskell" ] ;;
    - : string list = ["hello"; "world"; "ocaml"; "F#"; "Haskell"]
    #

    # [10. ; 2.323 ; 29.232 ; 100.597 ; -23.3 ] ;;
    - : float list = [10.; 2.323; 29.232; 100.597; -23.3]
    #

    # [(1, 'a') ; (2, 'b') ; (3, 'c')] ;;
    - : (int * char) list = [(1, 'a'); (2, 'b'); (3, 'c')]
    #

    (* Arrays / Mutable data structures  *)
    (* --------------------------------- *)

    # [|23; 100; 50; 80; 30; 50 |] ;;
    - : int array = [|23; 100; 50; 80; 30; 50|]

     # [|123.23; 10.23; 50.53; -80.23; 30.9734; 50.25 |] ;;
    - : float array = [|123.23; 10.23; 50.53; -80.23; 30.9734; 50.25|]

     # [|'o'; 'c'; 'a' ; 'm' ; 'l' |] ;;
    - : char array = [|'o'; 'c'; 'a'; 'm'; 'l'|]

    (* Unit / Represents side effects (Similar to Haskell IO ()*)
    (*-------------------------------------------------------- *)

    # ();;
    - : unit = ()
    #

    # print_string ;;
    - : string -> unit = <fun>

    # read_line ;;
    - : unit -> string = <fun>

    # let read_two_lines () = read_line (); read_line (); print_string "two lines" ;;
    val read_two_lines : unit -> unit = <fun>

    # read_two_lines () ;;
    2323
    5353535
    two lines- : unit = ()


    (* Option Type / Equivalent to Haskell Maybe *)

    # None ;;
    - : 'a option = None
    # Some 10 ;;
    - : int option = Some 10
    # Some 'a' ;;
    - : char option = Some 'a'
    # Some "OCaml" ;;
    - : string option = Some "OCaml"
    #



```



##### Operators


Float Functions must use +. /. -. *. operators since Ocaml doesn't support
operator overload.

Boolean Operators

```ocaml

    (* NOT                  *)
    (*----------------------*)

    # not ;;
    - : bool -> bool = <fun>
    #

    # not false ;;
    - : bool = true
    # not true ;;
    - : bool = false

    (* AND                  *)
    (*----------------------*)

    # (&&) ;;
    - : bool -> bool -> bool = <fun>

    # (&&) true false ;;
    - : bool = false
    #


    # true && true ;;
    - : bool = true
    # true && false ;;
    - : bool = false
    #

    (* OR                   *)
    (*----------------------*)

    # (||) ;;
    - : bool -> bool -> bool = <fun>

    # (||) false true ;;
    - : bool = true
    #

    # true || true ;;
    - : bool = true
    # true || false ;;
    - : bool = true
    # false || false ;;
    - : bool = false
    #
```

Comparison
```ocaml

    (* Equality *)
    # (=) ;;
    - : 'a -> 'a -> bool = <fun>
    # (==) ;;
    - : 'a -> 'a -> bool = <fun>

    (* Inequality *)
    # (<>) ;;
    - : 'a -> 'a -> bool = <fun>
    # (!=) ;;
    - : 'a -> 'a -> bool = <fun>
    #


    # (<) ;;
    - : 'a -> 'a -> bool = <fun>
    # (<=) ;;
    - : 'a -> 'a -> bool = <fun>
    # (>=) ;;
    - : 'a -> 'a -> bool = <fun>
    # (>) ;;
    - : 'a -> 'a -> bool = <fun>
    #

    # max ;;
    - : 'a -> 'a -> 'a = <fun>
    # min ;;
    - : 'a -> 'a -> 'a = <fun>


    # compare ;;
    - : 'a -> 'a -> int = <fun>
    #

    # 2 <> 4 ;;
    - : bool = true
    # 2 <> 2 ;;
    - : bool = false
    #
    # 3 != 4 ;;
    - : bool = true
    # 3 != 3 ;;
    - : bool = false
    #


    # max 3 100 ;;
    - : int = 100
    # min 2.323 100.33 ;;
    - : float = 2.323
    #

```


```ocaml
     # (+) ;;
    - : int -> int -> int = <fun>

     # (-) ;;
    - : int -> int -> int = <fun>

     # (+.) ;;
    - : float -> float -> float = <fun>

     (-.) ;;
    - : float -> float -> float = <fun>


    (* Interger Operators *)
    (*--------------------*)
    # 23 + 234 ;;
    - : int = 257
    # 1000 / 4 ;;
    - : int = 250
    #

    # 1005 mod 10 ;;
    - : int = 5
    #

    # abs (-10) ;;
    - : int = 10
    # abs 10 ;;
    - : int = 10
    #
    # pred 100 ;;
    - : int = 99
    # pred 99 ;;
    - : int = 98
    # succ 100 ;;
    - : int = 101
    # succ 101 ;;
    - : int = 102
    #

    (* Float Point *)
    (*-------------*)

    # 100. +. 23.23 ;;
    - : float = 123.23
    #
    # 0.545 *. 100. ;;
    - : float = 54.5000000000000071
    #
    # 1000. /. 4. ;;
    - : float = 250.
    #
    # 1000.0 -. 4.0 ;;
    - : float = 996.
    #

    (* Pow function 2^4 = 16 / only works for float points *)
    # 2.0 ** 4. ;;
    - : float = 16.
    #
    # ( **) ;;
    - : float -> float -> float = <fun>
    #
```

##### Variable Declaration

```ocaml
> let x = 2 ;;
val x : int = 2

(* Declaration with Type *)
> let a : float  = 2.323 ;;
val a : float = 2.323

(* Characters and String *)

> 'a' ;;
- : char = 'a'
> "hello world" ;;
- : string = "hello world"

(* Lists *)

>  [1, 2, 3, 4 , 5, 6] ;;
- : (int * int * int * int * int * int) list = [(1, 2, 3, 4, 5, 6)]
>

>  [2.323, 534.23, 83.434, 54.3323 ] ;;
- : (float * float * float * float) list = [(2.323, 534.23, 83.434, 54.3323)]
>

>  ["hello", "world", "ocaml", "amazing" ] ;;
- : (string * string * string * string) list = [("hello", "world", "ocaml", "amazing")]
>

(* Tuples *)
>  (90, 100 ) ;;
- : int * int = (90, 100)

>  (232, 23.232, "hello ", 'c' ) ;;
- : int * float * string * char = (232, 23.232, "hello ", 'c')
>


>  ("hello", 23.23 ) ;;
- : string * float = ("hello", 23.23)

```



##### Polymorphic Functions

```ocaml
> let id = fun x -> x ;;
val id : 'a -> 'a = <fun>

>  id 10.23 ;;
- : float = 10.23
>  id 100  ;;
- : int = 100
>  id "Hello world" ;;
- : string = "Hello world"
>

```

##### Number Formats


* Int   -  Default Interger format 31 bits signed int on 32 bits machine and 63 bits on a 64 bits machine
* [Int32](http://caml.inria.fr/pub/docs/manual-ocaml/libref/Int32.html) - 32 bits signed int
* [Int64](http://caml.inria.fr/pub/docs/manual-ocaml/libref/Int64.html) - 63 bits signed int

* Float -  IEEE.754 - 64 bits double precision float point numbers.

* [Num](http://caml.inria.fr/pub/docs/manual-ocaml/libref/Num.html) - Arbitrary Precision Integer
* [Big_Int](http://caml.inria.fr/pub/docs/manual-ocaml/libref/Big_int.html) - Arbitrary Precision Integer

```ocaml

(* 31 bits signed int, since this machine has 32 bits word lenght*)

    # Pervasives.min_int ;;
    - : int = -1073741824
    # Pervasives.max_int ;;
    - : int = 1073741823

(* 32 bits signed int *)

    # Int32.max_int ;;
    - : int32 = 2147483647l
    # Int32.min_int ;;
    - : int32 = -2147483648l

(* 64 bits signed int *)
    # Int64.min_int ;;
    - : int64 = -9223372036854775808L
    # Int64.max_int ;;
    - : int64 = 9223372036854775807L

(* IEEE.754 - 64 bits double precision - float point *)

    # Pervasives.min_float ;;
    - : float = 2.22507385850720138e-308
    # Pervasives.max_float ;;
    - : float = 1.79769313486231571e+308
```

Numeric Literals

```ocaml
(* Default - Pervasives 31 bit signed int (32 bits machine) *)

    # 23213 ;;
    - : int = 23213

    # 0xf43aaec ;;
    - : int = 256092908

    # 0b10001110011 ;;
    - : int = 1139

(* 32 bits signed int *)

    # 23213l ;;
    - : int32 = 23213l

    # 0xf4123l ;;
    - : int32 = 999715l

    # 0b1111100011110011l ;;
    - : int32 = 63731l

(* 64 bits signed int *)

    # 1000000L ;;
    - : int64 = 1000000L

     # 0xff2562abcL ;;
    - : int64 = 68490242748L

    # 0b11111000111100110011111101L ;;
    - : int64 = 65260797L
```

Number Conversion / Type Casting

```ocaml

    # Int32.of_int 10002373 ;;
    - : int32 = 10002373l
    # Int32.of_float 232322.323 ;;
    - : int32 = 232322l
    # Int32.of_string "98232376" ;;
    - : int32 = 98232376l

    # Int32.to_int 100033l ;;
    - : int = 100033


    # Int64.of_int 100 ;;
    - : int64 = 100L
    # Int64.of_float 1000239823.2323 ;;
    - : int64 = 1000239823L
    # Int64.of_string "34234912" ;;
    - : int64 = 34234912L

    # Int64.to_int 2323884L ;;
    - : int = 2323884


     # int_of_float (-1235.34083402321) ;;
    - : int = -1235
    # Pervasives.int_of_float (-1235.34083402321) ;;
    - : int = -1235
     # int_of_string "9123" ;;
    - : int = 9123

    # float_of_int 1000 ;;
    - : float = 1000.
```

Math Operations. In Ocaml the same operator cannot be used for more than one type, so to add ints (+) must be used, to add floats (+.), to add Int32, (Int32.add) and to add Int64, (Int64.add).

```ocaml

    # (+) ;;
    - : int -> int -> int = <fun>

    # (+.) ;;
    - : float -> float -> float = <fun>

    # Int32.add ;;
    - : int32 -> int32 -> int32 = <fun>

    # Int64.add ;;
    - : int64 -> int64 -> int64 = <fun>

    # Big_int.add_big_int ;;
    - : Big_int.big_int -> Big_int.big_int -> Big_int.big_int = <fun>

    # 23423 + 1212 ;;
    - : int = 24635

    # 32.34 +. 232.22 ;;
    - : float = 264.56

    # Int32.add 2323l 6023l ;;
    - : int32 = 8346l

    # Int64.add 232L 3434L ;;
    - : int64 = 3666L

    (* Simplifying Number Operators *)

    # let fun1 x y = 10 * x - 4 * y ;;
    val fun1 : int -> int -> int = <fun>

    # fun1 45 23 ;;
    - : int = 358

    # fun1 45L 23L ;;
    Error: This expression has type int64 but an expression was expected of type int

    (* Defining the function to 64 bits *)

     # let fun1L x y =
                 let (-) = Int64.sub in
                 let ( * ) = Int64.mul in
                 10L * x - 4L * y
        ;;val fun1L : int64 -> int64 -> int64 = <fun>

    # fun1L 45L 23L ;;
    - : int64 = 358L

    (* Trick Creating an Operator Module *)

    # module OP =
    struct
        module FL =
        struct
            let (+) = (+.)
            let (-) = (-.)
            let ( * ) = ( *. )
            let (/) = (/.)
        end

        module I32 =
        struct
            let (+) = Int32.add
            let (-) = Int32.sub
            let (/) = Int32.div
            let ( * ) = Int32.mul
        end

        module I64 =
        struct
            let (+) = Int64.add
            let (-) = Int64.sub
            let ( * ) = Int64.mul
            let (/) = Int64.div
        end

    end
      ;;
    module OP :
      sig
        module FL :
          sig
            val ( + ) : float -> float -> float
            val ( - ) : float -> float -> float
            val ( * ) : float -> float -> float
            val ( / ) : float -> float -> float
          end
        module I32 :
          sig
            val ( + ) : int32 -> int32 -> int32
            val ( - ) : int32 -> int32 -> int32
            val ( / ) : int32 -> int32 -> int32
            val ( * ) : int32 -> int32 -> int32
          end
        module I64 :
          sig
            val ( + ) : int64 -> int64 -> int64
            val ( - ) : int64 -> int64 -> int64
            val ( * ) : int64 -> int64 -> int64
            val ( / ) : int64 -> int64 -> int64
          end
      end
    #

(* Defined for int *)

    # let fun1 x y = 10 * x - 4 * y ;;
    val fun1 : int -> int -> int = <fun>

    # fun1 100 20 ;;
    - : int = 920

(* Defined for Int32 *)

    # let fun1_int32 x y  =
        let open OP.I32 in
        10l * x - 4l * y
    ;;
    val fun1_int32 : int32 -> int32 -> int32 = <fun>

    # fun1_int32 100l 20l ;;
    - : int32 = 920l

    (* OR for short *)
    
    # let fun1_int32_ x y  = OP.I32.(10l  * x - 4l * y) ;;
    val fun1_int32_ : int32 -> int32 -> int32 = <fun>                               
    
    # fun1_int32_ 100l 20l ;;
    - : int32 = 920l  

(* Defined for Int64 *)

    # let fun1_int64 x y  =
            let open OP.I64 in
            10L * x - 4L * y
        ;;
    val fun1_int64 : int64 -> int64 -> int64 = <fun>

    # fun1_int64 100L 20L ;;
    - : int64 = 920L

    (* OR *)
    
    # let fun1_int64_ x y = OP.I64.(10L * x - 4L * y) ;;
    val fun1_int64_ : int64 -> int64 -> int64 = <fun>

(* Defined for Float *)

    # let fun1_float x y  =
                let open OP.FL in
                10. * x - 4. * y
            ;;
    val fun1_float : float -> float -> float = <fun>

    # fun1_float 100. 20. ;;
    - : float = 920.

    (* OR *)
    
    # let fun1_float_ x y  = OP.FL.(10.  * x - 4. * y) ;;
    val fun1_float_ : float -> float -> float = <fun>
    
    # fun1_float_ 100. 20. ;;
    - : float = 920.    

```


##### Math / Float Functions

Documentation:

OCaml's floating-point complies with the [IEEE 754 standard](http://en.wikipedia.org/wiki/IEEE_floating_point),  double precision (64 bits) numbers.

* http://caml.inria.fr/pub/docs/manual-ocaml/libref/Pervasives.html

```ocaml

(* Operators *)

    # (+.) ;;
    - : float -> float -> float = <fun>
    # (-.) ;;
    - : float -> float -> float = <fun>
    # (/.) ;;
    - : float -> float -> float = <fun>
    # ( *. ) ;;
    - : float -> float -> float = <fun>

     (* Power operator/ Exponentiation *)
     # ( ** ) ;;
    - : float -> float -> float = <fun>

    # List.map ( (+.) 2.323) [10.23; 3.4; 30. ; 12. ] ;;
    - : float list = [12.553; 5.723; 32.323; 14.323]

    # List.map ( ( *. ) 3.) [10.23; 3.4; 30. ; 12. ] ;;
    - : float list = [30.69; 10.2; 90.; 36.]

    # List.map (fun x -> 2. ** x ) [1. ; 2.; 3.; 4.; 5.; 6.; 7.; 8.] ;;
    - : float list = [2.; 4.; 8.; 16.; 32.; 64.; 128.; 256.]

(* Absolute Value *)

    # abs_float ;;
    - : float -> float = <fun>
    #

(* Square Root *)

    # sqrt ;;
    - : float -> float = <fun>

(* Trigonometric *)

    # sin ;;
    - : float -> float = <fun>
    # cos ;;
    - : float -> float = <fun>
    # tan ;;
    - : float -> float = <fun>
    # atan ;;
    - : float -> float = <fun>
    # atan2 ;;
    - : float -> float -> float = <fun>
    #
    # acos ;;
    - : float -> float = <fun>
    # asin ;;
    - : float -> float = <fun>
    #

(* Hyperbolic Functions *)
    # cosh ;;
    - : float -> float = <fun>
    # sinh ;;
    - : float -> float = <fun>
    # tanh ;;
    - : float -> float = <fun>
    #


(* Logarithm and exp *)
    # log ;;
    - : float -> float = <fun>
    # log10 ;;
    - : float -> float = <fun>
    # exp ;;
    - : float -> float = <fun>

    (* exp x -. 1.0, *)
    # expm1 ;;
    - : float -> float = <fun>

    (*  log(1.0 +. x)  *)
    # log1p ;;
- : float -> float = <fun>


(* Remove Decimal Part *)
    # floor ;;
    - : float -> float = <fun>
    # ceil ;;
    - : float -> float = <fun>
    # truncate ;;
    - : float -> int = <fun>
    #
    # int_of_float ;;
    - : float -> int = <fun>
    #


(* Float Constants *)

    # infinity ;;
    - : float = infinity
    #
    # neg_infinity ;;
    - : float = neg_infinity
    #

    # max_float ;;
    - : float = 1.79769313486231571e+308
    # min_float ;;
    - : float = 2.22507385850720138e-308


    # nan ;;
    - : float = nan
    #

    # 1. /. 0. ;;
    - : float = infinity
    #
    # -1. /. 0. ;;
    - : float = neg_infinity
    #
```

##### Function Declaration

```ocaml

> let x = 34 ;;
val x : int = 34

> x ;;
- : int = 34

> let x = 10 in
  let y = 20 in
  let z = x*y in
  z - x - y
;;
- : int = 170

> let x = 10.25 in
  let y = 30.   in
  x *. y
;;
- : float = 307.5

>  let f x = 10 * x + 4 ;;
val f : int -> int = <fun>

>  f 4 ;;
- : int = 44

>  f 5 ;;
- : int = 54
>

> let f (x, y) = x +  y ;;
val f : int * int -> int = <fun>

> f (2, 5) ;;
- : int = 7

> f (10, 5) ;;
- : int = 15


> let add_floats x y = x +. y ;;
val add_floats : float -> float -> float = <fun>

> add_floats 10. 50.343 ;;
- : float = 60.343


> let a_complex_function x y =
    let a = 10 * x in
    let b = 5 * y + x in
    a + b
;;
val a_complex_function : int -> int -> int = <fun>

(*
    a_complex_function 2 3
        a = 10 * x -->  a = 10*2 = 20
        b = 5 * 3  -->  b = 5*3 + 2 = 17
        a + b      -->  20 + 17 = 37
*)
> a_complex_function 2 3 ;;
- : int = 37


(* Function Inside functions *)

> let func1 x y =
    let ft1 x y = 10*x + y in
    let ft2 x y z = x + y - 4 * z in
    let ft3 x y = x - y in
    let z = 10 in
    (ft1 x y) + (ft2 x y z) - (ft3 x y)
;;

> func1 4 5 ;;
- : int = 15

> func1 14 5 ;;
- : int = 115

> func1 20 (-10) ;;
- : int = 130

(* Returning More than one value *)

> let g x y =  (10* x, x + y) ;;

> g 4 5 ;;
- : int * int = (40, 9)
```

Declaring Functions with type signature.

```ocaml
> let func1 (x:int) (y:float) : float = (float_of_int x) +. y ;;
val func1 : int -> float -> float = <fun>

> func1 10 2.334 ;;
- : float = 12.334


> let func2 (xy: (int * int)) : int  = (fst xy) + (snd xy) ;;
val func2 : int * int -> int = <fun

> func2 (5, 6) ;;
- : int = 11

> let show (x:float) = Printf.printf "%.3f" x ;;
val show : float -> unit = <fun>

> show 3.232 ;;
3.232- : unit = ()


> let showxy (x, y) : unit = Printf.printf "%.3f\n" (x +. y) ;;
val showxy : float * float -> unit = <fun

> showxy (32.323, 100.232) ;;
132.555
- : unit = ()


> let double_list (list_of_floats : float list) : float list =
    List.map (fun x -> 2.0 *. x) list_of_floats ;;
val double_list : float list -> float list = <fun>

> double_list [1. ; 2. ; 3. ; 4. ; 5. ] ;;
- : float list = [2.; 4.; 6.; 8.; 10.]

```

Declaring function type with anonymous functions.

```ocaml
> let func2' : (int * int) -> int = fun (a, b) -> a + b ;;
val func2' : int * int -> int = <fun>

> func2' (5, 6) ;;
- : int = 11

> let showxy' : float * float -> unit = fun (x, y) ->
   Printf.printf "%.3f\n" (x +. y) ;;

> showxy' (23.3, 5.342021321) ;;
28.642
- : unit = ()
```

Declaration functions that takes another function as argument

```ocaml
> let apply_to_fst f (x, y) = (f x, y) ;;
val apply_to_fst : ('a -> 'b) -> 'a * 'c -> 'b * 'c = <fun>

let apply_to_fst2 : ('a -> 'c) -> 'a * 'b  -> 'c * 'b =
    fun f (x, y) ->  (f x, y)

> let f x = x + 10 ;;
val f : int -> int = <fun>

> apply_to_fst f (10, "hello world") ;;
- : int * string = (20, "hello world")

> apply_to_fst2 f (10, "hello world") ;;
- : int * string = (20, "hello world")
```

Declaring functions with custom types

```ocaml
> type tuple_of_int = int * int ;;
type tuple_of_int = int * int

> type func_float_to_string = float -> string ;;
type func_float_to_string = float -> string


> type func_tuple_of_ints_to_float = int * int -> float ;;
type func_tuple_of_ints_to_float = int * int -> float

> let x: tuple_of_int = (10, 4) ;;
val x : tuple_of_int = (10, 4)

> let f : func_float_to_string = fun x -> "x = " ^ (string_of_float x) ;;
val f : func_float_to_string = <fun>

> f 2.23 ;;
- : string = "x = 2.23"

> let funct : tuple_of_int -> int = fun (x, y) -> x + y ;;
val funct : tuple_of_int -> int = <fun>

> funct (10, 100) ;;
- : int = 11

> let fxy : func_tuple_of_ints_to_float =
    fun (x, y) -> 10.4 *. (float_of_int x) -. 3.5 *. (float_of_int y) ;;
val fxy : func_tuple_of_ints_to_float = <fun>

> fxy ;;
- : func_tuple_of_ints_to_float = <fun>

> fxy (10, 5) ;;
- : float = 86.5


> type list_of_float = float list ;;
type list_of_float = float list

> let double (xs: list_of_float) : list_of_float = List.map (fun x -> 2.0 *. x) xs ;;
val double : list_of_float -> list_of_float = <fun>

> double [1. ; 2. ; 3. ; 4. ; 5. ] ;;
- : list_of_float = [2.; 4.; 6.; 8.; 10.]

> double ;;
- : list_of_float -> list_of_float = <fun>

> let double2 : list_of_float -> list_of_float =
  fun xs -> List.map (fun x -> 2.0 *. x) xs ;;
val double2 : list_of_float -> list_of_float = <fun>

> double2 ;;
- : list_of_float -> list_of_float = <fun>

> double2 [1. ; 2. ; 3. ; 4. ; 5. ] ;;
- : list_of_float = [2.; 4.; 6.; 8.; 10.]
```

Functions with Named Parameters

```ocaml
> let f1 ~x ~y = 10* x - y ;;
val f1 : x:int -> y:int -> int = <fun>

> f1 ;;
- : x:int -> y:int -> int = <fun>

> f1 4 5 ;;
- : int = 35

> f1 20 15 ;;
- : int = 185

(* Currying Funcctions with named parameters *)
> f1 20 ;;
Error: The function applied to this argument has type x:int -> y:int -> int                      This argument cannot be applied without label

> f1 ~x:20 ;;
- : y:int -> int = <fun>

> let f1_20 = f1 ~x:20 ;;
val f1_20 : y:int -> int = <fun>

> f1_20 10 ;;
- : int = 190

> f1_20 40 ;;
- : int = 160

> List.map f1_20 [1; 2; 10; 20; 30] ;;
Error: This expression has type y:int -> int but an expression was expected of type 'a -> 'b

> List.map (fun y -> f1_20 y) [1; 2; 10; 20; 30] ;;
- : int list = [199; 198; 190; 180; 170]

> let show_msg x ~msg () = Printf.printf "%s = %d" msg x ;;
val show_msg : int -> msg:string -> unit -> unit = <fun>

> show_msg 2 "Hello world" () ;;
Hello world = 2- : unit = ()

> show_msg 20  ;;
- : msg:string -> unit -> unit = <fun>

> let f = show_msg 20 ;;
val f : msg:string -> unit -> unit = <fun>

> f "x" ();;
x = 20- : unit = ()

> f "y" () ;;
y = 20- : unit = ()

> List.iter f ["x" ; "y" ; "z" ; "w"] ;;
Error: This expression has type msg:string -> unit -> unit
but an expression was expected of type 'a -> unit

> List.iter (fun msg -> f msg ()) ["x" ; "y" ; "z" ; "w"] ;;
x = 20y = 20z = 20w = 20- : unit = ()
```

Functions with Optional Parameters:

```ocaml

(*
    If the parameter x is not given, x will be set to 100
*)
> let f ?(x = 100)  y = 10*x - 5*y ;;
val add : ?x:int -> int -> int = <fun>

> f ;;
- : ?x:int -> int -> int = <fun>

(*
    f 20 ==> (10*x - 5*y) 100 20
             (10 * 10 - 5 * 20 )
             (1000 - 100)
             900
 *)
> f 20 ;;
- : int = 900


(*
    f 60 ==> (10*x - 5*y) 100 60
             (10*100 - 5*60)
             (1000 - 300)
             700
*)
> f 60 ;;
- : int = 700

> List.map f [10; 20; 30; 40; 50] ;;
- : int list = [950; 900; 850; 800; 750]


> f ~x:40 20 ;;
- : int = 300


> f ~x:50 20 ;;
- : int = 400

> f ~x:40 ;;
- : int -> int = <fun>

> let f_x40  = f ~x:40 ;;
val f_x40 : int -> int = <fun>

> List.map f_x40 [10; 20; 30; 40; 50] ;;
- : int list = [350; 300; 250; 200; 150]

> List.map (f ~x: 40) [10; 20; 30; 40; 50] ;;
- : int list = [350; 300; 250; 200; 150]


> let rectangle_area ?(width = 30) ~height = width*height ;;
val rectangle_area : ?width:int -> height:int -> int = <fun>

> rectangle_area 20 ;;
- : int = 600

> rectangle_area 30 ;;
- : int = 900

> List.map rectangle_area [10; 20; 30; 40; 50] ;;
Error: This expression has type ?width:int -> height:int -> int
but an expression was expected of type 'a -> 'b

> List.map (fun h -> rectangle_area h ) [10; 20; 30; 40; 50] ;;
- : int list = [300; 600; 900; 1200; 1500]

> rectangle_area ~width: 200 ;;
- : height:int -> int = <fun>

>: rectangle_area ~width: 200 ~height: 20 ;;
- : int = 4000

> List.map (fun h -> rectangle_area ~height:h ~width:30 )
[10; 20; 30; 40; 50] ;;
- : int list = [300; 600; 900; 1200; 1500]

```

##### Function Composition

Operators:

```ocaml
(* Composition Operator *)
let (<<) f g x = f (g x) ;;

(* F# Piping Composition Operator *)
let (>>) f g x = g( f x) ;;

(* F# Piping Operator *)
let (|>) x f  = f x ;;

let (<|) f x = f x ;;

```

Example: Composition Operator

```ocaml
>  let f1 x = 10 + x ;;
val f1 : int -> int = <fun>

>  let f2 x = 2 * x ;;
val f2 : int -> int = <fun>

>  let f3 x = x - 8 ;;
val f3 : int -> int = <fun>

>  f1( f2 (f3 10)) ;;
- : int = 14
>

>  (f1 << f2 << f3) 10 ;;
- : int = 14
>


>  let f = f1 << f2 << f3 ;;
val f : int -> int = <fun>
>  f 10 ;;
- : int = 14
>  f 20 ;;
- : int = 34
>

```

Example: Pipe Operators

```ocaml
>
  10 |> f3 |> f2 |> f1 ;;
- : int = 14

>
  10 |> f3 ;;
- : int = 2

>  2 |> f2 ;;
- : int = 4

>  4 |> f1 ;;
- : int = 14
>


> 10 |> (f3 >> f2 >> f1) ;;
- : int = 14

> f3 >> f2 >> f1  <| 10 ;;
- : int = 14


> let f = f3 >> f2 >> f1 ;;
val f : int -> int = <fun>

> f 10 ;;
- : int = 14
```

##### Lambda Functions/ Anonymous Functions

```ocaml
fun x -> x+1                       : int -> int
fun x -> x +. 1.0                  : float -> float
fun x -> x ^ x                     : string -> string
fun (x,y) -> x + y                 : (int * int) -> int
fun (x,y) -> (y,x)                 : ('a*'b) -> ('b*'a)
fun x y -> (x,y)                   : 'a -> 'b -> ('a*'b)
fun x y z -> (x,y,z)               : 'a -> 'b -> 'c -> ('a*'b*'c)
```


##### Control Structures

Conditional Expressions

```ocaml
let test x =
if x > 0
then print_string "x is positive"
else print_string "x is negative"
;;


val test : int -> unit = <fun>

> test 10 ;;
x is positive- : unit = ()

> test (-10) ;;
x is negative- : unit = ()
```

For Loop:

```ocaml
> for i = 0 to 5 do Printf.printf "= %i\n" i  done ;;
= 0
= 1
= 2
= 3
= 4
= 5
- : unit = ()

> for i = 10 downto 1 do
  Printf.printf "%d .. " i
done;
  ;;
10 .. 9 .. 8 .. 7 .. 6 .. 5 .. 4 .. 3 .. 2 .. 1 .. - : unit = ()
```

While Loop:

```ocaml
> let j = ref 5 ;;
val j : int ref = {contents = 5}

> while !j > 0 do Printf.printf "x = %d\n" !j ; j := !j -1 done ;;
x = 5
x = 4
x = 3
x = 2
x = 1
- : unit = ()

```

Infinite While Loop

```
utop # while true do
    print_string "hello world / hit return to continue ";
    read_line ();
done;
;;

Characters 77-90:                                                                       Warning 10: this expression should have type unit.                                      Characters 77-90:
Warning 10: this expression should have type unit.
hello world / hit return to continue
hello world / hit return to continue
hello world / hit return to continue
hello world / hit return to continue ^CInterrupted.

utop # let mainloop () =
      while true do
          print_string "hello world / hit return to continue ";
          read_line ();
      done;

  ;;
Warning 10: this expression should have type unit.
val mainloop : unit -> unit = <fun>

utop # mainloop()   ;;
hello world / hit return to continue
hello world / hit return to continue
hello world / hit return to continue
hello world / hit return to continue
hello world / hit return to continue
```


<!--
    ---------------------------------------------------------------
-->

## Standard Library Modules

* Documentation: http://caml.inria.fr/pub/docs/manual-ocaml/libref/

### List

The module List has almost all functions to process lists which are immutable data structures.

* http://caml.inria.fr/pub/docs/manual-ocaml/libref/List.html

Cons and Nil
```ocaml
[] : 'a list                    (*     Nil *)
:: : 'a -> 'a list -> 'a list   (* :: Cons *)
```

Example:
```ocaml
>   23::[] ;;
- : int list = [23]
>  12::23::[] ;;
- : int list = [12; 23]
>  34::12::23::[] ;;
- : int list = [34; 12; 23]

>  1::[2; 34; 55] ;;
- : int list = [1; 2; 34; 55]
>

>  []::[] ;;
- : 'a list list = [[]]
>  []::[]::[] ;;
- : 'a list list = [[]; []]
```

List Functions

```ocaml

(* Concatenate Lists *)
> (@) ;;
- : 'a list -> 'a list -> 'a list = <fun>
> [1; 2; 3; 4; 5] @ [5; 6; 8] ;;
- : int list = [1; 2; 3; 4; 5; 5; 6; 8]
>

(* First element of a list / head of the list *)
> List.hd [1 ; 2; 3; 4 ; 6 ; 9] ;;
- : int = 1

(* Remove first Element of a list/ tail of the list *)
  List.tl [1 ; 2; 3; 4; 5; 6; 9] ;;
- : int list = [2; 3; 4; 5; 6; 9]

>  sum [1 ; 2; 3; 4; 5; 6; 7] ;;
- : int = 28

(* Reverse List *)
>  List.rev [1; 2; 3; 4] ;;
- : int list = [4; 3; 2; 1]

(* Pick Element *)
> List.nth [0; 1; 2; 3; 4 ] 0 ;;
- : int = 0
> List.nth [0; 1; 2; 3; 4 ] 1 ;;
- : int = 1
> List.nth [0; 1; 2; 3; 4 ] 2 ;;
- : int = 2
> List.nth [0; 1; 2; 3; 4 ] 3 ;;
- : int = 3

(* List Lenght *)
> List.length [0; 1; 2; 3; 4 ] ;;
- : int = 5
> List.length [0; 1; 2] ;;
- : int = 3

(* Combine - Equivalent to Haskell zip *)
> List.combine ;;
- : 'a list -> 'b list -> ('a * 'b) list = <fun>

> List.combine [1; 2; 3] ['a'; 'b'; 'c' ] ;;
- : (int * char) list = [(1, 'a'); (2, 'b'); (3, 'c')]

> List.combine [1; 2; 3; 5 ] ['a'; 'b'; 'c' ] ;;
Exception: Invalid_argument "List.combine".
>

(* Split - Equivalent to Haskell unzip *)

> List.split  [(1, 'a'); (2, 'b'); (3, 'c')] ;;
- : int list * char list = ([1; 2; 3], ['a'; 'b'; 'c'])
>

>  List.split  [(1, 'a'); (2, 'b'); (3, 'c')] ;;
- : int list * char list = ([1; 2; 3], ['a'; 'b'; 'c'])
>


(* Equivalent to Haskell (++) *)

> List.append [0; 2; 3; 5; 6 ] [1; 2; 6] ;;
- : int list = [0; 2; 3; 5; 6; 1; 2; 6]

(* Find *)

> List.find  (fun x -> x < 10) [1; 2 ; 3; 45] ;;
- : int = 1
> List.find  (fun x -> x > 10) [1; 2 ; 3; 45] ;;
- : int = 45
>
> List.find  (fun x -> x > 10) [1; 2 ; 3] ;;
Exception: Not_found.
>

> List.find_all  (fun x -> x > 10) [-10; 20 ; 3; 12; 100; 4; 35] ;;
- : int list = [20; 12; 100; 35]

> List.exists ;;
- : ('a -> bool) -> 'a list -> bool = <fun>
> List.exists ((==) 10) [1; 2; 30; 50 ; 3];;
- : bool = false
> List.exists ((==) 10) [1; 2; 30; 10 ; 50 ; 3];;
- : bool = true

> List.for_all2 (>=) [1;2;3] [2;3;4];;
- : bool = false
> List.exists2 (<) [1;2;3] [1;2;3] ;;
- : bool = false
>  List.exists2 (<) [1;2;0] [1;2;3] ;;
- : bool = true
>  List.exists2 (<) [1;2;0] [1;2];;
Exception: Invalid_argument "List.exists2".
>

> List.assoc 3 [(0, "a"); (1, "b"); (2, "c"); (3, "d")] ;;
- : string = "d"
> List.assoc 0 [(0, "a"); (1, "b"); (2, "c"); (3, "d")] ;;
- : string = "a"
> List.assoc 5 [(0, "a"); (1, "b"); (2, "c"); (3, "d")] ;;
Exception: Not_found.
>

> List.mem_assoc 5 [(0, "a"); (1, "b"); (2, "c"); (3, "d")] ;;
- : bool = false
> List.mem_assoc 3 [(0, "a"); (1, "b"); (2, "c"); (3, "d")] ;;
- : bool = true
> List.mem_assoc 2 [(0, "a"); (1, "b"); (2, "c"); (3, "d")] ;;
- : bool = true
>

> List.remove_assoc 0 [0,"a"; 1,"b"; 2,"c"; 3,"d"];;
- : (int * string) list = [(1, "b"); (2, "c"); (3, "d")]



(* Partition *)

> List.partition ;;
- : ('a -> bool) -> 'a list -> 'a list * 'a list = <fun>
>
> List.partition   (fun x -> x > 10) [-10; 20 ; 3; 12; 100; 4; 35] ;;
- : int list * int list = ([20; 12; 100; 35], [-10; 3; 4])


(* Flatten  *)

> List.flatten ;;
- : 'a list list -> 'a list = <fun>
>

> List.flatten [[1]; [2; 4] ; [6; 90; 100] ; []] ;;
- : int list = [1; 2; 4; 6; 90; 100]
>


(*      MAP         *)
(*------------------*)

> List.map ;;
- : ('a -> 'b) -> 'a list -> 'b list = <fun>
>
> List.map (( *) 10 )  [-10 ; 20 ; 5 ; 50; 100; 3 ] ;;
- : int list = [-100; 200; 50; 500; 1000; 30]

> List.map ((+) 10) [-10 ; 20 ; 5 ; 50; 100; 3 ] ;;
- : int list = [0; 30; 15; 60; 110; 13]
>

> let f x = 10 * x - 5 ;;
val f : int -> int = <fun>
> List.map f  [-10 ; 20 ; 5 ; 50; 100; 3 ] ;;
- : int list = [-105; 195; 45; 495; 995; 25]
>

> List.map (fun x -> 10.5 *. x -. 4. ) [2. ; 3. ; 5. ; 10. ; 20. ];;
- : float list = [17.; 27.5; 48.5; 101.; 206.]
>

(*      MAPI        *)
(*------------------*)
> List.mapi ;;
- : (int -> 'a -> 'b) -> 'a list -> 'b list = <fun>

> List.mapi (fun  index element -> index, element) [17.; 27.5; 48.5; 101.; 206.] ;;
- : (int * float) list = [(0, 17.); (1, 27.5); (2, 48.5); (3, 101.); (4, 206.)]


(* MAP2 / Haskell zipWith *)

> List.map2 ;;
- : ('a -> 'b -> 'c) -> 'a list -> 'b list -> 'c list = <fun>
>

> List.map2 (+) [10; 20; 30; 100] [15; 35; 25; 80] ;;
- : int list = [25; 55; 55; 180]

> List.map2 ( *) [10; 20; 30; 100] [15; 35; 25; 80] ;;
Warning 2: this is not the end of a comment.
- : int list = [150; 700; 750; 8000]

> List.map2 (fun x y -> (x, y)) [10; 20; 30; 100] [15; 35; 25; 80] ;;
- : (int * int) list = [(10, 15); (20, 35); (30, 25); (100, 80)]

> List.map2 (fun x y -> (x, y)) [10; 20; 30; 100] [15; 35; 25] ;;
Exception: Invalid_argument "List.map2".
>


> let fxy x y = 10*x - 4*y ;;
val fxy : int -> int -> int = <fun>
>
> List.map2 fxy [10; 20; 30; 100] [15; 35; 25; 80] ;;
- : int list = [40; 60; 200; 680]
>


(*      FILTER      *)
(*------------------*)
> List.filter ;;
- : ('a -> bool) -> 'a list -> 'a list = <fun>
>
>  List.filter ((<) 10) [-10 ; 20 ; 5 ; 50; 100; 3 ] ;;
- : int list = [20; 50; 100]
>

(*  FOLFR and FOLDL - Equivalent to Haskell foldr and foldl
 *   The fold functions are known as reduce. (i.e Python reduce (left fold))
 *)

> List.fold_right ;;
- : ('a -> 'b -> 'b) -> 'a list -> 'b -> 'b = <fun>

    (*
        f x y = x + 10* y
        foldr  f [1; 2; 3; 5; 6] 0

    Evaluation:

    (f 1 (f 2 (f 3  (f 5 (f 6 0)))))    f 6   0  = 6 + 10*0    =     6
    (f 1 (f 2 (f 3  (f 5 6))))          f 5   6  = 5 + 10*6    =     65
    (f 1 (f 2 (f 3  65)))               f 3  65  = 3 + 10*65   =    653
    (f 1 (f 2 653))                     f 2 653  = 2 + 10*653  =    6532
    (f 1 6532)                          f 1 6532 = 1 + 10*6532 =   65321
    65321
    *)
> List.fold_right (fun x y -> x + 10*y) [1; 2; 3; 5; 6] 0 ;;
- : int = 65321
>

> List.fold_left ;;
- : ('a -> 'b -> 'a) -> 'a -> 'b list -> 'a = <fun>
>
    (*
        f x y = 10*x + y
        flodl f 0 [1; 2; 3; 5; 6]

    Evaluation:

    (f (f (f ( f (f 0 1) 2 ) 3) 5) 6)     f 0    1 = 10*0    + 1   =    1
    (f (f (f ( f 1 2 ) 3) 5) 6)           f 1    2 = 10*1    + 2   =   12
    (f (f (f 12 3) 5) 6)                  f 12   3 = 10*12   + 3   =   123
    (f (f 123 5) 6)                       f 123  5 = 10*123  + 5   =  1235
    (f 1235 6)                            f 1235 6 = 10*1235 + 5   = 12356
    12356
    *)

> List.fold_left (fun x y -> 10*x +  y) 0 [1; 2; 3; 5; 6]  ;;
- : int = 12356
>

> List.fold_left (+) 0 [ 1; 2; 3; 4; 5; 6 ] ;;
- : int = 21

> List.fold_left ( *) 1 [ 1; 2; 3; 4; 5 ] ;;
Warning 2: this is not the end of a comment.
- : int = 120



(*    ITER / MAP Non Pure Functions  *)
(*-----------------------------------*)

> List.iter ;;
- : ('a -> unit) -> 'a list -> unit = <fun>
>

> List.iter (Printf.printf "= %d \n") [-10; 3; 100; 50; 5; 20] ;;
= -10
= 3
= 100
= 50
= 5
= 20
- : unit = ()
>

(* ITERI    *)

> List.iteri ;;
- : (int -> 'a -> unit) -> 'a list -> unit = <fun>

> List.iteri (Printf.printf "idendx = %d element = %d\n") [-10; 3; 100; 50; 5; 20] ;;
idendx = 0 element = -10
idendx = 1 element = 3
idendx = 2 element = 100
idendx = 3 element = 50
idendx = 4 element = 5
idendx = 5 element = 20
- : unit = ()


(* ITER2    *)
> List.iter2 ;;
- : ('a -> 'b -> unit) -> 'a list -> 'b list -> unit = <fun>
>
> List.iter2 (Printf.printf "%s = %d\n") ["x"; "y"; "z"] [1; 2; 3 ] ;;
x = 1
y = 2
z = 3
- : unit = ()
>


```

### Array

Arrays as oppose to lists are mmutable data structures.

* http://caml.inria.fr/pub/docs/manual-ocaml/libref/Array.html

```ocaml
> [| 10; 2 ; 10; 25 ; 100; 0 ; 1 |]  ;;
- : int array = [|10; 2; 10; 25; 100; 0; 1|]

> let a = [| 10; 2 ; 10; 25 ; 100; 0 ; 1 |]  ;;
val a : int array = [|10; 2; 10; 25; 100; 0; 1|]

> a.(0) ;;
- : int = 10
> a.(1) ;;
- : int = 2
> a.(4) ;;
- : int = 100
> a.(10) ;;
Exception: Invalid_argument "index out of bounds".
>

> Array.length ;;
- : 'a array -> int = <fun>

> Array.length a ;;
- : int = 7

> Array.map ;;
- : ('a -> 'b) -> 'a array -> 'b array = <fun>

> Array.map (fun x -> x*2 + 1) a ;;
- : int array = [|21; 5; 21; 51; 201; 1; 3|]

> a.(0) <- 56 ;;
- : unit = ()
> a ;;
- : int array = [|56; 2; 10; 25; 100; 0; 1|]
> a.(5) <- 567 ;;
- : unit = ()
> a ;;
- : int array = [|56; 2; 10; 25; 100; 567; 1|]

(*  Maps a function that takes the index of element
    as first argument and then the element as second
    argument.

    f(index, element)
*)
> Array.mapi ;;
- : (int -> 'a -> 'b) -> 'a array -> 'b array = <fun>
 Array.mapi (fun idx e -> idx, e) [|56; 2; 10; 25; 100; 567; 1|]  ;;
- : (int * int) array = [|(0, 56); (1, 2); (2, 10); (3, 25); (4, 100); (5, 567); (6, 1)|]


 > Array.to_list ;;
- : 'a array -> 'a list = <fun>
> Array.to_list a ;;
- : int list = [56; 2; 10; 25; 100; 567; 1]


> Array.of_list ;;
- : 'a list -> 'a array = <fun>
> Array.of_list [56; 2; 10; 25; 100; 567; 1] ;;
- : int array = [|56; 2; 10; 25; 100; 567; 1|]

> Array.get ;;
- : 'a array -> int -> 'a = <fun>
 > Array.get a 0 ;;
- : int = 56
> Array.get a 20 ;;
Exception: Invalid_argument "index out of bounds".




> let a = [|56; 2; 10; 25; 100; 567; 1|]  ;;
val a : int array = [|56; 2; 10; 25; 100; 567; 1|]
> Array.set ;;
- : 'a array -> int -> 'a -> unit = <fun>

> Array.set a  0 15 ;;
- : unit = ()
> Array.set a  4 235 ;;
- : unit = ()
> a ;;
- : int array = [|15; 2; 10; 25; 235; 567; 1|]

> Array.fold_right ;;
- : ('b -> 'a -> 'a) -> 'b array -> 'a -> 'a = <fun>
> Array.fold_right (+) [|56; 2; 10; 25; 100; 567; 1|]  0 ;;
- : int = 761

> Array.fold_left ;;
- : ('a -> 'b -> 'a) -> 'a -> 'b array -> 'a = <fun>
> Array.fold_left (+) 0 [|56; 2; 10; 25; 100; 567; 1|]  ;;
- : int = 761

> Array.iter (Printf.printf "x = %d\n") [|56; 2; 10; 25; 100; 567; 1|]  ;;
x = 56
x = 2
x = 10
x = 25
x = 100
x = 567
x = 1
- : unit = ()
>

(*  Applies a function that takes the index of element
    as first argument and then the element as second
    argument.

    f(index, element)
*)
> Array.iteri ;;
- : (int -> 'a -> unit) -> 'a array -> unit = <fun>

> Array.iteri (Printf.printf "x[%d] = %d\n") [|56; 2; 10; 25; 100; 567; 1|]  ;;
x[0] = 56
x[1] = 2
x[2] = 10
x[3] = 25
x[4] = 100
x[5] = 567
x[6] = 1
- : unit = ()



> Array.init ;;
- : int -> (int -> 'a) -> 'a array = <fun>

> Array.init 10 (fun x -> 5. *. float_of_int x)  ;;
- : float array = [|0.; 5.; 10.; 15.; 20.; 25.; 30.; 35.; 40.; 45.|]
> Array.init 10 (fun x -> 5 * x + 10 )  ;;
- : int array = [|10; 15; 20; 25; 30; 35; 40; 45; 50; 55|]

> Array.make_matrix ;;
- : int -> int -> 'a -> 'a array array = <fun>
> Array.make_matrix 2 3 1 ;;
- : int array array = [|[|1; 1; 1|]; [|1; 1; 1|]|]

```

### String

There are more useful string functions on the core library.

* http://caml.inria.fr/pub/docs/manual-ocaml/libref/String.html
* http://caml.inria.fr/pub/docs/manual-ocaml/libref/Char.html
* http://caml.inria.fr/pub/docs/manual-ocaml/libref/Str.html

**Char Module**

```ocaml

(* Acii code to Char *)

    # Char.chr ;;
    - : int -> char = <fun>

    # Char.chr 99 ;;
    - : char = 'c'

(* Char to Ascii Code*)


    # Char.code 'a' ;;
    - : int = 97

    # Char.code ;;
    - : char -> int = <fun>

(* Upper Case / Lower Case *)

    # Char.uppercase 'c' ;;
    - : char = 'C'
    # Char.lowercase 'A' ;;
    - : char = 'a'
```

**String Module**

```ocaml


(* Length of String *)

    # String.length "Hello world" ;;
    - : int = 11

(* Concatenate Strings *)
    # String.concat "\n" ["Hello"; "World"; "Ocaml" ] ;;
    - : string = "Hello\nWorld\nOcaml"

    # "Hello " ^ " World " ^ " Ocaml" ;;
    - : string = "Hello  World  Ocaml"

(* Uppercase / Lowercase *)
    # String.capitalize "hello world" ;;
    - : string = "Hello world"

    # String.lowercase "HELLO WORLD" ;;
    - : string = "hello world"

(* Get Character at position *)

    # String.get ;;
    - : string -> int -> char = <fun>
    # String.get  "Hello world" 0 ;;
    - : char = 'H'
    # String.get  "Hello world" 1 ;;
    - : char = 'e'
    
    # List.map (String.get "Buffalo") [0; 1; 2; 3; 4; 5; 6] ;;
    - : char list = ['B'; 'u'; 'f'; 'f'; 'a'; 'l'; 'o']
    # 

    # let str =  "My string" ;;
    val str : string = "My string"

    # str.[0] ;;
    - : char = 'M'
     # str.[5] ;;
    - : char = 'r'

(* Setting Characters *)

    # let str =  "My string" ;;
    val str : string = "My string"

    # s.[0] <- 'H' ;;
    - : unit = ()
     # s.[1] <- 'e' ;;
    - : unit = ()
     # s ;;
    - : string = "He to world"

(* Contains test if character is in the string *)

    # String.contains ;;
    - : string -> char -> bool = <fun>

    # String.contains "hello" 'c' ;;
    - : bool = false

    # String.contains "hello" 'h' ;;
    - : bool = true

(* Map Characters *)

    # String.map ;;
    - : (char -> char) -> string -> string = <fun>

    # let encode key chr = Char.chr ((Char.code chr) - key) ;;
    val encode : int -> char -> char = <fun>

    # let decode key chr = Char.chr (Char.code chr + key) ;;
    val decode : int -> char -> char = <fun>

    # let str = "Hello world" ;;
    val str : string = "Hello world"

    # String.map (encode 13) str ;;
- : string = ";X__b\019jbe_W"

    # String.map (decode 13) ";X__b\019jbe_W" ;;
    - : string = "Hello world"
```



**Str Module**

* http://caml.inria.fr/pub/docs/manual-ocaml/libref/Str.html

```ocaml

(* When in the toploop interactive shell *)
    #load "str.cma"

(* Compile String to a regular expression*)
    # Str.regexp ;;
    - : string -> Str.regexp = <fun>

    # Str.regexp ",";;
    - : Str.regexp = <abstr>

(* Split String *)

    # Str.split ;;
    - : Str.regexp -> string -> string list = <fun>

    # Str.split (Str.regexp ",") "23.232,9823,\"Ocaml Haskell FP\"";;
    - : string list = ["23.232"; "9823"; "\"Ocaml Haskell FP\""]

    # Str.split (Str.regexp "[,|;]") "23.232,9823;\"Ocaml Haskell FP\";400";;
    - : string list = ["23.232"; "9823"; "\"Ocaml Haskell FP\""; "400"]

    # Str.split (Str.regexp "[ ]") "23.232 9823 Ocaml Haskell FP 400";;
    - : string list = ["23.232"; "9823"; "Ocaml"; "Haskell"; "FP"; "400"]

    # Str.string_before ;;
    - : string -> int -> string = <fun>
    # Str.string_before "Hello world ocaml" 3 ;;
    - : string = "Hel"
    # Str.string_before "Hello world ocaml" 11 ;;
    - : string = "Hello world"

    # Str.string_after "Hello world ocaml" 11 ;;
    - : string = " ocaml"
    
    
    #  Str.split (Str.regexp "[ ]") "23.232 9823 Ocaml Haskell FP 400";;
    - : string list = ["23.232"; "9823"; "Ocaml"; "Haskell"; "FP"; "400"]
```

**Buffer Module**

* http://caml.inria.fr/pub/docs/manual-ocaml/libref/Str.html

It provides accumulative concatenation of strings in quasi-linear time (instead of quadratic time when strings are concatenated pairwise).

```ocaml

> Buffer.create ;;
- : int -> Buffer.t = <fun> 

(* Converts Buffer to String *)
Buffer.contents ;;
- : Buffer.t -> bytes = <fun>
> 

(* Add Char to Buffer *)
> Buffer.add_char ;;
- : Buffer.t -> char -> unit = <fun> 

Buffer.add_bytes ;;
- : Buffer.t -> bytes -> unit = <fun>
> 

(* Add String to Buffer *)
> Buffer.add_string ;;
- : Buffer.t -> bytes -> unit = <fun> 
>

(* Reset Buffer -> buffer = "" *)
> Buffer.reset ;;
- : Buffer.t -> unit = <fun>
> 

> let b = Buffer.create  0 ;;
val b : Buffer.t = <abstr> 

> Buffer.contents b ;;
- : bytes = ""


> Buffer.add_string  b "ocaml fp rocks ! ocaml is amazing" ;;
- : unit = ()

> Buffer.contents b ;;
- : bytes = "ocaml fp rocks ! ocaml is amazing"

> Buffer.add_string b " - Ocaml is strict FP" ;;
- : unit = ()  

Buffer.length b ;;
- : int = 33

> Buffer.contents b ;;
- : bytes = "ocaml fp rocks ! ocaml is amazing - Ocaml is strict FP"
> 


Buffer.reset b ;;
- : unit = () 

> Buffer.contents b ;;
- : bytes = ""



let c = Buffer.create 0 ;;
val c : Buffer.t = <abstr>
> Buffer.add_char c 'o' ;;
- : unit = ()
> Buffer.add_char c 'c' ;;
- : unit = ()
> Buffer.add_char c 'a' ;;
- : unit = () 
> Buffer.add_char c 'm' ;;
- : unit = () 
> Buffer.add_char c 'l' ;;
- : unit = () 

> Buffer.contents c ;;
- : bytes = "ocaml"

print_string (Buffer.contents c) ;;
ocaml- : unit = ()


> Buffer.nth c 0 ;;
- : char = 'o'
> Buffer.nth c 1 ;;
- : char = 'c'
> Buffer.nth c 2 ;;
- : char = 'a
> Buffer.nth c 3 ;;
- : char = 'm'
> Buffer.nth c 4 ;;
- : char = 'l'

List.map (Buffer.nth c) [0; 1; 2; 3; 4] ;;
- : char list = ['o'; 'c'; 'a'; 'm'; 'l']

> let s = "hindler milner type system" ;;
val s : bytes = "hindler milner type system"

> let b = Buffer.create 0 ;;
val b : Buffer.t = <abstr>

> Buffer.add_substring ;;
- : Buffer.t -> bytes -> int -> int -> unit = <fun>

Buffer.add_substring b s 0 4 ;;
- : unit = ()

> Buffer.contents b ;;
- : bytes = "hind"

Buffer.add_substring b s 4 5 ;;
- : unit = ()

> Buffer.contents b ;;
- : bytes = "hindler m"

> Buffer.add_substring b s 6 10 ;;
- : unit = ()

> Buffer.contents b ;;
- : bytes = "hindler mr milner t"


> Buffer.sub ;;
- : Buffer.t -> int -> int -> bytes = <fun>


let c = Buffer.create 0 ;;
val c : Buffer.t = <abstr>
> Buffer.add_string c "Ocaml is almost fast as C" ;;
- : unit = ()
> Buffer.sub c 0 1 ;;
- : bytes = "O" 
> Buffer.sub c 0 5 ;;
- : bytes = "Ocaml" 
> Buffer.sub c 0 10 ;;
- : bytes = "Ocaml is a"
> Buffer.sub c 5 15 ;;
- : bytes = " is almost fast"
> 
```

**String Processing**

Splitting a string into characters.

```ocaml

> let s = "p2p peer to peer connection" ;;
val s : bytes = "p2p peer to peer connection"

(* Imperative Approach *)


> let arr = Array.make ;;
val arr : int -> 'a -> 'a array = <fun>

let arr = Array.make (String.length s) '\000' ;;
val arr : char array = 
[|'\000'; '\000'; '\000'; '\000'; '\000'; '\000'; '\000'; '\000'; '\000';
'\000'; '\000'; '\000'; '\000'; '\000'; '\000'; '\000'; '\000'; '\000';
'\000'; '\000'; '\000'; '\000'; '\000'; '\000'; '\000'; '\000'; '\000'|]

> for i = 0 to (String.length s - 1) do
      arr.(i) <- s.[i]
  done ;;
- : unit = () 


> arr ;;
- : char array = 
[|'p'; '2'; 'p'; ' '; 'p'; 'e'; 'e'; 'r'; ' '; 't'; 'o'; ' '; 'p'; 'e'; 'e';
'r'; ' '; 'c'; 'o'; 'n'; 'n'; 'e'; 'c'; 't'; 'i'; 'o'; 'n'|]

let str2chars s = 
    let arr = Array.make (String.length s) '\000' in
    for i = 0 to (String.length s - 1) do
        arr.(i) <- s.[i]
    done ;
    arr 
;;
val str2chars : bytes -> char array = <fun> 
> 

str2chars "fox" ;;
- : char array = [|'f'; 'o'; 'x'|]



> str2chars "fox" |> Array.map Char.code ;;
- : int array = [|102; 111; 120|] 

> let str2chars_ s = 
    let arr = Array.make (String.length s) '\000' in
    for i = 0 to (String.length s - 1) do
        arr.(i) <- s.[i]
    done ;
    Array.to_list arr 
;;
val str2chars_ : bytes -> char list = <fun>
> 

> str2chars_ "UNIX" ;;
- : char list = ['U'; 'N'; 'I'; 'X']

> str2chars_ "UNIX" |> List.map Char.code ;;
- : int list = [85; 78; 73; 88]

(* Functional Approach *)

> let str2chars s = 
    let n = String.length s in
    let rec aux i alist = 
        if i =0 
        then []
        else s.[n-i]::(aux (i-1) alist)
    in aux n []
;;
val str2chars : bytes -> char list = <fun>

> str2chars "UNIX" ;;
- : char list = ['U'; 'N'; 'I'; 'X']

str2chars "UNIX" |> List.map Char.code ;;
- : int list = [85; 78; 73; 88]


(* Extract Digits *)

let digit_of_char c = 
    match c with
    | '0' -> 0
    | '1' -> 1
    | '2' -> 2
    | '3' -> 3
    | '4' -> 4
    | '5' -> 5
    | '6' -> 6
    | '7' -> 7
    | '8' -> 8
    | '9' -> 9
    | _   -> failwith "Not a digit"
;;
;;val digit_of_char : char -> int = <fun>

> str2chars "12334" |> List.map digit_of_char ;;
- : int list = [1; 2; 3; 3; 4]

> str2chars "12334x" |> List.map digit_of_char ;;
Exception: Failure "Not a digit".

> let str2digits s =  str2chars s |> List.map digit_of_char ;;
val str2digits : bytes -> int list = <fun>

> str2digits "1234" ;;
- : int list = [1; 2; 3; 4]

```

Join Chars

```ocaml
> let cs = ['o' ; 'c'; 'a'; 'm'; 'l' ] ;;
val cs : char list = ['o'; 'c'; 'a'; 'm'; 'l'] 

> Buffer.add_char ;;
- : Buffer.t -> char -> unit = <fun>

> List.iter ;;
- : ('a -> unit) -> 'a list -> unit = <fun>

List.iter (Buffer.add_char b) cs ;;
- : unit = ()

> Buffer.contents b ;;
- : bytes = "ocaml"

let chars2str cs = 
    let b = Buffer.create 0 in
    List.iter (Buffer.add_char b) cs ;
    Buffer.contents b 
;;
val chars2str : char list -> bytes = <fun> 
> 

> chars2str ['o' ; 'c'; 'a'; 'm'; 'l' ] ;;
- : bytes = "ocaml"

```

Strip Left Chars

```ocaml

> let trim chars s = 
    let n = String.length s in
    let b = Buffer.create 0 in
    
    for i = 0 to n - 1 do
        if List.mem s.[i] chars 
            then ()
            else Buffer.add_char b s.[i]
    done ;
    Buffer.contents b
;;


> trim ['-'; '.']  "-.--.--.....-trim chars---...----.-.-" ;;
- : bytes = "trim chars"

```




<!--
    ---------------------------------------------------------------
-->

### Sys

http://caml.inria.fr/pub/docs/manual-ocaml/libref/Sys.html

```ocaml

(* Command Line Arguments *)

    > Sys.argv ;;
    - : string array = [|"/home/tux/bin/ocaml"|]
    >

(* Current Directory *)

    > Sys.getcwd ;;
    - : unit -> string = <fun>

    > Sys.getcwd() ;;
    - : string = "/home/tux/PycharmProjects/ocaml/prelude"

(* Change Current Directory *)

    > Sys.chdir ;;
    - : string -> unit = <fun>

    > Sys.chdir "/" ;;
    - : unit = ()
    > Sys.getcwd () ;;
    - : string = "/"
    >

(* List directory *)

    > Sys.readdir ;;
    - : string -> string array = <fun>
    >

    > Sys.readdir("/") ;;
    - : string array =
    [|"tftpboot"; "var"; "cdrom"; "home"; "lost+found"; "netinterface.log";
      "opt"; "root.tar"; "tmp"; "sys"; "etc"; "root"; "srv"; "bin";
      "vmlinuz.old"; "sbin"; "backup.log"; "initrd.img.old"; "run"; "initrd.img";
      "lib"; "usr"; "vmlinuz"; "mnt"; "dev"; "media"; "boot"; "proc"|]
    >

(* Test if is directory *)

    > Sys.is_directory "/etc" ;;
    - : bool = true
    > Sys.is_directory "/etca" ;;
    Exception: Sys_error "/etca: No such file or directory".
    > Sys.is_directory "/etc/fstab" ;;
    - : bool = false
    >


(* Test if File exists *)

    > Sys.file_exists ;;
    - : string -> bool = <fun>

    > Sys.file_exists "/etc" ;;
    - : bool = true
    >
      Sys.file_exists "/etca" ;;
    - : bool = false
    >

    (* Execute shell command and return exit code
     *   0    For success
     *  not 0 For failure
     *)

    > Sys.command ;;
    - : string -> int = <fun>

    > Sys.command "uname -a" ;;
    Linux tux-I3000 3.13.0-36-generic >63-Ubuntu SMP Wed Sep 3 21:30:45 UTC 2014 i686 i686 i686 GNU/Linux
    - : int = 0
    >

    > Sys.command "nam" ;;
    sh: 1: nam: not found
    - : int = 127
    >


(* Constants and Flags *)

    (* Machine Word Size in bits *)
    >  Sys.word_size ;;
    - : int = 32
    >

    (* Machine Endianess *)
    > Sys.big_endian ;;
    - : bool = false
    >

    (*
     *  For Linux/BSD or OSX  --> "Unix"
     *  Windows               --> "Win32"
    *)
    > Sys.os_type ;;
    - : string = "Unix"
    >

    (*  Maximum length of strings and byte sequences. =
     *   =~  16 MB
     *)
    > Sys.max_string_length ;;
    - : int = 16777211
    >
```

Extra Example:

```ocaml

    # let (|> x f = f x ;;

    # let (<|) f x = f x ;;
    val ( <| ) : ('a -> 'b) -> 'a -> 'b = <fun>

    # Sys.chdir "/boot" ;;
    - : unit = ()

    # Sys.readdir "." ;;
    - : string array =
    [|"abi-3.13.0-35-generic"; "System.map-3.13.0-35-generic";
      "vmlinuz-3.13.0-35-generic"; "abi-3.13.0-36-generic";
      "config-3.13.0-35-generic"; "memtest86+_multiboot.bin";
      "initrd.img-3.13.0-35-generic"; "memtest86+.elf"; "memtest86+.bin";
      "System.map-3.13.0-36-generic"; "initrd.img-3.13.0-36-generic";
      "xen-4.4-amd64.gz"; "extlinux"; "config-3.13.0-36-generic";
      "vmlinuz-3.13.0-36-generic"; "grub"|]
    #


    (* List only directories *)
    # "." |> Sys.readdir |> Array.to_list  |> List.filter Sys.is_directory ;;
    - : string list = ["extlinux"; "grub"]

    (* List only files *)

    # "." |> Sys.readdir |> Array.to_list  |> List.filter (fun d -> not <| Sys.is_directory d );;
    - : string list =
    ["abi-3.13.0-35-generic"; "System.map-3.13.0-35-generic";
     "vmlinuz-3.13.0-35-generic"; "abi-3.13.0-36-generic";
     "config-3.13.0-35-generic"; "memtest86+_multiboot.bin";
     "initrd.img-3.13.0-35-generic"; "memtest86+.elf"; "memtest86+.bin";
     "System.map-3.13.0-36-generic"; "initrd.img-3.13.0-36-generic";
     "xen-4.4-amd64.gz"; "config-3.13.0-36-generic"; "vmlinuz-3.13.0-36-generic"]
    #

```

### Unix

Despite the name Unix, this is a multi platform module and also works in Windows OS.

Compile Standalone programs with Unix Library.

```
$ ocamlfind ocamlc -package more,unix program.ml -linkpkg -o program
$ ocamlfind ocamlopt -package more,unix program.ml -linkpkg -o progra
```

```ocaml


    (* It is only needed on the toploop shell *)
    # #load "unix.cma" ;;

    (* Get an environment variable *)
    # Unix.getenv "PATH" ;;
    - : string =
    "/home/tux/.opam/4.02.1/bin:/home/tux/bin:/usr/local/sbin:/usr/local/bin:
    /usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/home/tux/bin:
    /home/tux/usr/bin:/home/tux/.apps

    (*  Return the current time since 00:00:00 GMT, Jan. 1, 1970, in seconds
     *
     *)

      Unix.time ;;
    - : unit -> float = <fun>
    # Unix.time () ;;
    - : float = 1433237870.
    #

    (* Convert a time in seconds, as returned by Unix.time, into a date
     * and a time. Assumes UTC (Coordinated Universal Time), also known as GMT.
     *)

    # Unix.gmtime ;;
    - : float -> Unix.tm = <fun>
    #

    # Unix.gmtime @@ Unix.time () ;;
    - : Unix.tm =
    {Unix.tm_sec = 36; tm_min = 39; tm_hour = 9; tm_mday = 2; tm_mon = 5;
     tm_year = 115; tm_wday = 2; tm_yday = 152; tm_isdst = false}
    #

      Unix.gethostname () ;;
    - : string = "tux-I3000"
    #


```

## IO - Input / Output

Standard Print Functions

```ocaml

> print_string "Hello world!\n";;
Hello world!
- : unit = ()
>

> print_int 100 ;;
100- : unit = ()

> print_float 23.23 ;;
23.23- : unit = ()

> print_char 'c' ;;
c- : unit = ()
>

> let s = Printf.sprintf "x =  %s  y = %f z = %d" "hello" 2.2232 40  ;;
val s : string = "x =  hello  y = 2.223200 z = 40"

> print_string s ;;
x =  hello  y = 2.223200 z = 40- : unit = ()
```

Printf Module

```ocaml
> Printf.printf "%d" 100 ;;
100- : unit = ()

> Printf.printf "%d %f" 100 2.232 ;;
100 2.232000- : unit = ()

> Printf.printf "%s %d %f" "This is" 100 2.232 ;;
This is 100 2.232000- : unit = ()

> Printf.sprintf "x =  %s  y = %f z = %d" "hello" 2.2232 40 ;;
- : string = "x =  hello  y = 2.223200 z = 40"
```

**Files**

Writing a Text File:

```ocaml
utop # let file = open_out "/tmp/file.txt" ;;
val file : out_channel = <abstr>

utop # output_string file "hello\n" ;;
- : unit = ()

utop # output_string file "world\n" ;;
- : unit = ()

utop # close_out file ;;
- : unit = ()
```

Reading a Text File:

```
utop # let filein = open_in "/tmp/file.txt" ;;
val filein : in_channel = <abstr>

utop # input_line filein ;;
- : string = "hello"

utop # input_line filein ;;
- : string = "world"

utop # input_line filein ;;
Exception: End_of_file.


utop # let filein = open_in "/tmp/file.txt" ;;
val filein : in_channel = <abstr>

utop # input_char ;;
- : in_channel -> char = <fun>


utop # input_char filein ;;
- : char = 'h'
utop # input_char filein ;;
- : char = 'e'
utop # input_char filein ;;
- : char = 'l'
utop # input_char filein ;;
- : char = 'l'
utop #
```

### Type Casting

```ocaml

utop # float_of_int 2323 ;;
- : float = 2323.

utop # float_of_string "100.04523" ;;
- : float = 100.04523

utop # int_of_float 100.04523 ;;
- : int = 100

utop # int_of_string "90000" ;;
- : int = 90000

utop # let x = [20; 230; 10 ; 40; 50 ] ;;
val x : int list = [20; 230; 10; 40; 50]

utop # List.map float_of_int x ;;
- : float list = [20.; 230.; 10.; 40.; 50.]

utop # Array.of_list [20.; 230.; 10.; 40.; 50.]  ;;
- : float array = [|20.; 230.; 10.; 40.; 50.|]

utop # Array.to_list [|20.; 230.; 10.; 40.; 50.|]  ;;
- : float list = [20.; 230.; 10.; 40.; 50.]

utop # string_of_bool true ;;
- : string = "true"

utop # string_of_int 11000 ;;
- : string = "11000"

utop # string_of_float 23.2323 ;;
- : string = "23.2323"
```


<!--
    ---------------------------------------------------------------
-->


<!--
    ---------------------------------------------------------------
-->

## Algebraic Data Types and Pattern Matching

* Product Types: Corresponds to cartesian product. Examples: Tupĺes, Records, Constructs.
* Sum Types: Disjoin Unions of types. Used to express possibilities.


### Algebraic Data Types

#### Record Types

```ocaml
> type country = {
     name : string ;
     domain : string ;
     language : string ;
     id : int ;
  }
  ;;


> let brazil = {name = "Brazil" ; domain = ".br" ; language = "Portuguese" ; id = 100 } ;;
val brazil : country =
  {name = "Brazil"; domain = ".br"; language = "Portuguese"; id = 100}

>  brazil.name ;;
- : string = "Brazil"

> brazil.domain ;;
- : string = ".br"

> brazil.language ;;
- : string = "Portuguese"

> brazil.id ;;
- : int = 100


> let countries = [
    {name = "Brazil" ; domain = ".br" ; language = "Portuguese" ; id = 100 } ;
    {name = "United Kingdom" ; domain = ".co.uk" ; language = "English" ; id = 10 } ;
    {name = "South Africa" ; domain = ".co.za" ; language = "English" ; id = 40 } ;
    {name = "France" ; domain = ".fr" ; language = "French" ; id = 20 } ;
    {name = "Taiwan ROC" ; domain = ".tw" ; language = "Chinese" ; id = 54 } ;
    {name = "Australia" ; domain = ".au" ; language = "English" ; id = 354 } ;
      ]
  ;;
val countries : country list =
  [{name = "Brazil"; domain = ".br"; language = "Portuguese"; id = 100};
   {name = "United Kingdom"; domain = ".co.uk"; language = "English";
    id = 10};
   {name = "South Africa"; domain = ".co.za"; language = "English"; id = 40};
   {name = "France"; domain = ".fr"; language = "French"; id = 20};
   {name = "Taiwan ROC"; domain = ".tw"; language = "Chinese"; id = 54};
   {name = "Australia"; domain = ".au"; language = "English"; id = 354}]


> List.map (fun c -> c.name ) countries ;;
- : string list =
["Brazil"; "United Kingdom"; "South Africa"; "France"; "Taiwan ROC";
 "Australia"]


> List.map (fun c -> c.domain ) countries ;;
- : string list = [".br"; ".co.uk"; ".co.za"; ".fr"; ".tw"; ".au"]

> List.filter (fun c -> c.name = "France") countries ;;
- : country list =
[{name = "France"; domain = ".fr"; language = "French"; id = 20}]

> List.filter (fun c -> c.language = "English") countries ;;
- : country list =
[{name = "United Kingdom"; domain = ".co.uk"; language = "English"; id = 10};
 {name = "South Africa"; domain = ".co.za"; language = "English"; id = 40};
 {name = "Australia"; domain = ".au"; language = "English"; id = 354}]

```

#### Disjoint Union

```ocaml
type literal =
    | Integer   of int
    | Float     of float
    | String    of string

type operator =
    | Add
    | Div
    | Mul
    | Sub

```

#### Agreggated Data types



```ocaml
> type agg = Agg of int * string

>  let a = Agg (1, "hi") ;;
val a : agg = Agg (1, "hi")

>  a ;;
- : agg = Agg (1, "hi")
>
```

```ocaml
type shape =
      Rect    of float * float          (*width * lenght *)
    | Circle  of float                  (* radius *)
    | Triang  of float * float * float  (* a * b *  c *)

let pictures = [Rect (3.0, 4.0) ; Circle 5.0 ; Triang (5.0, 5.0, 5.0)]

let perimiter s =
    match s with
         Rect    (a, b)      ->  2.0 *. (a +. b)
       | Circle   r          ->  2.0 *. 3.1415 *. r
       | Triang  (a, b, c)   ->  a +. b +. c


>  perimiter (Rect (3.0, 4.0)) ;;
- : float = 14.

>  perimiter (Circle 3.0 ) ;;
- : float = 18.849


>  perimiter (Triang (2.0, 3.0, 4.0)) ;;
- : float = 9.
>

> List.map perimiter pictures ;;
- : float list = [14.; 31.4150000000000027; 15.]

```

#### Pattern Matching

##### Basic Pattern Matching

```ocaml
> match 4 with x -> x;;                        (* ⇒ 4 *)
- : int = 4

> match 4 with xyz -> xyz;;                    (* ⇒ 4 *)
- : int = 4

> match [3;4;5] with [a;b;c] -> b;;            (* ⇒ 4 *)
- : int = 4

> match (3, 7, 4) with ( a, b, c ) -> c;;      (* ⇒ 4 *)
- : int = 4

>  match (3, 4, (8,9)) with (a, b, c ) -> c;;   (* ⇒ (8, 9) *)
- : int * int = (8, 9)
>


> let sign x = match x with
    x when x < 0 -> -1
    | 0           -> 0
    | x           -> 1

;;
val sign : int -> int = <fun>


>  sign 10 ;;
- : int = 1
>  sign 0 ;;
- : int = 0
>  sign 100 ;;
- : int = 1
>  sign (-1) ;;
- : int = -1
>  sign (-100) ;;
- : int = -1
>

let getPassword passw = match passw with
    "hello"   -> "OK. Safe Opened."
    | _       -> "Wrong Password Pal."
  ;;

val getPassword : string -> string = <fun>
>
  getPassword "hellow" ;;
- : string = "Wrong Password Pal."
>  getPassword "Pass" ;;
- : string = "Wrong Password Pal."
>  getPassword "hello" ;;
- : string = "OK. Safe Opened."
>
```

##### Tuple Pattern Matching

Extracting elements of a tuple

```
> let fst (a, _) = a ;;
val fst : 'a * 'b -> 'a = <fun>
> let snd (_, b) = b ;;
val snd : 'a * 'b -> 'b = <fun>

> fst (2, 3) ;;
- : int = 2

> fst ("a", 23) ;;
- : string = "a"

> snd (2, 3) ;;
- : int = 3

> snd (2, "a") ;;
- : string = "a"

> List.map fst [("a", 12); ("b", 23) ; ("c", 23) ; ("d", 67)] ;;
- : string list = ["a"; "b"; "c"; "d"]

> List.map snd [("a", 12); ("b", 23) ; ("c", 23) ; ("d", 67)] ;;
- : int list = [12; 23; 23; 67]


>
let tpl3_1 (a, _, _) = a
let tpl3_2 (_, a, _) = a
let tpl3_3 (_, _, a) = a;;

> tpl3_1 (2, "ocaml", 2.323) ;;
- : int = 2
> tpl3_2 (2, "ocaml", 2.323) ;;
- : string = "ocaml"
> tpl3_3 (2, "ocaml", 2.323) ;;
- : float = 2.323
```

Returning Multiple Values

```ocaml

>  let divmod a b = a/b, a mod b ;;
val divmod : int -> int -> int * int = <fun>

  divmod 10 3 ;;
- : int * int = (3, 1)



> let swap (x, y) = (y, x) ;;
val swap : 'a * 'b -> 'b * 'a = <fun>

  swap (2, 3) ;;
- : int * int = (3, 2)

> List.map swap [("a", 12); ("b", 23) ; ("c", 23) ; ("d", 67)] ;;
- : (int * string) list = [(12, "a"); (23, "b"); (23, "c"); (67, "d")]
```

Source:

* http://xahlee.info/ocaml/pattern_matching.html


##### List Recursive Functions

```ocaml

    # let rec factorial1 n =
        match n with
        | 0 -> 1
        | 1 -> 0
        | k -> k * factorial1 (k - 1)
    ;;
    val factorial1 : int -> int = <fun>

    # let rec factorial2 = function
        | 0     -> 1
        | 1     -> 1
        | n     -> n * factorial2 (n - 1)
    ;;
    val factorial2 : int -> int = <fun>

    # factorial1 5 ;;
    - : int = 120

     # factorial2 5 ;;
    - : int = 120

    # let rec map f xs =   match xs with
        | []        ->  []
        | h::tl     ->  (f h)::(map f tl)
    ;;
    val map : ('a -> 'b) -> 'a list -> 'b list = <fun>

    # map ((+) 5) [10; 20; 25 ; 9] ;;
    - : int list = [15; 25; 30; 14]

    # let rec sumlist = function
        | []        -> 0
        | [a]       -> a
        | (hd::tl)  -> hd + sumlist tl
    ;;
    val sumlist : int list -> int = <fun>

    # sumlist [1; 2; 4; 5; 6; 7; 8; 9] ;;
    - : int = 42

    # let rec prodlist = function
        | []        -> 1
        | [a]       -> a
        | (hd::tl)  -> hd * prodlist tl
    ;;
    val prodlist : int list -> int = <fun>

    # prodlist [1 ; 2; 3; 4; 5; 6 ] ;;
    - : int = 720

    # let rec filter f xs = match xs with
        | []        -> []
        | h::tl     -> if f h
                       then h::(filter f tl)
                       else filter f tl

    ;;

     # filter (fun x -> x < 10) [1; 2; 10; 20; 4; 6; 15] ;;
    - : int list = [1; 2; 4; 6]


    # let rec take n xs =
        match (n, xs) with
        | (0, _    ) -> []
        | (_, []   ) -> []
        | (k, h::tl) -> k::(take (n-1) tl)
    ;;val take : int -> 'a list -> int list = <fun>

    # take 3 [1; 2; 10; 20; 4; 6; 15] ;;
    - : int list = [3; 2; 1]
    # take 20 [1; 2; 10; 20; 4; 6; 15] ;;
    - : int list = [20; 19; 18; 17; 16; 15; 14]


    # let rec drop n xs =
        if n < 0 then failwith "n negative"
        else
            match (n, xs) with
            | (0, _ )    ->  xs
            | (_, [])    ->  []
            | (k, h::tl) ->  drop (k-1) tl
    ;;val drop : int -> 'a list -> 'a list = <fun>

    # drop (-10)  [1; 2; 10; 20; 4; 6; 15] ;;
    Exception: Failure "n negative".

    # drop 10 [] ;;
    - : 'a list = []

    # drop 0 [1; 2; 10; 20; 4; 6; 15] ;;
    - : int list = [1; 2; 10; 20; 4; 6; 15]

    # drop 5 [1; 2; 10; 20; 4; 6; 15] ;;
    - : int list = [6; 15]

    # drop 25 [1; 2; 10; 20; 4; 6; 15] ;;
    - : int list = []

(*

    Haskell Function:
    foldl1 : ( a -> a -> a ) -> [a] -> [a]

    > foldl1 (\x y -> 10*x + y) [1, 2, 3, 4, 5]
    12345

    foldl1 f  [1, 2, 3, 4, 5]  =
    f 5 (f 4 (f 3 (f 1 2)))

    foldl f [x0, x1, x2, x3, x4, x5 ... ] =

    f xn (f xn-1 (f xn-2 ... (f x3 (f x2 (f x1 x0))))) ....

    From: http://en.wikipedia.org/wiki/Fold_%28higher-order_function%29
*)

    let rec foldl1 f xs =
        match xs with
        | []            -> failwith "Empty list"
        | [x]           -> x
        | (x::y::tl)    -> foldl1 f (f x y :: tl)
    ;;

    # foldl1 (fun x y -> 10*x + y) [1; 2; 3; 4; 5] ;;
    - : int = 12345


    # let rec foldr1 f xs =
        match xs with
        | []        -> failwith "Empty list"
        | [x]       -> x
        | x::tl     -> f x (foldr1 f tl)
    ;;
    val foldr1 : ('a -> 'a -> 'a) -> 'a list -> 'a = <fun>

    # foldr1 (fun x y -> x + 10*y) [1; 2; 3; 4; 5; 6] ;;
    - : int = 654321

    # let rec foldl f acc xs =
        match xs with
        | []     -> acc
        | x::tl  -> foldl f (f acc x) tl
    ;;

    # foldl (fun x y -> 10*x + y) 0  [1; 2; 3; 4; 5; 6] ;;
    - : int = 123456

    # let rec foldr f acc xs =
        match xs with
        | []    -> acc
        | x::tl -> f x (foldr f acc tl)
    ;;
    val foldr : ('a -> 'b -> 'b) -> 'b -> 'a list -> 'b = <fun>

    # foldr (fun x y -> x + 10*y) 0  [1; 2; 3; 4; 5; 6] ;;
    - : int = 654321


    # let rec range start stop step =
        if start > stop
        then []
        else start::(range  (start + step) stop step )
    ;;
    val range : int -> int -> int -> int list = <fun>

    # range 0 30 1 ;;
    - : int list =  [0; 1; 2; 3; 4; 5; 6; 7; 8; 9; 10; 11; 12; 13; 14;
    15; 16; 17; 18; 19; 20; 21; 22; 23; 24; 25;    26; 27; 28; 29; 30]

    # range 0 100 10 ;;
    - : int list = [0; 10; 20; 30; 40; 50; 60; 70; 80; 90; 100]

    # range (-100) 100 10 ;;
    - : int list = [-100; -90; -80; -70; -60; -50; -40; -30; -20;
    -10; 0; 10; 20; 30; 40; 50; 60; 70; 80; 90; 100]

```


#### Recursive Data Structures

##### Alternative List Implementation

List of Intergers

```ocaml
type intlist = Nil | Cons of (int * intlist)

let rec length (alist : intlist) : int =
    match alist with
    | Nil           -> 0
    | Cons(h, t)    -> 1 + (length t)


let is_empty (alist: intlist) : bool =
    match alist with
    | Nil           -> true
    | Cons(_, _)   ->  false

let rec sum (alist : intlist) : int =
    match alist with
    | Nil               -> failwith "Error empty list"
    | Cons (a, Nil)     -> a
    | Cons (h, remain)  -> h + sum remain

let rec product (alist : intlist) : int =
    match alist with
    | Nil               -> failwith "Error empty list"
    | Cons (a, Nil)     -> a
    | Cons (h, remain)  -> h * product remain

let head (alist: intlist) : int =
    match alist with
    | Nil           -> failwith "Error empty list"
    | Cons (a, _)   -> a

let rec last (alist : intlist) : int =
    match alist with
    | Nil               -> failwith "Error empty list"
    | Cons (a, Nil)     -> a
    | Cons (h, remain)  -> last remain


let rec map f alist =
    match alist with
    | Nil               -> Nil
    | Cons (e, remain)  -> Cons(f e, map f remain)

let rec iter (f : int -> unit) (alist : intlist) : unit =
    match alist with
    | Nil           -> failwith "Error empty list"
    | Cons(a,  Nil) -> f a
    | Cons(hd, tl)  -> f hd ; iter f tl


let rec filter f alist =
    match alist with
    | Nil               -> Nil
    | Cons (e, remain)  ->
                if (f e)
                    then Cons(e, filter f remain)
                    else filter f remain

let rec foldl1 func alist  =  match alist with
    | Nil                -> failwith "Error empty list"
    | Cons (a, Nil)      -> a
    | Cons (hd, tl)      -> func hd (foldl func tl)


let rec foldl func acc alist =
    match alist with
    | Nil               -> acc
    | Cons (hd, tl)     -> func hd (foldl func acc tl)


let rec nth alist n =
    match (alist, n) with
    | (Nil,     _       )   -> failwith  "Empty list"
    | (Cons(h, _  ),   1)   -> h
    | (Cons(hd, tl),   k)   -> nth tl  (k-1)

let rec take n alist =
    match (n, alist) with
    | (_, Nil)           -> Nil
    | (0, _  )           -> Nil
    | (k, Cons(hd, tl))  -> Cons(hd, take (k-1) tl)


let rec append ((list1:intlist), (list2:intlist)) : intlist =
    match list1 with
    | Nil               -> list2
    | Cons(hd, tl)      -> Cons(hd, append( (tl, list2)))

let rec reverse(list:intlist):intlist =
    match list with
      Nil -> Nil
    | Cons(hd,tl) -> append(reverse(tl), Cons(hd,Nil))
```

```ocaml
let l1 = Nil
let l2 = Cons(1, Nil)  (* [1] *)
let l3 = Cons(2, l2)   (* [2; 1] *)
let l4 = Cons(3, l3)   (* [3; 2; 1] *)
let l5 = Cons(1, Cons(2, Cons(3, Cons(4, Cons(5, Nil)))))

> List.map length [l1 ; l2 ; l4 ; l5 ] ;;
- : int list = [0; 1; 3; 5]

> List.map is_empty [l1 ; l2 ; l4 ; l5 ] ;;
- : bool list = [true; false; false; false]

>  sum l1 ;;
Exception: Failure "Error empty list".
>  sum l2 ;;
- : int = 1
>  sum l3 ;;
- : int = 3
>  sum l4 ;;
- : int = 6
>  sum l5 ;;
- : int = 15
>


>  product l1 ;;
Exception: Failure "Error empty list".
>  product l2 ;;
- : int = 1
>  product l3 ;;
- : int = 2
>  product l4 ;;
- : int = 6
>  product l5 ;;
- : int = 120
>


> List.map head [l2 ; l3 ; l4 ; l5 ] ;;
- : int list = [1; 2; 3; 1]


>  last l1 ;;
Exception: Failure "Error empty list".
>  last l2 ;;
- : int = 1
>  last l3 ;;
- : int = 1
>  last l4 ;;
- : int = 1
>  last l5 ;;
- : int = 5


> map ((+) 1) Nil ;;
- : intlist = Nil
> map ((+) 1) (Cons(1, Nil))  ;;
- : intlist = Cons (2, Nil)
> map ((+) 1) (Cons(1, Cons(3, Cons(4, Cons(5, Nil)))))  ;;
- : intlist = Cons (2, Cons (4, Cons (5, Cons (6, Nil))))


>  append(l4, l5) ;;
- : intlist = Cons  (3, Cons (2, Cons (1, Cons (1, Cons (2, Cons (3, Cons (4, Cons (5, Nil))))))))

> reverse l5 ;;
- : intlist = Cons (5, Cons (4, Cons (3, Cons (2, Cons (1, Nil)))))

>   let even x = x mod 2 == 0 ;;
>   filter even l5 ;;
- : intlist = Cons (2, Cons (4, Nil)

> l5 ;;
- : intlist = Cons (1, Cons (2, Cons (3, Cons (4, Cons (5, Nil)))))

>  nth l5 2 ;;
- : int = 2
>  nth l5 1 ;;
- : int = 1
>  nth l5 3 ;;
- : int = 3
>  nth l5 4 ;;
- : int = 4
>  nth l5 5 ;;
- : int = 5

> foldl1 (+) l5 ;;
- : int = 15

> foldl1 (fun x y -> x * y) l5 ;;
- : int = 120

> foldl1 (fun x y -> x + 10*y) l5 ;;
- : int = 54321


> foldl (+) 0 l5 ;;
- : int = 15

> foldl (fun x y -> x * y) 2 l5 ;;
- : int = 240


>
  take 0 l5 ;;
- : intlist = Nil
>  take 1 l5 ;;
- : intlist = Cons (1, Nil)
>  take 3 l5 ;;
- : intlist = Cons (1, Cons (2, Cons (3, Nil)))
>  take 4 l5 ;;
- : intlist = Cons (1, Cons (2, Cons (3, Cons (4, Nil))))
>  take 10 l5 ;;
- : intlist = Cons (1, Cons (2, Cons (3, Cons (4, Cons (5, Nil)))))


> iter (Printf.printf "= %d\n") l5 ;;
= 1
= 2
= 3
= 4
= 5
- : unit = ()
```

##### File Tree Recursive Directory Walk

```ocaml
> type fileTree =
  | File of string
  | Folder of string * fileTree list

let neg f x = not (f x) ;;
type fileTree = File of string | Folder of string * fileTree list
val neg : ('a -> bool) -> 'a -> bool = <fun>

let relpath p str = p ^ "/" ^ str

let scand_dir path =
    Sys.readdir path
    |> Array.to_list

let is_dir_relpath path rel =
    Sys.is_directory (relpath path rel)

let rec walkdir path =

    let files = path
    |> scand_dir
    |> List.filter @@ neg  (is_dir_relpath path)
    |> List.map (fun x -> File x) in

    let dirs = path
    |> scand_dir
    |> List.filter (is_dir_relpath path)
    |> List.map (fun x -> Folder (x, walkdir (relpath path x ))) in

      dirs @ files
  ;;
val relpath : string -> string -> string = <fun>
val scand_dir : string -> string list = <fun>
val is_dir_relpath : string -> string -> bool = <fun>
val walkdir : string -> fileTree list = <fun>
>

> Folder ("Documents", [File "file1.txt"; File "filte2.txt" ; 
Folder("Pictures", []) ; Folder("bin",[File "cmd.dat" ; File "thumbs.db" ]) 
]) 
;;
- : fileTree =
Folder ("Documents",
 [File "file1.txt"; File "filte2.txt"; Folder ("Pictures", []);
  Folder ("bin", [File "cmd.dat"; File "thumbs.db"])])


> walkdir "/boot" ;;
- : fileTree list =
[Folder ("grub",
  [Folder ("i386-pc",
    [File "cpio_be.mod"; File "reiserfs.mod"; File "disk.mod";
     File "dm_nv.mod"; File "xfs.mod"; File "zfscrypt.mod";
     File "setjmp.mod"; File "boot.img"; File "uhci.mod"; File "hwmatch.mod";
     File "ohci.mod"; File "xzio.mod"; File "btrfs.mod"; File "echo.mod";
     File "efiemu.mod"; File "ufs1_be.mod"; File "romfs.mod";
     File "minix2_be.mod"; File "fs.lst"; File "gdb.mod"; File "search.mod";
     File "part_gpt.mod"; File "halt.mod"; File "setjmp_test.mod";
    ...

>  walkdir "/home/tux/PycharmProjects/Haskell/haskell" ;;
- : fileTree list =
[Folder ("src",
  [File "stack.hs"; File "a.out"; File "OldState.hs";
   File "bisection_state.hs"; File "FPUtils.hs"; File "randomst.hs"]);
 Folder ("images",
  [File "haskellLogo.png"; File "number-system-in-haskell-9-638.jpg";
   File "euler_newton_cooling.png"; File "monadTable.png";
   File "coinflip.gid"; File "coinflip.gif"; File "chartF2table.png";
   File "chartF1table.png"; File "classes.gif"; File "qrcode_url.png"]);
 File "Functions.md"; File "List_Comprehension.md"; File "Haskell.md";
 File "Useful_Custom_Functions__Iterators_and_Operators.md";
 File "Miscellaneous.md"; File "Pattern_Matching.md"; File "Basic_Syntax.md";
 File "Functors__Monads__Applicatives_and_Monoids.md";
 File "Algebraic_Data_Types.md"; File "Libraries.md";
 File "Documentation_and_Learning_Materials.md"; File "Applications.md";
 File "Functional_Programming_Concepts.md"]
```



Sources:

* http://www.cs.cornell.edu/courses/cs3110/2008fa/lectures/lec04.html

<!--
    ---------------------------------------------------------------
-->

## Lazy Evaluation

OCaml, which is a ML derived language, uses strict evaluation by default as oppose to Haskell that uses lazy evaluation by default. However lazy evaluation (aka delayed evaluation) is optional in Ocaml. Lazy evaluation delays the computation until the result is needed. In this evaluation approach results that are not needed are never computed. It allows infinite lists.

In a strict language the arguments are always evaluated first.

OCaml / Strict Evaluation / Call by Value
```ocaml
> let give_me_a_three _ = 3
val give_me_a_three : 'a -> int = <fun>

> give_me_a_three (1/0);;
Exception: Division_by_zero.

>  List.length [21 ; 23 ; 123 ; 22/0 ] ;;
Exception: Division_by_zero.
```

In a lazy language the arguments are only used if the function needs them.

Haskell / Lazy Evaluation / Call be Need
```haskell
> let give_me_a_three _ = 3
> give_me_a_three (1/0)
3
>

> length [21, 23, 123, 22/0 ]
4
>
```

Ocaml allows **lazy** evaluation with the lazy module and explicit lay constructs.
'a Lazy.t is the polymorphic type of promises to return a value of type 'a.

```ocaml
> let lazy_expr = lazy (1/0) ;;
val lazy_expr : int lazy_t = <lazy>

> Lazy.force lazy_expr ;;
Exception: Division_by_zero.


let x = lazy (print_endline "hello" ) ;;
val x : unit lazy_t = <lazy>
> Lazy.force x ;;
hello
- : unit = ()

> let expr = lazy (List.length [21, 23, 123, 22/0 ]) ;;
val expr : int lazy_t = <lazy>

> Lazy.force expr
  ;;
Exception: Division_by_zero.
```

##### Infinite Lazy Lists

It is possible to define an infinite list, which is pair of head and value and a promise to the rest of the list.

```ocaml
 type 'a inf_list = Cons of 'a * 'a inf_list Lazy.t
```

```ocaml
type 'a t = Cons of 'a * ('a t lazy_t)

(* n, n+1, n+2, ... *)
let rec from n = Cons (n, lazy (from (n+1)))

let repeat x = Cons (x, lazy (repeat (x)))

let head (Cons (x, _)) = x
let tail (Cons (_, xs)) = Lazy.force xs

let take n s =
let rec take' m (Cons (x, xs)) l =
 if m = 0 then List.rev l
 else take' (m-1) (Lazy.force xs) (x :: l)
in
 take' n s []

let rec nth n (Cons (x, xs)) =
    if n = 1 then x
    else nth (n-1) (Lazy.force xs)



let rec map f (Cons (x, xs)) =
    Cons (f x, lazy (map f (Lazy.force xs)))


let rec filter f (Cons (x, xs)) =
    Cons (f x, lazy (filter f (Lazy.force xs)))
```

Example:
```ocaml
> take 6 (from 5) ;;
- : int list = [5; 6; 7; 8; 9; 10]

> from 5 |> take 6 ;;
- : int list = [5; 6; 7; 8; 9; 10]


> from 5  |> head ;;
- : int = 5


```

From:
* http://d.hatena.ne.jp/blanketsky/20070221/1172002969

Sources:

* https://ocaml.org/learn/tutorials/functional_programming.html
* http://ocaml.jp/Lazy%20Pattern
* http://c2.com/cgi/wiki?ExplicitLazyProgramming

Documentation of Lazy Module:

* http://caml.inria.fr/pub/docs/manual-ocaml/libref/Lazy.html

<!--
    ---------------------------------------------------------------
-->

## Foreign Function Interface FFI 

The OCaml library C Types allows easy and fast access to C libraries, Operating System services, shared libraries and to call functions and libraries written in another languages.

**Installation**

```
$ opam install ctypes ctypes-foreign
```



### Calling C Standard Library and Unix System Calls

**Examples**

```ocaml

> #require "ctypes" ;;
> #require "ctypes.foreign" ;;
> open Foreign ;;
> open PosixTypes ;;
> open Ctypes ;;
```

**Puts**

C prototype

```C
int puts(const char *s);
```

Ocaml

```ocaml
> let puts = foreign "puts" (string @-> returning int);;
val puts : bytes -> int = <fun>

> puts "hello world" ;;
hello world
- : int = 12
>
```

**Chdir**

C prototype

```C
int chdir  (const char *path);

// Changes  the current working directory of the calling
// process to the directory specified  in path.

```

OCaml
 
```ocaml 
> let chdir_ = foreign "chdir" (string @-> returning int) ;;
val chdir_ : bytes -> int = <fun>

chdir_ "/" ;;
- : int = 0

chdir_ "/wrong directory" ;;
- : int = -1

> let chdir path  =
      let _  = chdir_  path in ()
;;
val chdir : bytes -> unit = <fun>

> chdir "/" ;;
- : unit = ()

> chdir "/wrong directory" ;;
- : unit = ()
``` 

**Getcwd**

C prototype

```C
char *getcwd(char *buf, size_t size);
//  getcwd  get current working directory
```   

Ocaml

```ocaml 
> let getcwd =  foreign "getcwd" (void @-> returning string);;
val getcwd : unit -> bytes = <fun>

> getcwd () ;;
- : bytes = "/home" 
>
```

**System**

C prototype

```C
int system(const char *command);
// system - execute a shell command
```

Ocaml

```ocaml
> let system = foreign "system" (string @-> returning int) ;;
val system : bytes -> int = <fun>


> system "uname -a" ;;
Linux tuxhorse 3.19.0-18-generic #18-Ubuntu SMP Tue May 19 18:30:59 UTC 2015 i686 i686 i686 GNU/Linux
- : int = 0

> system "pwd" ;;
/home/tux/PycharmProjects/Haskell/ocaml
- : int = 0
>

> system "wrong command" ;;
sh: 1: wrong: not found
- : int = 32512
> 
```

**Sleep**

C prototype

```C
unsigned int sleep(unsigned int seconds);
// sleep - sleep for the specified number of seconds 
```

Ocaml

```ocaml
> let sleep1 = foreign "sleep" (int @-> returning int) ;; 
val sleep : int -> int = <fun>

> sleep1 3 ;;
- : int = 0

> let sleep2 = foreign "sleep" (int @-> returning void) ;; 
val sleep2 : int -> unit = <fun>
> sleep2 4 ;;
- : unit = ()
```

**Get host name**

C prototype

```C
int gethostname(char *name, size_t len);
 
 //  returns the null-terminated hostname in the character array name,
 //   which has a length of len bytes.
 
 //  On success, zero is returned.  On error, -1 is returned, and errno 
 //  is set appropriately
 // On  Linux,  HOST_NAME_MAX is defined with the value 64 (bytes - chars)
```

Ocaml 

```ocaml
> let get_host = foreign "gethostname" (ptr char @-> int @-> returning int) ;;
val get_host : char Ctypes_static.ptr -> int -> int = <fun>

> let host_name_max = 64 ;;
val host_name_max : int = 64

> let s = allocate_n char ~count:host_name_max ;;
val s : char Ctypes.ptr = Ctypes_static.CPointer <abstr>
 
> get_host s host_name_max ;;
- : int = 0

> string_from_ptr s ~length:host_name_max ;;
- : string =
"tuxhorse\000\000\000\000\.....\000\000\000\000"

> coerce (ptr char) string s ;;
- : string = "tuxhorse"

   (* Everything together      *)
   (* ---------------------------------*)
   
> let get_host = foreign "gethostname" (ptr char @-> int @-> returning int) ;;
val get_host : char Ctypes_static.ptr -> int -> int = <fun>

> let gethostname () = 
       let host_name_max = 64 in
       let s = allocate_n char ~count:host_name_max in
       let _ = get_host s host_name_max in
       coerce (ptr char) string s
    ;;
val gethostname : unit -> string = <fun>

> gethostname () ;;
- : string = "tuxhorse"
``` 
 
**Cube Root Function**

C prototype

```C
 #include <math.h>

 // The  cbrt()  function  returns the (real) cube root of x. 
 double cbrt(double x);
``` 

Ocaml 

```ocaml
> let cbrt = foreign "cbrt" (double @-> returning double) ;;
val cbrt : float -> float = <fun>

> List.map cbrt [1.; 8.; 64.; 125.; 216.; 1000.] ;;
- : float list = [1.; 2.; 4.; 5.; 6.; 10.]
```

**Popen**

Get command output to string.

C prototype

```C
 #include <stdio.h>

 FILE *popen(const char *command, const char *type);

 // popen, pclose - pipe stream to or from a process
 // The  popen()  function  opens  a  process  by creating a pipe, forking, 
 // and invoking the shell.  Since a pipe is by definition unidirectional, 
 // the type argument may specify only reading  or  writing,  not  both;  
 // the  resulting stream is correspondingly read-only or write-only
```

Ocaml

```ocaml
 #require "ctypes" ;;
 #require "ctypes.foreign" ;;
open PosixTypes ;;
open Ctypes ;;
open Foreign ;;

let is_null_ptr pointer_type pointer = 
 ptr_compare pointer (from_voidp pointer_type null) = 0 ;;

let string_of_ptrchar ptrch =  coerce (ptr char)  string ptrch ;;

let fgets = 
 foreign "fgets" (ptr char @-> int @-> ptr int @-> returning (ptr char)) ;;

let popen_ = 
 foreign "popen" (string @-> string @-> returning (ptr int)) ;;

let make_null_ptr ptrtype = from_voidp  ptrtype null ;;

exception Exit_loop ;;


let read_fd file_descriptor =
    let buffsize = 4000 in (* Buffer of 4000 bytes *)
    let v = Pervasives.ref (from_voidp  char null) in
    let b = Buffer.create buffsize in    
    let s = allocate_n char ~count:buffsize in
    
    let closure () = 
        try while true do
          v :=  fgets s (buffsize - 1) file_descriptor ;  
          if is_null_ptr char !v then raise Exit_loop ;
          Buffer.add_string b  (string_of_ptrchar s) ;
        done
        with Exit_loop -> () ;
    
    in closure () ; Buffer.contents b 
;;


let popen command = read_fd (popen_ command "r");;

> popen "ls" ;;
- : bytes = "images\nocamldep-sorter\nREADME.hml\nREADME.html\nREADME.md\nsrc\nTest.html\n" > 


> popen "ls" |> print_string ;;
images
ocamldep-sorter
README.hml
README.html
README.md
src
Test.html
- : unit = ()
> 

> popen "pwd" |> print_string ;;
/home/tux/PycharmProjects/Haskell/ocaml
- : unit = ()
>
```

### Calling Shared Libraries

#### Calling GNU Scientific Library

* Calling GSL - GNU Scientific Library

* [GSL Manual](https://www.gnu.org/software/gsl/manual/html_node)

```ocaml

> #require "ctypes" ;;
> #require "ctypes" ;;
> #require "ctypes.foreign" ;;
> open Foreign ;;
> open PosixTypes ;;
> open Ctypes ;;

> let gsl_lib = Dl.dlopen ~filename:"libgsl.so" ~flags:[Dl.RTLD_LAZY; Dl.RTLD_GLOBAL];;
val gsl_lib : Dl.library = <abstr>
> 


(*
 *   double gsl_sf_bessel_J0 (double x)
 *  
 *  gsl_sf_bessel_J0(5) = -1.775967713143382920e-01
 *
 *------------------------------------------------------------------
 *)
 
> let gsl_sf_bessel_J0 = foreign ~from:gsl_lib "gsl_sf_bessel_J0" (double @-> returning double ) ;;
val gsl_sf_bessel_J0 : float -> float = <fun>

> gsl_sf_bessel_J0 5.0 ;;
- : float = -0.177596771314338292
 
> List.map gsl_sf_bessel_J0 [1.0; 10.0; 100.0; 1000.0 ] ;;
- : float list =
[0.765197686557966494; -0.245935764451348265; 0.0199858503042231184;
 0.024786686152420169]

(*
 *   Polynomial Evaluation
 *  
 *  double gsl_poly_eval (const double c[], const int len, const double x)
 *   
 *   The functions described here evaluate the polynomial 
 *   P(x) = c[0] + c[1] x + c[2] x^2 + ... + c[len-1] x^{len-1} 
 *   
 *   using Horner’s method for stability. Inline versions of these 
 *   functions are used when HAVE_INLINE is defined. 
 *
 *   This function evaluates a polynomial with real coefficients for 
 *   the real variable x. 
 *
 * ------------------------------------------------------------------
 *)

    (*
     *  Let's evaluate  3 * x^2 + 2 * x + 1
     * 
     *  so it becomes:  [1. ; 2. ; 3. ]
     *
     *
     *  (3 * x^2 + 2 * x + 1) 5 = 86
     *  (3 * x^2 + 2 * x + 1) 7 = 162
     *  (3 * x^2 + 2 * x + 1) 9 = 262
     *)

> let gsl_poly_eval_ = foreign ~from:gsl_lib "gsl_poly_eval" 
(ptr double @-> int @-> double @-> returning double);;
val gsl_poly_eval_ : float Ctypes_static.ptr -> int -> float -> float = <fun>

> let poly =  CArray.of_list double  [1. ; 2. ; 3. ] ;;
val poly : float Ctypes.CArray.t =
  {Ctypes_static.astart = Ctypes_static.CPointer <abstr>; alength = 3}

> CArray.start poly ;;
- : float Ctypes.ptr = Ctypes_static.CPointer <abstr>
 
> gsl_poly_eval_ (CArray.start poly)  3 5.0 ;;
- : float = 86.
> gsl_poly_eval_ (CArray.start poly)  3 7.0 ;;
- : float = 162.
> gsl_poly_eval_ (CArray.start poly)  3 9.0 ;;
- : float = 262.


    (* Assemblying all pieces of code *)

> let gsl_poly_eval_ = foreign ~from:gsl_lib "gsl_poly_eval" 
  (ptr double @-> int @-> double @-> returning double);;
val gsl_poly_eval_ : float Ctypes_static.ptr -> int -> float -> float = <fun>
 
> let gsl_poly_val poly x = 
       let p = CArray.of_list double poly in
       gsl_poly_eval_ (CArray.start p) (List.length poly) x
  ;;
val gsl_poly_val : float list -> float -> float = <fun>
 

>  List.map (gsl_poly_val [1. ; 2. ; 3. ] ) [5.0 ; 7.0 ; 9.0] ;;
- : float list = [86.; 162.; 262.]

> let mypoly = gsl_poly_val [1. ; 2. ; 3. ]  ;;
val mypoly : float -> float = <fun>

> List.map mypoly [5.0 ; 7.0 ; 9.0] ;;
- : float list = [86.; 162.; 262.]
> 

```

#### Calling Python

* [Embedding Python in Another Application](https://docs.python.org/2/extending/embedding.html)

* [Initializing and finalizing the interpreter - Python/C API Reference Manual](https://docs.python.org/2/c-api/init.html)

* [The Very High Level Layer - Python/C API Reference Manual](https://docs.python.org/2/c-api/veryhigh.html)

```ocaml
> 
  open Foreign ;;
>  open PosixTypes ;;
> open Ctypes ;;
> 
  
>  let pylib = Dl.dlopen ~filename:"libpython2.7.so" ~flags:[Dl.RTLD_LAZY; Dl.RTLD_GLOBAL];;
val pylib : Dl.library = <abstr>
> let py_init = foreign "Py_Initialize" ~from:pylib (void @-> returning void) ;;
val py_init : unit -> unit = <fun>
> py_init () ;;
- : unit = ()

> let py_getPath = foreign "Py_GetPath" ~from:pylib (void @-> returning string) ;;
val py_getPath : unit -> string = <fun>
> py_getPath () ;;
- : string =
"/home/tux/lib:/usr/lib/python2.7/:/usr/lib/python2.7/plat-i386-linux-gnu:/usr/lib/python2.7/lib-tk:/usr/lib/python2.7/lib-old:/usr/lib/python2.7/lib-dynload"
> 
  
> let py_getVersion = foreign "Py_GetVersion" ~from:pylib (void @-> returning string) ;;
val py_getVersion : unit -> string = <fun>
> 
> py_getVersion() ;;
- : string = "2.7.9 (default, Apr  2 2015, 15:39:13) \n[GCC 4.9.2]"
> 
  
  let py_GetPlatform = foreign "Py_GetPlatform" ~from:pylib (void @-> returning string) ;;
val py_GetPlatform : unit -> string = <fun>
> py_GetPlatform () ;;
- : string = "linux2"
> 
  
> let pyRunSimpleString = foreign "PyRun_SimpleString" ~from:pylib (string @-> returning int) ;;
val pyRunSimpleString : string -> int = <fun>
> 
  pyRunSimpleString "print 'hello world'" ;;
hello world
- : int = 0

> pyRunSimpleString "f = lambda x: 10.5 * x - 4" ;;
- : int = 0
> pyRunSimpleString "print (map(f, [1, 2, 3, 4, 5, 6, 7])" ;;
  File "<string>", line 1
    print (map(f, [1, 2, 3, 4, 5, 6, 7])
                                       ^
SyntaxError: unexpected EOF while parsing
- : int = -1

> pyRunSimpleString "print (map(f, [1, 2, 3, 4, 5, 6, 7]))" ;;
[6.5, 17.0, 27.5, 38.0, 48.5, 59.0, 69.5]
- : int = 0

> pyRunSimpleString "import sys ; print sys.executable" ;;
/usr/bin/python
- : int = 0
> 

> let py_finalize = foreign "Py_Finalize" ~from:pylib (void @-> returning void) 
  ;;
val py_finalize : unit -> unit = <fun>
> py_finalize () ;;
- : unit = ()
> 

```

### Finding Shared Libraries


Unix Standard system calls on Linux.  You can see the Linux lib-C functions documentation by using the commands:

```bash
$ man puts
$ man getcwd
```

It is possible to find shared libraries used by executables by using the ldd command.

```bash
$ which rlwrap 
/usr/bin/rlwrap

$ ldd $(which rlwrap)
    linux-gate.so.1 =>  (0xb77bf000)
    libutil.so.1 => /lib/i386-linux-gnu/libutil.so.1 (0xb779f000)
    libreadline.so.6 => /lib/i386-linux-gnu/libreadline.so.6 (0xb775a000)
    libtinfo.so.5 => /lib/i386-linux-gnu/libtinfo.so.5 (0xb7736000)
    libc.so.6 => /lib/i386-linux-gnu/libc.so.6 (0xb757b000)
    /lib/ld-linux.so.2 (0xb77c0000)

$ ldd $(which ssh)
    linux-gate.so.1 =>  (0xb774a000)
    libselinux.so.1 => /lib/i386-linux-gnu/libselinux.so.1 (0xb763f000)
    libresolv.so.2 => /lib/i386-linux-gnu/libresolv.so.2 (0xb7626000)
    libcrypto.so.1.0.0 => /lib/i386-linux-gnu/libcrypto.so.1.0.0 (0xb7452000)
    libdl.so.2 => /lib/i386-linux-gnu/libdl.so.2 (0xb744d000)
    libz.so.1 => /lib/i386-linux-gnu/libz.so.1 (0xb7432000)
    libgssapi_krb5.so.2 => /usr/lib/i386-linux-gnu/libgssapi_krb5.so.2 (0xb73e2000)
    libc.so.6 => /lib/i386-linux-gnu/libc.so.6 (0xb7227000)
    libpcre.so.3 => /lib/i386-linux-gnu/libpcre.so.3 (0xb71b4000)
    /lib/ld-linux.so.2 (0xb774b000)
    libkrb5.so.3 => /usr/lib/i386-linux-gnu/libkrb5.so.3 (0xb70e1000)
    libk5crypto.so.3 => /usr/lib/i386-linux-gnu/libk5crypto.so.3 (0xb70af000)
    libcom_err.so.2 => /lib/i386-linux-gnu/libcom_err.so.2 (0xb70aa000)
    libkrb5support.so.0 => /usr/lib/i386-linux-gnu/libkrb5support.so.0 (0xb709d000)
    libpthread.so.0 => /lib/i386-linux-gnu/libpthread.so.0 (0xb707f000)
    libkeyutils.so.1 => /lib/i386-linux-gnu/libkeyutils.so.1 (0xb707a000)
    
$ # Trace System Calls

$ strace -c ls
build.sh  codes    haskell  Make      ocaml   README.back.md  README.md  Test.html
clean.sh  dict.sh  LICENSE  Makefile  papers  README.html     tags   tmp
% time     seconds  usecs/call     calls    errors syscall
------ ----------- ----------- --------- --------- ----------------
  0.00    0.000000           0         9           read
  0.00    0.000000           0         2           write
  0.00    0.000000           0        11           open
  0.00    0.000000           0        14           close
  0.00    0.000000           0         1           execve
  0.00    0.000000           0         9         9 access
  0.00    0.000000           0         3           brk
  0.00    0.000000           0         2           ioctl
  0.00    0.000000           0         3           munmap
  ... ....

$ ######### For Ubuntu / Debian Only Only ###########################
$ #
$ apt-cache search gsl | grep -i dev | grep -i lib
libgsl0-dev - GNU Scientific Library (GSL) -- development package
libghc-hmatrix-dev - Linear algebra and numerical computation in Haskell
libocamlgsl-ocaml-dev - GNU scientific library for OCaml


$ apt-cache search gsl | grep -i dev | grep -i lib
libgsl0-dev - GNU Scientific Library (GSL) -- development package
libghc-hmatrix-dev - Linear algebra and numerical computation in Haskell


$ apt-cache search python lib dev | grep -i dev | grep -i libpython
libpython-all-dev - package depending on all supported Python development packages
libpython-dev - header files and a static library for Python (default)
libpython2.7-dev - Header files and a static library for Python (v2.7)
```

### References

**OCAML Ctypes**


* [Book - Real World OCAML - Chapter 19. Foreign Function Interface](https://realworldocaml.org/v1/en/html/foreign-function-interface.html)

* [OCAML Ctypes Documentation](http://ocamllabs.github.io/ocaml-ctypes)
* [Ctypes FAQ - OCAML Labs](https://github.com/ocamllabs/ocaml-ctypes/wiki/FAQ)
* [Ctypes tutorial - OCAML Labs](https://github.com/ocamllabs/ocaml-ctypes/wiki/ctypes-tutorial)
* [Github - OCAML Labs - Ctypes](https://github.com/ocamllabs/ocaml-ctypes)
* [Mail List: ocaml-ctypes, a library for calling C functions directly from OCaml](https://sympa.inria.fr/sympa/arc/caml-list/2013-06/msg00046.html)
* [Repository Examples - OCAML Labs](https://github.com/ocamllabs/ocaml-ctypes/tree/master/examples)

**C Libraries Used in this section**

* [ Linux kernel and C library interfaces - Kernel.Org](https://www.kernel.org/doc/man-pages/)
* [Wikipedia - System call](http://en.wikipedia.org/wiki/System_call)
* [Linux System Call Table](http://docs.cs.up.ac.za/programming/asm/derick_tut/syscalls.html)
* [Linux Programmer's Manual SYSCALLS(2)](http://man7.org/linux/man-pages/man2/syscalls.2.html)
* [Linux System Calls Quick Reference](http://libflow.com/d/fz1qu63i/LINUX_System_Call_Quick_Reference)


* [GSL - GNU Scientific Library](https://www.gnu.org/software/gsl/)

## Creating Libraries, Modules and Compiling to Bytecode or Machine Code

### Loading Files in Interactive Shell

File: example.ml

```ocaml
let pi = 3.141515
let byte_size = 8
let kbyte_size = 8192


let avg x y = (x +. y) /. 2.0

let print_avg x y = print_float (avg x y)

let say_hello = print_string "Hello world OCaml"

let inc_int (x: int) : int = x+ 1 ;;

let is_zero x =
    match x with
    | 0 -> true
    | _ -> false


let is_one = function
    | 1 -> true
    | _ -> false

(* Recursive Function *)
let rec factorial n =
    if n = 0
        then 1
        else n*factorial (n-1)

module Week = struct

    type weekday = Mon | Tues | Wed | Thurs | Fri | Sun  ;;

    let from_weekday_to_string week =
        match week with
              Mon   -> "Monday"
            | Tues  -> "Tuesday"
            | Wed   -> "Wednesday"
            | Thurs -> "Thursday"
            | Fri   -> "Friday"
            | Sun   -> "Sunday"
end

module PhysicalConstants = struct
    let g = 9.81
    let gravity = 6.67384e-11
end
```

Loading File From interpreter/ toplevel.

```
    $ ocaml # or $utop

    # #use "example.ml" ;;
    val pi : float = 3.141515
    val byte_size : int = 8
    val kbyte_size : int = 8192
    val avg : float -> float -> float = <fun>
    val print_avg : float -> float -> unit = <fun>
    Hello world OCamlval say_hello : unit = ()
    val inc_int : int -> int = <fun>
    val is_zero : int -> bool = <fun>
    val is_one : int -> bool = <fun>
    val factorial : int -> int = <fun>
    module Week :
      sig
        type weekday = Mon | Tues | Wed | Thurs | Fri | Sun
        val from_weekday_to_string : weekday -> string
      end
    module PhysicalConstants : sig val g : float val gravity : float end
    #

    # pi ;;
    - : float = 3.141515
    # is_zero 10 ;;
    - : bool = false
    # is_zero 0 ;;
    - : bool = true
    # factorial 5 ;;
    - : int = 120
    #


    # PhysicalConstants.gravity ;;
    - : float = 6.67384e-11
    # PhysicalConstants.g ;;
    - : float = 9.81
    #


    # Week.Mon ;;
    - : Week.weekday = Week.Mon
    # Week.Tues ;;
    - : Week.weekday = Week.Tues

    # Week.from_weekday_to_string ;;
    - : Week.weekday -> string = <fun>

    # Week.from_weekday_to_string Week.Tues ;;
    - : string = "Tuesday"
    # Week.from_weekday_to_string Week.Mon ;;
    - : string = "Monday"
    #

    # open Week ;;
    # Mon ;;
    - : Week.weekday = Mon

    # Tues ;;
    - : Week.weekday = Tues
    #

    # List.map from_weekday_to_string [Mon ; Sun; Tues ] ;;
    - : string list = ["Monday"; "Sunday"; "Tuesday"]
    #

    # open PhysicalConstants ;;
    #
    # g ;;
    - : float = 9.81
    # gravity ;;
    - : float = 6.67384e-11
    #

```


### Compile Module to Bytecode

After the compiling example.ml files: example.cmi and example.cmo will be created. The module "Example" can be loaded without the source code after compiled into the toplevel shell.

```bash
$ ocamlc -c  example.ml

$ # Test file types

$ file example.cmi
example.cmi: OCaml interface file (.cmi) (Version 015)

$ file example.cmo
example.cmo: OCaml object file (.cmo) (Version 007)

$ # Check Module Type signature
$ ocamlc -i -c example.ml
val pi : float
val byte_size : int
val kbyte_size : int
val avg : float -> float -> float
val print_avg : float -> float -> unit
val say_hello : unit
val inc_int : int -> int
val is_zero : int -> bool
val is_one : int -> bool
val factorial : int -> int
module Week :
  sig
    type weekday = Mon | Tues | Wed | Thurs | Fri | Sun
    val from_weekday_to_string : weekday -> string
  end
module PhysicalConstants : sig val g : float val gravity : float end

$ Export Module signature
$ ocamlc -i -c example.ml > example.mli
```

Loading compiled bytecode into toplevel: $ ocaml

```
$ mv example.ml example2.ml
$ utop
utop # #load "example.cmo";;

utop # Example.avg 23.2 232.32
;;
- : float = 127.759999999999991

utop # Example.Week.from_weekday_to_string Example.Week.Sun ;;
- : string = "Sunday"

utop # Sun ;;
- : weekday = Sun

utop # Mon ;;
- : weekday = Mon

utop # List.map from_weekday_to_string [Mon ; Sun; Tues ] ;;
- : string list = ["Monday"; "Sunday"; "Tuesday"]

utop # Example.PhysicalConstants.g;;
- : float = 9.81

utop # Example.PhysicalConstants.gravity ;;
- : float = 6.67384e-11

utop # Example.factorial 5 ;;
- : int = 120

utop # Example.factorial 10 ;;
- : int = 3628800

utop # open Example ;;

utop # PhysicalConstants.g ;;
- : float = 9.81

utop # PhysicalConstants.gravity ;;
- : float = 6.67384e-11


```

Compile to Library

```
$ ocamlc -c example.ml
$ ocamlc -a example.cmo -o example.cma

$ file example.cma
example.cma: OCaml library file (.cma) (Version 008)

```

Loading example.cma

```
$ utop
utop # #load "example.cma" ;;
utop # Example.PhysicalConstants.g ;;
- : float = 9.81



$ file example.cmx
example.cmx: OCaml native object file (.cmx) (Version 011)
```

## Batteries Standard Library

[Batteries Documentation Reference](http://batteries.forge.ocamlcore.org/doc.preview:batteries-beta1/html/api/)

Install

```
$ opam install batteries

$ utop
> #require "batteries";;
> open Batteries ;;
>
```

### Getting Started

From: [Batteries Wiki](https://github.com/ocaml-batteries-team/batteries-included/wiki/Getting-started)

```ocaml
> #require "batteries";;
> open Batteries ;;
>
let main () =
  (1--999) (* the enum that counts from 1 to 999 *)
  |> Enum.filter (fun i -> i mod 3 = 0 || i mod 5 = 0)
  |> Enum.reduce (+) (* add all remaining values together *)
  |> Int.print stdout
;;
val main : unit -> unit = <fun>

> main () ;;
233168- : unit = ()
```

## References

### Articles


* [Ocaml for the masses - by Yaron Minks, Jane Stree Capital](http://cacm.acm.org/magazines/2011/11/138203-ocaml-for-the-masses/fulltext)


<!--
    ---------------------------------------------------------------
-->

### Links

* [OCaml Best Practices for Developers / Xen](http://wiki.xen.org/wiki/OCaml_Best_Practices_for_Developers#OCaml_Best_Practices_Guide)

* http://pleac.sourceforge.net/pleac_ocaml/packagesetc.html

* http://projects.camlcity.org/projects/dl/findlib-1.2.6/doc/guide-html/quickstart.html

* http://caml.inria.fr/pub/docs/manual-ocaml/extn.html

* http://www.loria.fr/~shornus/ocaml/slides-ocaml-3.pdf

* http://blog.enfranchisedmind.com/2007/01/ocaml-lazy-lists-an-introduction/



<!--
    ---------------------------------------------------------------
-->



### Books

* [Real World OCaml: Functional Programming for the Masses](https://realworldocaml.org/v1/en/html/index.html)
    * Authors: Jason Hickey, Anil Madhavapeddy and Yaron Minsky
    * Publisher: O'Reilly

* [Developing Applications With Objective OCaml](http://caml.inria.fr/pub/docs/oreilly-book/)
    * Authors: Emmanuel Chailloux, Pascal Manoury and Bruno Pagano
    * Publisher: O'Reilly France.
    * http://caml.inria.fr/pub/docs/oreilly-book/html/index.html

* [Introduction to Objective Caml](http://files.metaprl.org/doc/ocaml-book.pdf)
    * Authors: Jason Hickey
    * Url2: [Introduction to the Objective Caml Programming Language](http://main.metaprl.org/jyh/classes/cs134/cs134b/2007/public_html/assets/hickey.pdf)

* [Unix system programming in OCaml](Unix system programming in OCaml)
    * Authors: Xavier Leroy and Didier Rémy

* [Using, Understanding, and Unraveling - The OCaml Language From Practice to Theory and vice versa](http://caml.inria.fr/pub/docs/u3-ocaml/index.html), Didier Rémy

* OCaml Inria Manual:
    http://caml.inria.fr/pub/docs/manual-ocaml-4.00/

### Community

#### Online Resources

* Reddit:           http://reddit.com/r/ocaml
* Usenet:           comp.lang.ocaml
* StackOverflow
    * http://stackoverflow.com/tags/ocaml/info
    * http://stackoverflow.com/questions/tagged/ocaml
* Email List (lists.ocaml.org): http://lists.ocaml.org/listinfo

* https://ocaml.org/learn/

* [Resources for Caml users - Inria](http://caml.inria.fr/resources/index.en.html)

* http://langref.org/ocaml/pattern-matching

* http://rosettacode.org/wiki/Category:OCaml

* [PLEAC-Objective CAML](http://pleac.sourceforge.net/pleac_ocaml/)

* [Cornell University - CS3110 Spring 11 :: Data Structures and Functional Programming](http://www.cs.cornell.edu/Courses/cs3110/2011sp/lecturenotes.asp)


* [Universytet Wroclawiski | Lukaz Stafiniak / Functional Lectures] (http://www.ii.uni.wroc.pl/~lukstafi/pmwiki/index.php?n=Functional.Functional)

* [University of Southampton | Tutorial: OCaml for scientific computation - Dr. Thomas Fischbacher, Hans Fangohr](http://www.southampton.ac.uk/~fangohr/software/ocamltutorial/)

* [String Pattern Matching Examples](https://github.com/jimenezrick/ocaml-backpack/blob/master/src/backpackString.ml)


* [OCAML exceptions, a small tutorial](http://www.martani.net/2009/04/ocaml-exceptions-small-tutorial.html)

* [First Thoughts on OCaml](http://www.crmarsh.com/intro_to_ocaml/)


#### Blogs

* https://blogs.janestreet.com/

* http://www.ocamlpro.com/blog

* [Blog - Mirage OS](openmirage.org/blog/)

* [OCaml at LexiFi | LexiFi](https://www.lexifi.com/blogs/ocaml)

* [Alaska Ataca a Kamtchatka](alaska-kamtchatka.blogspot.com)

* [Oleg Kiselyov](http://okmij.org/ftp/ML/)



#### Hacker News Threads

* [OCaml for the Masses (2011)](https://news.ycombinator.com/item?id=8002188)

* [Python to OCaml: Retrospective (roscidus.com)](https://news.ycombinator.com/item?id=7858276)

* [Why OCaml, Why Now? (spyder.wordpress.com)](https://news.ycombinator.com/item?id=7416203)

* [Ask Hackers: Opinions on OCaml?](https://news.ycombinator.com/item?id=112129)

* [OCaml 4.03 will, “if all goes well”, support multicore (inria.fr)](https://news.ycombinator.com/item?id=9582980)

* [Code iOS Apps in OCaml](https://news.ycombinator.com/item?id=3740173)

* [OCaml Briefly (mads379.github.io)](https://news.ycombinator.com/item?id=8653207)

* [The fundamental problem of programming language package management](https://news.ycombinator.com/item?id=8226139)

* [Four MLs (and a Python) (thebreakfastpost.com)](https://news.ycombinator.com/item?id=9463254)

* [Jane Street releases open source alternative to OCaml's stdlib](https://news.ycombinator.com/item?id=3438084)

* [Why did Microsoft invest so much in F#?](https://news.ycombinator.com/item?id=1883679)


#### Stack Overflow Highlighted Questions


**Syntax / Implementation**

* [What does “let () = ” mean in Ocaml?](http://stackoverflow.com/questions/7524487/what-does-let-mean-in-ocaml)

* [What is the different between `fun` and `function` keywords?](http://stackoverflow.com/questions/1604270/what-is-the-different-between-fun-and-function-keywords)

* [Why is OCaml's (+) not polymorphic?](http://stackoverflow.com/questions/8017172/why-is-ocamls-not-polymorphic)

* [What are “`” in OCaml?](http://stackoverflow.com/questions/19498928/what-are-in-ocaml)

* [Explaining pattern matching vs switch](http://stackoverflow.com/questions/199918/explaining-pattern-matching-vs-switch)

* [Ocaml Variant Types](http://stackoverflow.com/questions/4777744/ocaml-variant-types)

* [Why is an int in OCaml only 31 bits?](http://stackoverflow.com/questions/3773985/why-is-an-int-in-ocaml-only-31-bits)

* [OCaml insert an element in list](http://stackoverflow.com/questions/9498746/ocaml-insert-an-element-in-list)

* [Ocaml - Accessing components in an array of records](http://stackoverflow.com/questions/14001569/ocaml-accessing-components-in-an-array-of-records)

* [What's the difference between “equal (=)” and “identical (==)” in ocaml?](http://stackoverflow.com/questions/13590307/whats-the-difference-between-equal-and-identical-in-ocaml/13590433#13590433)


* [Why is “1.0 == 1.0” false in Ocaml?](http://stackoverflow.com/questions/25878382/why-is-1-0-1-0-false-in-ocaml)

* [How to curry a function w.r.t. its optional arguments in OCaml](http://stackoverflow.com/questions/9647307/how-to-curry-a-function-w-r-t-its-optional-arguments-in-ocaml)

* [What's the difference between “let ()=” and “let _=” ;](http://stackoverflow.com/questions/11515240/whats-the-difference-between-let-and-let)

* [In OCaml, what type definition is this: 'a. unit -> 'a](http://stackoverflow.com/questions/23323032/in-ocaml-what-type-definition-is-this-a-unit-a?rq=1)

* [Hashtables in ocaml - Is it possible to store different types in the same hashtable (Hashtbl) in Ocaml? Are hashtables really restricted to just one type?](http://stackoverflow.com/questions/8962895/hashtables-in-ocaml/8963057#8963057)

* [How do you append a char to a string in OCaml?](http://stackoverflow.com/questions/8326926/how-do-you-append-a-char-to-a-string-in-ocaml/8329852#8329852)

* [Ocaml int to binary string conversion](http://stackoverflow.com/questions/9776245/ocaml-int-to-binary-string-conversion)

* [OCaml regex: specify a number of occurrences](http://stackoverflow.com/questions/14425762/ocaml-regex-specify-a-number-of-occurrences)

* [Side-effects and top-level expressions in OCaml](http://stackoverflow.com/questions/10459363/side-effects-and-top-level-expressions-in-ocaml)

* [OCaml passing labeled function as parameter / labeled function type equivalence](http://stackoverflow.com/questions/20931144/ocaml-passing-labeled-function-as-parameter-labeled-function-type-equivalence)

* [ocaml bitstring within a script](http://stackoverflow.com/questions/21225701/ocaml-bitstring-within-a-script)

* [What's the most-used data structure in OCaml to represent a Graph?](http://stackoverflow.com/questions/22067339/whats-the-most-used-data-structure-in-ocaml-to-represent-a-graph)


**Functional Programming**

* [State monad in OCaml](http://stackoverflow.com/questions/5843773/state-monad-in-ocaml)

* [The reverse state monad in OCaml](http://stackoverflow.com/questions/24943140/the-reverse-state-monad-in-ocaml)

* [What is the use of monads in OCaml?](http://stackoverflow.com/questions/29851449/what-is-the-use-of-monads-in-ocaml)

* [Monadic buffers in OCaml](http://stackoverflow.com/questions/25790177/monadic-buffers-in-ocaml)

* [Does OCaml have fusion laws](http://stackoverflow.com/questions/21031920/does-ocaml-have-fusion-laws)

* [Composite functions in ocaml](http://stackoverflow.com/questions/4997661/composite-functions-in-ocaml)

* [OCaml |> operator](http://stackoverflow.com/questions/30493644/ocaml-operator)

* [Is it possible to use pipes in OCaml?](http://stackoverflow.com/questions/8986010/is-it-possible-to-use-pipes-in-ocaml)

* [Is there an infix function composition operator in OCaml?](http://stackoverflow.com/questions/16637015/is-there-an-infix-function-composition-operator-in-ocaml/16637073#16637073)

* [List of Functional code snippets for Procedural Programmers?](http://stackoverflow.com/questions/832569/list-of-functional-code-snippets-for-procedural-programmers)

* [Application of Tail-Recursion in OCaml](http://stackoverflow.com/questions/19771283/application-of-tail-recursion-in-ocaml)

* [In Functional Programming, what is a functor?](http://stackoverflow.com/questions/2030863/in-functional-programming-what-is-a-functor)


* [In pure functional languages, is there an algorithm to get the inverse function?](http://stackoverflow.com/questions/13404208/in-pure-functional-languages-is-there-an-algorithm-to-get-the-inverse-function)

* [What is the benefit of purely functional data structure?](http://stackoverflow.com/questions/4399837/what-is-the-benefit-of-purely-functional-data-structure)


* [What's the closest thing to Haskell's typeclasses in OCaml?](http://stackoverflow.com/questions/14934242/whats-the-closest-thing-to-haskells-typeclasses-in-ocaml)


* [Defining functions pointfree-style in functional programming. What are the cons/pros?](http://stackoverflow.com/questions/7367655/defining-functions-pointfree-style-in-functional-programming-what-are-the-cons)


* [ What is **lenses** in OCaml's world ](http://stackoverflow.com/questions/28177263/what-is-lenses-in-ocamls-world)

* [Linked List Ocaml](http://stackoverflow.com/questions/1738758/linked-list-ocaml)

* [Functional programming languages introspection](http://stackoverflow.com/questions/3660957/functional-programming-languages-introspection)

* [When should objects be used in OCaml?](http://stackoverflow.com/questions/10779283/when-should-objects-be-used-in-ocaml)


* [Open and closed union types in Ocaml](http://stackoverflow.com/questions/5131954/open-and-closed-union-types-in-ocaml)

* [Choosing between continuation passing style and memoization](http://stackoverflow.com/questions/14781875/choosing-between-continuation-passing-style-and-memoization)


* [An concrete simple example to demonstrate GADT in OCaml?](http://stackoverflow.com/questions/27864200/an-concrete-simple-example-to-demonstrate-gadt-in-ocaml)

* [Generating primes without blowing the stack](http://stackoverflow.com/questions/18213973/generating-primes-without-blowing-the-stack)

* [Executing a list of functions](http://stackoverflow.com/questions/9054613/executing-a-list-of-functions)

* [Best way to represent a file/folder structure](http://stackoverflow.com/questions/30241118/best-way-to-represent-a-file-folder-structure)

* [How would you implement a Grid in a functional language?](http://stackoverflow.com/questions/27285170/how-would-you-implement-a-grid-in-a-functional-language)

* [OCaml recursive function to apply a function n times](http://stackoverflow.com/questions/15285386/ocaml-recursive-function-to-apply-a-function-n-times)

* [OCaml - Read csv file into array](http://stackoverflow.com/questions/28732351/ocaml-read-csv-file-into-array)


**Lazy Evaluation/ Delayed Evaluation/ Generators / Yield**

* [Are streams in ocaml really used?](http://stackoverflow.com/questions/30197961/are-streams-in-ocaml-really-used)

* [Ocaml: Lazy Lists - How can I make a lazy list representing a sequence of doubling numbers?](http://stackoverflow.com/questions/1631968/ocaml-lazy-lists)

* [Ocaml - Lazy.force](http://stackoverflow.com/questions/18401178/ocaml-lazy-force)

* [Simple Generators](http://stackoverflow.com/questions/13146128/simple-generators)

* [What does `[< >]` mean in OCaml?](http://stackoverflow.com/questions/16150173/what-does-mean-in-ocaml)

* [What is the purpose of OCaml's Lazy.lazy_from_val?](http://stackoverflow.com/questions/9760580/what-is-the-purpose-of-ocamls-lazy-lazy-from-val)

* [converting ocaml function to stream](http://stackoverflow.com/questions/16027627/converting-ocaml-function-to-stream)

* [Lazy “n choose k” in OCaml](http://stackoverflow.com/questions/3969321/lazy-n-choose-k-in-ocaml)

* [Thread safe lazy in OCaml](http://stackoverflow.com/questions/24123294/thread-safe-lazy-in-ocaml)

**SML Dialects**

* [What are the differences between SML and Ocaml?](http://stackoverflow.com/questions/699689/what-are-the-differences-between-sml-and-ocaml)

* [F# changes to OCaml](http://stackoverflow.com/questions/179492/f-changes-to-ocaml)


* [Converting F# seq expressions to OCaml](http://stackoverflow.com/questions/14428420/converting-f-seq-expressions-to-ocaml)


* [IEnumerable<T> in OCaml](http://stackoverflow.com/questions/6589959/ienumerablet-in-ocaml)

* [code compatibility between OCaml and F#](http://stackoverflow.com/questions/4239121/code-compatibility-between-ocaml-and-f)

* [If SML.NET had functors why can't F#?](http://stackoverflow.com/questions/14777522/if-sml-net-had-functors-why-cant-f)


* [Streams (aka “lazy lists”) and tail recursion](http://stackoverflow.com/questions/27045225/streams-aka-lazy-lists-and-tail-recursion)

**Standard Library**

* [How stable and widespread is “OCaml Batteries Included” and is it recommended?](http://stackoverflow.com/questions/3307936/how-stable-and-widespread-is-ocaml-batteries-included-and-is-it-recommended)

* [Cygwin & OCaml: OPAM + Batteries](http://stackoverflow.com/questions/15751851/cygwin-ocaml-opam-batteries)

* [using OCaml Batteries Included as a vanilla cma](http://stackoverflow.com/questions/10667329/using-ocaml-batteries-included-as-a-vanilla-cma?rq=1)

* [What are the pros and cons of Batteries and Core?](http://stackoverflow.com/questions/3889117/what-are-the-pros-and-cons-of-batteries-and-core?rq=1)


**Build / Compile**

* [Building library with ocamlbuild, installing it with ocamlfind - what's the best practice?](http://stackoverflow.com/questions/27771452/building-library-with-ocamlbuild-installing-it-with-ocamlfind-whats-the-best)

* [What is the preferred way to structure and build OCaml projects?](http://stackoverflow.com/questions/5956317/what-is-the-preferred-way-to-structure-and-build-ocaml-projects?rq=1)

* [How to make OCaml bytecode that works on Windows and Linux](http://stackoverflow.com/questions/17315402/how-to-make-ocaml-bytecode-that-works-on-windows-and-linux)

**Tool Chain**

* [IDE for OCaml language](http://stackoverflow.com/questions/14747939/ide-for-ocaml-language)

* [Saving my running toplevel for later](http://stackoverflow.com/questions/3966925/saving-my-running-toplevel-for-later)

* [Is there an enhanced interpreter toploop for OCaml?](http://stackoverflow.com/questions/1849245/is-there-an-enhanced-interpreter-toploop-for-ocaml)

* [Is it possible to use arrow keys in OCaml interpreter?](http://stackoverflow.com/questions/13225070/is-it-possible-to-use-arrow-keys-in-ocaml-interpreter/13225113#13225113)

* [Is it possible to make an opam “sandbox”?](http://stackoverflow.com/questions/27640897/is-it-possible-to-make-an-opam-sandbox)

* [For OCaml, is there a tool to quickly access function or library documentation from the command line?](http://stackoverflow.com/questions/24824849/for-ocaml-is-there-a-tool-to-quickly-access-function-or-library-documentation-f)

* [Annotations in OCaml](http://stackoverflow.com/questions/4063039/annotations-in-ocaml)

* [About the “topfind”?](http://stackoverflow.com/questions/8059657/about-the-topfind)

* [Get ocamlmerlin autocomplete in vim](http://stackoverflow.com/questions/21132371/get-ocamlmerlin-autocomplete-in-vim)

* [Is there a reason to retain .cmo or only .cma?](http://stackoverflow.com/questions/8630599/is-there-a-reason-to-retain-cmo-or-only-cma)


* [Ocaml utop library paths, Core module](http://stackoverflow.com/questions/20927592/ocaml-utop-library-paths-core-module?rq=1)

* [How to trace a program for debugging in OCaml ?](http://stackoverflow.com/questions/9426560/how-to-trace-a-program-for-debugging-in-ocaml)


* [Which is the current setup to use OCaml in Vim?](http://stackoverflow.com/questions/15514908/which-is-the-current-setup-to-use-ocaml-in-vim)


* [How can I use ocamlbrowser with opam packages?](http://stackoverflow.com/questions/26032055/how-can-i-use-ocamlbrowser-with-opam-packages)

**Misc**

* [How do I inteface OCaml with iPhone API?](http://stackoverflow.com/questions/1094866/how-do-i-inteface-ocaml-with-iphone-api)

* [Accessing libraries written in OCaml and C++ from Ruby code](http://stackoverflow.com/questions/13535149/accessing-libraries-written-in-ocaml-and-c-from-ruby-code)


* [Code generation for mathematical
problems](http://stackoverflow.com/questions/13203089/code-generation-for-mathematical-problems)

* [Ocaml for ARM cortex M4?](http://stackoverflow.com/questions/27061506/ocaml-for-arm-cortex-m4)

* [Cross-compiling ocaml apps for ARM](http://stackoverflow.com/questions/9472371/cross-compiling-ocaml-apps-for-arm)

* [How to represent a simple finite state machine in Ocaml?](http://stackoverflow.com/questions/9434050/how-to-represent-a-simple-finite-state-machine-in-ocaml)

* [How to convert numbers among hex, oct, decimal and binary in OCaml?](http://stackoverflow.com/questions/15524450/how-to-convert-numbers-among-hex-oct-decimal-and-binary-in-ocaml)

#### See Also

* [Haskell for OCaml programmers](http://science.raphael.poss.name/haskell-for-ocaml-programmers.html)
* [OCaml for Haskellers](http://blog.ezyang.com/2010/10/ocaml-for-haskellers/)
* [ML Dialects and Haskell: SML, OCaml, F#, Haskell](http://hyperpolyglot.org/ml)
* [A Concise Introduction to Objective Caml](http://www.csc.villanova.edu/~dmatusze/resources/ocaml/ocaml.html)
* [Namespacing Variants in ML](http://keleshev.com/namespacing-variants-in-ml)

* [Haifeng's Random Walk - OCaml: Algebraic Data Types](https://haifengl.wordpress.com/2014/07/07/ocaml-algebraic-data-types/)

* [Appendix B  Variant types and labeled arguments](http://caml.inria.fr/pub/docs/u3-ocaml/ocaml051.html)

* [Thomas Leonard's blog - OCaml Tips](http://roscidus.com/blog/blog/2013/10/13/ocaml-tips/)
* [Thomas Leonard's blog - Experiences With OCaml Objects](http://roscidus.com/blog/blog/2013/09/28/ocaml-objects/)

* [Unreliable Guide to OCaml Modules](http://lambdafoo.com/blog/2015/05/15/unreliable-guide-to-ocaml-modules/)


* [A draft of library to handle physical units in OCaml](http://blog.bentobako.org/index.php?post/2011/12/02/A-draft-of-library-to-handle-physical-units-in-OCaml)

* [Detecting and removing computer virus with OCaml](http://javiermunhoz.com/blog/2014/04/19/detecting-and-removing-computer-virus-with-ocaml.html)

#### Quick Reference Sheets

* https://github.com/OCamlPro/ocaml-cheat-sheets

* [OCaml Cheat Sheets](http://www.ocamlpro.com/blog/2011/06/03/cheatsheets.html)

* https://www.lri.fr/~conchon/IPF/fiches/ocaml-lang.pdf

* http://cl-informatik.uibk.ac.at/teaching/ss06/ocaml/content/w1-2x2.pdf

* https://www.lri.fr/~conchon/IPF/fiches/tuareg-mode.pdf

* [The OCaml Build Process by Example](http://blog.eatonphil.com/2015/03/28/the-ocaml-build-process/)

### References By Subject

**Lazy Evaluation**

* http://cl-informatik.uibk.ac.at/teaching/ws09/fp/ohp/week12-1x2.pdf
* http://typeocaml.com/2014/11/13/magic-of-thunk-lazy/
* http://www.ii.uni.wroc.pl/~lukstafi/pmwiki/uploads/Functional/functional-lecture07.pdf

* [Lazy Evaluation and Stream Processing](http://www.ii.uni.wroc.pl/~lukstafi/pmwiki/uploads/Functional/functional-lecture07.pdf)
    * http://www.ii.uni.wroc.pl/~lukstafi/pmwiki/index.php?n=Functional.Functional

* Lazy Expression Evaluation with Demand Paging / In Virtual Memory Managementhttp://www.ijeat.org/attachments/File/v2i1/A0690092112.pdf

<!--
    ---------------------------------------------------------------
-->
