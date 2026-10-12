# Go — Interview

Interview answers cover the full Devinterview.io Golang set.

### Related docs

- [Go — Interview](./17_go_interview.md)

### Sections

- [Interview Practice](#interview-practice)

---

## Interview Practice

Questions from [Devinterview.io — Golang](https://devinterview.io/questions/web-and-mobile-development/golang-interview-questions/).

### Golang Fundamentals

<details>
<summary>What is Go and why was it created?</summary>

### What is Go and why was it created?

**Definition:**

Go, also referred to as Golang, is an open-source programming language developed by a team at Google and made available to the public in 2009. The design of Go is influenced by its renowned creators: Rob Pike, Ken Thompson, and Robert Griesemer.

The language aimed to address specific challenges experienced by Google developers, and also sought to amalgamate the best features from different languages.

**How it works:**

#### Key Objectives of Go's Design

- **Simplicity**: Go was designed with a minimalistic approach to minimize complexity. Its design steers clear of excessive abstractions and programmer 'magic'.

- **Efficiency**: It was crucial for Go to be efficient and expressive in both time and space.

- **Safety**: The creators aimed to make Go a safe, statically-typed language.

- **Concurrent Programming**: Go's design intends to make concurrent programming pragmatic and straightforward.

This was achieved, to a great extent, through features such as goroutines and channels.

- **Being a System Language**: Go was envisioned as a language suitable for system-level programming. This means it is feasible to use Go to create operating systems, write device drivers, or handle system operations.

#### Key Features

- **Open Source**: Go is open source, which means its source code is openly available. You can view, modify, and distribute it under the license's terms.

- **Statically Typed**: Like Java and C++, Go requires you to specify types of variables and function return values explicitly. These types are checked at compile-time for safety and accuracy.

- **Memory Management**: Go developers don't have to deal with low-level memory operations like in C/C++. Instead, Go uses a garbage collector to release memory from objects that aren't in use.

- **Concurrent Programming**: Go directly supports concurrent operations through the use of goroutines and channels.

- **In-Built Toolset**: Go comes with numerous tools, such as the go command-line tool, that automates many development tasks. For instance, you can use go build to compile your program and go test to run your tests.

- **Portability**: Go was designed to be compatible with multiple systems and architectures.

- **Unicode Support**: Go is thoroughly Unicode-compliant.

- **Support for Networking**: Go comes with libraries to handle network operations efficiently, making it an optimum language for developing network systems.

**When to use:**

#### Who Uses Go?

Several prominent companies make extensive use of Go in critical, performance-driven systems, such as:

- **Google**: Go is often used in internal systems, and many cloud services like YouTube, Google Search, and others heavily rely on Go for their backend tasks.

- **Dropbox**: Dropbox has employed Go to enhance performance in software components that require synchronization and other tasks.

- **Docker**: Go plays a key part in enabling Docker to achieve cross-platform compatibility and resource efficiency.

- **SoundCloud**: SoundCloud has utilized Go for deploying and managing their infrastructure.

- **BBC Worldwide**: Go is instrumental in enabling real-time data processing for BBC Worldwide, ensuring viewers receive the most current content.

Beyond these, Go is increasingly favored for cloud-native applications and microservices due to its performance, efficiency in resource management, and robust standard library. This popularity is forecasted to grow as more companies recognize the advantages Go brings to the table.

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/golang-interview-questions/
</details>

<details>
<summary>Explain the workspace architecture in Go.</summary>

### Explain the workspace architecture in Go.

**Definition:**

The Go Language uses a simplified workspace architecture that sets it apart from many other programming languages.

**How it works:**

#### Golang Workspace Architecture

The workspace essentially consists of three directories:

- **src**: This is where the source code resides.

- **pkg**: The pkg directory houses the package objects created during the build process. This segregation helps establish a clear distinction between the code and the build output.

- **bin**: The directory where the compiled application will be located, once it's been built.

The leader in the Go Distribution "Golang", Google clarifies that these are not just suggestions but are observed standards by the Go tool.

#### GOPATH

The GOPATH environment variable plays a pivotal role. It is the starting point for finding Go code, and all the mentioned directories reside within it.

Under the $GOPATH/src directory, the Go tool expects to see your application source packages.

"go get" Utility
This command is a critical tool for Golang developers. You can use it to fetch and manage dependencies from remote repositories like GitHub. For instance, if you run go get github.com/gorilla/mux, it will fetch the mux package from GitHub and save it in your $GOPATH.

Following the Go community's best practices, ensure that you have "go.mod" and "go.sum" files at the root of your project. The "go.mod" file maintains module registration and dependency requirements, while the "go.sum" file records the version of the dependencies.

#### For local development

Assume your GOPATH is ~/go. For a personal project, with a URL of bob.com/project, your source code would be found at ~/go/src/bob.com/project.

#### Development in a team

If, say, a Git repository houses a project with an import URL of gitlab.com/team/project, the code would be located at ~/go/src/gitlab.com/team/project.

#### Potential GOPATH Best Practices

- **Isolation**: Having one GOPATH per project ensures a clean slate and avoids conflicts arising from different project dependencies.

- **Loose Configuration**: Avoid setting GOPATH in your shell configuration profile, which may lead to unwanted interference in other projects.

- **Multiple Workspaces**: For different language versions or distinct environments, consider having separate workspaces and their accompanying GOPATHs. This segregation prevents projects built in older versions from updating to the new tooling.

#### For version control systems

- **GitHub & Co**: The import path is often linked to the repository URL.

- **Local Repositories**: If your repository is local and not connected to a remote VCS service, you could use any import path structure.

#### Best Practices

Combine all your codebases under a version control system for easy management and deployment.
Keep external dependencies outside the $GOPATH, possibly using a package manager.

**Examples:**

#### Example Paths

The tool will expect your code under a relevant folder in src. If your version control system is Git, the folder may be in line with Git-conventions: for instance, github.com/username/project. Without Git, you can think of it like this: all code should reside in a directory somewhat influenced by its import URL.

#### Plaintext Go Get Example: Fetching the "mux" Package

```bash
$ go get github.com/gorilla/mux
```

This command will oversee downloading the GitHub repository for the "mux" package and storing it locally for you as a requirement.

#### Go Get with the Verbose Flag

```bash
$ go get -v github.com/gorilla/mux
```

Adding the verbose option here makes the command more communicative, providing details about the actions being taken.

#### Directory Structure After Fetching with Go Get

Upon executing the go get command, the mux package from the gorilla repository will be obtained, and it shall be located in the following path:

```text
~/go/src/github.com/gorilla/mux
```

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/golang-interview-questions/
</details>

<details>
<summary>How is the GOPATH environment variable used?</summary>

### How is the GOPATH environment variable used?

**Definition:**

In Go, the GOPATH environmental variable is crucial, serving as the root for both package management and your code repository, and separating your development area from your installed packages.

**How it works:**

#### Workflow without Modules (Go 1.10 and before)

#### Source Files Location

Your Go source files likely reside in the bin, pkg, and src directories inside your GOPATH directory.
Inside the src directory, you generally organize your code using your module path.

#### Installing Packages

The go get command is responsible for fetching and installing packages. For example, running go get <package-name> from anywhere in your workspace installs the package to the global package cache.

#### Building and Running Code

When you build or run your programs, Go looks for imported packages in your workspace first (in GOPATH/src) before checking in the global package cache. For commands and tools, Go expects to find their source files directly in GOPATH/src.

#### Silos of Code

Using GOPATH led to a specific structure and workflow, which sometimes isolated your projects and workspace.

Now let's focus on improved workflows with Modules.

#### Keys to the Go Module Workflow

- **Module Awareness**: Starting with Go 1.11, Go is more modular by design. If your code is in a directory with a go.mod file, it's a module. No need for a GOPATH.

- **Central Module Cache**: In module-aware mode, all the dependencies are stored in a central cache, eliminating the need for repeated downloads.

- **Version Control**: Go modules encourage clear versioning for project dependencies, offering enhanced development reliability, reproducibility, and portability.

- **Clean Independence**: Each module gets its isolated space, enforcing a clear boundary between dependencies.

- **Code Sharing**: Unlike the previous setup, where all codes resided under a single directory, Go modules let you work on and track your modules in any suitable location.

- **Global Visibility**: Your modules, defined by a go.mod file, are accessible across systems, aiding in easy sharing and collaboration.

- **VCS Integration**: Go allows direct interaction with various version control systems, such as Git, by recognizing URLs for modules.

- **Dependency Report**: Go now ensures you have visibility over your project's dependencies by adding a go.mod file, avoiding the potentially overwhelming directory structure under GOPATH.

- **Graceful Transition**: Go supports the gradual shift to the new module-aware mode, letting users choose their migration pace.

Let's consider the newer go mod workflow in Go.

#### How the go mod Workflow Simplifies

- **Self-Contained**: Code can reside anywhere on your system, making it independent from a central location like GOPATH.

- **Versioning Control**: Libraries get versioned with precision, ensuring a consistent and stable build environment.

- **Module Cache**: This efficient central cache eliminates the repetitive download headache.

- **Reduced Pitfalls**: No system-wide changes and global caches to manage—each module is in a reliable, separate state.

- **Mechanism for Isolation**: Go doesn't halt on its usual package structure; you still get a self-contained module, therefore keeping your environment predictable and manageable.

- **Effortless Setup**: New users or contributors can promptly set up the workspace without worrying about GOPATH correctness.

**Examples:**

#### Code Example: Enabling Modules

You can invoke modules by initializing a go.mod file in your project root:

```bash
# Initialize a go.mod file
go mod init <module-path>
```

This action automatically activates module-aware mode, rendering GOPATH irrelevant.

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/golang-interview-questions/
</details>

<details>
<summary>What are Go's key features compared to other programming languages?</summary>

### What are Go's key features compared to other programming languages?

**Definition:**

Go (Golang) offers several features that set it apart from other programming languages:

**How it works:**

- **Concurrently Using Goroutines and Channels**: Go excels in multi-threading and concurrency management through goroutines (lightweight threads) and channels (communication mechanism between goroutines).

- **Simplicity in Syntax**: Go employs C-like syntax with a subtle touch of its own design. While the language sets clear style guidelines, it's more lenient compared to others, such as Python.

- **Goroutines**: These lightweight threads serve for concurrent execution. Go's run-time scheduler maps them to OS threads, optimizing performance.

- **Robust Standard Library**: Go's standard library is extensive and maintains a consistent design, making it more developer-friendly.

- **Integrated Tooling**: Go's built-in tools provide package management, code formatting, and ease in testing, leading to streamlined development processes.

- **Speed and Performance**: Golang's compilation, execution, and resource utilization often outperform interpreted languages, such as Python or Ruby.

- **Automatic Memory Management**: Go's garbage collector efficiently manages memory, freeing developers from the burden of memory allocation/deallocation.

- **Native Compilation**: Go programs compile directly into machine code, ensuring portability across hardware without any external runtime dependencies.

- **Structural Typing and Interfaces**: Go offers a distinctive approach to object-oriented programming through interfaces and structural typing.

- **Statically Linked Binaries**: Go's binaries bundle the necessary libraries, simplifying deployment and reducing the risk of library conflicts compared to languages like C.

- **Language Stability**: Go follows a "minimal versioning" policy for its standard library, ensuring code stability and backward compatibility. It also leads to fewer surprises during upgrades.

- **Implicit Interface Implementation**: Types can implement interfaces implicitly, reducing the need for boilerplate code.

- **Package Management with go mod**: Starting with Go 1.11, go mod provides dependency management at the package level while offering versioning control and module encapsulation.

- **Concise Error Handling**: Go's single error return value, supplemented by its errors package, makes error handling compact and straightforward.

- **Code Generation Efficiency**: The Go compiler generates optimized machine code, resulting in faster executables.

- **Web Servers with Standard Library**: Go's standard library includes an HTTP package, simplifying web server development without relying on third-party frameworks.

- **GoDoc for Documentation**: GoDoc automates code documentation, enhancing code maintainability and developer collaboration.

- **Goroutines and Concurrency Primitives**: Go's 'select' statement and the 'context' package provide powerful mechanisms for coordination and time-outs in concurrent applications.

- **Full Support for Unicode**: Go treats text as Unicode by default, ensuring internationalization and text processing are seamless.

- **Permissive for Loop**: Go's for loop accommodates various styles, fostering code adaptability.

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/golang-interview-questions/
</details>

<details>
<summary>Describe how packages are structured in a Go program.</summary>

### Describe how packages are structured in a Go program.

**Definition:**

In Go, all code resides within Packages. Each package has its designated role, with clear specifications on visibility, accessibility, and its place in the broader program.

**How it works:**

#### Key Concepts

- **Visibility**: Elements starting with a lowercase letter are only visible within their package (package-private in other languages). Exported elements, starting with an uppercase letter, are accessible outside the package (similar to public in other languages).

- **Organization**: Code is organized at three levels - main program, packages, and individual files.

- **Package Naming**: Most Go developers use lowercase, single-word package names.

- **.package file**: Denotes an importable package.

#### Internal Mechanisms

Go uses a module system to manage dependencies and ensure package versioning and compatibility. Modules allow developers to designate a directory as a package root, containing a go.mod file facilitating dependency resolution.

#### Tips

Packages help to compartmentalize code, making it more manageable.
Following Go's conventions in file organization and package naming is crucial for maintainability and clarity.
Proper usage of the module system ensures that a program is self-contained and encapsulated.

**Examples:**

#### Code Example: Go Package Definition

Here is the Go code:

```go
package geometry

import "math"
```

// A Point represents a point in the 2D plane.

```go
type Point struct {
    X, Y float64
}

// Distance calculates the Euclidean distance between two points.
func Distance(p1, p2 Point) float64 {
    return math.Sqrt(math.Pow(p2.X-p1.X, 2) + math.Pow(p2.Y-p1.Y, 2))
}
```

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/golang-interview-questions/
</details>

<details>
<summary>What are slices in Go, and how do they differ from arrays?</summary>

### What are slices in Go, and how do they differ from arrays?

**Definition:**

Slices in Go are dynamic, flexible and reference-based data structures that enable efficient list management. They are built on top of arrays and offer features such as automatic resizing and ease of use.

**How it works:**

#### Slice Characteristics

- **Dynamic Sizing**: Slices are resizable, ensuring adaptability as elements are added or removed.

- **Reference Semantics**: Underneath, slices reference an array, meaning any modifications to the slice apply to the referred array.

- **Bounds Checking**: Slices automatically handle out-of-bounds errors, offering an added layer of safety during indexing.

- **Reslice Efficiency**: Operations like slicing a slice are highly efficient and do not involve element copying.

- **Append-Optimized**: The append function allows slices to smartly manage capacity, optimizing their resizing behavior.

- **Built-in Methods**: Golang provides versatile built-in methods for slices, including append, copy, and sub- and sub-sub-slicing.

#### Key Distinctions Between Slices and Arrays

- **Structural Difference**: Arrays have a fixed size, defined at the point of declaration, while slices can dynamically expand or contract.

- **Directness of Data**: Arrays stash data directly, but slices act as abstractions, referencing arrays.

- **Memory Management**: The Go runtime governs the memory of slices, yet the size of arrays is static and set in stone at the outset.

- **Pointer-Level Reference**: Slices, as dynamic arrays of a kind, can be likened to pointers to the backing array.

#### Append Capability

The append function amplifies the flexibility of slices when it comes to growing or joining.

**Examples:**

#### Slice Declaration Syntax

You can declare a slice without any initial contents:

```go
var s []int // creates an empty slice, equivalent to "nil"
```

#### Or initialize it with elements

```go
s := []int{1, 2, 3}  // creates and initializes a slice with {1, 2, 3}
```

#### Slicing Arrays and Slices

Go, following the "less is more" philosophy, integrates slicing straightforwardly. Both slices and arrays can be sliced using the same syntax:

```go
arr := [5]int{1, 2, 3, 4, 5}
slice1 := arr[1:3]  // creates a slice from arr with elements {2, 3}

otherSlice := slice1[1:2]  // creates a slice from slice1 with element {3}
```

Here is the go code:

```go
package main

import "fmt"

func main() {
   s1 := []int{1, 2, 3}
   s2 := []int{4, 5}

   s1 = append(s1, 4, 5, 6)   // Result: s1 = [1, 2, 3, 4, 5, 6]
   s1 = append(s1, s2...)     // Merging two slices: s1 = [1, 2, 3, 4, 5, 6, 4, 5]
}
```

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/golang-interview-questions/
</details>

<details>
<summary>Could you explain what a goroutine is?</summary>

### Could you explain what a goroutine is?

**Definition:**

Goroutines are lightweight threads or, more accurately, cooperative concurrent functions in Golang that manage their execution. They allow for independent code segments to execute concurrently.

**How it works:**

#### Key Characteristics

- **Efficiency**: Goroutines are more memory and processing efficient than traditional threads because they're multiplexed over a small number of OS threads.

- **Go Select-Weile**: Concurrency and communication between Goroutines are elemental in Go, often achieved through channel communication and the select statement.

#### Setup and Control

Goroutines are created using the go keyword, which precedes a function call. The process involves the Go runtime, making Goroutines distinct from typical functions.

- **Simplicity**: The process of creating and controlling Goroutines is streamlined.

#### Best Practices

- **Use in Moderation**: Goroutines provide useful concurrency but avoid exceeding practical limits.

- **Responsible Interaction**: Goroutines should communicate and share memory using predefined structures like channels.

Now, let's look at the Go code example:

**Examples:**

#### Code Example: Goroutines in Go

Here is the Go code:

```go
package main

import (
  "fmt"
  "time"
)

func task(message string) {
  for i := 0; i < 5; i++ {
    fmt.Println(message)
    time.Sleep(time.Millisecond * 100)
  }
}

func main() {
  go task("goroutine")
  task("normal function")
  time.Sleep(time.Second)
}
```

In this program, task("normal function") is called as a regular function, and go task("goroutine") is called as a Go routine. The output will show concurrent execution.

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/golang-interview-questions/
</details>

<details>
<summary>How does Go handle memory management?</summary>

### How does Go handle memory management?

**Definition:**

Go provides automatic memory management through a garbage collector. This ensures developers do not need to manage memory allocation or deallocation explicitly.

**How it works:**

#### Key Concepts

- **Garbage Collection (GC)**: Go uses GC to identify and reclaim memory that is no longer in use, preventing memory leaks.

- **Concurrency-Awareness**: Go's GC is designed to work efficiently in concurrent environments, affecting production real-time applications.

- **Pros**: Go's garbage collector simplifies memory management, reducing the likelihood of common issues like dangling pointers or memory leaks.

- **Cons**: While it generally has minimal impact, the GC process can cause slight performance fluctuations, which real-time applications need to account for.

- **Policy Customization**: Go allows developers to influence the GC's behavior using environment variables and runtime functions. Some commands all programmers to tune GOGC for memory efficiency. Nonetheless, these aren't guarantees but heuristics for the GC.

**Examples:**

#### Code Example: Memory Garbage Collection

Here is Go code:

```go
package main

import "fmt"
import "runtime"

func main() {
    // Create lots of memory-consuming objects
    for i := 0; i < 100000000; i++ {
        _ = make([]byte, 100)
        if i%10000000 == 0 {
            fmt.Printf("Allocated %d bytes\n", i*100)
        }
    }

    // Request garbage collection
    runtime.GC()
    var ms runtime.MemStats
    runtime.ReadMemStats(&ms)
    fmt.Println("Memory allocated, not yet garbage collected:", ms.Alloc)

    // It would be a rare use case to invoke manual GC in production
}
```

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/golang-interview-questions/
</details>

<details>
<summary>Can you list and explain the basic data types in Go?</summary>

### Can you list and explain the basic data types in Go?

**Definition:**

Go provides a robust set of data types, addressing various data needs efficiently.

**How it works:**

#### Primary Data Types

- **Numeric Types**: Integers (int and byte), Floating-Points (float32 and float64).

- **String**: Unicode characters encoded in UTF-8.

- **Boolean**: Represents true/false states.

#### Composite Data Types

- **Arrays**: Fixed-size sequences of elements of the same type.

- **Slices**: Similar to arrays but with dynamic sizing.

- **Maps**: Key-value pairs suitable for quick lookups.

- **Structs**: Encapsulation of various data types inside a single entity.

- **Pointers**: Holds the memory address of a value.

#### Derived/Special Types

- **Constants**: Immutable values known at compile time.

- **Functions**: First-class citizens, enabling higher-order functionality.

- **Channels**: Facilitates communication among goroutines in concurrent programs.

- **Interfaces**: Defines behavior by prescribing a set of method signatures.

- **Errors**: A built-in interface type to represent error conditions.

- **Type aliases**: Allows for type redefinition without direct inheritance or subclassing.

#### User-Defined Types

- **Named Types**: Enhanced readability and type compatibility through custom, user-defined type names.

- **Underlying Types**: Primarily one of the built-in types.

**Examples:**

#### Code Example: Data Type Use

Here is the Go code:

```go
package main

import (
	"fmt"
)

func main() {
	// Basic types
	var myInt int = 42
	var myFloating float64 = 3.1415
	var myStr string = "Hello, Go!"
	var isTrue bool = true 

	// Derived and composite types
	var myArray [3]int = [3]int{1, 2, 3}
	mySlice := []int{2, 3, 4, 5} // Type inference
	myMap := map[string]int{"one": 1, "two": 2}
	myStruct := struct {
		Name string
		Age  int
	}{Name: "Alice", Age: 30}

	var myPointer *int = &myInt
	const myConstant = 100
	// function type
	var myFunction func(string) string
	myFunction = func(str string) string {
		return "Hello, " + str
	}

	var myChannel = make(chan int)
	var myInterface interface{} = myStr

	var myAlias int32 = 42
  var myUserDefinedType MyCustomType = 100 // User defined type

	fmt.Printf("Output: %v %v %v %v\n", myInt, myFloating, myStr, isTrue)
	fmt.Printf("Arrays/Slices: %v %v\n", myArray, mySlice)
	fmt.Println("Maps: ", myMap)
	fmt.Println("Struct: ", myStruct)
	fmt.Println("Pointer: ", *myPointer)
	fmt.Printf("Constant: %v\n", myConstant)
	fmt.Println("Function: ", myFunction("Go!"))
  fmt.Println("Type Aliases: ", myAlias)
	fmt.Println("User Defined Type: ", myUserDefinedType)
	// Other types
	fmt.Println("Channel: ", myChannel)
	fmt.Println("Interface: ", myInterface)
}
```

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/golang-interview-questions/
</details>

<details>
<summary>What is the zero value of a variable in Go?</summary>

### What is the zero value of a variable in Go?

**Definition:**

In Go, variables automatically initialize to their zero values if not explicitly set. Zero values provide sensible defaults, especially when dealing with dynamically allocated variables like pointers.

**How it works:**

#### Zero Value Definitions

- **Uninitialized Variables**: Variables declared but left unassigned will take on their zero values.

- **Slices and Maps**: Their zero values are nil, indicating that no underlying data is assigned.

- **Pointers**: Their zero value is also nil.

- **Interfaces**: Their state is nil unless initialized to a specific type with a non-nil value.

**Examples:**

#### Example Codes: Zero Value in Go

#### Example 1: Basic Types

- **Bool**: false

- **Numeric Types (int, float, and their variations)**: 0

- **Complex**: 0 + 0i

- **String**: ""

```go
package main

import "fmt"

func main() {
    var (
        b bool
        i int
        f float64
        c complex128
        s string
    )

    fmt.Println(b, i, f, c, s)
}
```

#### Example 2: Slices and Maps

- **Slice**: nil

- **Map**: nil

```go
package main

import "fmt"

func main() {
    var (
        slice []int
        dmap  map[string]int
    )

    fmt.Println(slice, dmap)
}
```

#### Example 3: Pointers and Interfaces

- **Pointer**: nil

- **Interface**: nil

```go
package main

import "fmt"

type dataStruct struct {
    value int
}

func main() {
    var (
        pDataStruct *dataStruct
        any         interface{}
    )

    fmt.Println(pDataStruct, any)
}
```

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/golang-interview-questions/
</details>

<details>
<summary>How do you manage error handling in Go?</summary>

### How do you manage error handling in Go?

**Definition:**

Go has a unique approach to error handling compared to other languages. It uses explicit error return types. This design choice reduces ambiguity about functions that can potentially fail and obviates the need for try-catch blocks.

The Go philosophy stresses transparency and early reporting of issues. To adhere to these principles, the language offers a set of built-in tools and techniques.

**How it works:**

#### Best Practices in Go for Managing Errors

- **Minimize. panic()**: Use it for exceptional conditions like unrecoverable internal state

- **Log Smartly**: Specify logging levels selectively. Avoid logging the same message multiple times.

- **Wrap Errors to Retain Context**: Use fmt.Errorf() and errors.New() or the errors package for this.

- **Validate and Handle Returned Errors**: Don't ignore them without validation.

- **Custom Error Types Over string**: Custom types are expressive and can provide tailored behaviors.

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/golang-interview-questions/
</details>

<details>
<summary>Can you convert between different data types in Go? How?</summary>

### Can you convert between different data types in Go? How?

**Definition:**

Go offers a rich set of methods for efficiently managing and transforming data types.

**How it works:**

#### Type Conversions versus Type Assertions

- **Type Conversions**: Used to convert a value explicitly from one type to another within related types, like converting an integer to a float.

- **Type Assertions**: Applied to interface types, they are used to extract the actual data from the interface to a specified type.

#### Basic Data Types in Go

- **Numeric**: int, uint, float, complex.

- **Textual**: string, rune (integer type representing a Unicode code point).

**Examples:**

#### Code Example: Type Conversions and Type Assertions

Here is the Golang code:

```go
package main

import (
	"fmt"
	"strconv"
)

func main() {
	// Numeric to String
	num := 42
	str := strconv.Itoa(num)

	// String to Numeric
	str2 := "42"
	num2, err := strconv.Atoi(str2)
	if err != nil {
		panic(err)
	}

	// Interface with Asserted Types
	var myData interface{} = "Golang"
	val, ok := myData.(string)
	if ok {
		fmt.Println("The data is a string:", val)
	} else {
		fmt.Println("The data is not a string")
	}

	// Type Switch
	myFunc(47)

}

func myFunc(i interface{}) {
	switch v := i.(type) {
	case int:
		fmt.Println("It's an int!")
	case string:
		fmt.Println("It's a string!")
	default:
		fmt.Printf("Don't know the type %T!\n", v)
	}
}
```

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/golang-interview-questions/
</details>

<details>
<summary>What is a channel in Go, and when would you use one?</summary>

### What is a channel in Go, and when would you use one?

**Definition:**

In Go, a channel is a way to safely and efficiently share data among Goroutines. Its primary design goal is to streamline concurrent communication and data transfer.

**How it works:**

#### Key Characteristics

- **First-In-First-Out (FIFO) Order**: Data enqueued to the channel is sent out in the same order.

- **Type Safety**: Channels are type-specific to ensure clear data communication.

- **Blocking Synchronization**: When the data is sent or received, the sending or receiving Goroutine is blocked until the operation is complete, promoting synchronization.

**When to use:**

#### Common Use Cases

- **Goroutine Coordination**: Establish communication and synchronization among concurrent Goroutines.

- **Resource Sharing Among Goroutines**: Use channels to manage shared resources, such as a fixed number of database connections between Goroutines.

- **Error Handling**: Send possible errors to a central Goroutine for unified handling.

- **Stream Data Communication**: Facilitate streaming of data, such as real-time updates, using channels.

- **Batch Data Transfer**: Channels can be effective in grouping and sending data in batches. This is especially helpful if the receiving end is designed to work with data in batches, which might help to optimize the computational operations. It can be useful in scenarios like financial tickers where you might have multiple ticks in a second but will only process data once in a while.

- **Graceful Shutdown**: Use a channel to signal Goroutines to gracefully terminate or suspend tasks, like in a server that should start processing requests only after a certain setup is complete.

**Examples:**

Here is the Java code:

```java
import java.util.concurrent.ArrayBlockingQueue;
import java.util.concurrent.BlockingQueue;

public class BatchDataTransfer {
    public static void main(String[] args) {
        BlockingQueue<Integer> channel = new ArrayBlockingQueue<>(5);

        // Producer
        new Thread(() -> {
            for (int i = 0; i < 100; i++) {
                try {
                    channel.put(i);
                } catch (InterruptedException e) {
                    Thread.currentThread().interrupt();
                }
            }
        }).start();

        // Consumer
        new Thread(() -> {
            while (true) {
                try {
                    System.out.println(channel.take());
                } catch (InterruptedException e) {
                    Thread.currentThread().interrupt();
                }
            }
        }).start();
    }
}
```

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/golang-interview-questions/
</details>

<details>
<summary>Explain concurrency in Go and how it compares to parallelism.</summary>

### Explain concurrency in Go and how it compares to parallelism.

**Definition:**

Goroutines enable concurrent execution in Go by running functions independently. Tasks are handled through a unicast communication model where a sender can reach multiple receivers, commonly through channels. This stands in contrast to a bidirectional approach characteristic of shared memory systems.

**How it works:**

#### Difference from Traditional Systems

- **Memory Model**: Go follows a "Share by Communicating" philosophy. In contrast, traditional systems like Java or C# adopt a shared memory model.

- **Concurrency Style**: Go leverages the "Do not communicate by sharing memory; instead, share memory by communicating" paradigm. Languages like Python or Ruby often rely on shared memory for communication.

#### Leveraging Multi-Cores

Go employs the go keyword to launch routines, with efficient scheduling through its runtime. Multiple cores bring about parallelism, with the system handling the distribution.

#### Advantages

- **Ease of Use**: The go keyword and straightforward synchronization schemes simplify concurrent programming.

- **Memory Safety**: Go runtime provides automatic memory garbage collection, reducing the likelihood of memory leaks.

- **Improved Responsiveness**: Multiple tasks are handled independently, enhancing application performance.

- **Potential for Greater Speed**: By utilizing multiple cores, efficient code guarantees faster execution.

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/golang-interview-questions/
</details>

<details>
<summary>What does the range keyword do?</summary>

### What does the range keyword do?

**Definition:**

In Go, range is a versatile loop construct that simplifies iterating over data structures like arrays, slices, maps, strings, and channels. It efficiently handles different data types and abstracts away the underlying indexing or iteration mechanisms.

**How it works:**

#### Key Benefits

- **Clarity**: It makes the code more readable by focusing on elements rather than indices.

- **Safety**: Range loops guard against index out-of-bounds errors and ensure element synchronization in concurrent contexts.

- **Efficiency**: They eliminate the need for manual index management.

#### Iteration Mechanics

- **Slice/Array**: It yields both the current element and its index.

- **Map**: It produces key-value pairs.

- **Channel**: When used in a for loop with a channel, range retrieves data from the channel until it's closed.

**Examples:**

#### Code Example: Iterating with Range

Here is the Go code:

```go
package main

import (
	"fmt"
)

func main() {
	// Slice/Array
	numbers := []int{2, 3, 5, 7, 11, 13}
	for index, value := range numbers {
		fmt.Printf("Index: %d, Value: %d\n", index, value)
	}

	// Map
	person := map[string]string{"name": "Alice", "age": "30"}
	for key, value := range person {
		fmt.Printf("Key: %s, Value: %s\n", key, value)
	}

	// Channel
	ch := make(chan int)
	go func() {
		defer close(ch)
		for i := 0; i < 5; i++ {
			ch <- i
		}
	}()

	for val := range ch {
		fmt.Println("Channel value:", val)
	}
}
```

**Sources:**

- https://devinterview.io/questions/web-and-mobile-development/golang-interview-questions/
</details>

<details>
<summary>What is defer and when would you use it?</summary>

### What is defer and when would you use it?

**Definition:** `defer` schedules a function call to run when the surrounding function returns — after a `return`, at the end of the body, or while the goroutine is panicking.

**How it works:**

#### Evaluation and order

- **Arguments now**: the function value and its arguments are evaluated when the `defer` statement runs, not when the call runs.
- **LIFO**: deferred calls run in reverse order, immediately before the surrounding function returns to its caller.
- **Named results**: a deferred function literal can read and change named result parameters after the `return` statement sets them. The deferred call’s own results are discarded.
- **Nil function**: if the deferred function value is `nil`, the panic happens when that call is invoked, not when `defer` is executed.

**When to use:**

#### Cleanup next to acquisition

- Use it to unlock a mutex, close a file or response body, or recover from a panic in the same function that acquired the resource.
- Do not use it for work that must run at an arbitrary later time; that belongs in ordinary control flow or another goroutine.

**Examples:**

```go
func readFirst(path string) (b []byte, err error) {
	f, err := os.Open(path)
	if err != nil {
		return nil, err
	}
	defer f.Close()
	buf := make([]byte, 16)
	_, err = f.Read(buf)
	return buf, err
}
```

**Sources:**

- https://go.dev/doc/articles/defer_panic_recover.html
- https://go.dev/ref/spec#Defer_statements
- https://go.dev/doc/effective_go
</details>

<details>
<summary>Describe the select statement in Go.</summary>

### Describe the select statement in Go.

**Definition:** `select` chooses one send or receive that can proceed. It is the multi-way communications operation, analogous in shape to `switch`, with channel operations in the cases.

**How it works:**

#### How a select runs

- **Operands once**: channel operands, and the values to send, are evaluated exactly once, in source order, on entry. Side effects happen even if that case is not chosen.
- **Choice**: if several communications can proceed, one is chosen by uniform pseudo-random selection. If none can, and there is a `default`, that case runs. Otherwise `select` blocks.
- **Nil channels**: communication on a `nil` channel never proceeds. A `select` of only `nil` channels and no `default` blocks forever.
- **No fallthrough**: cases do not fall through, and empty `select {}` blocks forever.

**When to use:**

#### Multiplexing waits

- Use `select` when a goroutine must wait on more than one channel, including cancellation or a timeout.
- A single receive or send does not need `select`. `default` makes the operation non-blocking; use it only when dropping or retrying is the intended behavior.

**Examples:**

```go
func take(ctx context.Context, ch <-chan int) (int, error) {
	select {
	case v, ok := <-ch:
		if !ok {
			return 0, errors.New("closed")
		}
		return v, nil
	case <-ctx.Done():
		return 0, ctx.Err()
	}
}
```

**Sources:**

- https://go.dev/ref/spec#Select_statements
- https://go.dev/doc/effective_go
- https://pkg.go.dev/context
</details>

<details>
<summary>What are anonymous functions and closures in Go?</summary>

### What are anonymous functions and closures in Go?

**Definition:** A function literal is an anonymous function. Function literals are closures: they may refer to variables from the surrounding function, and those variables stay alive as long as the literal can reach them.

**How it works:**

#### What a literal captures

- **Shared variables**: the literal and the surrounding function share the same variables, not a copy taken at the `func` keyword.
- **No type parameters**: a function literal cannot declare its own type parameters.
- **Goroutine loop variable**: a goroutine started inside a loop sees the variable as it is when the literal runs. `go vet` reports the common capture mistake. Pass the value as a parameter if each iteration needs its own copy.

**When to use:**

#### Small local behavior

- Use a literal for a short callback, a deferred recover, or a function that must close over local state.
- Name a function when the behavior is reused, tested on its own, or long enough that a name documents it.

**Examples:**

```go
func prefixer(prefix string) func(string) string {
	return func(s string) string {
		return prefix + s
	}
}

func launch(ids []int) {
	var wg sync.WaitGroup
	for _, id := range ids {
		wg.Add(1)
		go func(id int) {
			defer wg.Done()
			fmt.Println(id)
		}(id)
	}
	wg.Wait()
}
```

**Sources:**

- https://go.dev/ref/spec#Function_literals
- https://go.dev/doc/codewalk/functions
- https://go.dev/doc/faq
</details>

<details>
<summary>How do you write a unit test in Go?</summary>

### How do you write a unit test in Go?

**Definition:** Put `func TestXxx(t *testing.T)` in a file whose name ends in `_test.go`, in the same directory as the package. `go test` builds a test binary and runs those functions. `Xxx` must not start with a lowercase letter.

**How it works:**

#### What the testing package expects

- **Same package or external test**: the file may use `package foo` or `package foo_test`. `_test.go` files are excluded from ordinary builds.
- **Failure**: call `t.Error`, `t.Errorf`, or `t.Fatal` (and related methods). `Fatal` stops that test function.
- **Table**: a slice of input/want structs keeps several cases in one `Test` function, which is the style used in the Go documentation’s ReverseRunes example.

**Examples:**

```go
func TestReverse(t *testing.T) {
	cases := []struct {
		in, want string
	}{
		{"ab", "ba"},
		{"", ""},
	}
	for _, c := range cases {
		if got := Reverse(c.in); got != c.want {
			t.Errorf("Reverse(%q) = %q, want %q", c.in, got, c.want)
		}
	}
}
```

**Sources:**

- https://pkg.go.dev/testing
- https://go.dev/doc/tutorial/add-a-test
- https://go.dev/doc/code
</details>

<details>
<summary>Can you explain what pointers are and how Go handles them?</summary>

### Can you explain what pointers are and how Go handles them?

**Definition:** A pointer type `*T` is the set of pointers to variables of base type `T`. An uninitialized pointer is `nil`. Go passes every argument by value, so a function receives a copy of the pointer, not a copy of the pointed-to variable.

**How it works:**

#### Pointers and values

- **Address and dereference**: `&x` takes the address of `x`. `*p` denotes the variable `p` points at.
- **Allocation**: `new(T)` allocates a zero `T` and returns `*T`. Composite literals also produce pointers: `&Point{X: 1}`.
- **Maps, slices, channels**: copying one copies a descriptor that points at shared data. Copying the descriptor does not copy that data.
- **Escape**: if a pointer to a local variable outlives the function, the compiler allocates that variable on the heap so the pointer does not dangle.
- **Interface pointers**: a pointer to a concrete type can implement an interface. A pointer to an interface value almost never does; the FAQ says to avoid `*SomeInterface` except in rare cases.

**When to use:**

#### Value or pointer receiver

- Use a pointer when the method must mutate the receiver, or when copying the value is the wrong cost for a large struct.
- Use a value when the method does not mutate and a copy is cheap and clear. Do not take the address of a map key or a string byte (`&s[i]` is illegal).

**Examples:**

```go
type Point struct{ X, Y int }

func (p *Point) Move(dx, dy int) {
	p.X += dx
	p.Y += dy
}

func origin() *Point {
	p := Point{}
	return &p
}
```

**Sources:**

- https://go.dev/ref/spec#Pointer_types
- https://go.dev/doc/faq
- https://go.dev/doc/effective_go
</details>

### Intermediate Golang Concepts

<details>
<summary>Explain Go's interface type and how it differs from other types.</summary>

### Explain Go's interface type and how it differs from other types.

**Definition:** An interface type specifies a type set. A variable of that interface can hold a value of any type in the set. A type implements a basic interface automatically when its method set includes the interface’s methods; there is no `implements` declaration.

**How it works:**

#### Interface values

- **Method list**: a basic interface is a list of methods. The zero value is `nil`, meaning no dynamic type and no dynamic value.
- **Implicit satisfaction**: a type satisfies every interface whose methods are a subset of its method set. It can satisfy many interfaces, and interfaces can be declared later, including for tests.
- **Not a class hierarchy**: the FAQ states there is no type hierarchy. Embedding and interfaces are not subclassing.
- **Nil pitfall**: an interface holding a typed nil pointer is not equal to a nil interface. Return an explicit `nil` error, not a nil pointer stored in an `error` variable.

**When to use:**

#### Accept interfaces, return concrete types

- Use a small interface at an API boundary when callers need to substitute another implementation, including a test double.
- Keep a concrete struct when there is only one implementation and the behavior is not a substitution point.

**Examples:**

```go
type Stringer interface{ String() string }

type User struct{ Name string }

func (u User) String() string { return u.Name }

func greet(s Stringer) string { return "hi " + s.String() }
```

**Sources:**

- https://go.dev/ref/spec#Interface_types
- https://go.dev/doc/faq
- https://go.dev/doc/effective_go
</details>

<details>
<summary>What are map and how do you manage them?</summary>

### What are map and how do you manage them?

**Definition:** A map is an unordered group of elements of one type, indexed by unique keys of another type. The key type must be comparable; functions, maps, and slices cannot be keys. An uninitialized map is `nil`.

**How it works:**

#### Creating, reading, deleting

- **make**: `make(map[K]V)` or `make(map[K]V, hint)` creates an empty map. The hint does not limit growth. A `nil` map reads like an empty map, but assigning an element panics.
- **Lookup**: `v := m[k]` returns the zero value when `k` is absent. `v, ok := m[k]` reports whether the key was present.
- **delete and clear**: `delete(m, k)` removes one key. `clear(m)` removes all entries. `len(m)` is the current number of elements.
- **Concurrency**: the FAQ states that maps are not safe for concurrent writes. Concurrent read-only use is allowed. A write concurrent with any other access is a data race; the runtime can crash the program if it detects a concurrent map write.

**When to use:**

#### Map versus a slice of pairs

- Use a map for key lookup. Use a slice when order matters or the collection is small and scanned linearly.
- If several goroutines mutate the map, give one goroutine ownership and send updates on a channel, or protect it with a `sync.Mutex`.

**Examples:**

```go
func counts(words []string) map[string]int {
	m := make(map[string]int)
	for _, w := range words {
		m[w]++
	}
	return m
}
```

**Sources:**

- https://go.dev/ref/spec#Map_types
- https://go.dev/doc/effective_go
- https://go.dev/doc/faq
</details>

<details>
<summary>Describe the blank identifier (_) and its use cases in Go.</summary>

### Describe the blank identifier (\_) and its use cases in Go.

**Definition:** The blank identifier `_` is an anonymous placeholder. It can appear in a declaration or on the left of an assignment, but it does not introduce a binding.

**How it works:**

#### Where `_` is legal

- **Ignore a result**: `n, _ := fmt.Println(s)` discards the error. Use this only when ignoring that value is intentional.
- **Imports**: `import _ "image/png"` runs the package’s `init` functions for a side effect, such as registering a decoder, without naming the package.
- **Unused range index**: `for _, v := range items` skips the index. `_` may also be a field used as padding in a struct.

**When to use:**

#### Silence a compiler error on purpose

- Use `_` when the language requires a variable and the program truly does not need the value.
- Do not use `_` to hide an `error` you still need to handle.

**Examples:**

```go
func first(items []string) (string, bool) {
	for _, v := range items {
		return v, true
	}
	return "", false
}
```

**Sources:**

- https://go.dev/ref/spec#Blank_identifier
- https://go.dev/doc/effective_go
</details>

<details>
<summary>How does Go handle method overloading?</summary>

### How does Go handle method overloading?

**Definition:** Go does not support method or operator overloading. For one receiver base type, non-blank method names must be unique, and method signatures must match an interface exactly.

**How it works:**

#### What you do instead

- **Different names**: `Read` and `ReadAt` are two methods, not two overloads of one name.
- **FAQ rationale**: the language designers left overloading out so method sets stay simple. Two methods that return different types are not the same method.
- **Generics are not overloading**: a generic function still has one name and is instantiated with type arguments. It does not pick a signature from a set of overloads.

**Examples:**

```go
type Buffer struct{ buf []byte }

func (b *Buffer) Write(p []byte) (int, error) {
	b.buf = append(b.buf, p...)
	return len(p), nil
}

func (b *Buffer) WriteString(s string) (int, error) {
	return b.Write([]byte(s))
}
```

**Sources:**

- https://go.dev/doc/faq
- https://go.dev/ref/spec#Method_declarations
</details>

<details>
<summary>What is type embedding in Go, and when would you use it?</summary>

### What is type embedding in Go, and when would you use it?

**Definition:** A struct field declared with a type and no name is an embedded field. Its unqualified type name is the field name. Promoted fields and methods of that field can be selected as if they were declared on the outer struct.

**How it works:**

#### Promotion rules

- **Form**: an embedded field is `T` or `*T`, where `T` is a named type that is not itself a pointer. `T` and `*T` cannot both be embedded under the same field name.
- **Method sets**: embedding `T` promotes methods with receiver `T` to both `S` and `*S`, and methods with receiver `*T` to `*S`. Embedding `*T` promotes both receiver forms to `S` and `*S`.
- **Not subclassing**: the outer value is not a subtype of the inner type. The FAQ describes embedding as analogous to, and not identical with, subclassing.

**When to use:**

#### Promote a small implementation

- Embed a type when the outer struct should expose that type’s fields and methods as its own, such as a `sync.Mutex` used by promoted locking methods you write yourself, or a logger.
- Prefer a named field when the relationship should stay visible at the call site (`s.clock.Now()` rather than a promoted `Now`).

**Examples:**

```go
type Logger struct{}

func (Logger) Info(msg string) { fmt.Println(msg) }

type Server struct {
	Logger
	Addr string
}

func (s Server) Start() { s.Info("listen " + s.Addr) }
```

**Sources:**

- https://go.dev/ref/spec#Struct_types
- https://go.dev/doc/faq
- https://go.dev/doc/effective_go
</details>

<details>
<summary>What is the purpose of the init() function?</summary>

### What is the purpose of the init() function?

**Definition:** `init` is a function with no arguments and no results, declared in the package block. After the package’s variables are initialized, Go calls every `init` in source order. Imported packages are initialized first, and each package is initialized once.

**How it works:**

#### Initialization order

- **Cannot be called**: the identifier `init` is not declared as a callable function. You cannot refer to it.
- **Many inits**: a package may declare several `init` functions, including more than one in a single file.
- **Variable order**: package-level variables are initialized in an order that respects lexical dependencies, then `init` functions run.
- **Goroutines**: since Go 1, code called from `init` may start goroutines.

**When to use:**

#### Registration, not business logic

- Use `init` to register a driver, a flag default, or another side effect that must happen when the package is imported.
- Keep request handling and I/O that can fail out of `init`. An error there is hard to return to a caller; `main` or an explicit setup function can return it.

**Examples:**

```go
func init() {
	http.HandleFunc("/health", func(w http.ResponseWriter, r *http.Request) {
		w.WriteHeader(http.StatusOK)
	})
}
```

**Sources:**

- https://go.dev/ref/spec#Package_initialization
- https://go.dev/doc/effective_go
</details>

<details>
<summary>How does Go's garbage collector work?</summary>

### How does Go's garbage collector work?

**Definition:** Go’s garbage collector is a concurrent, parallel, non-moving, non-generational mark-sweep collector. It reclaims heap memory that the program can no longer reach. Most values can live on the stack; the compiler places a value on the heap when a reference to it escapes.

**How it works:**

#### Mark, sweep, and the heap goal

- **Mark**: the collector traces from roots and marks reachable objects. The runtime source describes it as type-accurate (precise), concurrent with mutator threads, and using a write barrier.
- **Sweep**: after marking, unmarked heap memory becomes available for allocation. Go does not move objects to compact the heap.
- **GOGC**: the environment variable sets the trade-off between GC CPU and memory. The guide defines the target heap as live heap plus `(live heap + GC roots) * GOGC / 100`. The default proportion in the runtime commentary is 100.

**Examples:**

```go
func live() *int {
	n := 1
	return &n // n escapes to the heap
}
```

**Sources:**

- https://go.dev/doc/gc-guide
- https://go.dev/doc/faq
- https://pkg.go.dev/runtime
</details>

<details>
<summary>Can you describe how Go's type assertion works?</summary>

### Can you describe how Go's type assertion works?

**Definition:** For an interface value `x` (not a type parameter) and a type `T`, `x.(T)` asserts that `x` is not `nil` and that the value stored in `x` has type `T`. If `T` is an interface, it asserts that the dynamic type implements `T`.

**How it works:**

#### Single result or comma-ok

- **Panic form**: `v := x.(T)` panics at run time if the assertion is false.
- **Comma-ok**: `v, ok := x.(T)` yields `ok == false` and the zero value of `T` instead of panicking.
- **Type switch**: `switch v := x.(type)` is the multi-way form. In each case, `v` has that case’s type.

**When to use:**

#### Assertion or type switch

- Use the comma-ok form when one type is expected and failure is a normal result.
- Use a type switch when several concrete types are meaningful. Use `errors.As` for error trees instead of asserting an `error` by hand.

**Examples:**

```go
func asString(v any) (string, bool) {
	s, ok := v.(string)
	return s, ok
}
```

**Sources:**

- https://go.dev/ref/spec#Type_assertions
- https://go.dev/doc/effective_go
</details>

<details>
<summary>What are struct types in Go and how do you use them?</summary>

### What are struct types in Go and how do you use them?

**Definition:** A struct is a sequence of named fields, each with a name and a type. Fields may also be embedded. A field may have a string tag, which reflection can read; tags participate in struct type identity and are otherwise ignored by the language.

**How it works:**

#### Values, literals, and tags

- **Composite literal**: `Point{X: 1, Y: 2}` names fields. The zero struct has the zero value of every field.
- **Comparable**: structs can be compared with `==` when every field is comparable, so they can be map keys. Maps and slices as fields make the struct incomparable.
- **No recursive value fields**: a struct cannot contain itself by value. It can contain a pointer to itself.

**Examples:**

```go
type Point struct {
	X int `json:"x"`
	Y int `json:"y"`
}

func moved(p Point, dx int) Point {
	p.X += dx
	return p
}
```

**Sources:**

- https://go.dev/ref/spec#Struct_types
- https://go.dev/doc/effective_go
- https://pkg.go.dev/encoding/json
</details>

<details>
<summary>Describe the concept of package aliasing and when it would be used.</summary>

### Describe the concept of package aliasing and when it would be used.

**Definition:** An import binds a name, the package name, to an import path. The default name is the package’s own name. An explicit name in the import spec aliases that package for the file.

**How it works:**

#### Three import forms

- **Alias**: `import jsonv1 "encoding/json"` uses `jsonv1` as the qualifier.
- **Dot import**: `import . "fmt"` lets the file use `Println` without a qualifier. The spec allows it; Effective Go treats it as something to avoid in ordinary code.
- **Blank import**: `import _ "path"` evaluates the package only for its initialization side effects.

**When to use:**

#### Name collisions

- Alias an import when two packages would otherwise use the same name, or when the last path element is not the name you want at the call site.
- Leave the default name when it is already clear.

**Examples:**

```go
import (
	"math/rand"

	mrand "math/rand/v2"
)

func seed() int {
	return rand.Intn(10) + mrand.IntN(10)
}
```

**Sources:**

- https://go.dev/ref/spec#Import_declarations
- https://go.dev/doc/effective_go
</details>

<details>
<summary>How do you manage package versioning in Go modules?</summary>

### How do you manage package versioning in Go modules?

**Definition:** A module is released with a semantic version. The major number signals incompatible API changes, the minor number backward-compatible API changes, and the patch number changes that do not affect the public API. Major version 2 and higher use a `/vN` suffix in the module path.

**How it works:**

#### What a version tells a consumer

- **v0 and pre-release**: `v0.x.x` and versions such as `v1.2.3-beta.2` carry no stability guarantee.
- **v1 and later minors**: a new minor version may add API. Callers of the previous minor do not have to change. A new major version is a different import path (`example.com/mod/v2`).
- **Selection**: `go get example.com/mod@v1.2.3` adds or changes the requirement. The go command records the chosen versions in `go.mod` and checksums in `go.sum`.

**Examples:**

```go
module example.com/app

go 1.22

require example.com/widget v1.4.0
```

**Sources:**

- https://go.dev/doc/modules/version-numbers
- https://go.dev/doc/modules/managing-dependencies
- https://go.dev/doc/faq
</details>

<details>
<summary>Explain how to create and import custom packages in Go.</summary>

### Explain how to create and import custom packages in Go.

**Definition:** A package is one or more source files in the same directory, all declaring the same package name. A module’s `go.mod` sets the import-path prefix. Another package imports the directory’s path and uses only exported names.

**How it works:**

#### Layout inside a module

- **One directory, one package**: files in that directory are compiled together. Names defined in one file are visible to the others.
- **Import path**: for module `example.com/app`, package directory `store` is imported as `example.com/app/store`.
- **Commands**: a `main` package builds an executable. Library packages use any other package name, usually the last path element.

**When to use:**

#### A new package or more files

- Add another file in the same directory when the code shares the same API and the same unexported state.
- Make a new directory when the API is a separate concept. Put packages you do not want other modules to import under `internal/`.

**Examples:**

```go
package store

func Save(id string) error { return nil }
```

**Sources:**

- https://go.dev/doc/code
- https://go.dev/doc/modules/layout
</details>

<details>
<summary>Describe cross-compiling in Go.</summary>

### Describe cross-compiling in Go.

**Definition:** `GOOS` is the operating system to compile for, and `GOARCH` is the processor architecture. Setting them for a `go build` selects that target. Examples in the go command docs include `linux`, `darwin`, and `windows` for `GOOS`, and `amd64`, `386`, `arm`, and `ppc64` for `GOARCH`.

**How it works:**

#### Target selection

- **Per command**: `GOOS=linux GOARCH=amd64 go build` compiles that invocation for Linux on amd64 without changing the saved environment.
- **Defaults**: `go env GOOS` and `go env GOARCH` print the effective values. `go env -w` changes the user default.
- **cgo**: cross-compiling a package that uses cgo needs a C toolchain for the target. Pure Go packages do not.

**When to use:**

#### Ship a binary for another machine

- Cross-compile when the build machine is not the deployment OS or architecture.
- Build on the target, or with its C compiler, when the program depends on cgo.

**Examples:**

```go
// Build for Linux on amd64 from any host:
// GOOS=linux GOARCH=amd64 go build -o app ./cmd/app
```

**Sources:**

- https://pkg.go.dev/cmd/go
- https://go.dev/doc/install/source
</details>

<details>
<summary>What is reflection in Go and why is it useful?</summary>

### What is reflection in Go and why is it useful?

**Definition:** The `reflect` package inspects a value’s type and representation at run time. `reflect.TypeOf` and `reflect.ValueOf` start from an `any` value. Reflection is how `encoding/json` and `fmt` honor struct tags and unknown concrete types.

**How it works:**

#### Type and value

- **Kind first**: `Value.Kind` reports whether the value is a struct, slice, int, and so on. Calling a method that does not apply to that kind panics.
- **Laws**: to modify a value, the `Value` must be settable, which means it addresses an exported field or an element you can assign. `CanSet` reports that.
- **Cost**: reflection loses static checking and is slower than direct code. The standard library uses it at boundaries where the concrete type is not known in advance.

**When to use:**

#### A real boundary, not ordinary code

- Use reflection for generic encoding, formatting, or a tool that must walk arbitrary values.
- Prefer an interface, a type switch, or type parameters when the set of types is known.

**Examples:**

```go
func typeName(v any) string {
	return reflect.TypeOf(v).String()
}
```

**Sources:**

- https://pkg.go.dev/reflect
- https://go.dev/doc/effective_go
</details>

<details>
<summary>How does Go handle immutable strings?</summary>

### How does Go handle immutable strings?

**Definition:** A string is an immutable sequence of bytes. Once created, its contents cannot be changed. `len(s)` is the number of bytes, and `s[i]` is a byte. Taking the address of a string byte is illegal.

**How it works:**

#### Bytes, runes, and copies

- **Indexing is bytes**: a range loop `for i, r := range s` yields byte index `i` and rune `r`. Rune `r` may cover more than one byte.
- **Build a new string**: concatenation and `string([]byte(...))` allocate a new string. To edit text, mutate a `[]byte` or a `strings.Builder`, then convert once.
- **Sharing**: converting between `string` and `[]byte` copies. The immutability rule is what makes the string safe to share across goroutines.

**Examples:**

```go
func shout(s string) string {
	b := []byte(s)
	for i, c := range b {
		if c >= 'a' && c <= 'z' {
			b[i] = c - 'a' + 'A'
		}
	}
	return string(b)
}
```

**Sources:**

- https://go.dev/ref/spec#String_types
- https://go.dev/doc/effective_go
</details>

<details>
<summary>What is the syntax to create a variable that is not exported outside of a package?</summary>

### What is the syntax to create a variable that is not exported outside of a package?

**Definition:** An identifier is exported only when its first character is a Unicode uppercase letter and it is declared in the package block or is a field or method name. Any other name, including one that starts with a lowercase letter, is not exported.

**How it works:**

#### Visibility is the first letter

- **Package-level**: `var retryDelay = time.Second` is visible in every file of the package and invisible to importers.
- **Fields and methods**: `type User struct { name string }` keeps `name` unexported. Another package cannot select `u.name`.
- **Same directory**: unexported names are shared by all files of the package. They are not file-private.

**Examples:**

```go
type User struct {
	Name string
	age  int
}

func (u User) Age() int { return u.age }
```

**Sources:**

- https://go.dev/ref/spec#Exported_identifiers
- https://go.dev/doc/effective_go
</details>

<details>
<summary>How do you implement an enum in Go?</summary>

### How do you implement an enum in Go?

**Definition:** Go has no enum type. The usual form is a set of constants, often with `iota`, sometimes given a named integer type so the values have a distinct type.

**How it works:**

#### iota in a const block

- **Index**: inside one `const` declaration, `iota` is the zero-based index of that ConstSpec. It resets to zero in the next `const`.
- **Repeated expression**: omitting the expression repeats the previous one, so `Sunday = iota` followed by `Monday` numbers them 0, 1, …
- **Not closed**: nothing stops a caller from converting an arbitrary integer to the named type. A String method makes printing and logs readable.

**When to use:**

#### Named constants or plain ints

- Use a named type plus `iota` when the set is ordered and you want the compiler to reject mixing it with a raw `int` without a conversion.
- Use individual `const` values with explicit numbers when the numbers come from a protocol and are not a dense sequence.

**Examples:**

```go
type Weekday int

const (
	Sunday Weekday = iota
	Monday
	Tuesday
)
```

**Sources:**

- https://go.dev/ref/spec#Iota
- https://go.dev/doc/effective_go
</details>

<details>
<summary>What are some ways to manage state in a concurrent Go program?</summary>

### What are some ways to manage state in a concurrent Go program?

**Definition:** The memory model requires programs that modify data accessed by multiple goroutines to serialize that access. The documented tools are channel operations and the primitives in `sync` and `sync/atomic`.

**How it works:**

#### Ownership, locks, atomics

- **One owner**: the share-memory codewalk keeps a map inside the goroutine that alone updates it, and other goroutines send values on a channel.
- **Mutex**: `sync.Mutex` or `sync.RWMutex` around the read-modify-write. The lock must be held on every access that the memory model does not already synchronize.
- **atomic**: `sync/atomic` covers individual words and the pointer/value helpers. A data race is a concurrent write overlapping another read or write, unless every access is atomic.

**When to use:**

#### Channel or mutex

- Pass ownership on a channel when the value moves from producer to consumer and only one goroutine should touch it at a time.
- Use a mutex when many goroutines share one struct in place and the critical section is short.

**Examples:**

```go
type counter struct {
	mu sync.Mutex
	n  int
}

func (c *counter) Add(d int) {
	c.mu.Lock()
	defer c.mu.Unlock()
	c.n += d
}
```

**Sources:**

- https://go.dev/ref/mem
- https://go.dev/doc/codewalk/sharemem
- https://pkg.go.dev/sync
</details>

<details>
<summary>How does Go's panic and recover mechanism work?</summary>

### How does Go's panic and recover mechanism work?

**Definition:** `panic` stops normal execution of the goroutine and runs its deferred calls. `recover` stops that panic only when called directly from a deferred function, and it returns the value passed to `panic`. A `recover` anywhere else returns `nil`.

**How it works:**

#### Panic is not the error path

- **Deferred during panic**: deferred functions still run. A deferred function may call `recover`. If it does not, the panic continues up to the caller after the deferred calls finish.
- **Re-panic**: Effective Go’s regexp example recovers, checks the concrete type, and panics again when the value is not the expected parse error.
- **Libraries**: return `error` for conditions the caller can handle. Panic for programmer errors and impossible states. A server may recover at a request boundary so one handler does not kill the process; `net/http` does this for `ServeHTTP`.

**When to use:**

#### error return or panic

- Return an error for a missing file, a bad request, or a failed query.
- Panic, and recover at a boundary you own, when continuing would corrupt state. Do not recover in a library and hide the panic from the caller.

**Examples:**

```go
func safely(work func()) (err error) {
	defer func() {
		if v := recover(); v != nil {
			err = fmt.Errorf("panic: %v", v)
		}
	}()
	work()
	return nil
}
```

**Sources:**

- https://go.dev/doc/articles/defer_panic_recover.html
- https://go.dev/doc/effective_go
- https://pkg.go.dev/builtin
</details>

<details>
<summary>What is the purpose of the context package in Go?</summary>

### What is the purpose of the context package in Go?

**Definition:** `context.Context` carries a deadline, a cancellation signal, and request-scoped values across API boundaries. A server creates a Context for an incoming request, and outgoing calls accept that Context or a child derived with `WithCancel`, `WithDeadline`, `WithTimeout`, or `WithValue`.

**How it works:**

#### Cancellation propagates down

- **Parent cancels children**: when a Context is canceled, every Context derived from it is canceled too. `ctx.Done()` is closed when the Context is done, and `ctx.Err()` explains why.
- **First parameter**: package docs tell you to pass `ctx context.Context` as the first parameter, and not to store a Context inside a struct.
- **Values are request-scoped**: `WithValue` is for data that travels with the request, such as a request ID, not for optional parameters.

**When to use:**

#### Any call that should be cancelable

- Take a Context in functions that block on I/O, locks that wait, or work the caller may abandon.
- Do not put a Context on a pure computation that cannot be interrupted and has no request-scoped data.

**Examples:**

```go
func fetch(ctx context.Context, url string) error {
	ctx, cancel := context.WithTimeout(ctx, 2*time.Second)
	defer cancel()
	req, err := http.NewRequestWithContext(ctx, http.MethodGet, url, nil)
	if err != nil {
		return err
	}
	resp, err := http.DefaultClient.Do(req)
	if err != nil {
		return err
	}
	defer resp.Body.Close()
	return nil
}
```

**Sources:**

- https://pkg.go.dev/context
- https://pkg.go.dev/net/http
</details>

### Advanced Golang Concepts

<details>
<summary>How does Go implement object-oriented programming concepts?</summary>

### How does Go implement object-oriented programming concepts?

**Definition:** Go has types and methods and allows an object-oriented style, without classes or a type hierarchy. Methods can be declared on any defined type in the same package, not only on structs. Interfaces are satisfied implicitly. Embedding promotes fields and methods.

**How it works:**

#### What replaces classes

- **Methods**: `func (p *Point) Scale(f float64)` binds `Scale` to base type `Point`. The receiver is a value or a pointer, passed by value like any other argument.
- **Interfaces**: behavior the caller needs is a small interface. There is no inheritance tree to implement.
- **Composition**: a struct holds other values by named fields or by embedding. The FAQ says embedding is analogous to subclassing and not the same thing.

**Examples:**

```go
type Reader interface{ Read([]byte) (int, error) }

type upper struct{ r Reader }

func (u upper) Read(p []byte) (int, error) {
	n, err := u.r.Read(p)
	for i := 0; i < n; i++ {
		if p[i] >= 'a' && p[i] <= 'z' {
			p[i] = p[i] - 'a' + 'A'
		}
	}
	return n, err
}
```

**Sources:**

- https://go.dev/doc/faq
- https://go.dev/doc/effective_go
- https://go.dev/ref/spec#Method_declarations
</details>

<details>
<summary>Explain the difference between concurrency and parallelism in Go, with examples.</summary>

### Explain the difference between concurrency and parallelism in Go, with examples.

**Definition:** Concurrency is the structure of a program: independently executing pieces, coordinated with goroutines and channels. Parallelism is those pieces actually running at the same time on different processors. The FAQ states that concurrency enables parallelism only when the problem can be split into pieces that run in parallel.

**How it works:**

#### Goroutines and GOMAXPROCS

- **Many goroutines, one CPU**: with `GOMAXPROCS` set to 1, goroutines still interleave, including while others block in system calls, but Go code does not run on two CPUs at once.
- **Default parallelism**: the FAQ says `GOMAXPROCS` defaults to the number of available CPU cores, so a program that can run in parallel does so unless you change it.
- **Not every concurrent program speeds up**: the prime sieve in the spec launches many goroutines and has no significant parallelism. Adding CPUs can make that kind of program slower.

**Examples:**

```go
func fanIn(ctx context.Context, a, b <-chan int) <-chan int {
	out := make(chan int)
	go func() {
		defer close(out)
		for a != nil || b != nil {
			select {
			case v, ok := <-a:
				if !ok {
					a = nil
					continue
				}
				out <- v
			case v, ok := <-b:
				if !ok {
					b = nil
					continue
				}
				out <- v
			case <-ctx.Done():
				return
			}
		}
	}()
	return out
}
```

**Sources:**

- https://go.dev/doc/faq
- https://pkg.go.dev/runtime
</details>

<details>
<summary>What is the Go memory model?</summary>

### What is the Go memory model?

**Definition:** The Go memory model specifies when a read of a variable in one goroutine is guaranteed to observe a write to that variable in another goroutine. Its advice is to serialize shared mutation with channel operations or with `sync` and `sync/atomic`, and not to rely on clever unsynchronized code.

**How it works:**

#### Data-race-free means sequentially consistent

- **Data race**: a write concurrent with another read or write of the same location, unless all of those accesses are atomic. A race-free program behaves as if the goroutines were multiplexed on one processor.
- **Happens-before**: a send on a channel happens before the corresponding receive completes. Unlock of a mutex happens before a later lock of that mutex returns. Without such an edge, a read may not see a write.
- **Compiler and CPU**: the model is what lets the compiler and hardware reorder. Observing a program with a race and expecting a particular interleaving is outside the guarantee.

**Examples:**

```go
func publish(msg string) string {
	var ready = make(chan struct{})
	var box string
	go func() {
		box = msg
		close(ready) // happens before the receive below
	}()
	<-ready
	return box
}
```

**Sources:**

- https://go.dev/ref/mem
- https://go.dev/doc/articles/race_detector
</details>

<details>
<summary>Can you manually trigger garbage collection in Go?</summary>

### Can you manually trigger garbage collection in Go?

**Definition:** Yes. `runtime.GC` runs a garbage collection and blocks the caller until that collection is complete. It may also block the whole program. It is a tool for tests and diagnostics, not a normal request path.

**How it works:**

#### What a manual cycle does

- **Full cycle**: the runtime comment defines a cycle as sweep termination, mark, mark termination, and sweep. `GC` does not return until a full cycle has finished.
- **Not a substitute for GOGC**: steady-state pacing is controlled by `GOGC` and the guide’s heap goal. Calling `GC` on a hot path adds stop-the-world portions of the cycle.
- **Finalizers**: `runtime.GC` does not promise that finalizers have already run; the guide notes that `GC` waits until cleanups are queued, not until they finish.

**When to use:**

#### Tests and measurements

- Call `runtime.GC` in a test that must observe reclaimed memory or a finalizer boundary you are explicitly testing.
- Leave production pacing to the collector. Tune `GOGC` or the memory limit documented in the GC guide when the process needs a different CPU/memory trade-off.

**Examples:**

```go
func collectForTest() {
	runtime.GC()
}
```

**Sources:**

- https://pkg.go.dev/runtime#GC
- https://go.dev/doc/gc-guide
</details>

<details>
<summary>Describe the Go race detector and when you would use it.</summary>

### Describe the Go race detector and when you would use it.

**Definition:** The race detector instruments a program to report data races: two goroutines accessing the same variable concurrently, at least one access a write. Enable it with the `-race` flag on `go test`, `go run`, `go build`, or `go install`.

**How it works:**

#### Reports and cost

- **Report**: a warning includes stack traces of the conflicting accesses and of where the goroutines were created.
- **Overhead**: for a typical program the docs cite about 5–10× memory and 2–20× execution time. It also allocates extra memory per `defer` and `recover` that is not released until the goroutine exits.
- **Requirements**: cgo must be enabled, and non-Darwin systems need a C compiler. Supported `GOOS/GOARCH` pairs are listed in the race detector article; it is not every port.

**When to use:**

#### Tests and short reproductions

- Run `-race` on tests and on a staging binary when the code shares memory between goroutines.
- Do not leave `-race` on a latency-sensitive production binary; the documented overhead is too large for that role.

**Examples:**

```go
// go test -race ./...
func TestCounter(t *testing.T) {
	var c counter
	var wg sync.WaitGroup
	for i := 0; i < 8; i++ {
		wg.Add(1)
		go func() {
			defer wg.Done()
			c.Add(1)
		}()
	}
	wg.Wait()
}
```

**Sources:**

- https://go.dev/doc/articles/race_detector
- https://go.dev/ref/mem
</details>

<details>
<summary>How can you manage dependencies in a Go project?</summary>

### How can you manage dependencies in a Go project?

**Definition:** Dependencies are modules. `go mod init` creates `go.mod`. `go get` adds, upgrades, or downgrades a module. `go mod tidy` adds missing requirements and removes unused ones so `go.mod` matches the imports in the build.

**How it works:**

#### go.mod and the checksum database

- **require**: each requirement is a module path and a version. Indirect requirements are modules needed only by other modules.
- **go.sum**: records cryptographic checksums of module content. The go command uses them to detect a changed download.
- **Private modules**: `GOPRIVATE` lists path prefixes that must be fetched directly and not compared with the public checksum database.

**Examples:**

```go
// go get golang.org/x/text@v0.3.5
// go mod tidy
```

**Sources:**

- https://go.dev/doc/modules/managing-dependencies
- https://go.dev/doc/faq
- https://pkg.go.dev/cmd/go
</details>

<details>
<summary>What optimizations does Go employ for its compiler?</summary>

### What optimizations does Go employ for its compiler?

**Definition:** The `gc` compiler compiles ahead of time to native machine code. There is no language virtual machine. The FAQ states that an implementation may avoid copies when that does not change pass-by-value semantics, and that a variable whose address escapes a function is allocated on the garbage-collected heap.

**How it works:**

#### Optimizations the docs actually name

- **Escape analysis**: if a pointer to a local would outlive the function, the variable is heap-allocated. Otherwise it can stay on the stack and the collector never sees it.
- **Copy elision is constrained**: the compiler may skip a copy only when observable semantics stay “everything is passed by value”.
- **Inlining and register allocation** exist in `gc`, but the language specification does not promise particular inlining thresholds. Treat `go build -gcflags=-m` output as compiler diagnostics, not as a language rule.

**Examples:**

```go
func keeps() *int {
	x := 3
	return &x // reported as escaping to heap
}
```

**Sources:**

- https://go.dev/doc/faq
- https://go.dev/doc/gc-guide
</details>

<details>
<summary>How does Go handle JSON encoding and decoding?</summary>

### How does Go handle JSON encoding and decoding?

**Definition:** `encoding/json` maps JSON to Go values in `Marshal` and `Unmarshal`, as defined for RFC 7159. Exported struct fields become object members. A `json` struct tag renames a field or sets options such as `omitempty`.

**How it works:**

#### Tags and empty values

- **Name**: `` json:"myName" `` encodes the field as `myName`. `` json:"-" `` always omits the field.
- **omitempty**: the field is omitted when it is false, 0, a nil pointer, a nil interface, or an array, slice, map, or string of length zero.
- **Unmarshal**: `json.Unmarshal` stores into exported fields. Unknown object members are ignored unless you decode into a `json.RawMessage` or a map you control.

**Examples:**

```go
type Pet struct {
	Name      string   `json:"name"`
	Nicknames []string `json:"nicknames,omitempty"`
}

func encode(pets []Pet) ([]byte, error) {
	return json.Marshal(pets)
}
```

**Sources:**

- https://pkg.go.dev/encoding/json
- https://go.dev/doc/articles/json_and_go.html
</details>

<details>
<summary>Explain the use of build tags in Go.</summary>

### Explain the use of build tags in Go.

**Definition:** A build constraint, also called a build tag, decides whether a file is part of the package. It is a line comment `//go:build` near the top of the file, after only blank lines and other comments, and before the `package` clause.

**How it works:**

#### Expressions and file names

- **Boolean expression**: `//go:build linux && amd64` includes the file only when both tags match. `||` and `!` are also part of the constraint language documented by `go help buildconstraint`.
- **Name suffixes**: a file named `foo_linux.go` has an implicit constraint for that GOOS, in addition to any explicit `//go:build` line.
- **Custom tags**: `go test -tags=integration` sets extra tags the constraint can name.

**When to use:**

#### A file that is not for every target

- Use a build tag when a file calls an OS-specific API or belongs only to an integration test.
- Keep portable code untagged so one implementation is what every build compiles.

**Examples:**

```go
//go:build linux

package sys

func osName() string { return "linux" }
```

**Sources:**

- https://pkg.go.dev/cmd/go
- https://pkg.go.dev/go/build
</details>

<details>
<summary>How do you create a custom error type in Go?</summary>

### How do you create a custom error type in Go?

**Definition:** Any type with an `Error() string` method implements the built-in `error` interface. Callers can then use a type assertion or `errors.As` to read fields that a plain string cannot carry.

**How it works:**

#### Wrapping and inspection

- **Pointer or value**: if `Error` has a pointer receiver, return a pointer. Returning a nil pointer as an `error` interface makes the interface non-nil. Return a bare `nil` for success.
- **Unwrap**: a method `Unwrap() error` lets `errors.Is` and `errors.As` walk the chain. `fmt.Errorf` with `%w` returns an error whose `Unwrap` yields the operand.

**When to use:**

#### Extra data the caller must see

- Define a type when the caller branches on a field, such as a path or a status code.
- Use `fmt.Errorf("open %s: %w", path, err)` when the only addition is context around an existing error.

**Examples:**

```go
type PathError struct {
	Path string
	Err  error
}

func (e *PathError) Error() string { return e.Path + ": " + e.Err.Error() }
func (e *PathError) Unwrap() error { return e.Err }
```

**Sources:**

- https://pkg.go.dev/errors
- https://go.dev/doc/faq
- https://go.dev/doc/effective_go
</details>

<details>
<summary>Discuss the use of Go routines and thread safety.</summary>

### Discuss the use of Go routines and thread safety.

**Definition:** A goroutine is not an operating-system thread. The scheduler multiplexes goroutines onto threads. Shared variables are safe only when every conflicting access is synchronized. Maps, slices’ backing arrays, and ordinary struct fields are not synchronized for you.

**How it works:**

#### What the runtime does and does not lock

- **Scheduler**: an M is an OS thread, a G is a goroutine, and a P is the resource required to execute Go code. Blocking in a system call does not by itself make your data race-free.
- **Channels and mutexes**: a channel send/receive or a `sync.Mutex` establishes the happens-before edge the memory model requires.
- **Detect**: `go test -race` reports unsynchronized concurrent access. The race detector article defines a race as two goroutine accesses to one variable with at least one write.

**Examples:**

```go
func worker(jobs <-chan int, wg *sync.WaitGroup) {
	defer wg.Done()
	for n := range jobs {
		_ = n
	}
}
```

**Sources:**

- https://go.dev/doc/faq
- https://go.dev/ref/mem
- https://go.dev/doc/articles/race_detector
</details>

<details>
<summary>Explain how reflection is used to manipulate objects in Go.</summary>

### Explain how reflection is used to manipulate objects in Go.

**Definition:** `reflect.ValueOf` wraps a value. If that Value is settable, `Set` and the typed setters replace it. Settable means the Value addresses an exported, addressable location — typically `ValueOf(&x).Elem()` or an exported field obtained from an addressable struct.

**How it works:**

#### CanSet, then Set

- **Unaddressable copy**: `reflect.ValueOf(x)` is a copy and `CanSet` is false. Taking a pointer and calling `Elem` addresses `x`.
- **Exported only**: reflection cannot set an unexported field of a struct defined in another package.
- **Kind checks**: `SetInt` panics if the kind is not an integer kind. Check `Kind` or use `CanSet` before writing.

**Examples:**

```go
func setInt(p any, n int64) error {
	v := reflect.ValueOf(p)
	if v.Kind() != reflect.Pointer || !v.Elem().CanSet() {
		return errors.New("want settable pointer")
	}
	v.Elem().SetInt(n)
	return nil
}
```

**Sources:**

- https://pkg.go.dev/reflect
- https://go.dev/doc/effective_go
</details>

<details>
<summary>How do you use third-party packages in Go?</summary>

### How do you use third-party packages in Go?

**Definition:** A third-party package is a normal import path backed by a module. `go get example.com/mod@v1.2.3` adds the requirement. Import the package path, call its exported names, and commit `go.mod` and `go.sum`.

**How it works:**

#### Fetching and upgrading

- **Version on the command**: `@v1.2.3` selects a release. `@latest` selects the highest available release according to the go command’s version rules.
- **Build list**: the main module plus requirements form one build list. You do not vendor by default; `go mod vendor` is optional.
- **Compatibility**: the FAQ’s import compatibility rule says a new package at the same path must stay compatible. A breaking change belongs in a new major path.

**Examples:**

```go
// go get golang.org/x/text@v0.3.5
```

**Sources:**

- https://go.dev/doc/modules/managing-dependencies
- https://go.dev/doc/faq
- https://pkg.go.dev/cmd/go
</details>

<details>
<summary>Discuss the benefits and challenges of using channels for IPC in Go.</summary>

### Discuss the benefits and challenges of using channels for IPC in Go.

**Definition:** A channel lets concurrently executing functions send and receive values of one element type. In Go that communication is between goroutines in one process. It is the language’s way to transfer ownership of data, which the share-memory codewalk contrasts with locks around a shared structure.

**How it works:**

#### What channels synchronize

- **Happens-before**: the send happens before the corresponding receive completes, so the receiver observes the writes that prepared the value.
- **Direction**: `chan<- T` can only send and `<-chan T` can only receive. The spec allows converting a bidirectional channel to a directional one.
- **Not OS IPC**: a Go channel does not connect two operating-system processes. Pipes, sockets, and `os/exec` do that. Closing a channel is a signal to receivers, and sending on a closed channel panics.

**When to use:**

#### Handoff versus a mutex

- Use a channel when the value should move to another goroutine, or when `select` must also wait on cancellation.
- Use a mutex when many goroutines update one struct in place and a channel would only add copying.

**Examples:**

```go
func gen(n int) <-chan int {
	out := make(chan int)
	go func() {
		defer close(out)
		for i := 0; i < n; i++ {
			out <- i
		}
	}()
	return out
}
```

**Sources:**

- https://go.dev/ref/spec#Channel_types
- https://go.dev/ref/mem
- https://go.dev/doc/codewalk/sharemem
</details>

<details>
<summary>How would you avoid deadlocks in concurrent programming in Go?</summary>

### How would you avoid deadlocks in concurrent programming in Go?

**Definition:** A deadlock here means goroutines stuck waiting on each other so no communication or lock can proceed. The spec says a `select` on only `nil` channels with no `default` blocks forever, and an unbuffered send proceeds only when a receive is ready.

**How it works:**

#### Waits that cannot complete

- **Pair every send**: an unbuffered `ch <- v` with no receiver blocks the sender. Buffer, or make sure another goroutine receives.
- **Lock order**: if two goroutines need two `sync.Mutex` values, acquire them in the same order in every goroutine so each wait can still be satisfied.
- **Cancellation**: include `<-ctx.Done()` in a `select` so a blocked wait ends when the caller cancels. Do not hold a mutex across a channel operation that may wait.

**Examples:**

```go
func sendOrDone(ctx context.Context, ch chan<- int, v int) error {
	select {
	case ch <- v:
		return nil
	case <-ctx.Done():
		return ctx.Err()
	}
}
```

**Sources:**

- https://go.dev/ref/spec#Select_statements
- https://go.dev/ref/spec#Channel_types
- https://pkg.go.dev/context
</details>

<details>
<summary>What is the Stringer interface and why is it important?</summary>

### What is the Stringer interface and why is it important?

**Definition:** `fmt.Stringer` is implemented by any value with a `String() string` method. That method defines the native format. `fmt` uses it for verbs that accept a string and for unformatted printers such as `Print`.

**How it works:**

#### Printing hooks

- **Stringer**: `String()` is for `%s`, `%v`, and `Print`. It is not used for `%#v`.
- **GoStringer**: `GoString() string` supplies `%#v`.
- **Formatter**: a `Format(f fmt.State, verb rune)` method takes over formatting for that value when you need verb-specific output.

**When to use:**

#### A readable default

- Implement `Stringer` when the default field dump is not what logs and tests should show.
- Leave it unimplemented when `%v` on the struct fields is already the right format.

**Examples:**

```go
type UserID int

func (id UserID) String() string {
	return fmt.Sprintf("user:%d", int(id))
}
```

**Sources:**

- https://pkg.go.dev/fmt#Stringer
- https://go.dev/doc/effective_go
</details>

<details>
<summary>How do you manage database connections in Go?</summary>

### How do you manage database connections in Go?

**Definition:** `database/sql` is a generic interface around SQL databases and must be used with a driver. `sql.Open` prepares a `*sql.DB`. That handle is safe for concurrent use by multiple goroutines and maintains its own pool of idle connections. It does not, by itself, create a network connection until a query needs one.

**How it works:**

#### Pool, context, and lifetime

- **One DB value**: open it at startup and close it when the process leaves. `PingContext` checks that a connection can be obtained.
- **Limits**: `SetMaxOpenConns`, `SetMaxIdleConns`, and `SetConnMaxLifetime` bound the pool. Leave them at zero only when the defaults are acceptable.
- **Cancellation**: pass a `Context` to `QueryContext` and `ExecContext`. Drivers that do not support cancellation do not return until the query finishes.

**When to use:**

#### A long-lived pool

- Keep one `*sql.DB` for the process and share it across requests.
- Do not call `Open` and `Close` per request; that discards the pool the package is built to maintain.

**Examples:**

```go
func openDB(driver, dsn string) (*sql.DB, error) {
	db, err := sql.Open(driver, dsn)
	if err != nil {
		return nil, err
	}
	db.SetMaxOpenConns(16)
	return db, nil
}
```

**Sources:**

- https://pkg.go.dev/database/sql
</details>

<details>
<summary>Describe the http package in Go for web programming.</summary>

### Describe the http package in Go for web programming.

**Definition:** `net/http` provides HTTP client and server implementations. `Get`, `Head`, `Post`, and `PostForm` issue requests. A server registers handlers and serves them with `ListenAndServe` or `ListenAndServeTLS`.

**How it works:**

#### Handlers and clients

- **Handler**: `ServeHTTP(ResponseWriter, *Request)` is the server contract. `HandleFunc` adapts an ordinary function. `HandlerFunc` in Effective Go shows that adapter.
- **Close the body**: the package docs require the caller to close `resp.Body` after `Get` or `Do`.
- **ServeMux**: register patterns on a mux and pass it to `ListenAndServe`. Since Go 1.22, method and wildcard patterns such as `GET /users/{id}` are part of `ServeMux`, and `Request.PathValue` reads the wildcard.

**Examples:**

```go
func main() {
	mux := http.NewServeMux()
	mux.HandleFunc("GET /health", func(w http.ResponseWriter, r *http.Request) {
		w.WriteHeader(http.StatusOK)
	})
	log.Fatal(http.ListenAndServe(":8080", mux))
}
```

**Sources:**

- https://pkg.go.dev/net/http
- https://go.dev/doc/articles/wiki
- https://go.dev/doc/effective_go
</details>

<details>
<summary>What is cgo and when would you use it?</summary>

### What is cgo and when would you use it?

**Definition:** cgo builds Go packages that call C. Importing the pseudo-package `C` refers to C types and functions. A comment immediately above `import "C"` is the preamble compiled as a C header. The FAQ warns that linking C into a Go process gives up Go’s memory safety and stack management.

**How it works:**

#### The foreign-function boundary

- **gc cannot call C directly**: the FAQ says `gc` uses a different calling convention and linker. cgo is the supported mechanism. SWIG extends it to C++.
- **Pointer rules**: Go 1.6 defined rules for sharing Go pointers with C. Passing a Go pointer into C that retains it beyond the call is outside those rules.
- **Build cost**: cgo needs a C compiler for the target and complicates cross-compilation.

**When to use:**

#### A C library you cannot rewrite

- Use cgo when the required library exists only in C and a pure Go port is not available.
- Stay in Go when the standard library or a pure Go module already provides the feature. The FAQ says to do this with care because of the safety you give up.

**Examples:**

```go
/*
#include <stdlib.h>
*/
import "C"

func alloc(n int) {
	p := C.malloc(C.size_t(n))
	C.free(p)
}
```

**Sources:**

- https://pkg.go.dev/cmd/cgo
- https://go.dev/doc/faq
- https://go.dev/doc/go1.6
</details>

<details>
<summary>Explain the difference between iota and const in Go.</summary>

### Explain the difference between iota and const in Go.

**Definition:** `const` declares a constant. `iota` is a predeclared identifier that, inside a constant declaration, is the index of the current ConstSpec, starting at zero. `iota` is not a separate kind of declaration.

**How it works:**

#### Constants versus the counter

- **const**: binds names to constant expressions. Typed constants take the declared type. Untyped constant expressions keep arbitrary precision until assigned to a variable, as the FAQ describes.
- **iota**: `c0 = iota` then `c1 = iota` yields 0 and 1. In the next `const` block, `iota` starts at 0 again. Several uses of `iota` in one ConstSpec share one value.
- **A const without iota**: `const pi = 3.14159` does not involve `iota` at all.

**Examples:**

```go
const (
	_ = iota
	Read
	Write
)
```

**Sources:**

- https://go.dev/ref/spec#Iota
- https://go.dev/ref/spec#Constant_declarations
- https://go.dev/doc/faq
</details>

### Go Tools, Testing, and Performance

<details>
<summary>What is go vet and when would you use it?</summary>

### What is go vet and when would you use it?

**Definition:** `go vet` reports suspicious constructs, such as `Printf` calls whose arguments do not match the format string. It uses heuristics: a report is not guaranteed to be a real bug, and it does not catch every bug the compiler misses. The exit code is non-zero when a problem is reported.

**How it works:**

#### What vet checks

- **Invocation**: `go vet` vets the package in the current directory. `go vet my/project/...` vets a tree.
- **Format strings and common mistakes**: the documented example is a `Printf` whose arguments do not align with the format. The FAQ also points at `go vet` for the loop-variable capture mistake in goroutine literals.
- **Not a test**: vet does not run the program. `go test` runs tests. The compiler still has to accept the package.

**When to use:**

#### Before review and in CI

- Run `go vet ./...` on changes that format strings, start goroutines, or use locks and copies `vet` knows about.
- Do not treat a clean vet run as proof the program is correct.

**Examples:**

```go
// go vet ./...
func announce(name string) {
	fmt.Printf("hello %s\n", name)
}
```

**Sources:**

- https://pkg.go.dev/cmd/vet
- https://go.dev/doc/faq
</details>

<details>
<summary>Describe the purpose of Go's benchmarking tools.</summary>

### Describe the purpose of Go's benchmarking tools.

**Definition:** A benchmark is `func BenchmarkXxx(b *testing.B)` in a `_test.go` file. `go test` runs it enough times to measure the loop body. The body iterates `b.N` times. Results are printed when you pass `-bench`.

**How it works:**

#### Measuring a function

- **b.N**: the benchmark function must execute the measured work `b.N` times. The testing harness chooses `b.N`.
- **Flags**: `go test -bench=.` runs benchmarks matching the regexp `.`. `-benchmem` adds allocation statistics. `b.ReportAllocs` asks the same benchmark to report allocations.
- **Not a unit test**: a benchmark does not replace `Test` functions. It answers how much time or allocation a chosen input costs.

**Examples:**

```go
func BenchmarkReverse(b *testing.B) {
	s := "hello"
	for i := 0; i < b.N; i++ {
		Reverse(s)
	}
}
```

**Sources:**

- https://pkg.go.dev/testing
- https://pkg.go.dev/cmd/go
</details>

<details>
<summary>How do you profile Go applications?</summary>

### How do you profile Go applications?

**Definition:** The runtime emits profiles in the format the `pprof` tool reads. Collect them with `go test` or from the endpoints in `net/http/pprof`. The diagnostics guide groups this with tracing, debugging, and runtime statistics.

**How it works:**

#### Profiles the runtime already defines

- **cpu**: where the program spends time while using CPU, not while sleeping or waiting for I/O.
- **heap**: allocation samples, for current and historical memory use and for leak investigations.
- **goroutine, block, mutex, threadcreate**: goroutine stacks, where goroutines block on synchronization, contention on mutexes, and where new OS threads are created.
- **Isolation**: the diagnostics page notes that precise memory profiling skews CPU profiles, so collect one kind at a time when you need a precise result.

**Examples:**

```go
import _ "net/http/pprof"

func debugServer() {
	go func() {
		log.Println(http.ListenAndServe("localhost:6060", nil))
	}()
}
```

**Sources:**

- https://go.dev/doc/diagnostics
- https://pkg.go.dev/runtime/pprof
- https://pkg.go.dev/net/http/pprof
</details>

<details>
<summary>Discuss the process of writing unit tests in Go and the use of the testing package.</summary>

### Discuss the process of writing unit tests in Go and the use of the testing package.

**Definition:** `testing` supports automated tests run by `go test`. A test is `TestXxx(*testing.T)` in a `_test.go` file. Failure is reported with `Error`, `Errorf`, `Fatal`, and the related methods. The same package also defines benchmarks (`BenchmarkXxx`), examples (`ExampleXxx`), and fuzz tests (`FuzzXxx`).

**How it works:**

#### Inside one Test function

- **t.Helper**: mark a helper so failure lines point at the caller.
- **t.Run**: subtests take a name and a function. `go test -run` can select a subtest.
- **Parallel**: `t.Parallel` marks a test that may run at the same time as other parallel tests. Shared state then needs the same synchronization as production code.

**Examples:**

```go
func TestAdd(t *testing.T) {
	t.Run("zero", func(t *testing.T) {
		if Add(0, 0) != 0 {
			t.Fatal("Add(0, 0)")
		}
	})
}
```

**Sources:**

- https://pkg.go.dev/testing
- https://go.dev/doc/code
</details>

<details>
<summary>What is the httptest package in Go, and why is it useful?</summary>

### What is the httptest package in Go, and why is it useful?

**Definition:** `net/http/httptest` provides HTTP servers and requests for tests. `NewServer` starts an HTTP server on a system-chosen port on the loopback interface and returns a `Server` whose `URL` is the base URL.

**How it works:**

#### In-process HTTP

- **Server**: pass the handler under test. Call `Close` when the test finishes, usually with `defer`.
- **TLS**: `NewTLSServer` serves HTTPS with a test certificate. `EnableHTTP2` on an unstarted server controls HTTP/2 before `StartTLS`.
- **ResponseRecorder**: records a `ResponseWriter` so a handler can be called without a socket.

**Examples:**

```go
func TestHealth(t *testing.T) {
	srv := httptest.NewServer(http.HandlerFunc(func(w http.ResponseWriter, r *http.Request) {
		w.WriteHeader(http.StatusNoContent)
	}))
	defer srv.Close()
	resp, err := http.Get(srv.URL)
	if err != nil {
		t.Fatal(err)
	}
	defer resp.Body.Close()
	if resp.StatusCode != http.StatusNoContent {
		t.Fatalf("status %d", resp.StatusCode)
	}
}
```

**Sources:**

- https://pkg.go.dev/net/http/httptest
- https://pkg.go.dev/net/http
</details>

<details>
<summary>How do you perform static code analysis in Go?</summary>

### How do you perform static code analysis in Go?

**Definition:** The toolchain’s static checks are the compiler and `go vet`. The compiler rejects programs that break the language. `go vet` reports suspicious constructs with heuristics and a non-zero exit status when it reports a problem. `gofmt` enforces layout, which the FAQ treats as part of recognizable Go style.

**How it works:**

#### What is in the official toolchain

- **vet**: `go vet ./...` before review. It does not guarantee every report is a real defect.
- **gofmt**: the FAQ says `gofmt` replaces a style guide for layout. `go fmt` runs it.
- **Other analyzers**: tools outside the main distribution, such as staticcheck, are separate modules. They are not part of `go vet` unless you add them yourself.

**Examples:**

```go
// go fmt ./...
// go vet ./...
```

**Sources:**

- https://pkg.go.dev/cmd/vet
- https://go.dev/doc/faq
- https://pkg.go.dev/cmd/gofmt
</details>

<details>
<summary>How can you improve the performance of Go code?</summary>

### How can you improve the performance of Go code?

**Definition:** Measure first. Benchmarks (`testing.B`) and profiles (`runtime/pprof`, documented in the diagnostics guide) show whether time is in CPU, allocations, or blocking. The GC guide then explains the CPU-versus-memory trade-off controlled by `GOGC`.

**How it works:**

#### Changes that match the docs

- **Fewer heap objects**: a value that does not escape stays on the stack and is not scanned by the collector. The GC guide is about understanding that cost, not about micro-optimizing blindly.
- **Algorithms before flags**: a profile that shows one function dominating CPU is a reason to change that function. Raising `GOGC` trades memory for less frequent collections.
- **Concurrency where the work is parallel**: the FAQ warns that extra goroutines do not speed a program that has no parallel work.

**Examples:**

```go
func BenchmarkAppend(b *testing.B) {
	for i := 0; i < b.N; i++ {
		buf := make([]byte, 0, 64)
		buf = append(buf, "ok"...)
		_ = buf
	}
}
```

**Sources:**

- https://go.dev/doc/diagnostics
- https://go.dev/doc/gc-guide
- https://pkg.go.dev/testing
</details>

<details>
<summary>Explain how to use go doc to document Go code.</summary>

### Explain how to use go doc to document Go code.

**Definition:** Doc comments sit immediately above a package clause or a top-level declaration. `go doc` prints the comment for a package or symbol. pkg.go.dev renders the same comments for public modules. A package comment’s first sentence starts with “Package Name”.

**How it works:**

#### Comments the go command reads

- **Complete sentences**: the comment guide says doc comments are written as sentences. Links use `[Name]` or `[pkg.Name]` syntax.
- **One package comment**: if several files have package comments, they are concatenated. Put the package comment in one file.
- **Lookup**: `go doc fmt.Println` prints that symbol. `go doc -all fmt` prints the package.

**Examples:**

```go
// Package greet formats short greetings.
package greet

// Hello returns a greeting for name.
func Hello(name string) string { return "Hello, " + name }
```

**Sources:**

- https://go.dev/doc/comment
- https://go.dev/doc/faq
</details>

<details>
<summary>What are the best practices for logging in Go?</summary>

### What are the best practices for logging in Go?

**Definition:** `log/slog` is the standard library’s structured logger. A record has a time, a level, a message, and key-value attributes. A `Logger` is bound to a `Handler` that decides how to write the record. Top-level functions such as `Info` and `Error` use the default logger.

**How it works:**

#### Levels, handlers, and context

- **Levels**: `Debug`, `Info`, `Warn`, and `Error` are the package levels. A handler can drop records below its minimum.
- **Attributes**: pass alternating key-value arguments, or `slog.String` and the other typed helpers, so values stay structured instead of being interpolated into the message.
- **Request context**: `Logger.InfoContext` and the other `*Context` methods take a `context.Context`. The package documents extracting attributes from that context when a handler is set up to do so.

**When to use:**

#### Structured logs at the boundary

- Use `slog` when operators will query fields. Include stable keys such as request id and error.
- Use `log` for a short program that only needs a line on stderr. Do not log secrets or full request bodies.

**Examples:**

```go
func started(id string) {
	slog.Info("request started", "id", id)
}
```

**Sources:**

- https://pkg.go.dev/log/slog
- https://pkg.go.dev/log
</details>

<details>
<summary>Describe code coverage in Go and how to measure it.</summary>

### Describe code coverage in Go and how to measure it.

**Definition:** `go test -coverprofile=cover.out` writes a coverage profile for package tests. `go tool cover -func` or `-html` turns that file into a report. Since Go 1.20, `go build -cover` instruments an application binary so integration tests can collect coverage too; those binaries write data under `GOCOVERDIR`.

**How it works:**

#### Unit tests and integration binaries

- **Package tests**: the cover profile counts which statements the tests executed. It does not prove the assertions are meaningful.
- **Main module by default**: `go build -cover` instruments packages in the main module. Dependencies are left out unless you extend the documented selection.
- **Report**: `go tool cover -html=cover.out` opens a line-by-line view.

**Examples:**

```go
// go test -coverprofile=cover.out ./...
// go tool cover -func=cover.out
```

**Sources:**

- https://go.dev/doc/build-cover
- https://pkg.go.dev/cmd/cover
</details>

### Go Patterns and Best Practices

<details>
<summary>What design patterns are commonly used in Go?</summary>

### What design patterns are commonly used in Go?

**Definition:** Go does not have a pattern library in the language. The patterns that show up in the standard library and Effective Go are small interfaces, composition by struct fields or embedding, functional values passed as arguments, and synchronization with channels or `sync`.

**How it works:**

#### Idioms with a spec or package behind them

- **Interface as the caller’s need**: `io.Reader` is one method. Callers accept the interface; concrete types such as `*os.File` implement it without declaring that they do.
- **Decorator by wrapping**: a type that holds an `io.Reader` and implements `Read` itself adds behavior. Effective Go’s discussion of interfaces is the basis, not a class-hierarchy decorator.
- **Once**: `sync.Once` runs a function a single time, with the memory-model edge documented on `Once`. That is the usual one-time initialization, rather than a lock you write by hand.

**Examples:**

```go
type Reader interface{ Read([]byte) (int, error) }
```

**Sources:**

- https://go.dev/doc/effective_go
- https://pkg.go.dev/io
- https://pkg.go.dev/sync
</details>

<details>
<summary>Describe a use case for the factory pattern in Go.</summary>

### Describe a use case for the factory pattern in Go.

**Definition:** A factory in Go is an ordinary function that returns an interface or a struct, hiding which concrete type was chosen. `http.NewRequestWithContext` and `sql.Open` are standard-library functions of that shape: the caller names what it needs and receives a ready value.

**How it works:**

#### A constructor function

- **NewX**: the name `NewServer` or `Open` is the convention. It can return `(*T, error)` when setup can fail.
- **Interface return**: return an interface when the caller must not depend on the concrete type, for example a storage implementation chosen by configuration.
- **No registry required**: a `switch` on a string in that function is enough. Reflection is unnecessary when the set of types is closed.

**Examples:**

```go
type Store interface{ Get(id string) (string, error) }

func NewStore(kind string) (Store, error) {
	switch kind {
	case "mem":
		return memStore{}, nil
	default:
		return nil, fmt.Errorf("unknown store %q", kind)
	}
}

type memStore struct{}

func (memStore) Get(id string) (string, error) { return id, nil }
```

**Sources:**

- https://go.dev/doc/effective_go
- https://pkg.go.dev/database/sql
</details>

<details>
<summary>When would you use the decorator pattern in Go?</summary>

### When would you use the decorator pattern in Go?

**Definition:** Use it when you have an interface and want to add behavior without editing the concrete type. The wrapper stores the inner value and implements the same interface, calling through after or before its own work. `http.Handler` middleware is this pattern.

**How it works:**

#### Wrapping a small interface

- **Same methods**: the wrapper must implement every method of the interface or it is a different type.
- **HandlerFunc**: Effective Go shows `HandlerFunc` adapting a function to `http.Handler`. A middleware returns a new `Handler` that calls the next one.

**When to use:**

#### Add behavior from the outside

- Wrap a `Reader`, `Handler`, or your own one-method interface when several callers need the extra behavior and the inner type should stay unchanged.
- Edit the concrete type when there is only one implementation and a wrapper would hide the only call path.

**Examples:**

```go
func withLog(next http.Handler) http.Handler {
	return http.HandlerFunc(func(w http.ResponseWriter, r *http.Request) {
		log.Println(r.Method, r.URL.Path)
		next.ServeHTTP(w, r)
	})
}
```

**Sources:**

- https://go.dev/doc/effective_go
- https://pkg.go.dev/net/http
</details>

<details>
<summary>Explain the importance of Go's fmt package.</summary>

### Explain the importance of Go's fmt package.

**Definition:** `fmt` implements formatted I/O. `Printf`, `Sprintf`, and `Fprintf` format values. `Print` and `Println` do not take a format string. `Stringer`, `GoStringer`, and `error` plug a value’s own text into that formatting.

**How it works:**

#### Verbs and errors

- **%v, %s, %#v, %T**: default format, string (or `Stringer`), Go syntax, and type.
- **%w**: `fmt.Errorf` with `%w` returns an error that unwraps to the operand, so `errors.Is` and `errors.As` still see it.
- **Scanning**: `Scan`, `Fscan`, and `Sscan` read formatted text. They are the inverse direction of `Print`.

**Examples:**

```go
func label(id int, err error) string {
	return fmt.Sprintf("id=%d err=%v", id, err)
}
```

**Sources:**

- https://pkg.go.dev/fmt
- https://go.dev/doc/effective_go
</details>

<details>
<summary>What are the recommended practices for error handling in Go?</summary>

### What are the recommended practices for error handling in Go?

**Definition:** Functions that can fail return an `error` value, usually last. The caller checks `err != nil` immediately. Add context with `fmt.Errorf` and `%w` when the extra text helps, and use `errors.Is` or `errors.As` when the caller must recognize a specific error. Panic is not this path.

**How it works:**

#### Handle, wrap, or return

- **Nil interface**: return a bare `nil`, not a nil pointer of a concrete error type stored in an `error` variable. The FAQ shows that the latter is a non-nil interface.
- **%w once**: wrap when the caller should still match the inner error. Do not wrap in a loop in a way that builds an unbounded chain.
- **Sentinel or type**: `errors.Is(err, os.ErrNotExist)` matches a sentinel through wrappers. `errors.As` fills a pointer to a type that may sit in the chain.

**Examples:**

```go
func read(path string) ([]byte, error) {
	b, err := os.ReadFile(path)
	if err != nil {
		return nil, fmt.Errorf("read %s: %w", path, err)
	}
	return b, nil
}
```

**Sources:**

- https://go.dev/doc/effective_go
- https://go.dev/doc/faq
- https://pkg.go.dev/errors
</details>

<details>
<summary>How do you avoid and detect memory leaks in Go?</summary>

### How do you avoid and detect memory leaks in Go?

**Definition:** The collector frees unreachable heap objects. A leak in Go is memory that is still reachable and therefore not collected: a goroutine blocked forever, a growing global map, or a slice that keeps a large backing array alive. The heap profile in the diagnostics guide is how you see those allocations.

**How it works:**

#### Reachability and profiles

- **Cancel goroutines**: a goroutine stuck on a channel with no sender or receiver stays reachable, and so does everything it references. A `Context` and `select` on `ctx.Done()` let it return.
- **Slice the window you need**: `s[:n]` still references the whole backing array. `bytes.Clone` or a copy of the small window lets the rest be collected.
- **Heap profile**: `go test -memprofile` or `net/http/pprof` heap data, read with `go tool pprof`, shows which stacks allocate retained memory.

**Examples:**

```go
func keepPrefix(s []byte, n int) []byte {
	out := make([]byte, n)
	copy(out, s[:n])
	return out
}
```

**Sources:**

- https://go.dev/doc/gc-guide
- https://go.dev/doc/diagnostics
- https://pkg.go.dev/runtime/pprof
</details>

<details>
<summary>What is idiomatic Go code and how do you ensure it?</summary>

### What is idiomatic Go code and how do you ensure it?

**Definition:** The FAQ says there is no separate style guide, and that Effective Go plus `gofmt` define the recognizable style. Idiomatic code is formatted by `gofmt`, uses mixedCaps names, returns errors instead of panicking for ordinary failure, and keeps interfaces small.

**How it works:**

#### Checks that are in the toolchain

- **gofmt**: run `go fmt` so layout is not a review topic. The FAQ says the repository itself is gofmt’d.
- **go vet and go test**: vet for suspicious constructs, tests for behavior.
- **Names**: Effective Go uses `MixedCaps` and `mixedCaps`, not underscores. Exported names start with an uppercase letter because that is the export rule.

**Examples:**

```go
// go fmt ./...
func (s *Server) Listen() error { return nil }
```

**Sources:**

- https://go.dev/doc/effective_go
- https://go.dev/doc/faq
- https://pkg.go.dev/cmd/gofmt
</details>

<details>
<summary>Discuss the singleton pattern in Go.</summary>

### Discuss the singleton pattern in Go.

**Definition:** `sync.Once` runs a function exactly once. The documentation states that the return from `f` synchronizes-before the return from any `once.Do(f)`. A package-level variable initialized in `init` or by a package-level expression is the other form: it also runs once, during package initialization.

**How it works:**

#### Once versus a package variable

- **Do**: concurrent callers of `once.Do(f)` all wait until `f` has returned; only one runs `f`.
- **Must not be copied**: the `Once` docs say a `Once` must not be copied after first use.
- **init**: use a package-level value when the program always needs it. Use `Once` when the cost should wait until first use.

**When to use:**

#### One shared resource

- Use `Once` for a lazy connection or template parse that every goroutine may trigger.
- Pass the dependency as a parameter when tests must substitute it. A process-wide singleton is then the wrong shape.

**Examples:**

```go
var (
	once sync.Once
	db   *sql.DB
)

func DB() *sql.DB {
	once.Do(func() {
		db, _ = sql.Open("example", "")
	})
	return db
}
```

**Sources:**

- https://pkg.go.dev/sync#Once
- https://go.dev/ref/spec#Package_initialization
</details>

<details>
<summary>What are the best practices for structuring Go projects?</summary>

### What are the best practices for structuring Go projects?

**Definition:** The module layout document says to start with the simplest tree that matches the program. A basic package is `go.mod` plus Go files in the module root. Commands live in directories whose package name is `main`. Packages that must not be imported by other modules go under `internal/`.

**How it works:**

#### Root, cmd, internal

- **One package, many files**: split files inside one directory while they share one package name and one API.
- **cmd**: the layout doc places command entry points in a directory such as `cmd/name`, with `package main` there.
- **internal**: code under `internal` can be imported only by packages rooted at the parent of that `internal` directory. The go command enforces this.

**Examples:**

```go
// module example.com/app
// cmd/app/main.go    package main
// internal/store/db.go
// go.mod
```

**Sources:**

- https://go.dev/doc/modules/layout
- https://go.dev/doc/code
</details>

<details>
<summary>How do Go routines affect the design and structure of a Go program?</summary>

### How do Go routines affect the design and structure of a Go program?

**Definition:** Goroutines are anonymous. The FAQ says they have no ID the program can look up, so you do not structure a server as “the request goroutine owns all the state”. You pass values and a `Context`, and you let a function start more goroutines when the work splits.

**How it works:**

#### Structure around communication

- **No goroutine-local storage in the language**: request data travels in arguments and in `context.Context`, which is why `net/http` handlers take a `Request` rather than reading ambient goroutine state.
- **Ownership**: the share-memory codewalk puts a mutable map in one goroutine and sends updates in. That is a design choice the memory model can explain.
- **Lifetime**: every `go` statement needs a way to finish: a closed channel, `WaitGroup`, or a canceled `Context`. Otherwise the goroutine and what it references remain reachable.

**Examples:**

```go
func serve(ctx context.Context, jobs <-chan int) {
	for {
		select {
		case <-ctx.Done():
			return
		case j, ok := <-jobs:
			if !ok {
				return
			}
			_ = j
		}
	}
}
```

**Sources:**

- https://go.dev/doc/faq
- https://go.dev/doc/codewalk/sharemem
- https://pkg.go.dev/context
</details>

### Go Networking and Protocols

<details>
<summary>How does Go handle HTTP/2?</summary>

### How does Go handle HTTP/2?

**Definition:** Go 1.6 added transparent HTTP/2 support in `net/http`. Clients and servers automatically use HTTP/2 when using HTTPS. There is no exported API in `net/http` for HTTP/2 details, just as there is none for HTTP/1.1 details.

**How it works:**

#### Automatic on HTTPS

- **Disable**: set `Transport.TLSNextProto` or `Server.TLSNextProto` to a non-nil empty map.
- **Details**: programs that must adjust HTTP/2 specifics import `golang.org/x/net/http2` and use `ConfigureServer` and `ConfigureTransport`, as the Go 1.6 notes describe.
- **Handler panics**: if `ServeHTTP` panics, the server recovers, logs a stack, and either closes the connection or sends an HTTP/2 `RST_STREAM`, depending on the protocol.

**Examples:**

```go
func serveHTTPS() error {
	return http.ListenAndServeTLS(":443", "cert.pem", "key.pem", nil)
}
```

**Sources:**

- https://go.dev/doc/go1.6
- https://pkg.go.dev/net/http
</details>

<details>
<summary>Describe TCP/UDP network programming in Go.</summary>

### Describe TCP/UDP network programming in Go.

**Definition:** Package `net` provides a portable interface for network I/O, including TCP/IP and UDP. `Dial` connects with a network name such as `tcp` or `udp`. `Listen` and `ListenPacket` accept connections or packets. `net/http` is built on these types; use `net` when you are not speaking HTTP.

**How it works:**

#### Connections and packets

- **Conn**: `Dial("tcp", addr)` returns a `Conn`. `Read` and `Write` move bytes. `Close` releases it; `defer` is the usual place.
- **PacketConn**: UDP uses `ListenPacket` and `ReadFrom` / `WriteTo`, because a packet has a remote address on each read.
- **Context**: `Dialer.DialContext` stops connecting when the context is canceled. Deadlines on a `Conn` bound a single read or write.

**When to use:**

#### Raw sockets or HTTP

- Use `net` for a custom TCP or UDP protocol.
- Use `net/http` when the protocol is HTTP. It already handles connections, HTTP/2 on HTTPS, and request contexts.

**Examples:**

```go
func dial(ctx context.Context, addr string) (net.Conn, error) {
	var d net.Dialer
	return d.DialContext(ctx, "tcp", addr)
}
```

**Sources:**

- https://pkg.go.dev/net
</details>

<details>
<summary>Explain what RPC is and how Go supports it.</summary>

### Explain what RPC is and how Go supports it.

**Definition:** RPC calls an exported method of an object over a network or other I/O connection. The standard library package `net/rpc` does this and is frozen: it is not accepting new features. A server registers an object; exported methods with the required signature become remote methods.

**How it works:**

#### The frozen net/rpc contract

- **Method shape**: the method is exported, its type is exported, it has two arguments of exported or built-in types, the second is a pointer, and it returns `error`.
- **Register**: `Register` publishes the object under its type name. Registering two objects of the same type is an error.
- **Codec**: `ServeConn` serves a single connection. HTTP can carry the codec, as the package documents. gRPC is a separate module, not `net/rpc`.

**When to use:**

#### net/rpc or something maintained

- Use `net/rpc` only to interoperate with an existing Go `net/rpc` service. The package docs say it is frozen.
- For a new service, prefer HTTP with `net/http` or a maintained RPC module you add explicitly. Do not invent methods on `net/rpc` beyond the documented signature.

**Examples:**

```go
type Arith int

func (t *Arith) Double(arg int, reply *int) error {
	*reply = arg * 2
	return nil
}
```

**Sources:**

- https://pkg.go.dev/net/rpc
</details>

<details>
<summary>How do you implement RESTful services in Go?</summary>

### How do you implement RESTful services in Go?

**Definition:** A REST-style service in Go is an `http.Handler` that maps methods and paths to handlers and encodes responses, usually with `encoding/json`. `ServeMux` registers those patterns. The standard library does not add a separate REST framework.

**How it works:**

#### Method, path, status, body

- **Patterns**: Go 1.22 `ServeMux` patterns can include the method, as in `GET /users/{id}`. `Request.PathValue` reads `{id}`.
- **Status**: `WriteHeader` sends the status. `http.Error` writes a plain-text error and a status. JSON errors are still just `WriteHeader` plus `json.NewEncoder`.
- **Context**: `r.Context()` is canceled when the client disconnects or the server’s timeout fires. Pass it into database calls.

**Examples:**

```go
func userHandler(w http.ResponseWriter, r *http.Request) {
	id := r.PathValue("id")
	w.Header().Set("Content-Type", "application/json")
	json.NewEncoder(w).Encode(map[string]string{"id": id})
}
```

**Sources:**

- https://pkg.go.dev/net/http
- https://pkg.go.dev/encoding/json
</details>

<details>
<summary>Discuss Go's support for WebSocket protocols.</summary>

### Discuss Go's support for WebSocket protocols.

**Definition:** The standard library does not implement the WebSocket protocol. `golang.org/x/net/websocket` implements a client and server for RFC 6455. Its own package comment says it lacks features found in more actively maintained packages, and it names `github.com/gorilla/websocket` and `github.com/coder/websocket`.

**How it works:**

#### Upgrade is not in net/http

- **x/net/websocket**: `Handler` adapts a function of a `*Conn` to an `http.Handler`. The import path is `golang.org/x/net/websocket`.
- **Maintained alternatives**: the package comment points at other modules when you need features it lacks. Adding one is an ordinary `go get` of that module.
- **HTTP underneath**: the handshake is still an HTTP request upgraded on a connection. `net/http` serves that request; it does not parse WebSocket frames.

**When to use:**

#### A maintained module for new work

- Read the `x/net/websocket` comment before choosing it. It documents the gap itself.
- Use `net/http` alone when the API is request/response and does not need a long-lived frame protocol.

**Examples:**

```go
import "golang.org/x/net/websocket"

func echo(ws *websocket.Conn) {
	io.Copy(ws, ws)
}

func register() {
	http.Handle("/echo", websocket.Handler(echo))
}
```

**Sources:**

- https://pkg.go.dev/golang.org/x/net/websocket
- https://pkg.go.dev/net/http
</details>

<details>
<summary>What is protobuf and how is it used in Go?</summary>

### What is protobuf and how is it used in Go?

**Definition:** Protocol buffers are a separate project, not a Go language feature. The FAQ says Go support lives in an open-source compiler plugin and library, historically `github.com/golang/protobuf`. The current module is `google.golang.org/protobuf`. Generated Go types are ordinary structs; your program imports the generated package.

**How it works:**

#### Generate, then call

- **Not encoding/json**: protobuf has its own wire format. `encoding/json` does not read `.proto` files.
- **Module**: add `google.golang.org/protobuf` with `go get` like any other dependency, and commit `go.mod`.
- **gRPC**: the gRPC Go implementation is another module (`google.golang.org/grpc`). It is not required to use protobuf by itself.

**Examples:**

```go
import "google.golang.org/protobuf/proto"

func size(m proto.Message) int {
	return proto.Size(m)
}
```

**Sources:**

- https://go.dev/doc/faq
- https://pkg.go.dev/google.golang.org/protobuf/proto
</details>

<details>
<summary>Explain how to perform file uploads and downloads using Go.</summary>

### Explain how to perform file uploads and downloads using Go.

**Definition:** Uploads in `net/http` are multipart form data. `Request.ParseMultipartForm` parses the body, and `Request.FormFile` returns the first file part and a `FileHeader`. Downloads set response headers and copy bytes to the `ResponseWriter`. `ServeContent` and `ServeFile` send a file with range handling.

**How it works:**

#### Multipart and Content-Disposition

- **ParseMultipartForm(maxMemory)**: file parts beyond what is kept in memory are stored in temporary files, via `mime/multipart`.
- **Close the file**: `FormFile` returns an `multipart.File`. Close it when you finish reading.
- **Download**: `http.ServeFile` refuses to serve a path whose elements include `..`, as the Go 1.6 notes say. Set `Content-Disposition` yourself when the browser should download rather than display.

**Examples:**

```go
func upload(w http.ResponseWriter, r *http.Request) {
	f, hdr, err := r.FormFile("file")
	if err != nil {
		http.Error(w, err.Error(), http.StatusBadRequest)
		return
	}
	defer f.Close()
	_, _ = io.Copy(io.Discard, f)
	fmt.Fprintln(w, hdr.Filename)
}
```

**Sources:**

- https://pkg.go.dev/net/http
- https://pkg.go.dev/mime/multipart
- https://go.dev/doc/go1.6
</details>

<details>
<summary>How do you secure web applications in Go?</summary>

### How do you secure web applications in Go?

**Definition:** Serve HTTPS with `ListenAndServeTLS` or a `Server` whose `TLSConfig` is a `crypto/tls.Config`. Go’s HTTP server then negotiates TLS, and on that path it uses HTTP/2 automatically. Application checks — authentication, authorization, and limiting request bodies — are still your handlers’ job.

**How it works:**

#### Transport and request limits

- **TLS**: `crypto/tls` implements the protocol. Certificates are files passed to `ListenAndServeTLS` or fields on `tls.Config`.
- **Body limits**: `http.MaxBytesReader` limits how many bytes a handler reads from the body. `ParseMultipartForm` takes a max-memory argument for the same reason.
- **Timeouts**: set `Server.ReadHeaderTimeout`, `ReadTimeout`, and `WriteTimeout` so a slow client cannot hold a goroutine without bound. Timeouts are fields on `http.Server`.

**Examples:**

```go
func serve() error {
	srv := &http.Server{
		Addr:              ":443",
		ReadHeaderTimeout: 5 * time.Second,
		Handler:           routes(),
	}
	return srv.ListenAndServeTLS("cert.pem", "key.pem")
}
```

**Sources:**

- https://pkg.go.dev/net/http
- https://pkg.go.dev/crypto/tls
- https://go.dev/doc/go1.6
</details>

<details>
<summary>What is a middleware in Go's HTTP server and how do you use it?</summary>

### What is a middleware in Go's HTTP server and how do you use it?

**Definition:** Middleware is not a language keyword. It is a function that takes an `http.Handler` and returns an `http.Handler`. The returned handler does its work and then calls `next.ServeHTTP`. `http.HandlerFunc` adapts a function to that interface, as Effective Go shows.

**How it works:**

#### Wrap the handler

- **Order**: the first wrapper you apply is the outer one. It sees the request before the inner handler.
- **Context values**: a wrapper may call `r = r.WithContext(ctx)` and pass the new request inward. Handlers must use the `Request` they are given.
- **Short-circuit**: the wrapper can write a status and return without calling `next`, for example when authentication fails.

**Examples:**

```go
func method(want string, next http.Handler) http.Handler {
	return http.HandlerFunc(func(w http.ResponseWriter, r *http.Request) {
		if r.Method != want {
			http.Error(w, "method", http.StatusMethodNotAllowed)
			return
		}
		next.ServeHTTP(w, r)
	})
}
```

**Sources:**

- https://pkg.go.dev/net/http
- https://go.dev/doc/effective_go
</details>

<details>
<summary>How can Go be used to build microservices architecture?</summary>

### How can Go be used to build microservices architecture?

**Definition:** Go has no microservice framework in the standard library. A service is a `main` package that listens with `net/http` or `net`, calls other services with an `http.Client` that uses contexts, and is built as one static binary from its module.

**How it works:**

#### Process boundaries you already have tools for

- **One module, one binary**: `go build` in that module produces the process you deploy. Cross-compile with `GOOS` and `GOARCH` when the build host differs.
- **Cancellation across calls**: pass `r.Context()` into outgoing `NewRequestWithContext` so a canceled inbound request cancels the outbound one.
- **Independent failure**: each service returns `error` and HTTP statuses. A panic in one `ServeHTTP` is recovered by `net/http` for that request; it is not a distributed transaction.

**Examples:**

```go
func call(ctx context.Context, url string) (*http.Response, error) {
	req, err := http.NewRequestWithContext(ctx, http.MethodGet, url, nil)
	if err != nil {
		return nil, err
	}
	return http.DefaultClient.Do(req)
}
```

**Sources:**

- https://pkg.go.dev/net/http
- https://go.dev/doc/code
- https://pkg.go.dev/context
</details>

### Go Frameworks, Libraries, and Interoperability

<details>
<summary>What are some popular frameworks for web development in Go, and how do you decide which one to use?</summary>

### What are some popular frameworks for web development in Go, and how do you decide which one to use?

**Definition:** The standard library server is `net/http`. Third-party frameworks are ordinary modules you `go get`; the Go project does not ship Gin, Echo, or Chi as part of the toolchain. Decide by whether `ServeMux`, `Handler`, and middleware functions already express the routes.

**How it works:**

#### Stdlib first

- **net/http**: routing, handlers, clients, TLS, and HTTP/2 on HTTPS are already there. Go 1.22 patterns reduced the need for a third-party router for method-and-path matching.
- **A module**: if you add a framework, it is a requirement in `go.mod` with a semantic version, reviewed like any other dependency.
- **Handlers stay Handler**: any framework you keep should still let you write `http.Handler` so tests can use `httptest`.

**When to use:**

#### Add a framework when the stdlib API is the bottleneck

- Stay on `net/http` for a service whose routes fit `ServeMux` patterns and small middleware.
- Add a module when you need a feature `net/http` does not provide, and pin its version in `go.mod`.

**Examples:**

```go
func newMux() *http.ServeMux {
	mux := http.NewServeMux()
	mux.HandleFunc("GET /health", func(w http.ResponseWriter, r *http.Request) {
		w.WriteHeader(http.StatusNoContent)
	})
	return mux
}
```

**Sources:**

- https://pkg.go.dev/net/http
- https://go.dev/doc/modules/managing-dependencies
- https://pkg.go.dev/net/http/httptest
</details>

<details>
<summary>Can Go interact with other programming languages, and if so, how?</summary>

### Can Go interact with other programming languages, and if so, how?

**Definition:** Yes, with an explicit boundary. cgo calls C from Go by importing `C`. SWIG extends that to C++. The FAQ says `gc` cannot call C directly because of its calling convention. The compiler also targets WebAssembly for some ports, which is how Go code runs in a host that executes Wasm.

**How it works:**

#### C, Wasm, and processes

- **cgo**: a preamble comment plus `import "C"`. You give up Go’s memory safety for that boundary.
- **Processes**: `os/exec` runs another program and talks to it over stdin, stdout, and exit status. That is ordinary I/O, not a shared address space.
- **WebAssembly**: the FAQ notes that some implementations compile to WebAssembly. `GOARCH=wasm` is that target, including `wasip1` in the go command’s build modes.

**Examples:**

```go
import "os/exec"

func runEcho() ([]byte, error) {
	return exec.Command("echo", "hi").Output()
}
```

**Sources:**

- https://go.dev/doc/faq
- https://pkg.go.dev/cmd/cgo
- https://pkg.go.dev/os/exec
</details>

<details>
<summary>Discuss the use of GraphQL in Go applications.</summary>

### Discuss the use of GraphQL in Go applications.

**Definition:** GraphQL is not part of the Go standard library or the language. A GraphQL HTTP API is still an `http.Handler`: it reads a query from the request body and writes a JSON response. The schema engine, if you use one, is a third-party module recorded in `go.mod`.

**How it works:**

#### HTTP plus a module

- **POST a query**: many GraphQL HTTP servers accept a JSON body on a single path. `json.NewDecoder(r.Body)` reads it. That part is `encoding/json` and `net/http`.
- **No stdlib executor**: do not expect `net/http` to parse a GraphQL document. That parser comes from a module you choose and version.
- **Context**: pass `r.Context()` into resolvers so a disconnected client cancels work.

**Examples:**

```go
func graphql(w http.ResponseWriter, r *http.Request) {
	defer r.Body.Close()
	var body struct {
		Query string `json:"query"`
	}
	if err := json.NewDecoder(r.Body).Decode(&body); err != nil {
		http.Error(w, err.Error(), http.StatusBadRequest)
		return
	}
	_ = body.Query
}
```

**Sources:**

- https://pkg.go.dev/net/http
- https://pkg.go.dev/encoding/json
</details>

<details>
<summary>What are some use cases for the Go mobile library?</summary>

### What are some use cases for the Go mobile library?

**Definition:** `golang.org/x/mobile` holds packages and build tools for using Go on mobile platforms. The repository describes building all-Go apps and building libraries for SDK apps via `gobind`. The project states that it is experimental.

**How it works:**

#### Apps and bound libraries

- **All-Go apps**: the `golang.org/x/mobile/app` package is the documented starting point for an app written in Go.
- **gobind**: `golang.org/x/mobile/cmd/gobind` is the starting point for a library that an Android or iOS SDK app calls.
- **Experimental**: the repository README says to use it at your own risk and that the Go team does not provide end-user support for it.

**When to use:**

#### A Go library inside a mobile SDK app

- Use `gomobile`/`gobind` when an existing Android or iOS app should call a Go package.
- Prefer a small HTTP service, or a native UI, when the mobile UI is the product and the experimental status is unacceptable.

**Examples:**

```go
import "golang.org/x/mobile/app"

func main() {
	app.Main(func(a app.App) {})
}
```

**Sources:**

- https://pkg.go.dev/golang.org/x/mobile
- https://pkg.go.dev/golang.org/x/mobile/app
- https://pkg.go.dev/golang.org/x/mobile/cmd/gobind
</details>

<details>
<summary>How does Go integrate with cloud services?</summary>

### How does Go integrate with cloud services?

**Definition:** Go integrates with a cloud the same way it integrates with any network service: an `http.Client` or a vendor module you `go get`. There is no cloud SDK inside the standard library. Official provider libraries are third-party modules with their own `go.mod` versions.

**How it works:**

#### HTTP, modules, and a single binary

- **Client**: `http.NewRequestWithContext` plus a timeout context is the portable call. Close `resp.Body`.
- **Modules**: `go get` the provider module at a semantic version. `GOPRIVATE` applies if the module path is private.
- **Deploy**: `GOOS` and `GOARCH` cross-compile the `main` package. The result is a binary the host runs; the language does not embed a cloud runtime.

**Examples:**

```go
func get(ctx context.Context, url string) error {
	req, err := http.NewRequestWithContext(ctx, http.MethodGet, url, nil)
	if err != nil {
		return err
	}
	resp, err := http.DefaultClient.Do(req)
	if err != nil {
		return err
	}
	defer resp.Body.Close()
	return nil
}
```

**Sources:**

- https://pkg.go.dev/net/http
- https://go.dev/doc/modules/managing-dependencies
- https://pkg.go.dev/cmd/go
</details>

<details>
<summary>What libraries are available for building GUI applications in Go?</summary>

### What libraries are available for building GUI applications in Go?

**Definition:** The standard library has no GUI toolkit and no windowing package. Desktop GUI libraries are third-party modules. The Go project’s own UI-adjacent tree for mobile is `golang.org/x/mobile`, and that repository describes itself as experimental.

**How it works:**

#### What you can cite from Go itself

- **No stdlib widgets**: `net/http` can serve a browser UI. That is a server, not a native window.
- **x/mobile**: experimental support for mobile apps and for binding a Go library into an SDK app.
- **Other toolkits**: a desktop toolkit is a module you add and version. The go command does not bless one. Read that module’s own docs before depending on an API.

**Examples:**

```go
func uiNote() string {
	return "no GUI package in std; serve HTML with net/http or add a module"
}
```

**Sources:**

- https://pkg.go.dev/golang.org/x/mobile
- https://pkg.go.dev/net/http
</details>

<details>
<summary>How would you integrate a Go application with a message queue?</summary>

### How would you integrate a Go application with a message queue?

**Definition:** The standard library has no message-queue client. Integration is a third-party module, or a protocol you speak with `net` if the broker is plain TCP. What Go does provide is the concurrency around the client: a goroutine that receives, a `Context` that cancels, and acknowledgment logic you write in ordinary code.

**How it works:**

#### A consumer loop

- **Module**: `go get` the broker’s Go client and import it. Pin the version. The API is that module’s, not `net/http`’s.
- **Cancel**: the loop selects on `ctx.Done()` so shutdown stops receiving.
- **At-least-once**: if the client’s ack is a separate call, a crash between handling and ack redelivers. Make the handler safe to run twice, or use the client’s transaction API if it documents one.

**Examples:**

```go
func consume(ctx context.Context, next func(context.Context) error) error {
	for {
		if err := ctx.Err(); err != nil {
			return err
		}
		if err := next(ctx); err != nil {
			return err
		}
	}
}
```

**Sources:**

- https://pkg.go.dev/context
- https://go.dev/doc/modules/managing-dependencies
- https://pkg.go.dev/net
</details>

<details>
<summary>What options are there for ORM in Go?</summary>

### What options are there for ORM in Go?

**Definition:** `database/sql` is not an ORM. It is a generic SQL interface plus a driver: you write SQL, and `QueryContext` returns `*Rows` you scan into variables. ORMs and query generators are third-party modules on top of `database/sql` or their own drivers.

**How it works:**

#### SQL in the standard library

- **Scan**: `rows.Scan(&id, &name)` copies columns into Go values. You own the mapping.
- **A module**: an ORM is added with `go get` and imported like any dependency. Its behavior is that module’s documentation, not `database/sql`’s.
- **Drivers**: `sql.Open` needs a registered driver. A blank import of the driver package runs its `init`.

**When to use:**

#### SQL you can see, or a module

- Use `database/sql` when the queries are the API and you want the driver pool from the standard library.
- Add an ORM module when the team accepts its mapping rules and its version is pinned. Keep `sql` for the statements that need to stay visible.

**Examples:**

```go
func names(ctx context.Context, db *sql.DB) ([]string, error) {
	rows, err := db.QueryContext(ctx, "select name from users")
	if err != nil {
		return nil, err
	}
	defer rows.Close()
	var out []string
	for rows.Next() {
		var name string
		if err := rows.Scan(&name); err != nil {
			return nil, err
		}
		out = append(out, name)
	}
	return out, rows.Err()
}
```

**Sources:**

- https://pkg.go.dev/database/sql
- https://go.dev/doc/modules/managing-dependencies
</details>

<details>
<summary>Discuss how to use Docker with Go applications.</summary>

### Discuss how to use Docker with Go applications.

**Definition:** Docker is not a Go tool. What Go contributes is a binary from `go build`. A container image copies that binary and runs it. `CGO_ENABLED=0` builds without cgo so the binary does not depend on a C library from the build machine. Cross-compile with `GOOS` and `GOARCH` to match the image.

**How it works:**

#### A binary the image can exec

- **Build**: `CGO_ENABLED=0 GOOS=linux GOARCH=amd64 go build -o app ./cmd/app` produces a Linux amd64 binary without cgo.
- **Listen address**: bind `http.Server` to `0.0.0.0:port` or `:port` if the process must accept connections from outside the container’s loopback.
- **Modules in the build**: the image build needs `go.mod` and `go.sum` so `go build` resolves the same versions.

**Examples:**

```go
func listen() error {
	return http.ListenAndServe(":8080", nil)
}
```

**Sources:**

- https://pkg.go.dev/cmd/go
- https://go.dev/doc/code
- https://pkg.go.dev/net/http
</details>

<details>
<summary>Explain the role of WebAssembly with Go.</summary>

### Explain the role of WebAssembly with Go.

**Definition:** The FAQ says Go programs are compiled ahead of time to native code, or to JavaScript or WebAssembly for some implementations. `GOARCH=wasm` is that architecture. The go command’s `-buildmode=c-shared` notes say that on `wasip1` the build produces a WASI reactor/library whose callable symbols are functions exported with `//go:wasmexport`.

**How it works:**

#### wasm and wasip1

- **One CPU in the wasm port**: `runtime.GOMAXPROCS` forces `n` to 1 when `GOARCH` is `wasm`, because that port’s comment says WebAssembly has no threads yet.
- **js/wasm versus wasip1**: `GOOS=js GOARCH=wasm` targets a JavaScript host. `GOOS=wasip1 GOARCH=wasm` targets WASI. They are different operating-system values in the go command.
- **Not the default**: a normal `go build` targets the host `GOOS`/`GOARCH`. Wasm is selected by setting those variables.

**When to use:**

#### A Wasm host that must run this module

- Compile to `js/wasm` when the host is a browser or another JavaScript embedder that loads the Go wasm runtime.
- Compile to `wasip1` when the host speaks WASI. Do not expect `GOMAXPROCS` above 1 on the wasm architecture; the runtime clamps it.

**Examples:**

```go
// GOOS=wasip1 GOARCH=wasm go build -o app.wasm .
```

**Sources:**

- https://go.dev/doc/faq
- https://pkg.go.dev/cmd/go
- https://pkg.go.dev/runtime#GOMAXPROCS
</details>

