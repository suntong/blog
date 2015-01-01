# Using aria2

[category Tech][tags Debian,Ubuntu,Linux,web,downloader,CLI]

What is the best CLI downloader under Linux? Of course it is `wget` you may say, well wait until you see `aria2`.

[more]

## The Joy of Using aria2

The `wget` is actually pretty good, I've been using it since day one when I am in Linux. However, if you are into heavy downloading (well, does multi-thread and rapidshare rings any bell?), then you *must* have `aria2`.

The following is just a re-post of a message from `aria2`'s home page, http://sourceforge.net/projects/aria2/ by `aria2` user *fooby*, with some minimum editing:

> Wow, I have finally found the ultimate CLI downloader. I have spent the last day searching for an OS X alternative to the single-threaded wget and curl, that was multi-threaded and would handle HTTPS traffic, and I tried 'em all. I tried `puf`, which doesn't handle HTTPS at all, `axel`, which pretends to handle HTTPS but I caught it actually using HTTP instead (`tcpdump` is your little snitch friend). Naughty, naughty, `axel`, for giving us a false sense of security... That's worse than no security at all.
> 
> Then I found the truly amazing `aria2`. It not only worked first time, right out of the box, handling HTTPS traffic flawlessly (believe me, I checked), but it downloaded files at record speeds -- my full allotted bandwidth. I have been looking for a CLI alternative to `DownThemAll` for Firefox because DTA cripples Firefox and slows the entire system on my pretty fast 8-core Mac Pro. It just struggles to maintain a difficult connection, and I have plenty of those. `aria2` handles these with ease and does not in any way slow my system down. The download speed is much more consistent than DTA too -- no choppiness like in DTA when it loses the connection for many seconds. It just sits there in the background downloading away at max speed. Wow. Goodbye DTA...
> 
> I used the following syntax to get these speeds:

>    aria2c --file-allocation=none -c -x 10 -s 10 -d "mydir" URL

> Note that `aria2c` is what HomeBrew has named `aria2` -- don't ask why, I don't know... `--file-allocation=none` speeds up the initialization of the download which can take quite a long time for a multi-GB file otherwise. `-c` allows continuation of a download if it was incomplete the first time. This came in really handy when, for some reason, the speed started flagging and I `ctrl-c`ed out of the download and restarted it. It resumed right where it left off at max speed. Nice. `-x 10` give 10 connections per server to speed things along. I suspect `-s 10` is unnecessary but I prefer to err on the side of overkill. `- d` downloads files to a directory. To install aria2 on OS X, install HomeBrew if you don't have it and then type: `brew install aria2` To view the huge man page: `man aria2c`.
> 
> The above usage is only the tip of the iceberg for this incredible app. I only download one file at a time from normal HTTP(S) URLs but it is capable of downloading multiple files simultaneously from mixed HTTP(S), FTP, BitTorrent, and Metalink sources. And the options are more than anyone can handle in a lifetime. As I said, the man page is huge. No other download client even comes close to the many uses and capabilities of this app. It is truly *The next generation download utility* (as stated on the home page of `aria2` on sourceforge.net). I have only scratched the surface.
> 
> Kudos to all the devs who labored hard and long to bring this app to fruition and make it available for free to all of us. Great job!


## More Comments

- Yes, `-x 10` defines 10 connections per server. However,
- What `-s 10` limits is how many concurrent download per file. Default is 5.
- There is actually another one, `-j`, which specifies the maximum number of
  parallel downloads for every queue item. Default is 5.

## Rapidshare Example

Taken from https://wiki.archlinux.org/index.php/aria2#Example_aria2.rapidshare:

~~~
http-user=USER_NAME
http-passwd=PASSWORD
allow-overwrite=true
dir=/file/Downloads
file-allocation=falloc
enable-http-pipelining=true
input-file=/file/input.rapidshare
log-level=error
max-connection-per-server=2
summary-interval=120
~~~

## Download Managers

#### The list
<br/>

If you prefer download managers, there are actually a lot of frontends that can make use of `aria2`. Check https://wiki.archlinux.org/index.php/aria2#Frontends for details.

#### uget
<br/>

What readily available in Debian/Ubuntu is `uget`:

~~~
Package: uget
Priority: optional
Section: universe/net
Installed-Size: 1013
Maintainer: Ubuntu Developers <ubuntu-devel-discuss@lists.ubuntu.com>
Version: 1.10.4-1ubuntu1
Depends: libappindicator3-1 (>= 0.2.92), libc6 (>= 2.4), libcairo2 (>= 1.2.4), libcurl3 (>= 7.16.2), libgdk-pixbuf2.0-0 (>= 2.22.0), libglib2.0-0 (>= 2.35.9), libgstreamer0.10-0 (>= 0.10.0), libgtk-3-0 (>= 3.0.0), libnotify4 (>= 0.7.0), libpango-1.0-0 (>= 1.14.0), libpangocairo-1.0-0 (>= 1.14.0)
Filename: pool/universe/u/uget/uget_1.10.4-1ubuntu1_amd64.deb
Size: 225130
MD5sum: 7dfc084bce5ede217327af851337e905
SHA1: 0cd54de9ea297d6dc72701f2147869b5a7a2bede
SHA256: 6d3727b0871636dbfdb721726fd1bbb4b843d799f72cc67fa888d26b5929f5ce
Description-en: easy-to-use download manager written in GTK+
 Uget (formerly urlgfe) is a simple, lightweight and easy-to-use
 download manager.
 It provides the following features:
  * Resume downloads.
  * Queue downloads.
  * Classify downloads in categories.
  * Mozilla Firefox integration (through Flashgot plugin).
  * Clipboard monitoring.
  * Import downloads import from HTML files.
  * Batch download.
 .
 It also can be launched from the command line.
Description-md5: 06b0431c1271b5ee4b240555ec0b8988
Homepage: http://urlget.sourceforge.net/
~~~

<br />
#### Webui
<br/>

I found the most intuitive front end is [`Webui`](https://github.com/ziahamza/webui-aria2) -- the Html frontend for `aria2`. Using it cannot be any more simpler -- *"just download and open index.html in any web browser"*.

![webui-aria2](https://github.com/ziahamza/webui-aria2/blob/master/screenshots/overview.png?raw=true "webui-aria2")

Or you could just head on to http://ziahamza.github.io/webui-aria2/ and just start downloading files! Even no installation necessary! Of course, if you properly install it on your web server, you can do your downloading even if you are away from home on holidays.

