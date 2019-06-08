---
title: Simple web server in Perl and Go, Revisit
published: true
description: website performance based on implementation of Perl or Go
cover_image: https://i.stack.imgur.com/zBz23.png
tags: webdev,go,html,performance
---

Following on my previous [Simple web server implemented in Perl and Go](https://dev.to/suntong/simple-web-server-implemented-in-perl-and-go-b43/) 
 blog, which has been pointed out as unfair comparison. This time, thanks to [Axel Wagner](https://dev.to/themerovius), who replaced the net/http.Server layer with direct translation of Perl code, the code is now reading and writing directly to a socket, just as what the Perl code is doing. 

And here are the test results from my machine again (which I [published here](https://gist.github.com/suntong/757d287f4fe44a543b15fa73d6f30abb)):

100 concurrency requests (the number of multiple requests to perform at a time):

![Ab100](https://img.vim-cn.com/93/4bdabb7ad9b7e8bb68176c759d640c1e747b6f.svg "100 concurrency requests")

500 concurrency requests:

![Ab500](https://img.vim-cn.com/c6/635682fa3a90f909fa2dd4c8ec6cfb57e13388.svg "500 concurrency requests")


Once again,

- This is [the implementation in Perl, with Cache-Control Header added](https://github.com/suntong/dbab/blob/master/src/bin/dbab-svr)
- And this is [the implementation in Go](https://github.com/suntong/dbab-go/blob/master/dbab-svr/main.go)
- As discussed previously, statistic summaries of Apache Bench are a bit confusing so I'm leaving them out this time. Also concluded was that the measured & reported response time are truly trustworthy. 

From the charts, we can see that the response time for Go 500 concurrency requests is almost identical to [previous result](https://dev.to/suntong/simple-web-server-implemented-in-perl-and-go-b43/), while the 100 concurrency is actually worth than before.

In summary, I'm still seeing Perl performs much better than Go, with this direct translation. Why? 
