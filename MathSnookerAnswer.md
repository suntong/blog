# Answer to the math programming question

[category Tech][tags programming,go]

This is the answer to the [previous math programming question](https://sfxpt.wordpress.com/2015/02/22/testing-your-math-and-programming-skills/), a problem *difficult enough* to be interesting, yet *simple enough* to be finished in a short time. Here is the answer:

<!--more-->

http://pastie.org/9973586

https://github.com/suntong001/lang/blob/master/lang/Go/src/text/Snooker.go

*NB, I also tried to post the code at http://play.golang.org/, but the random seeding is not working there, so the result is really bad there*. 

This is actually a tuned down version of what I did when I was in the computer programming competition in the first year of my high school. I was doing four-side bouncing at that time, and I didn't have the math skill required by then. Luckily the competition is not about who solve the problem the fastest, but who could come up with the most interesting problem and could also solve it as well. Took me more than a week working on the computer to solve it (using `line` to draw on graphic canvas), using the limited knowledge I had by then. 

As you can see, with the proper math skills, this two-side bouncing can be very simple. Because I still haven't been writing `go` code frequently enough for me to remember everything, I have to consult almost everything to books or Google, including how to define constants or two dimensional array; how to make array of *characters*, use `rand` in go, and `Abs` for `int` as well, or even how to write a for loop in `go`. Even so, the core code was finished in about half an hour. 

OK, do you know how it works? Can you tweak it to make it a three-side, or four-side bouncing game?

