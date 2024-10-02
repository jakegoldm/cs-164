# CS 164: Compilers #

Fall 2024, Mike Vanier

Textbook: "Essentials of Compilation: An incremental Approach in Racket" by Jeremy Siek

[Course Website](https://mvanier.github.io/cs164/Fall2024/book/)

## Lecture 1: Introduction ## 

A compiler is a computer program that translates source code in some language into machine code that can be run directly on a computer. There would be no computer revolution without compilers. Everything "practical" in CS depends on compilers either directly (usually) or indirectly. 

All compilers are composed of "passes" which are transformations of the code. Most compilers only have a few passes. Instead of having a few complex passes, the "nanopass" approach has many simple passes. Each pass is very short and only does one thing, allowing it to be tested in isolation from all other passes. Some passes have boilerplate code, which can be tedious to write. An example of a pass could be "convert `and` and `or` forms into equivalent `if` expressions."

Using custom "source" language. Sample input code:
```
(define (add (x : Integer) (y : Integer)) : Integer
    (+ x y))
(add 40 2)
```

Compilers we will write:  
* Variables and Arithmetic
* Register Allocation
* Conditionals
* Loops and Dataflow Analysis
* Tuples and Garbage Collection
* Functions

The target language will be Intel x86-64 assembly language. The output files can be compiled to machine code on any x86 processor with a few addtional tools (assembler, linker, etc...). Compilers will be written in OCaml. 

## Lecture 2: Tools of the Trade ##

OCaml will be a fundamental tool (as well as opam package manager). GNU make and the dune compilation manager will be used for building. 

The first step in any compiler is generating an "abstract syntax tree" or AST. This is done by the parser. The AST should correspond to the syntax of the language being compiled in a 1-1 manner, but in a form which is easy for the programming language to manipulate. The "tree" in AST is becuase the datatypes are often recursive, and a single expression forms a tree of "nodes." 

Early compilers will only have a few Intermediate Languages (ILs). MOst of these will invovle large-scale changes to the program. Each pass corresponds to a new IL. 

Boilerplate code is "just-to-make-the-recursion-work" for algebraic datatypes. 

Polymrophic variants are more flexible versions of algebraic datatypes. Some authors (Jacques Garrigue) recommend using polymorphic variants for ASTs in order to reduce boilerplate. They are, however, trickier to work with. 

Another little-known feature of OCaml is that it has programmable preprocessors (basically a macro system) called PPXs. A PPX is a program that can analyze the AST of OCaml itself and transform it in arbitrary ways. The PPX will generate the code to convert arbitrary OCaml datatypes to S-expressions (nested lists of symbols). This is very useful for inspecting compiler outputs after each pass. Use the `[@@deriving sexp]` annotation. 