# Dart Compiler

In dart there are multiple compilers

![compiler](images/dart_compiler.png 'Compiler')

the first 3 compiler are for mobile and desktop platform and last compiler is for Web

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
1. **JIT (Just In Time)**: In this pattern only that part of code is compiled which is needed. suppose you have 10000 line of code  and at a moment you only need 100 line to compile then JIT will compile only thos 100 lines.

In Jit pattern already compiled code are not recompiled, only the new code that you add is compiled. This behaviour is called as incremental compilation. This enable us to use things **Hot Reload** in flutter.

#### Drawbacks of JIT
* JIT does not transform dart code into machine code, but rather into intermediatery Language(for faster development cycle) that can be run by dart virtual machine.
*  We shoult not use JIT in production phase beacuse at that time user does not care about hot reload or fast testing. All he cares about is that app should open on his device as fast as possible.

2. **AOT (Ahead of Time)**: It is recomended to use AOT compiler during production phase because, during this phase we want our app to be compiled in native machine code not intermediatery code. If we are in android then we want our app to be compiled in native android so that device can run it quickly.

In this patter entire code is compiled once, and if you make any chnage then agiain the entire code will compile.

# **How Dart run a program**
Dart Vm is a collection of components aiding towards naitevly executing our dart  code. 

Major components of dart VM are:
**1. The RunTime System**
**2. Development Experiance Component**
**3. JIT and AOT compilation pipelines**

Dart VM provides an executable environment for our dart program. Inside VM code runs in their own isolates. These isolates act like 
Any code in VM is running within some isolate which can be described as an isolated dart universe with its own memory(also know as heap),its own thread of control called the mutatorthread and its own helper thread.

Heap is a garbage collection managed memorystoreage for all the objects allocatedby the code running in this specific isolate. the garbage collector attempts to reclaim memory which was allocted by the program but is no longer refrenced.

Each isloate has a single mutator thread which execute the dart  code but benifits from multiple helper threads which handles VM's internal tasks.

![Dat VM](images/dartVM.png)

Dart Vm can ececute dart apps in 2 ways:
**1. From source by using JIT/AOT compiler**: When you execute dart run command in terminal then, sorce code is converted to binary code and the generated binary code is consumed by dart VM for excution.
* Common front end(CFE): it converts the dart code to kerbel binary
* Kernel Binary(.dill): it conatins the abstract syntax tree of the code. This file is then sent to VM, where its is parsed to create objects representing various program entities like classes,and libraries

Below image describes source code compilation through JIT compiler only.
![Compilation from source code](images/source_compilation.png)  


**2. From snapshots (jit,aot or kernel snapshot)**
The snapshot contains a representation of all those entities allocated in the DartVM heap. Entities that are needed to start the execution process. This heap is traversed and all the objects are serialized into a file called snapshot.

Snapshot is optimized for fast startup time. DartVM can run the code by quickly deserializing the snapshot and accessing all the necessary data.

There are three snapshot types:

* JIT- snapshot
* AOT — snapshot
* Kernel — snapshot
![Snapshot](images/snapshot.png)




# Resources

1. [Medium Article](https://proandroiddev.com/flutters-compilation-patterns-24e139d14177)
2. [Midum Article-Dart VM](https://medium.com/@flutterdevs/compilers-and-snapshots-in-dart-3abdd1ee56e9)

