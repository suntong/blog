# Accessing Go command line parameters

[category Tech][tags go,programming,CLI]

This is the first article of the [mini series](https://sfxpt.wordpress.com/2015/06/16/providing-options-for-go-applications/) on how to provide options for Go applications. Today's focus is on the default command line arguments/parameters/options, i.e., how to access Go command line parameters, and how to specify flags (command line parameters/options) to Go applications.

<!--more-->

Just like C/C++, Go can access command line parameters. And no surprisingly at all the syntax is almost exactly as C. Here is an simple example:

```
package main

import (
        //      "fmt"
        "os"
        "path/filepath"
)

func main() {
        println("I am ", os.Args[0])

        baseName := filepath.Base(os.Args[0])
        println("The base name is ", baseName)

        // The length of array a can be discovered using the built-in function len
        println("Argument # is ", len(os.Args))

        // the first command line arguments
        if len(os.Args) > 1 {
                println("The first command line argument: ", os.Args[1])
        }
}

```

We can see that it can get/access the program name, the number of command line parameters, and each individual as well. Here is a sample test result, from compile to run:

```
$ go build CommandLineArgs.go

$ CommandLineArgs test one 
I am  CommandLineArgs
The base name is  CommandLineArgs
Argument # is  3
The first command line argument:  test
```

Although go is a compiling language, it can almost use-as/seem-like an interpreting language as well. Here is what happens when we run it without compiling first:

```
$ go run CommandLineArgs.go test one 
I am  /tmp/go-build829589774/command-line-arguments/_obj/exe/CommandLineArgs
The base name is  CommandLineArgs
Argument # is  3
The first command line argument:  test

```

We can see that Go compile it on the fly and the compiled executable is put under `/tmp`. The executable file name without the path (i.e., base name) is obtained by the built-in `filepath.Base` function.

That's the end of today's topic, the full source code is available [here](https://github.com/suntong001/lang/blob/master/lang/Go/src/sys/CommandLineArgs.go).
