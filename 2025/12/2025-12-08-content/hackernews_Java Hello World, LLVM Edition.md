---
source: hackernews
title: Java Hello World, LLVM Edition
url: https://www.javaadvent.com/2025/12/java-hello-world-llvm-edition.html
date: 2025-12-08
---

[Skip to the content](#site-content)

* [Home](https://www.javaadvent.com/)
* [About](https://www.javaadvent.com/about-page)
* [Calendar](https://www.javaadvent.com/calendar)
* [Archive](https://www.javaadvent.com/archive)
* [Sponsors](https://www.javaadvent.com/sponsors)
* Search

* [Home](https://www.javaadvent.com/)
* [About](https://www.javaadvent.com/about-page)
* [Calendar](https://www.javaadvent.com/calendar)
* [Archive](https://www.javaadvent.com/archive)
* [Sponsors](https://www.javaadvent.com/sponsors)

Search

[JVM Advent](https://www.javaadvent.com)

The JVM Programming Advent Calendar

![](https://i0.wp.com/www.javaadvent.com/content/uploads/2021/12/Feature-Image-Day-7.png?fit=800%2C800&ssl=1)

December 7, 2025

# Java Hello World, LLVM Edition

After exploring Java bytecode in previous years ([2022](https://www.javaadvent.com/2022/12/jvm-hello-world.html), [2023](https://www.javaadvent.com/2023/12/my-first-compiler.html), [2024](https://www.javaadvent.com/2024/12/peering-through-the-peephole-build-a-peephole-optimiser-using-the-new-java-class-file-api.html)), this year we’ll take an unexpected detour for a Java advent: instead of generating Java bytecode, we’ll use Java to build and execute [LLVM IR](https://llvm.org/docs/LangRef.html), the intermediate language behind compilers like clang.

Using Java’s [Foreign Function & Memory (FFM) API](https://docs.oracle.com/en/java/javase/22/core/foreign-function-and-memory-api.html), we’ll call the LLVM C API, generate a “Hello, World!” program, and even JIT-compile it to native code – all from Java.

The task is simple: create a program that simply prints “Hello, World!”. But we must do this from Java via LLVM.

## What is LLVM?

The [LLVM Project](https://llvm.org/), a collection of modular compiler and toolchain technologies, began as a research project over 20 years ago at the University of Illinois. It has grown significantly, underpinning many compilers and tools like clang.

The core libraries provide a source & target independent optimizer along with code generation for a multitude of target machines. They are built around the [LLVM IR](https://llvm.org/docs/LangRef.html), an intermediate representation, which we’ll generate & execute from Java.

## Installing LLVM

To use the LLVM C API from Java, we’ll need LLVM’s shared libraries and headers installed locally. There is an automatic installation script available to easily install LLVM on Ubuntu/Debian systems, for example to install LLVM 20:

```

$ wget https://apt.llvm.org/llvm.sh
$ chmod +x llvm.sh
$ ./llvm.sh 20
```

Once we have LLVM installed we can use the LLVM tooling to execute textual-form LLVM IR and we’ll also be able to use the LLVM C API in Java via the FFM API.

## LLVM IR

LLVM IR is a strongly-typed, SSA-based intermediate language. It abstracts away most machine-specific details, making it easier to represent high-level constructs in a compiler-friendly format. There are three equivalent representations of the IR: an in-memory format, a bitcode format for serialisation and [a human readable assembly language representation](http://llvm.org/docs/LangRef.html).

The textual form of the LLVM IR for our “Hello, World!” looks like this:

```

@str = private constant [14 x i8] c"Hello, World!\00"

declare i32 @puts(ptr)

define i32 @main() {
  call i32 @puts(ptr @str)
  ret i32 0
}
```

Eventually, we’ll generate this via Java but, for now, if you save this in a file called helloworld.ll you can try executing it with the LLVM interpreter, [lli](https://llvm.org/docs/CommandGuide/lli.html):

```

$ lli helloworld.ll
Hello, World!
```

There are a few types of entities used in the helloworld.ll example:

* A global variable containing the string “Hello World!”
* A declaration of the external [libc puts](https://man7.org/linux/man-pages/man3/puts.3.html) function
* A definition of the main function
* Instructions to call puts and return an integer exit code

You can dive deeper into the [LLVM “Hello, World!” example here](https://jameshamilton.eu/programming/llvm-hello-world) if you like before continuing to the next section, where we’ll start using the Java FFM API.

## What is the Java FFM API?

The [Foreign Function and Memory (FFM) API](https://docs.oracle.com/en/java/javase/22/core/foreign-function-and-memory-api.html) enables Java programs to interoperate with code and data outside the Java runtime. The API is a replacement for the older JNI API that enables Java programs to call native libraries in a safer way. The API can be used to call foreign functions and safely access foreign memory that is not managed by the JVM.

A companion to the FFM API is a tool named [jextract](https://docs.oracle.com/en/java/javase/21/core/call-native-functions-jextract.html) that can automatically generate Java bindings from a C header file. `jextract` parses C header files and automatically generates the Java source code with method handles and type-safe FFM bindings.

We’ll use the `jextract` tool to generate bindings for the LLVM C API and those bindings will allow us to call the LLVM API from Java.

## Getting started

First, let’s create a simple project to start. We’ll use maven to build our project but you can use another build tool if you like, it’s not important:

```

$ mvn archetype:generate -DgroupId=com.example -DartifactId=jvm-llvm-helloworld -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
```

Once you have a project skeleton, update the pom.xml file to set the Java version >= 22:

```

 <properties>
    <maven.compiler.source>25</maven.compiler.source>
    <maven.compiler.target>25</maven.compiler.target>
 </properties>
```

Then build and run the program to check everything is OK:

```

$ mvn clean install
$ java -cp target/jvm-llvm-helloworld-1.0-SNAPSHOT.jar com.example.App
Hello World!
```

The maven generated sample already printed “Hello, World!” but that’s too easy! We’ll remove that and generate it via LLVM in the following sections.

Let’s now create the LLVM bindings using `jextract` so that we can use the LLVM API.

## Creating LLVM bindings

We’ll use jextract to generate bindings from the LLVM C API header files. Make sure LLVM is available on your system (see Installing LLVM above) and you’ll also need to download [jextract](https://docs.oracle.com/en/java/javase/21/core/call-native-functions-jextract.html).

The following jextract command (on Linux) will create Java bindings for the specified LLVM C headers, placing the generated code into the `com.example.llvm` package within the `src/main/java` directory, with the main header class named `LLVM`.

```

$ jextract -l LLVM-20 -I /usr/include/llvm-c-20 \
     -I /usr/include/llvm-20 \
     -t com.example.llvm \
     --output src/main/java \
     --header-class-name LLVM \
     /usr/include/llvm-c-20/llvm-c/Core.h \
     /usr/include/llvm-c-20/llvm-c/Support.h \
     /usr/include/llvm-c-20/llvm-c/ExecutionEngine.h \
     /usr/include/llvm-c-20/llvm-c/Target.h \
     /usr/include/llvm-c-20/llvm-c/TargetMachine.h
```

To test the generated bindings, let’s print the LLVM version using the static method generated for LLVM version string constant: edit the sample’s App.java file to print the version using the following:

If you run this, you’ll see the LLVM version printed:

```

$ java -cp target/jvm-llvm-helloworld-1.0-SNAPSHOT.jar --enable-native-access=ALL-UNNAMED com.example.App
LLVM version: 20.0.0
```

Note the use of `--enable-native-access=ALL-UNNAMED` to prevent warnings about native code access; I’ll omit this for brevity in later commands.

## Memory Segments

The `LLVM_VERSION_STRING` method returns a [MemorySegment](https://docs.oracle.com/en/java/javase/22/docs/api/java.base/java/lang/foreign/MemorySegment.html) rather than a Java String. In the FFM API, a `MemorySegment` represents a contiguous region of memory—either on or off the Java heap—enabling safe, structured access to native memory.

Let’s take a look at the implementation in the generated source file:

```

   public static MemorySegment LLVM_VERSION_STRING() {
    class Holder {
      static final MemorySegment LLVM_VERSION_STRING
         = LLVM.LIBRARY_ARENA.allocateFrom("20.0.0");
    }
    return Holder.LLVM_VERSION_STRING;
  }
```

This method allocates memory containing the version string that contains the version number. The allocated MemorySegment is returned from the method and to get the String back into Java-land we need to call `getString(0)` on the memory segment which reads a null-terminated string at the given offset (`0`), using the UTF-8 charset.

Memory segments are managed through arenas (such as the `LLVM.LIBRARY_ARENA` in the code above), which bridge Java’s managed heap and foreign memory spaces by applying familiar resource management patterns like try-with-resources.

Since we’ll need to allocate native memory, let’s declare an [Arena](https://docs.oracle.com/en/java/javase/22/docs/api/java.base/java/lang/foreign/Arena.html):

```

 public static void main(String[] args)
 {
    try (Arena arena = Arena.ofConfined()) {
       // TODO
    }
 }
```

## Creating an LLVM module

As a reminder, we need to recreate the following LLVM IR via the LLVM C API:

```

declare i32 @puts(ptr)

@str = constant [14 x i8] c"Hello, World!\00"

define i32 @main() {
  call i32 @puts(ptr @str)
  ret i32 0
}
```

Let’s start by creating an LLVM module – the container for all functions and globals – and print it so that we can run it through the LLVM interpreter:

```

public static void main(String[] args)
{
  try (Arena arena = Arena.ofConfined()) {
    var module = LLVMModuleCreateWithName(arena.allocateFrom("hello"));

    // TODO: Fill in the module

  	var llvmIrCharPtr = LLVMPrintModuleToString(module);

    try {
      System.out.println(llvmIrCharPtr.getString(0));
    } catch (Exception e) {
      System.err.println("Failed to write LLVM IR: failed to get error message: " + e.getMessage());
    }

	   // Clean up LLVM resources
     LLVMDisposeMessage(llvmIrCharPtr);
     LLVMDisposeModule(module);
  }
}
```

If we execute this now, we’ll see an empty IR module:

```

$ java -cp target/jvm-llvm-helloworld-1.0-SNAPSHOT.jar com.example.App
; ModuleID = 'hello'
source_filename = "hello"
```

If you pass this output through the LLVM interpreter, you’ll see that it tries to execute the module but cannot find the entry point main function:

```

$ java -cp target/jvm-llvm-helloworld-1.0-SNAPSHOT.jar com.example.App | lli
Symbols not found: [ main ]
```

We now have an LLVM module, but it has no executable code – the interpreter rightly complains that main is missing; so let’s add the main function.

## Adding a main function

The entry point to our program is the function named main which takes no parameters and returns an integer exit code, where a non-negative integer denotes success. We can add a function to the module using the [LLVMAddFunction](https://llvm.org/doxygen/group__LLVMCCoreModule.html#gaaf70ab92a261e636dc0b2cf30cfede9a) function, along with the [LLVMFunctionType](https://llvm.org/doxygen/classllvm_1_1FunctionType.html) and [LLVMInt32Type](https://llvm.org/doxygen/group__LLVMCCoreTypeInt.html#ga71ee1444644798c8750ffb5be6a06819) functions to create the function type.

Notice that all of these functions return a `MemorySegment` and all 3 `LLVMAddFunction` parameters are `MemorySegment`s.

```

public static void main(String[] args)
{
  try (Arena arena = Arena.ofConfined()) {
    var module = LLVMModuleCreateWithName(arena.allocateFrom("hello"));

    // Create main function signature: int main()
    var int32Type = LLVMInt32Type();
    var mainType = LLVMFunctionType(int32Type, NULL, 0, 0);
    var mainName = arena.allocateFrom("main");
    var mainFunc = LLVMAddFunction(module, mainName, mainType);

    // TODO: Add the code

  	var llvmIrCharPtr = LLVMPrintModuleToString(module);

    try {
      System.out.println(llvmIrCharPtr.getString(0));
    } catch (Exception e) {
      System.err.println("Failed to write LLVM IR: failed to get error message: " + e.getMessage());
    }

	   // Clean up LLVM resources
     LLVMDisposeMessage(llvmIrCharPtr);
     LLVMDisposeModule(module);
  }
}
```

If you execute this now you’ll see a declaration of the main function but it has no body so the LLVM interpreter will produce the same error:

```

$ java -cp target/jvm-llvm-helloworld-1.0-SNAPSHOT.jar com.example.App
; ModuleID = 'hello'
source_filename = "hello"

declare i32 @main()

$ java -cp target/jvm-llvm-helloworld-1.0-SNAPSHOT.jar com.example.App|lli
Symbols not found: [ main ]
```

Next we’ll add some instructions to the body of the function.

## Adding an entry basic block

In order to add code to a function we need to add at least 1 basic block – the entry block. A basic block is a sequence of instructions within a function that executes straight through from start to finish, with no branches in the middle. These blocks form the nodes of the Control-Flow Graph (CFG), and they connect to each other based on how control flows between them.

Basic blocks can be added to a function with the [LLVMAppendBasicBlock](https://llvm.org/doxygen/group__LLVMCCoreValueBasicBlock.html#gaf1760061b837b6b255224f243cfe94c8) function:

```

public static void main(String[] args)
{
  try (Arena arena = Arena.ofConfined()) {
    var module = LLVMModuleCreateWithName(arena.allocateFrom("hello"));

    // Create main function signature: int main()
    var int32Type = LLVMInt32Type();
    var mainType = LLVMFunctionType(int32Type, NULL, 0, 0);
    var mainName = arena.allocateFrom("main");
    var mainFunc = LLVMAddFunction(module, mainName, mainType);

	  var entry = LLVMAppendBasicBlock(mainFunc, arena.allocateFrom("entry"));

	  // TODO: Add the instructions

  	var llvmIrCharPtr = LLVMPrintModuleToString(module);

    try {
      System.out.println(llvmIrCharPtr.getString(0));
    } catch (Exception e) {
      System.err.println("Failed to write LLVM IR: failed to get error message: " + e.getMessage());
    }

	   // Clean up LLVM resources
     LLVMDisposeMessage(llvmIrCharPtr);
     LLVMDisposeModule(module);
  }
}
```

If you run the program through `lli` now, you’ll see a different error:

```

$ java -cp target/jvm-llvm-helloworld-1.0-SNAPSHOT.jar com.example.App | lli
lli: <stdin>:6:1: error: expected instruction opcode
}
```

That makes sense, we don’t yet have any instructions in our function!

## Building instructions

To add instructions, we first create an instruction builder using the [LLVMCreateBuilder](https://llvm.org/doxygen/group__LLVMCCoreInstructionBuilder.html#ga0b336d71db0aa80eef35fe0572ca69bb) function. This gives us an LLVMBuilder that we can use to insert new instructions into a basic block.

We’ll also use the [LLVMPositionBuilderAtEnd](https://llvm.org/doxygen/group__LLVMCCoreInstructionBuilder.html#gab9bdbf21d7fd0bc5a2ee669b333ced2a) function to position the builder at the end of the entry block and [LLVMBuildRet](https://llvm.org/doxygen/group__LLVMCCoreInstructionBuilder.html#gab246fd9294b3801060f0ceb972c262d0) to build a return instruction:

```

public static void main(String[] args)
{
  try (Arena arena = Arena.ofConfined()) {
    var module = LLVMModuleCreateWithName(arena.allocateFrom("hello"));

    // Create main function signature: int main()
    var int32Type = LLVMInt32Type();
    var mainType = LLVMFunctionType(int32Type, NULL, 0, 0);
    var mainName = arena.allocateFrom("main");
    var mainFunc = LLVMAddFunction(module, mainName, mainType);

	  var entry = LLVMAppendBasicBlock(mainFunc, arena.allocateFrom("entry"));
 	  var builder = LLVMCreateBuilder();
    LLVMPositionBuilderAtEnd(builder, entry);

    // TODO: Call puts “Hello, World!”

	  // Return 0
    LLVMBuildRet(builder, LLVMConstInt(int32Type, 0, 0));

  	var llvmIrCharPtr = LLVMPrintModuleToString(module);

    try {
      System.out.println(llvmIrCharPtr.getString(0));
    } catch (Exception e) {
      System.err.println("Failed to write LLVM IR: failed to get error message: " + e.getMessage());
    }

	   // Clean up LLVM resources
     LLVMDisposeMessage(llvmIrCharPtr);
     LLVMDisposeBuilder(builder);
     LLVMDisposeModule(module);
  }
}
```

If you run the program and pass the output through `lli` now, you’ll see nothing happen:

```

$ java -cp target/jvm-llvm-helloworld-1.0-SNAPSHOT.jar com.example.App | lli
```

Great news – the errors are gone! Checking the return code confirms the program exited successfully, returning 0.

```

$ echo $?
0
```

Try changing the 0 to some other number to confirm that the value is indeed coming from the exit code returned by the LLVM IR program!

## Global variables

A global variable, defined at the top-level in LLVM IR, defines a region of memory with a fixed address that is allocated when the program is loaded, rather than dynamically at runtime. Globals can be declared as constant if their values will never change.

We’ll add the string “Hello, World!” to our LLVM program as a global constant.

```

public static void main(String[] args)
{
  try (Arena arena = Arena.ofConfined()) {
    var module = LLVMModuleCreateWithName(arena.allocateFrom("hello"));

    // Create main function signature: int main()
    var int32Type = LLVMInt32Type();
    var mainType = LLVMFunctionType(int32Type, NULL, 0, 0);
    var mainName = arena.allocateFrom("main");
    var mainFunc = LLVMAddFunction(module, mainName, mainType);

	  var entry = LLVMAppendBasicBlock(mainFunc, arena.allocateFrom("entry"));
 	  var builder = LLVMCreateBuilder();
    LLVMPositionBuilderAtEnd(builder, entry);

	  // Create a global string constant containing "Hello, World!"
    var helloStr =
           LLVMBuildGlobalStringPtr(builder,
                arena.allocateFrom("Hello, World!"),
                arena.allocateFrom("hello_str"));

    // TODO: Call puts “Hello, World!”

	  // Return 0
    LLVMBuildRet(builder, LLVMConstInt(int32Type, 0, 0));

  	var llvmIrCharPtr = LLVMPrintModuleToString(module);

    try {
      System.out.println(llvmIrCharPtr.getString(0));
    } catch (Exception e) {
      System.err.println("Failed to write LLVM IR: failed to get error message: " + e.getMessage());
    }

	   // Clean up LLVM resources
     LLVMDisposeMessage(llvmIrCharPtr);
     LLVMDisposeBuilder(builder);
     LLVMDisposeModule(module);
  }
}
```

We don’t use the `hello_str` yet so running `lli` would produce the same as before, but you can see the string is now declared in the LLVM IR (prefixed with @ because it is a global, like the main function):

```

$ java -cp target/jvm-llvm-helloworld-1.0-SNAPSHOT.jar com.example.App
; ModuleID = 'hello'
source_filename = "hello"

@hello_str = private unnamed_addr constant [14 x i8] c"Hello, World!\00", align 1

define i32 @main() {
entry:
  ret i32 0
}
```

Let’s add the final instruction next – a call to `puts` to print the string.

## Calling functions

Before we can call the libc puts function we must declare it in the module by first building the function type and then calling `LLVMAddFunction` to add it to the module:

```

public static void main(String[] args)
{
  try (Arena arena = Arena.ofConfined()) {
    var module = LLVMModuleCreateWithName(arena.allocateFrom("hello"));

    // Create main function signature: int main()
    var int32Type = LLVMInt32Type();
    var mainType = LLVMFunctionType(int32Type, NULL, 0, 0);
    var mainName = arena.allocateFrom("main");
    var mainFunc = LLVMAddFunction(module, mainName, mainType);

	  var entry = LLVMAppendBasicBlock(mainFunc, arena.allocateFrom("entry"));
 	  var builder = LLVMCreateBuilder();
    LLVMPositionBuilderAtEnd(builder, entry);

	  // Create a global string constant containing "Hello, World!"
    var helloStr =
           LLVMBuildGlobalStringPtr(builder,
                arena.allocateFrom("Hello, World!"),
                arena.allocateFrom("hello_str"));

    // Create puts function type: int puts(char*)
    var putsParamTypes = arena.allocate(ADDRESS, 1);
    var charPtrType = LLVMPointerType(LLVMInt8Type(), 0);
    putsParamTypes.set(ADDRESS, 0, charPtrType);
    var putsType = LLVMFunctionType(int32Type, putsParamTypes, 1, 0);
    // Add puts function to the module
    var putsFunc = LLVMAddFunction(module, arena.allocateFrom("puts"), putsType);

    // TODO: Call puts “Hello, World!”

	  // Return 0
    LLVMBuildRet(builder, LLVMConstInt(int32Type, 0, 0));

  	var llvmIrCharPtr = LLVMPrintModuleToString(module);

    try {
      System.out.println(llvmIrCharPtr.getString(0));
    } catch (Exception e) {
      System.err.println("Failed to write LLVM IR: failed to get error message: " + e.getMessage());
    }

	   // Clean up LLVM resources
     LLVMDisposeMessage(llvmIrCharPtr);
     LLVMDisposeBuilder(builder);
     LLVMDisposeModule(module);
  }
}
```

Now that we’ve declared the function we can call it with the `@hello_str` global as a parameter using the [LLVMBuildCall2](https://llvm.org/doxygen/group__LLVMCCoreInstructionBuilder.html#ga40cf5b22d9d28f1f82e76048a69d537a) function:

```

public static void main(String[] args)
{
  try (Arena arena = Arena.ofConfined()) {
    var module = LLVMModuleCreateWithName(arena.allocateFrom("hello"));

    // Create main function signature: int main()
    var int32Type = LLVMInt32Type();
    var mainType = LLVMFunctionType(int32Type, NULL, 0, 0);
    var mainName = arena.allocateFrom("main");
    var mainFunc = LLVMAddFunction(module, mainName, mainType);

	  var entry = LLVMAppendBasicBlock(mainFunc, arena.allocateFrom("entry"));
 	  var builder = LLVMCreateBuilder();
    LLVMPositionBuilderAtEnd(builder, entry);

	  // Create a global string constant containing "Hello, World!"
    var helloStr =
           LLVMBuildGlobalStringPtr(builder,
                arena.allocateFrom("Hello, World!"),
                arena.allocateFrom("hello_str"));

    // Create puts function type: int puts(char*)
    var putsParamTypes = arena.allocate(ADDRESS, 1);
    var charPtrType = LLVMPointerType(LLVMInt8Type(), 0);
    putsParamTypes.set(ADDRESS, 0, charPtrType);
    var putsType = LLVMFunctionType(int32Type, putsParamTypes, 1, 0);
    // Add puts function to the module
    var putsFunc = LLVMAddFunction(module, arena.allocateFrom("puts"), putsType);

    // Create puts function call
    var callArgs = arena.allocate(ADDRESS, 1);
    callArgs.set(ADDRESS, 0, helloStr);
    LLVMBuildCall2(builder, putsType, putsFunc, callArgs, 1, arena.allocateFrom("puts"));

	  // Return 0
    LLVMBuildRet(builder, LLVMConstInt(int32Type, 0, 0));

  	var llvmIrCharPtr = LLVMPrintModuleToString(module);

    try {
      System.out.println(llvmIrCharPtr.getString(0));
    } catch (Exception e) {
      System.err.println("Failed to write LLVM IR: failed to get error message: " + e.getMessage());
    }

	   // Clean up LLVM resources
     LLVMDisposeMessage(llvmIrCharPtr);
     LLVMDisposeBuilder(builder);
     LLVMDisposeModule(module);
  }
}
```

Running the program’s output through `lli` will finally display the expected result: “Hello, World!”:

```

$ java -cp target/jvm-llvm-helloworld-1.0-SNAPSHOT.jar com.example.App | lli
Hello, World!
```

Congratulations, you’ve successfully used the Java FFM API to call the LLVM C API to build an LLVM module that contains code to print “Hello, World!”.

## Just-in-time (JIT) Compilation

So far, we’ve been printing LLVM IR and letting `lli` execute it. But LLVM also exposes a JIT compiler API, allowing us to generate and execute machine code in-memory. Let’s see how to JIT our “Hello, World!” directly from Java.

LLVM IR is target independent but once we start compiling to native code we must know which machine we are targeting. We’ll target x86 Linux in the following code; if you’re using ARM, Mac or Windows you’ll need to adjust the code for your machine.

The first step is to initialise and create an LLVM JIT compiler for the target machine:

```

public static void main(String[] args)
{
  try (Arena arena = Arena.ofConfined()) {
    var module = LLVMModuleCreateWithName(arena.allocateFrom("hello"));

    // Create main function signature: int main()
    var int32Type = LLVMInt32Type();
    var mainType = LLVMFunctionType(int32Type, NULL, 0, 0);
    var mainName = arena.allocateFrom("main");
    var mainFunc = LLVMAddFunction(module, mainName, mainType);

	  var entry = LLVMAppendBasicBlock(mainFunc, arena.allocateFrom("entry"));
 	  var builder = LLVMCreateBuilder();
    LLVMPositionBuilderAtEnd(builder, entry);

	  // Create a global string constant containing "Hello, World!"
    var helloStr =
           LLVMBuildGlobalStringPtr(builder,
                arena.allocateFrom("Hello, World!"),
                arena.allocateFrom("hello_str"));

    // Create puts function type: int puts(char*)
    var putsParamTypes = arena.allocate(ADDRESS, 1);
    var charPtrType = LLVMPointerType(LLVMInt8Type(), 0);
    putsParamTypes.set(ADDRESS, 0, charPtrType);
    var putsType = LLVMFunctionType(int32Type, putsParamTypes, 1, 0);
    // Add puts function to the module
    var putsFunc = LLVMAddFunction(module, arena.allocateFrom("puts"), putsType);

    // Create puts function call
    var callArgs = arena.allocate(ADDRESS, 1);
    callArgs.set(ADDRESS, 0, helloStr);
    LLVMBuildCall2(builder, putsType, putsFunc, callArgs, 1, arena.allocateFrom("puts"));

	  // Return 0
    LLVMBuildRet(builder, LLVMConstInt(int32Type, 0, 0));

    // Initialize LLVM JIT + x86 Target
    LLVMLinkInMCJIT();
    LLVMInitializeX86Target();
    LLVMInitializeX86TargetInfo();
    LLVMInitializeX86TargetMC();
    LLVMInitializeX86AsmPrinter();
    LLVMInitializeX86AsmParser();

    // Create JIT execution engine
    var jitCompiler = arena.allocate(ADDRESS);
    var jitErrorMsgPtrPtr = arena.allocate(ADDRESS);
    LLVMCreateJITCompilerForModule(jitCompiler, module, /* optimization level = */ 2, jitErrorMsgPtrPtr);

    // Disable the IR printing now
  	// var llvmIrCharPtr = LLVMPrintModuleToString(module);
    //
    // try {
    //  System.out.println(llvmIrCharPtr.getString(0));
    // } catch (Exception e) {
    //   System.err.println("Failed to write LLVM IR: failed to get error message: " + e.getMessage());
    // }

	   // Clean up LLVM resources
     // LLVMDisposeMessage(llvmIrCharPtr);
     LLVMDisposeBuilder(builder);
     LLVMDisposeModule(module);
  }
}
```

`LLVMCreateJITCompilerForModule` sets up a JIT execution engine to compile an LLVM module to native machine code. `LLVMCreateJITCompilerForModule` will return a 1 upon failure and then we can check the error message string for more information but to simplify things we’ll ignore error handling for now.

Requesting the address of the main function triggers its compilation – LLVM generates the machine code only when it’s first needed, hence the name Just-In-Time compilation. We can retrieve a pointer to the compiled function using `LLVMGetPointerToGlobal`:

```

public static void main(String[] args)
{
  try (Arena arena = Arena.ofConfined()) {
    var module = LLVMModuleCreateWithName(arena.allocateFrom("hello"));

    // Create main function signature: int main()
    var int32Type = LLVMInt32Type();
    var mainType = LLVMFunctionType(int32Type, NULL, 0, 0);
    var mainName = arena.allocateFrom("main");
    var mainFunc = LLVMAddFunction(module, mainName, mainType);

	  var entry = LLVMAppendBasicBlock(mainFunc, arena.allocateFrom("entry"));
 	  var builder = LLVMCreateBuilder();
    LLVMPositionBuilderAtEnd(builder, entry);

	  // Create a global string constant containing "Hello, World!"
    var helloStr =
           LLVMBuildGlobalStringPtr(builder,
                arena.allocateFrom("Hello, World!"),
                arena.allocateFrom("hello_str"));

    // Create puts function type: int puts(char*)
    var putsParamTypes = arena.allocate(ADDRESS, 1);
    var charPtrType = LLVMPointerType(LLVMInt8Type(), 0);
    putsParamTypes.set(ADDRESS, 0, charPtrType);
    var putsType = LLVMFunctionType(int32Type, putsParamTypes, 1, 0);
    // Add puts function to the module
    var putsFunc = LLVMAddFunction(module, arena.allocateFrom("puts"), putsType);

    // Create puts function call
    var callArgs = arena.allocate(ADDRESS, 1);
    callArgs.set(ADDRESS, 0, helloStr);
    LLVMBuildCall2(builder, putsType, putsFunc, callArgs, 1, arena.allocateFrom("puts"));

	  // Return 0
    LLVMBuildRet(builder, LLVMConstInt(int32Type, 0, 0));

    // Initialize LLVM JIT + x86 Target
    LLVMLinkInMCJIT();
    LLVMInitializeX86Target();
    LLVMInitializeX86TargetInfo();
    LLVMInitializeX86TargetMC();
    LLVMInitializeX86AsmPrinter();
    LLVMInitializeX86AsmParser();

    // Create JIT execution engine
    var jitCompiler = arena.allocate(ADDRESS);
    var jitErrorMsgPtrPtr = arena.allocate(ADDRESS);
    LLVMCreateJITCompilerForModule(jitCompiler, module, /* optimization level = */ 2, jitErrorMsgPtrPtr);

    var executionEngine = jitCompiler.get(ADDRESS, 0);
    var addressOfMainFunc = LLVMGetPointerToGlobal(executionEngine, mainFunc);

    // Disable the IR printing now
  	// var llvmIrCharPtr = LLVMPrintModuleToString(module);
    //
    // try {
    //  System.out.println(llvmIrCharPtr.getString(0));
    // } catch (Exception e) {
    //   System.err.println("Failed to write LLVM IR: failed to get error message: " + e.getMessage());
    // }

	   // Clean up LLVM resources
     // LLVMDisposeMessage(llvmIrCharPtr);
     LLVMDisposeBuilder(builder);
     LLVMDisposeModule(module);
  }
}
```

Now that we’ve compiled the function, we need a way to invoke it from Java. To do this, we use the foreign linker to create a `MethodHandle` for the JIT-compiled main function. This handle acts as a callable reference to the native code:

```

public static void main(String[] args)
{
  try (Arena arena = Arena.ofConfined()) {
    var module = LLVMModuleCreateWithName(arena.allocateFrom("hello"));

    // Create main function signature: int main()
    var int32Type = LLVMInt32Type();
    var mainType = LLVMFunctionType(int32Type, NULL, 0, 0);
    var mainName = arena.allocateFrom("main");
    var mainFunc = LLVMAddFunction(module, mainName, mainType);

	  var entry = LLVMAppendBasicBlock(mainFunc, arena.allocateFrom("entry"));
 	  var builder = LLVMCreateBuilder();
    LLVMPositionBuilderAtEnd(builder, entry);

	  // Create a global string constant containing "Hello, World!"
    var helloStr =
           LLVMBuildGlobalStringPtr(builder,
                arena.allocateFrom("Hello, World!"),
                arena.allocateFrom("hello_str"));

    // Create puts function type: int puts(char*)
    var putsParamTypes = arena.allocate(ADDRESS, 1);
    var charPtrType = LLVMPointerType(LLVMInt8Type(), 0);
    putsParamTypes.set(ADDRESS, 0, charPtrType);
    var putsType = LLVMFunctionType(int32Type, putsParamTypes, 1, 0);
    // Add puts function to the module
    var putsFunc = LLVMAddFunction(module, arena.allocateFrom("puts"), putsType);

    // Create puts function call
    var callArgs = arena.allocate(ADDRESS, 1);
    callArgs.set(ADDRESS, 0, helloStr);
    LLVMBuildCall2(builder, putsType, putsFunc, callArgs, 1, arena.allocateFrom("puts"));

	  // Return 0
    LLVMBuildRet(builder, LLVMConstInt(int32Type, 0, 0));

    // Initialize LLVM JIT + x86 Target
    LLVMLinkInMCJIT();
    LLVMInitializeX86Target();
    LLVMInitializeX86TargetInfo();
    LLVMInitializeX86TargetMC();
    LLVMInitializeX86AsmPrinter();
    LLVMInitializeX86AsmParser();

    // Create JIT execution engine
    var jitCompiler = arena.allocate(ADDRESS);
    var jitErrorMsgPtrPtr = arena.allocate(ADDRESS);
    LLVMCreateJITCompilerForModule(jitCompiler, module, /* optimization level = */ 2, jitErrorMsgPtrPtr);

    var executionEngine = jitCompiler.get(ADDRESS, 0);
    var addressOfMainFunc = LLVMGetPointerToGlobal(executionEngine, mainFunc);

    // Create method handle to the int main() function that
    // we just created and compiled.
    var functionHandle = Linker.nativeLinker().downcallHandle(
        addressOfMainFunc,
        FunctionDescriptor.of(/* returnType = */ JAVA_INT)
    );

    // Disable the IR printing now
  	// var llvmIrCharPtr = LLVMPrintModuleToString(module);
    //
    // try {
    //  System.out.println(llvmIrCharPtr.getString(0));
    // } catch (Exception e) {
    //   System.err.println("Failed to write LLVM IR: failed to get error message: " + e.getMessage());
    // }

	   // Clean up LLVM resources
     // LLVMDisposeMessage(llvmIrCharPtr);
     LLVMDisposeBuilder(builder);
     LLVMDisposeModule(module);
  }
}
```

The `downcallHandle` method tells Java how to interpret the native function’s signature – in this case, a function that takes no arguments and returns an int.

Now we can invoke the compiled native function directly from Java, just like a regular method call:

```

public static void main(String[] args)
{
  try (Arena arena = Arena.ofConfined()) {
    var module = LLVMModuleCreateWithName(arena.allocateFrom("hello"));

    // Create main function signature: int main()
    var int32Type = LLVMInt32Type();
    var mainType = LLVMFunctionType(int32Type, NULL, 0, 0);
    var mainName = arena.allocateFrom("main");
    var mainFunc = LLVMAddFunction(module, mainName, mainType);

	  var entry = LLVMAppendBasicBlock(mainFunc, arena.allocateFrom("entry"));
 	  var builder = LLVMCreateBuilder();
    LLVMPositionBuilderAtEnd(builder, entry);

	  // Create a global string constant containing "Hello, World!"
    var helloStr =
           LLVMBuildGlobalStringPtr(builder,
                arena.allocateFrom("Hello, World!"),
                arena.allocateFrom("hello_str"));

    // Create puts function type: int puts(char*)
    var putsParamTypes = arena.allocate(ADDRESS, 1);
    var charPtrType = LLVMPointerType(LLVMInt8Type(), 0);
    putsParamTypes.set(ADDRESS, 0, charPtrType);
    var putsType = LLVMFunctionType(int32Type, putsParamTypes, 1, 0);
    // Add puts function to the module
    var putsFunc = LLVMAddFunction(module, arena.allocateFrom("puts"), putsType);

    // Create puts function call
    var callArgs = arena.allocate(ADDRESS, 1);
    callArgs.set(ADDRESS, 0, helloStr);
    LLVMBuildCall2(builder, putsType, putsFunc, callArgs, 1, arena.allocateFrom("puts"));

	  // Return 0
    LLVMBuildRet(builder, LLVMConstInt(int32Type, 0, 0));

    // Initialize LLVM JIT + x86 Target
    LLVMLinkInMCJIT();
    LLVMInitializeX86Target();
    LLVMInitializeX86TargetInfo();
    LLVMInitializeX86TargetMC();
    LLVMInitializeX86AsmPrinter();
    LLVMInitializeX86AsmParser();

    // Create JIT execution engine
    var jitCompiler = arena.allocate(ADDRESS);
    var jitErrorMsgPtrPtr = arena.allocate(ADDRESS);
    LLVMCreateJITCompilerForModule(jitCompiler, module, /* optimization level = */ 2, jitErrorMsgPtrPtr);

    var executionEngine = jitCompiler.get(ADDRESS, 0);
    var addressOfMainFunc = LLVMGetPointerToGlobal(executionEngine, mainFunc);

    // Create method handle to the int main() function that
    // we just created and compiled.
    var functionHandle = Linker.nativeLinker().downcallHandle(
        addressOfMainFunc,
        FunctionDescriptor.of(/* returnType = */ JAVA_INT)
    );

    // Execute the main function via the method handle.
    try {
      int result = (int) functionHandle.invoke();
      System.out.println("main() returned: " + result);
    } catch (Throwable e) {
      System.err.println("Error calling JIT function: " + e.getMessage());
    }

    // Disable the IR printing now
  	// var llvmIrCharPtr = LLVMPrintModuleToString(module);
    //
    // try {
    //  System.out.println(llvmIrCharPtr.getString(0));
    // } catch (Exception e) {
    //   System.err.println("Failed to write LLVM IR: failed to get error message: " + e.getMessage());
    // }

	   // Clean up LLVM resources
     // LLVMDisposeMessage(llvmIrCharPtr);
     LLVMDisposeBuilder(builder);
     LLVMDisposeModule(module);
  }
}
```

When `functionHandle.invoke()` runs, Java crosses into the native world and calls the machine code that was just compiled by the LLVM JIT compiler.

And that’s it, you can now run the Java application without the LLVM interpreter and see the resulting “Hello, World!”:

```

$ java -cp target/jvm-llvm-helloworld-1.0-SNAPSHOT.jar com.example.App
Hello, World!
```

Congratulations, you’ve now JIT-compiled Hello World, with the help of Java’s FFM API calling LLVM’s C API.

## Next steps

In this Java advent we built and executed native machine code from pure Java and a little help from LLVM – no JNI, no C glue, just memory segments, method handles, and a modern FFI. By the end, we had just a simple program that prints “Hello, World!” but it shows the potential of the Java FFM API and the things you can do when Java and native code work together.

Now see what else you can do, for example, try generating other instructions: print more text, do simple calculations, or even build tiny programs entirely in LLVM from Java.

The full code for this post is available [on GitHub over here](https://github.com/mrjameshamilton/java-llvm-helloworld).

![](https://secure.gravatar.com/avatar/ba9492d96e5b0bf5ea269360a8a81e6df6a18d89d79ff77831450e1e74232f27?s=80&d=retro&r=g)

#### Author: [James Hamilton](https://www.javaadvent.com/author/jhamilton)

I’m a senior software engineer working at Diffblue where we build a tool for automated Java unit test generation using code analysis techniques and reinforcement learning. I previously worked at Guardsquare on JVM/Android related tools & libraries including ProGuardCORE, ProGuard and DexGuard.

[LinkedIn](https://www.linkedin.com/in/mrjameshamilton/)  [Github](https://github.com/mrjameshamilton)

### Share this:

* [Click to share on X (Opens in new window)
  X](https://www.javaadvent.com/2025/12/java-hello-world-llvm-edition.html?share=twitter)
* [Click to share on Reddit (Opens in new window)
  Reddit](https://www.javaadvent.com/2025/12/java-hello-world-llvm-edition.html?share=reddit)
* [Click to share on Facebook (Opens in new window)
  Facebook](https://www.javaadvent.com/2025/12/java-hello-world-llvm-edition.html?share=facebook)
* More

* Click to email a link to a friend (Opens in new window)
  Email
* [Click to print (Opens in new window)
  Print](https://www.javaadvent.com/2025/12/java-hello-world-llvm-edition.html#print?share=print)
* [Click to share on LinkedIn (Opens in new window)
  LinkedIn](https://www.javaadvent.com/2025/12/java-hello-world-llvm-edition.html?share=linkedin)
* [Click to share on Tumblr (Opens in new window)
  Tumblr](https://www.javaadvent.com/2025/12/java-hello-world-llvm-edition.html?share=tumblr)
* [Click to share on Pocket (Opens in new window)
  Pocket](https://www.javaadvent.com/2025/12/java-hello-world-llvm-edition.html?share=pocket)
* [Click to share on Pinterest (Opens in new window)
  Pinterest](https://www.javaadvent.com/2025/12/java-hello-world-llvm-edition.html?share=pinterest)

### Like this:

Like Loading...

### *Related*

In [2025](https://www.javaadvent.com/category/2025)

[java](https://www.javaadvent.com/tag/java) [Java Advent](https://www.javaadvent.com/tag/java-advent) [jdk](https://www.javaadvent.com/tag/jdk) [JVM](https://www.javaadvent.com/tag/jvm)

[Next Post](https://www.javaadvent.com/2025/12/comparing-transitive-dependency-version-resolution-in-rust-and-java.html)

### Leave a Reply[Cancel reply](/2025/12/java-hello-world-llvm-edition.html#respond)

This site uses Akismet to reduce spam. [Learn how your comment data is processed.](https://akismet.com/privacy/)

[December 6, 2025

## Comparing transitive dependency version resolution in Rust and Java

By: Nicolas Fränkel](https://www.javaadvent.com/2025/12/comparing-transitive-dependency-version-resolution-in-rust-and-java.html)
[December 4, 2025

## Test Your Test

By: Andres Sacco](https://www.javaadvent.com/2025/12/test-your-test.html)
[December 3, 2025

## How Understanding Request Flow in Spring Boot Changed the Way I Code

By: Mohammed](https://www.javaadvent.com/2025/12/how-understanding-request-flow-in-spring-boot-changed-the-way-i-code.html)

© 2025 [JVM Advent](https://www.javaadvent.com) | Powered by [![steinhauer.software Logo](https://www.javaadvent.com/content/themes/hitchcock-advent-theme/assets/images/steinhauer.software.png)steinhauer.software](https://steinhauer.software)

Theme by [Anders Norén](https://www.andersnoren.se)

![](//analytics.e-mehlbox.eu/matomo.php?idsite=2&rec=1)

%d
