Learning Go
===

Follow up notes and code examples/exercises from
Book by Jon Bodner

Abstract
---

This is a collection of a follow up notes and code samples taken from the book 
["Learning Go"](https://www.goodreads.com/book/show/55841848-learning-go "on goodreads.com") 
by Jon Bonder.

Версию на русском можно найти на [livelib.ru](https://www.livelib.ru/book/1007447983-go-idiomy-i-patterny-proektirovaniya-dzhon-bodner "на livelib.ru").

All code collected in a tree-formed directory structure where `ch0N` is a short 
for the corresponding chapter.

Every book example code file will have preceding `exmpl_` prefix.

Chapters
---

### Chapter 1. Setting Up Your Go Environment

This chapter all about go toolset and how to setup it.

#### Install the Go Tools

Downloads page at [official Go site](https://go.dev/dl) provide you with 
installers for different OS and CPU architecture.

Author suggest to use [Homebrew](https://brew.sh/) for MacOS and [Chocolatey](https://chocolatey.org/) 
for Windows, as for the Linux most convenient way to use `tag.gz` archive and 
unpack it.

> For the Windows users I suggest to use another package manager, called [Scoop](https://scoop.sh/).

In every OS you need to add go path to user-level or system-level environment 
called PATH.

```
$ go version
```

Will tell the currently used version.

```
$ go env
```

Will list you with all Go related environments. The most crucial at this stage 
are `GOROOT`, place where go lives, and `GOPATH`, where go will store and look 
for dependencies and installed with `go install` binaries.

#### The Go Workspace

Use 
```
$ go install ...
```
to install additional or third-party tools.

By default `GOPATH` points to `$HOME/go` where all source codes lives in a `src` 
directory and all binary in a `bin` folder of it.

Explicitly defined `$GOPATH` is a good idea. It can be done via 
```
export GOPATH=$...
export PATH=$PATH:$GOPATH/bin
```
in Unix-like systems. And via
```
setx GOPATH %...
setx path "%path%;%GOPATH\bin"
```

#### The go Command

Go ships with compiler, formatter, linter, dependency manager, test runner, etc.

##### Run and Build

Code can be run or can be build. Run will compile, run, and wipe build 
artefact on termination.

Author suggest to write "hello world" program and run it.
```
$ go run ./hello.go
```

> Appropriate code sample can be found in `./ch01/hello.go` file.

Build will compile code into binary, by default it will be named same as entry 
point code file or declared package, and can me modified via `-o` flag with 
custom name given.
```
$ go build ./exmpl_hello.go
$ go build -o hello_world ./exmpl_hello.go
```

##### Getting 3rd-Party Go Tools

In Go world there are no centrally hosted package registry, but rather it uses 
source code repository as a reference point for the package.

Any package or go written tool can be installed via
```
$ go install somevcs.registry/author/package@version
```

By setting version to `latest` last available version in this repository will 
be installed.

Dependency manager also will download all package dependency and build project, 
placing binaries to `$GOPATH/bin` directory.

> If path to the package differs from one above, `$GOPROXY` environment 
variable might be used to account those changes.

Author suggest to install `hey` utility to try `go install`.

##### Formatting Your Code

In go world there is no formatting variation, and language authors doesn't left 
any space for dispute about it.

```
$ go fmt ./...
$ gofmt -l -w ./...
```
Will check format of given code, fix what it can, and warn about lines that off.

Author suggest to try `goimports` that extends `go fmt` and sort imports in 
alphabetical order.

```
$ goimports -l -w .
```

> Go fmt won't fix misplaced braces. And that's why: despite that lack of 
semicolon at the end of each command, go lexer will virtually inserts them 
after certain tokens.

##### Linting and Vetting

Author suggest to read [Effective Go](https://go.dev/doc/effective_go) article 
as well as [Code Review Comments](https://github.com/golang/go/wiki/CodeReviewComments) 
page.

Tools like
```
$ golint ./...
```
and
```
$ go vet ./...
```
is used to enforce code style and mitigate inefficiency.

Though, `golangci-lint` suggested to be used as a right tool of collaborative 
work, that insure many aspects of a code regarding code-style and code-quality.

```
$ golangci-lint run ./...
```

Additionally this tool can be tuned via `.golangci.yml` config file placed on 
project level.

#### Choose Your Tools

Any text editor basically, and [Go Playground](https://go.dev/play/).

Go Playground limited, rather then original runtime, but yet useful, to sketch 
something.

In Go Playground you can imitate multi-file structure, by adding 
```
-- filename.go --
```
prior file contents.

#### Makefiles

Author suggest to look at make as an automation tool. But it required to be 
installed upfront.

Example of Makefile can be found in `./ch01/Makefile`.

#### Staying Up to Date

Be cautious while switching between different version of go tools.

### Chapter 2. Primitive Types and Declarations

#### Build-in Types

> `./ch02/exmpl_buildin.go` contains build in types. And by running this 
program, variables will be declared, initialized with type-related zero values 
and are printed.

A [list of go types](https://pkg.go.dev/builtin#pkg-types) could be found in 
original documentation.

#### The Zero Value

Go compiler will initialize uninitialized variables with zero values regarding 
declared types. 

So that variable of `string` type will be empty string, or `int` variable will 
be set to 0.

#### Literals

Literals ≠ variables.

> `./ch02/exmpl_literals.go` contains corresponding codes examples of a 
literals.

##### Integer number

Integers can be set as a base ten, or other via prefixes: 
* binary `0b`, 
* octal `0o` (`0`),
* hexadecimal `0x`.

Author do not recommend to use `0` prefix, as it might confused other developers.

Underscore symbol can be used for better representation.

##### Floating point

Literals with floating point can be defined as a set on numbers with stop 
delimiter, in exponential form or even in hexadecimal.

Underscore symbol can be used for better representation.

##### Rune

This type of literals represent character and surrounded within single quotes 
`'`. Like so: `'a'`.

Run can be written as an unicode character, 8-bit octal, 8-bit hexadecimal, 
16-bit hexadecimal, or 32-bit unicode number. Also with rune can be defined one 
of a special character.

##### String

String literal can be given withing double quotes `"` around it, or within 
backtick, as a so called raw string. The difference is that string in double 
quotes some of a symbol sequence will be interpreted as non-printable 
characters. 

#### Booleans

true / false, nothing fancy. Uninitialized boolean variable gets `false` on 
compile time.

#### Numeric Types

Go have 12 different types of numeric grouped into 3 category. 

Author suggest to use `int`, `uint`, and `float64` unless algorithm dictates 
other.

Math operations can be done with integers, unless type boundaries wouldn't be 
exceeded, or it wouldn't be division by zero.

Arithmetic operations can be combined with assignment.

Integers can be compared, and bit-manipulated.

Go have 2 types to represent a number with floating point: `float32` and 
`float64` by [IEEE 754](https://en.wikipedia.org/wiki/IEEE_754) specification.

Author suggest to use number with float points less, or at least use 64 bit 
version of it due to higher precision. And do not compare directly, but use 
comparison with epsilon.

Almost every arithmetic operations can be used with float except modulo 
division.

Division by zero in float world have caveats, it would not raise panic, as 
integer would, but will return `±Inf` if dividend was nonzero, and `None` if 
it was zero.

There is another subcategory of numbers called complex.

#### Runes and Strings

Strings consists of runes and are comparable but immutable. Despite that `rune` 
is an alias on `int32` and `byte` is an alias to `uint8` author suggest to 
preserve clarity in the code and use appropriate type when it needs to be used.

#### Explicit Type Conversion

Unlike literals, that can be promoted to a certain type, values of a variable 
needs to be converted manually.

This means that, 0 is zero and nothing more, not `false` by any means.

#### var / :=

Variable in Go can be declared via `var` keyword or `:=` operator. There's a 
reason for that diversity, -- the context.

`var` is an only way to declare variable outside function scope, and `:=` only 
works only within function scope.

With `var` keyword you can declare variable and define type for it, but omit 
the value of initialization, compiler will set type-regarding zero value.

`:=` force you to specify variable with value.

In both cases, type definition might be omitted while value is present, and 
type for this variables will be defined by value.

In both cases, several variables can be initialized within a series of values, 
given on the right side of operation, both series should be presented as 
coma-separated lists.

`var` allowed to define a scope of variables, enumerated in a parentheses.

> `./ch02/exmpl_varassignment.go` contains those variations.

Variables declared outside function scope is not a good idea! Use `cont` 
instead.

Go heavily relies on naming convention, where variables, constants, types, and 
other package level entities expose to outer wold if they starts with Upper 
case letter.

Defined variable cant be reassigned with value of different type.

#### const

Pretty much the same as `var` declaration rules applied, but constant **must**
be initialized right away. Needles to say, that defined `constants` are 
immutable.

Acceptable by constant types are limited to numeric literals, boolean, runes, 
strings and some built-in functions and expressions.

Basically with `const` keyword program will label literal with given name.

In addition to that, if type directed in `const` declaration, then it cannot be 
use as a value to the variable of other type.

> Go force you to use declared variable, and count references on them in 
compile type. It would not compile otherwise.

#### Naming Variables and Constants

Name can contains any unicode compatible symbols. But be aware of using rare 
symbols or once that look alike others, it will misguide others.

CamelCase notation is suggested to be used, and as it was mentioned 1st letter 
defines visibility.

Go authors do not suggest to use type prefixes of any kind.

And more exposed names should be more meaningful.

### Chapter 3. Composite Types

`Array`, `slice`, `map`, `struct` and other are complex types.

#### Arrays -- Too Rigid to Use Directly 

Author said, that arrays are rarely used directly, because of their nature. 
They declared with predefined size `[3]int`, and size is a part of type.

Type of array's element must be a part of declaration.

Array variable can be left uninitialized, and compiler will set type defined 
zero values to all it's elements.

Array can be declared via given initialization value `[3]int{1,2,3}`.

Initialization values set can have indexes `[10]int{9:10}`, so that value will 
take certain place in upcoming array.

Size of array can be calculated from within initialization values set, for that 
use `[...]int{1,2,3}` instead of certain number of an elements.

Arrays are comparable via `==` and `!=`.

Arrays in Go are one-dimensional, but can be nested `[5][5]int`, and might be 
initialized like so: `[3][2]int{[2]int{1, 2}, [2]int{3, 4}, [2]int{5, 6}}`.

Array elements can be referenced via index, that starts with 0, and they can be 
re-assigned.

Arrays are not extendible, so it can't be appended, and it's elements can't be 
referenced out of setted boundaries, it will raise _panic_.

`len` is a builtin function, that can be applied to return length of specified 
array.

Array is a backing store for _slices_.

#### Slices

Author suggest to use slices rather than arrays.

Slices can grow, and they grow twice each time, when reach their capacity, 
until reaches 1024 elements, then grow rate decrease to quarter.

Unlike arrays, slice are better as an function arguments, because of the lack 
of size limitation.

To declare slices use empty square braces `[]` unlike array, where number of 
elements `[5]` or variadic operator `[...]` required to put into.

Multidimensional slices can be declared same way as arrays.

Unlike array, slices can't be comparable to each other, but they can be compared 
with `nil`, and if array equal `nil`, then array was uninitialized.
And the length of such array will be 0.

`len` function can be used.

##### len

`len` one of builtin function that overloaded and can be used within several 
types.

##### append

`append` function another builtin function that allows slice to grow in size. 
It's a pure function, that accepts 2 or more arguments, where the first one is 
a slice and the rest are a new appendable elements, and it returns a result of 
the operation.

```
x = append(x, 1, 2, 3, 4)
```

Two or more slices can be merged by applying `append` with variadic operation:
```
x = append(x, y...)
```

We certainly can do prepend, just by reverse which to append.

##### cap

Capacity is another measurement of a slice. It's number of a preallocated 
elements, or consecutive locations in memory.

Each time new element(-s) added, it might exhaust allocated capacity 
`(len == cap)`, and if no space left, Go runtime will allocate new consecutive 
memory portion, twice a size (unless it reaches 1024, - then it will grow only 
a quoter), and will copy all data from previous slice memory to newly created.

> Go runtime isn't separate VM that needs to be installed, but it's a part of 
future executable that included in compile time, regarding application 
requirements.

> Any new slice allocation and copying - it's a time consuming operation.

> `cap` can be applied on `array` but it will present it's length.

> Check how slice grows over append iteration in `./ch03/exmpl_capacity.go`

##### make

`make` is another builtin function, that helps to declare and initialize slices 
when a length or a capacity can be specified.

> Function descriptor `make(type, length, capacity)`

> In case length presented slice will be initialized with zero values of this 
length, and `append` will _add_ new elements after them.

> Check how make can be used on slices in `./ch03/exmpl_make.go`

##### Slicing Slices

Basically it's offsetting via numbers in square braces `[start:end:limit]`. 
Unlike **python**, negative or overflown indexes can't be used, and in Go 
there is a third slicing value, but it's not a step, but a limit of capacity.

Slicing wouldn't create a copy of data, but it will reference original one. 
So by changing value by index in one one slice, all it's related slices will be 
changed as well.

> Be cautious mixing slicing and appending new elements, it might end up with 
chaotic result.

Array can be sliced as well, and this way converted to a slice.

##### copy

