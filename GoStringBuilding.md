# String Building the Go way

[category Tech][tags programming,go]

To build strings gradually, CSharp has `StringBuilder`. What does Go have? Superficially, Go appears not having/providing such thing as StringBuilder, but actually, it has something much more powerful.

Let's see the Go's idiosyncrasy of string building.

<!--more-->

## My intuitive code

Here is my pretty intuitive code that clearly shows what I'm trying to do:

https://gist.github.com/8c69f994a404738204ff

## Problems

The code is so intuitive that no explanation is necessary -- I'm just building strings piece by piece. However, when talking about efficiency, this is not good. Why? Because,

- First thing obvious is that, in Go, string is a primitive type, it's readonly, every manipulation to it will create a new string.
- What's less obvious is that, except using the internal buffer (a simple []byte), `fmt.Sprintf` always uses an extra allocation, then convert []byte to string (https://golang.org/src/fmt/print.go?s=4905:4982#L176 vs https://golang.org/src/fmt/print.go?s=4905:4982#L201).

## The Go way

The [How to efficiently concatenate strings in Go](http://stackoverflow.com/questions/1760757/how-to-efficiently-concatenate-strings-in-go) suggests to use `var buffer bytes.Buffer` and `buffer.WriteString`, which is *"incredibly fast. Made some naive "+" string concat in my program go from 3 minutes to 1.3 seconds."* That's a huge improvement. However, I do need  to use some kind of `printf` to do the appending. Shall I go with `buf.WriteString(fmt.Sprintf(...)))` or `fmt.Fprint(buf, "%s more %s", buf, more)`...? 

Long story short, here it is, the Go way:

https://gist.github.com/e4cc51fb6e6c3eefe497

By Dan Kortschak @adelaide.edu.au posted to golang-nuts (http://play.golang.org/p/8Jq_dPuwNF).

On first look, I wouldn't believe that it is working, because it keeps writing to the same place `&buf`, i.e., the address of buf. I thought doing this way, the later output will overwrite previous ones. If it works, then each `fmt.Fprintf(&buf` will advance buf magically, while in the end,
`return buf.String()` will magically return buf to the top of buffer
of the original place.

I.e., there are to many magics happening here for me to understand, but it actually works, http://play.golang.org/p/aG925WNos7 is a quick proof. Here are the things helped me to understand, most of which are from Dan Kortschak:

- Looking from the outside, the `buf` is of type `bytes.Buffer`, which always points to the beginning of the  buffer; whereas `&buf` is treated as the type of `io.Writer`. I.e., "all the semantics of writing to a file-like object hold here".
- For inside behavior, `Buffer.Write` advances the *write offset* (just the length of the `[]byte` used internally). `Buffer.String` returns the string conversion of the bytes from the *read offset* of the buffer to the current write offset - in the case here, the read offset is zero since no read has been done. (The `String` method is not a read operation BTW)

So that is the perfect solution I was looking for, which I can't
possibly come up with myself today.

