# Understanding [`zig-bootstrap`]

How does this work?  Why do we currently need this?

It's all about reproducibility.

[`zig-bootstrap`]: https://github.com/ziglang/zig-bootstrap



## Moving parts

Right now, Zig uses LLVM and Clang.  It uses LLVM to emit bitcode and compile
object files.  It uses Clang to parse C and C++.

So, right now, a Zig compiler binary must link against those libraries, and a
few others, too.

`zig-bootstrap` is primarily a tool for building the library objects to link
against, and it does that in a reproducible manner.

(It's secondarily a tool for building a Zig compiler that links with those
reproducible objects.)



## What goes into an object file

Generally speaking, we can imagine that a build creates two related outputs:

  1. Binary format; the literal file on disk
  2. Functionality; what the emitted code does

Assuming our build toolchain works correctly (fingers crossed!), (2) depends
only on the source code and a few simple configuration options.  However, (1)
also depends on:

  - Build system (*e.g.* CMake or Zig)
  - Compiler driver (*e.g.* Clang driver or GCC driver or `zig cc`)
  - Compiler (*e.g.* LLVM or GCC)
  - Linker (*e.g.* LLD or mold)
  - Other things, depending on how pedantic you are

Those dependencies are themselves binary objects, but when they are used to
build other objects, *they are only used for their functionality*.  The output
from *running* those dependencies only depends on the *source code* of those
dependencies.

In other words, when speaking of a Zig compiler, we can say that its
*functionality* depends only on the version numbers of those libraries; and
that the exact binary file also depends on the build system that produced them.

Let's say that an object is **functionality-reproducible** when it does the
right thing.  Let's say it's **binary-reproducible** when it also has
predictable file contents.



## What `zig-bootstrap` does

`zig-bootstrap` bootstraps from *the host build system* to a
*functionality-reproducible Zig/C/C++ compiler*.  Then it makes a
*binary-reproducible Zig/C/C++ compiler*.

- We want LLVM, Clang, LLD, *etc.* built by *e.g.* Zig 0.11+LLVM/Clang 17…
  - These objects should be binary-reproducible
  - But the toolchain producing them (Zig 0.11+LLVM 17) only needs to be
    functionality-reproducible!

- … so we need Zig 0.11+LLVM 17 built by any means necessary
  - This is done using the host build system
  - All that matters is that this build is functional enough to *rebuild* LLVM

To this end, `zig-bootstrap`:

1. Starts from the host build system.  Nothing is reproducible.
2. Builds the library dependencies using the host build system.  Now the
   dependencies are functionality-reproducible.
3. Compiles and links Zig against these dependencies.  Now the Zig/C/C++
   compiler is functionality-reproducible.
4. Rebuilds the dependencies.  Now the dependencies are binary-reproducible.
5. Compiles and links Zig against them.  Now the Zig/C/C++ compiler is
   binary-reproducible.



## Hacking on Zig

This is all confused when hacking on the Zig compiler.  In theory, you could
redo the bootstrap for every change.  In practice, a freshly-hacked Zig compiler
links against binary-reproducible dependencies from an earlier, more stable Zig.



— 2023 October 25
