# How to bootstrap C++

I've got plans to bootstrap a C compiler from machine code.  My first step has
been designing and implementing a low-level language, Topple.  The next steps
will produce another, more complicated language, and eventually a C compiler.

This plan will work and I'll be able to compile *e.g.* GNU coreutils, or maybe
part of a BSD.  But the system that will result is just a sophisticated toy.
Lots of software is written in C++, not C.  The bootstrapping project I
currently have planned is suitable as a foundation to build on, but it's not
enough to directly build modern software.

There needs to be a C++ compiler.

(I don't intend to build one.)

To my knowledge, there has never been a direct bootstrap from C to C++.  All of
today's C++ compilers are written in C++.  The bootstrap chain ends at cfront,
which transformed C++ code into C code, but which was itself written in C++.

## Why are C++ compilers complicated?

Because C++ is finicky.  The specification is long and very tricky.  And
because compilers have optimizers.

Ultimately I don't think C++ is a very good language for writing compilers in.
Maybe it's the best one that *exists* (it's certainly the most popular), but
that doesn't mean it's *good*.

The core LLVM 15 library is roughly 1.4 million lines of code.  It takes hours
to compile.

I don't think they need to be that complicated.  A new compiler can and should
be much simpler, especially since GCC and LLVM already exist if you need them.
Ultra-fast code needn't be a priority.  Ultra-fast compilers *should* be.

How can we design a clean C++ compiler?

## C++ bytecode

I don't have this all worked out.  But I suspect that a bytecode would work
well.  Compiling C++ to machine code is really hard, especially with exception
handling.  Figuring out a custom bytecode to handle destructors and method
dispatch sounds like a better idea than just jumping in and trying to build
stuff.

This is especially tempting because C++ has no defined ABI!  There's no reason
to get bogged down in platform details because (I think) they mostly can be
abstracted away.

Here's my proposed design:

- parser: C++ source => bytecode
- type checker: bytecode => OK?
- linker: bytecode => bytecode
- virtual machine, probably written in C

The type checker and linker could also do some small optimizations, like
method call lookup.  But the bytecode produced from parsing should be usable as
is.

This design hinges on the cleanliness of the bytecode.  A C++ bytecode needs at
least:

- function definition
- class definition
- enumeration definition
- literal materialization
- assignment
- class construction
- member access
- function and named method dispatch
- operator dispatch (`+`, `-`, `(...)`, *etc.*)
- pointer, address, and reference operations
- casting (C-style, const, static, reinterpret)
- control flow (`if`, `switch`, `for`, `while`, and friends)
- exception handling (`throw` expressions and `catch` blocks)

I think it might be possible to mostly ignore templates and simply compile
their bodies once.  Obviously there are uncountable corner cases but I think
it's an interesting design goal.

## Challenges / How to implement

Challenges?  Writing a C++ compiler?  Surely not.

This design lives and dies on its **bytecode**.  I wouldn't write a single line
of compiler code until I'd fleshed out the instruction set and
**hand-compiled** hundreds of examples to experiment.  I probably wouldn't even
choose a language to write the compiler in until I'd mostly figured out the
bytecode.  The instructions I've outlined above certainly aren't enough, not
even close.

Moreover, the bytecode needs to handle nearly **100% of C++**.  That's
including the hard parts of the language.  It doesn't make sense to try to
guess which features aren't necessary.

You'll need to figure out a specific version of the language to target.  I'd
choose **C++03**, since that's enough to bootstrap GCC at the time of writing.
But I'd also try to make sure the bytecode is flexible enough to expand to
later versions of the standard.

I'd expect the virtual machine to be written in C.  That's a reasonable
restriction and guides which kinds of bytecode instructions might appear.

There needs to be a custom **object file format** to handle this bytecode.  I
don't think there's any way to use *e.g.* ELF without basically inventing a new
format anyway.  This necessitates, among other things, a **custom linker** for
that custom format.

In order to compile a native executable, the VM will probably declare
`extern ByteCode bc[];` or something similar.  That means the system linker
will need to link against the bytecode, which probably necessitates a **system
object file emitter**.  (This isn't definitely necessary but I'm sure that
compiling *e.g.* GCC would produce millions of instructions, and I'm not
confident that a typical C compiler could handle hundreds of megabytes of
source code.)

Speaking of which, at some point the virtual machine will need to **call native
code**.  Might be libc, might be external C code, might be assembly (opaque
native object files).  I don't know how to approach this because I don't know
where this boundary is in practice.  But given how much regular C++ library
code lives in the LLVM and GCC source trees, including custom data structures,
I'm optimistic that there's less native code necessary than you might think.

Speaking of *that*, the **C++ standard library** is absolutely wildly,
unfathomably enormous.  It seems impossible for one person to implement alone.
It seems like a better idea to reuse another project's standard library.  But
that might raise the language requirements.  Besides, with cleanliness a higher
priority important than runtime speed, reusing a standard library might be
counterproductive anyway.

## Conclusion

Despite all these challenges and details, I'd love to see this kind of project.
I think the world could really use a small, understandable C++ implementation.
I want to know what clean and approachable tools would look like in this space.

This is absolutely worth exploring and experimenting with.

— 2023 March 9
