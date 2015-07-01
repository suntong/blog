# Passing options to Go from command line 

[category Tech][tags go,programming,CLI]

This is the second article of the [mini series](https://sfxpt.wordpress.com/2015/06/16/providing-options-for-go-applications/) on how to provide options for Go (golang) applications. Today's focus is on the built-in command line parameters/options, i.e., how to pass options to Go applications. 

<!--more-->

Besides [accessing command line parameters directly like C](https://sfxpt.wordpress.com/2015/06/17/accessing-go-command-line-parameters/), Go has built-in functionality to handle command line parameters, so that it is easier to pass options to Go applications -- it is the built-in `flag` package.

Here is an simple example showing how to use it:


```
package main

import (
	"flag"
	"fmt"
	"os"
	"strconv"
)

var (
	// main operation modes
	write = flag.Bool("w", false, "write result back instead of stdout\n\t\tDefault: No write back")

	// layout control
	tabWidth = flag.Int("tabwidth", 8, "tab width\n\t\tDefault: Standard")

	// debugging
	cpuprofile = flag.String("cpuprofile", "", "write cpu profile to this file\n\t\tDefault: no default")
)

func usage() {
	// Fprintf allows us to print to a specifed file handle or stream
	fmt.Fprintf(os.Stderr, "\nUsage: %s [flags] file [path ...]\n\n",
		"CommandLineFlag") // os.Args[0]
	flag.PrintDefaults()
	os.Exit(0)
}

func main() {
	fmt.Printf("Before parsing the flags\n")
	fmt.Printf("T: %d\nW: %s\nC: '%s'\n",
		*tabWidth, strconv.FormatBool(*write), *cpuprofile)

	flag.Usage = usage
	flag.Parse()

	// There is also a mandatory non-flag arguments
	if len(flag.Args()) < 1 {
		usage()
	}

	fmt.Printf("Testing the flag package\n")
	fmt.Printf("T: %d\nW: %s\nC: '%s'\n",
		*tabWidth, strconv.FormatBool(*write), *cpuprofile)

	for index, element := range flag.Args() {
		fmt.Printf("I: %d C: '%s'\n", index, element)
	}
}
```

In this simple example,

- Three kind of flags are illustrated, `Int`, `String`, and Boolean type `Bool`.
- Each flag is defined with its type, command line options string, the default value it takes, and an explain for the help.
- Also shown is how to handle the case that there are some mandatory non-flag arguments, just like the Unix command `cp`, it need to take two mandatory non-flag arguments.
- Finally, it showcases how to deal with flag options, and non-flag arguments.

Let's see how it runs, first the usage prompt:

```
$ go run CommandLineFlag.go
Before parsing the flags
T: 8
W: false
C: ''

Usage: CommandLineFlag [flags] file [path ...]

  -cpuprofile="": write cpu profile to this file
                Default: no default
  -tabwidth=8: tab width
                Default: Standard
  -w=false: write result back instead of stdout
                Default: No write back

```

I've settled down with this short + long description layout/arrangement. The short description comes on the same line as the option, whereas the long description comes on the second line and on. If it is too long, you need to handle properly line wrapping them yourself. 

Here is how it runs:

```
$ go run CommandLineFlag.go aa bb
Before parsing the flags
T: 8
W: false
C: ''
Testing the flag package
T: 8
W: false
C: ''
I: 0 C: 'aa'
I: 1 C: 'bb'

$ go run CommandLineFlag.go -tabwidth=2 -w aa
Before parsing the flags
T: 8
W: false
C: ''
Testing the flag package
T: 2
W: true
C: ''
I: 0 C: 'aa'

```

Two cases are illustrated, one without the any flags, the other with two.

For simple cases when you just need to pass something to Go application, and you don't want to write your own command line parameters/options parser, or use any third party ones, this simple Go `flag` package can foot the bill nicely and will do a decent job. However, it is not as powerful as the de-facto POSIX `getopt` command line parameters parser standard, which is available universally anywhere in C/C++, Perl or Shell script, because,

- there is no short/long option distinction.
- you can't combine short options together.
- you can't abbreviation long option so that as long as it is not ambiguous.
- and you can't place options anywhere on the command line, e.g., at the very end, after the non-flag arguments.

But still, it is a built-in package that comes with no extra cost, so you can just use it without any hassles. Moreover, the types it provide, e.g., Boolean type `Bool` type, are very convenient. It even provides an interval type that will comes very handy if you need one. I found it helps me a lot. 

That's the end of today's topic, the full source code is available [here](https://github.com/suntong001/lang/blob/master/lang/Go/src/sys/CommandLineFlag.go).
