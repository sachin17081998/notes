# Dart Compiler

In dart there are multiple compilers

![compiler](images/dart_compiler.png 'Compiler')

the dirst 3 compiler are for mobile and desktop platform and last compiler is for Web

During application development there are two importanat phases
1. **Development Phase**: In this phase you write your code. During this phase you generally want the following things:
  * Easy to test and debug code
  * Live Metric Support
  * Fast Coding style
  * incremental recompilation

2. **Production Phase**: In this phase developed apps are run and used at real devices. During this phase we generally want following things:
  * App should start and run fast
  * App doesnt need extra debugging feature
  * No need of hot reload
  * app should be compiled in machine code

In Dart there are two types of compilation pattern.
1. **JIT (Just In Time)**: In this pattern only that part of code is compiled which is needed. suppose you have 10000 line of code and you need and at a moment you only need 100 line to compile then JIT will compile only thos 100 lines.

In Jit pattern already compiled code are not recompiled, only the new code that you add is compiled. This behaviour is called as incremental compilation. This enable us to use things **Hot Reload** in flutter.

#### Drawbacks of JIT
* JIT does not transform dart code into machine code, but rather into intermediatery Language(for faster development cycle) that can be run by dart virtual machine.
*  We shoult not use JIT in production phase beacuse at that time user does not care about hot reload or fast testing. All he cares about is that app should open on his device as fast as possible.

2. **AOT (Ahead of Time)**: It is recomended to use AOT compiler during production phase because, during this phase we want our app to be compiled in native machine code not intermediatery code. If we are in android then we want our app to be compiled in native android so that device can run it quickly.

In this patter entire code is compiled once, and if you make any chnage then agiain the entire code will compile.


# Resources

1. [Medium Article](https://proandroiddev.com/flutters-compilation-patterns-24e139d14177)

