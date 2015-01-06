# Chrome massive memory leak and solution

[category Tech][tags Debian,Ubuntu,Linux,web,browser]

Right after I upgraded to Ubuntu 14.10 Utopic, I noticed that the `chromium-browser` is freezing up my system. Without a doubt, I know it is leaking memory, and confirmed it right away with a quick search -- [Chrome massive memory leak](https://productforums.google.com/forum/#!topic/chrome/86yzpxX7aws%5B1-25-false%5D) -- *"Surely enough the leak is still there it seems... The leak only seems to grow when ... is the current active tab/window...(if I move away) the leak stops growing, until i switch back. Which then the leak instantly jumps in size (10 MB, 100MB, 1GB?) for however long I was away from that tab"*. That explains quite nicely on how my system is freezing. It always happens when I come back to my never-closed `chromium` browser to look into my saved tabs for things. Then the hard disk light starts flashing forever, and gradually & eventually the system cannot respond to any further events.

The problem is that the issue has been reported and confirmed for over 3 months now, but there is still no fixes available. How to survive such periodic system freeze?

<!--more-->

## The Great Suspender

As a workaround, I found ["The Great Suspender"](https://chrome.google.com/webstore/detail/the-great-suspender/klbibkeccnjlkjkiokjodocebajanakg):

> Unload, park, suspend tabs to reduce memory footprint of chrome. 
> Tabs can auto-suspend after a configurable period of time or be suspended manually. Tabs can be whitelisted to avoid automatic suspension. Suspended tabs are retained after closing and reopening browser, preventing many tabs from all reloading after a restart...

That has been working very nicely. My used memory will always jump to over 95% just after a few minutes I star my chromium browser, but having use the Great Suspender, the used memory will stay around below 30% for quite a while now. 

Now I only need to close my chromium browser and reopen it every 4 to 5 days, instead of every 2 or 3 hours previously. I'm really happy about it.

## Use the cache!

There will be one problem though, for those people who are not using a local cache server -- the Great Suspender will suspend the idle tab, and completely free up its allocated memory, which means, when you revisit the tab again, every single bite of the page will have to be re-downloaded. If you are not using a local cache server, then that delay, even though might be just in a few seconds ranges, will become more and more intolerable, because our mindset is that once I have it, it should be there when I want it. If I have to re-download every single bite of the page, I might just as well close the tab entirely. This will defeat the propose of having the Great Suspender extension at the first place after all.

Having a local caching server will be an entire different story. The page might still need a second or so to render, but that's only the page-rendering time, the network delay will be gone! Cool. Well, if you have a local caching server, why not speed things further up by using the [dbab server](http://sfxpt.wordpress.com/2014/11/30/use-new-dbab-to-set-proxy-automatically/) to block ads from being downloaded and cached? [Check it out](http://sfxpt.wordpress.com/2014/11/30/use-new-dbab-to-set-proxy-automatically/)!
