# Bring back the Traditional Emacs

[category Tech][tags Debian,Ubuntu,linux,emacs,editor]

Want to have the Emacs that you used to love? Read on, or jump to [my conclusion](#emacs-traditional). 

[more]

# Emacs developing is heading to the wrong way

It has been [joked](https://en.wikipedia.org/wiki/Editor_war#Humor) that GNU EMACS stands for "Generally Not Used, Except by Middle-Aged Computer Scientists". While trying to transform it into modern world, I think Emacs has lost it core value and its sight. The modernization is leading it to the wrong direction. 

I use Emacs, but I guess fewer and fewer are using Emacs nowadays. I used Gnus as well, but I guess very very few of those who use Emacs know what Gnus is. The Emacs modernization is not attracting new engaged users, but only "mouse-and-menu" users, yet it is making its old die-hard users pissed off. 

Of all these year, the Emacs devs threw in fancy bell and whistles like scroll-bars, tool-bar, menu-bars, and so on. However, if you look at those die-hard Emacs user's init script, the first thing they all do is to turn them off. What a waste! There is a huge disconnect between Emacs devs and its users, so that what the devs are working on are not welcome by the users at all. Take a look an ordinary Emacs user Bob Proulx's journey fighting those fancy bell and whistles, taken from

https://lists.debian.org/debian-user/2012/09/msg01049.html


>  > Since you've been using Emacs for the past 15 years, I hope you do
>  > realize you can disable all the GUI clutter by setting:
>  > 
>  > (menu-bar-mode -1)
>  > (tool-bar-mode -1)
>  > (scroll-bar-mode -1)
>  > 
>  > in your ~/.emacs ...
> 
> _I have a lot of that in my .emacs file.  Here is a small listing of
> these types of fixes that I use_.  
> 
> ~~~
> (if (>= emacs-major-version 20)
>     (menu-bar-mode -1))
> 
> (if (>= emacs-major-version 21)
>     (if window-system
>         (tool-bar-mode -1)))
> 
> (if (>= emacs-major-version 22)
>     (progn
>       ;; Have *Buffer List* use old-style header without white on green highlight.
>       (setq Buffer-menu-use-header-line nil)
>       ;; Disable dark blue on dark background in minibuffer.
>       (set-face-foreground 'minibuffer-prompt nil)))
> 
> (if (>= emacs-major-version 23)
>     (progn
>       (setq transient-mark-mode nil)
>       (setq line-move-visual nil)
>       (setq search-whitespace-regexp nil)
>       (setq split-width-threshold nil)))
> 
> ;; Disable nasty white on green highlighting in electric-buffer-mode.
> ;; We use eval-after-load to monkey patch after ebuf-menu is loaded
> ;; as that's where the "bad" definition of electric-buffer-mode is located.
> (eval-after-load "ebuff-menu" '(defun electric-buffer-update-highlight ()))
> 
> ;; Stop the annoying question about exiting with shell processes still running.
> (eval-after-load 'shell
>   '(add-hook 'comint-exec-hook
> 	     '(lambda ()
> 		(set-process-query-on-exit-flag (get-process "shell") nil))))
> ~~~


Note the keyword is *a small listing*, as for any ordinary Emacs user, I'm sure most of you will understand what it means. My .emacs file is also full of such fixes, that it is beyond maintainable. The Emacs guys themselves coded a word for it -- [Dot Emacs Bankruptcy](http://www.emacswiki.org/emacs/DotEmacsBankruptcy).

You don't need to understand elispt to tell Bob's efforts after efforts trying to fix those nuisance. What's more ironic is that, and I highly concur, as put by Bob:

> _And yes I know that every one of these features that I am turning off were highly desired by someone else who wanted them enough to code them in as the new default_.

Again, WHAT A WASTE!

# The last straw that breaks the horse

More than two years ago, ever since Ubuntu 12.04, I started my journey to look for an emacs replacement, because my app-defaults settings that has been working for more than 10 years no longer worked any more. Emacs went to GTK+ version 3, and [there is no easy way to configure GTK3 Emacs](http://superuser.com/questions/597076/gtk-3-menu-configuration-for-emacs).

I was so frustrated with this [stupid GTK+3 based Emacs](#emacs-build) that I started to learn building Emacs and packaging Debian packages myself. That's a huge undertaking if you know what are involved in building Emacs and packaging Debian. Learning to package Debian packages is daunting tasks alone, building Emacs is even more challenging because nowhere you can find on the Internet people talking about it -- you are totally on your own. This clearly shows how frustrated I really was with GTK+ 3 based Emacs.

<a name="emacs-traditional"/>
&nbsp;

# The Traditional Emacs

Two years later, I'm now proudly present you, the [Traditional Emacs](https://launchpad.net/~suntong001/+archive/ubuntu/ppa/+packages)!

For all those people who use Emacs,  

- do you know that just to get Emacs installed onto your system, it'd take more than 800 Megs of disk space? This hasn't includes those Emacs modes, tweaks that you need to install on top of them.
- do you know that with [the gigantic list of libraries](#emacs-lib) that Emacs depends on, your Emacs is capable to be used as a picture-viewer, and music-player? 

Emacs is already the slowest text editor to get started, and yet Emacs developers are still piling up loads of things for it to do, like picture-viewing, and music-playing. However, if none of us is using Emacs as a picture-viewer, or music-player, then what the hell does it need such a [huge list of libraries](#emacs-lib) that are useless for text editing? Look at where the Emacs developers have put their [efforts](#emacs-build) on!? No wonder the vi advocates have been [known](https://en.wikipedia.org/wiki/Editor_war) to describe Emacs as "a great operating system, lacking only a decent editor". 

The [Traditional Emacs](https://launchpad.net/~suntong001/+archive/ubuntu/ppa/+packages) is the answer! No fancy bell and whistles, no "modernized"  GTK+3 look, just the plain old and clean text editor.

[![Old dark background GTK theme](http://xpt.sourceforge.net/techdocs/misc/ce01-DarkBackgroundIsGoodForYou/screenshot.png)](http://xpt.sourceforge.net/techdocs/misc/ce01-DarkBackgroundIsGoodForYou/screenshot.png)

Moreover, I got the beloved traditional scroll-bars back, i.e., the one that xterm uses. The above is still an old screen-shot that give you the look and feel. The scroll-bars now looks and behaves exactly like the xterm's. I call it precise-scrolling -- if you know how to use it, you cannot live without it. 

# Appendix

<a name="emacs-build"/>
&nbsp;

## **How Emacs is built by default**


Se how many "useless" things it depends just to be a text editor!:

    What window system should Emacs use?                    x11
    What toolkit should Emacs use?                          GTK3
    Where do we find X Windows header files?                Standard dirs
    Where do we find X Windows libraries?                   Standard dirs
    Does Emacs use -lXaw3d?                                 no
    Does Emacs use -lXpm?                                   yes
    Does Emacs use -ljpeg?                                  yes
    Does Emacs use -ltiff?                                  yes
    Does Emacs use a gif library?                           yes -lgif
    Does Emacs use -lpng?                                   yes
    Does Emacs use -lrsvg-2?                                yes
    Does Emacs use imagemagick?                             yes
    Does Emacs use -lgpm?                                   yes
    Does Emacs use -ldbus?                                  yes
    Does Emacs use -lgconf?                                 yes
    Does Emacs use GSettings?                               yes
    Does Emacs use -lselinux?                               yes
    Does Emacs use -lgnutls?                                yes
    Does Emacs use -lxml2?                                  yes
    Does Emacs use -lfreetype?                              yes
    Does Emacs use -lm17n-flt?                              yes
    Does Emacs use -lotf?                                   yes
    Does Emacs use -lxft?                                   yes
    Does Emacs use toolkit scroll bars?                     yes

<a name="emacs-lib"/>
[&nbsp;](http://sfxpt.wordpress.com/)

## **How many libs Emacs actually depends on** 


In oder to be able to build Emacs,

"The following NEW packages will be installed:"

    at-spi2-core{a} autoconf{a} automake{a} autotools-dev{a} bsd-mailx{a} 
    dbus-x11{a} dconf-gsettings-backend{a} dconf-service{a} diffstat{a} 
    gconf-service{a} gconf-service-backend{a} gconf2{a} gconf2-common{a} 
    gir1.2-atk-1.0{a} gir1.2-freedesktop{a} gir1.2-gconf-2.0{a} 
    gir1.2-gdkpixbuf-2.0{a} gir1.2-gtk-3.0{a} gir1.2-pango-1.0{a} 
    gir1.2-rsvg-2.0{a} hicolor-icon-theme{a} imagemagick{a} 
    imagemagick-common{a} libasound2{a} libasound2-dev{a} 
    libatk-bridge2.0-0{a} libatk-bridge2.0-dev{a} libatk1.0-dev{a} 
    libatspi2.0-0{a} libbz2-dev{a} libcairo-gobject2{a} 
    libcairo-script-interpreter2{a} libcairo2-dev{a} libcdt4{a} libcgraph5{a} 
    libcolord1{a} libdatrie-dev{a} libdbus-1-dev{a} libdconf1{a} 
    libdjvulibre-dev{a} libdjvulibre-text{a} libdjvulibre21{a} libelfg0{a} 
    libexif-dev{a} libexif12{a} libexpat1-dev{a} libfftw3-double3{a} 
    libfontconfig1-dev{a} libfreetype6-dev{a} libgconf-2-4{a} 
    libgconf2-dev{a} libgcrypt11-dev{a} libgd2-noxpm{a} 
    libgdk-pixbuf2.0-dev{a} libgif-dev{a} libgif4{a} libglib2.0-bin{a} 
    libglib2.0-data{a} libglib2.0-dev{a} libgnutls-dev{a} 
    libgnutls-openssl27{a} libgnutlsxx27{a} libgpg-error-dev{a} libgpm-dev{a} 
    libgraph4{a} libgraphviz-dev{a} libgtk-3-0{a} libgtk-3-common{a} 
    libgtk-3-dev{a} libgvc5{a} libgvpr1{a} libharfbuzz-dev{a} libice-dev{a} 
    libilmbase-dev{a} libilmbase6{a} libjasper-dev{a} libjbig-dev{a} 
    libjpeg-dev{a} libjpeg-turbo8-dev{a} libjpeg8-dev{a} libjs-jquery{a} 
    liblcms2-2{a} liblcms2-dev{a} liblockfile-dev{a} liblqr-1-0{a} 
    liblqr-1-0-dev{a} libltdl-dev{a} libltdl7{a} liblzma-dev{a} liblzo2-2{a} 
    libm17n-0{a} libm17n-dev{a} libmagick++-dev{a} libmagick++5{a} 
    libmagickcore-dev{a} libmagickcore5{a} libmagickcore5-extra{a} 
    libmagickwand-dev{a} libmagickwand5{a} libncurses5-dev{a} 
    libopenexr-dev{a} libopenexr6{a} libotf-dev{a} libotf0{a} 
    libp11-kit-dev{a} libpango1.0-dev{a} libpathplan4{a} libpcre3-dev{a} 
    libpcrecpp0{a} libpixman-1-dev{a} libpng12-dev{a} libpthread-stubs0{a} 
    libpthread-stubs0-dev{a} librsvg2-2{a} librsvg2-common{a} librsvg2-dev{a} 
    libselinux1-dev{a} libsepol1-dev{a} libsm-dev{a} libtasn1-3-dev{a} 
    libthai-dev{a} libtiff5-dev{a} libtiffxx5{a} libtinfo-dev{a} 
    libwayland-dev{a} libwayland0{a} libwmf-dev{a} libwmf0.2-7{a} 
    libx11-dev{a} libxau-dev{a} libxaw7-dev{a} libxcb-render0-dev{a} 
    libxcb-shm0-dev{a} libxcb1-dev{a} libxcomposite-dev{a} libxcursor-dev{a} 
    libxdamage-dev{a} libxdmcp-dev{a} libxdot4{a} libxext-dev{a} 
    libxfixes-dev{a} libxft-dev{a} libxi-dev{a} libxinerama-dev{a} 
    libxkbcommon-dev{a} libxkbcommon0{a} libxml2-dev{a} libxmu-dev{a} 
    libxmu-headers{a} libxpm-dev{a} libxrandr-dev{a} libxrender-dev{a} 
    libxt-dev{a} m17n-contrib{a} m17n-db{a} pkg-config{a} postfix{a} quilt{a} 
    sharutils{a} ssl-cert{a} texinfo{a} x11proto-composite-dev{a} 
    x11proto-core-dev{a} x11proto-damage-dev{a} x11proto-fixes-dev{a} 
    x11proto-input-dev{a} x11proto-kb-dev{a} x11proto-randr-dev{a} 
    x11proto-render-dev{a} x11proto-xext-dev{a} x11proto-xinerama-dev{a} 
    xaw3dg{a} xaw3dg-dev{a} xorg-sgml-doctools{a} xtrans-dev{a} zlib1g-dev{a} 
  
<a name="emacs-traditional-build"/>
[&nbsp;](http://sfxpt.wordpress.com/)

## **How emacs-traditional is built**


    What window system should Emacs use?                    x11
    What toolkit should Emacs use?                          MOTIF
    Where do we find X Windows header files?                Standard dirs
    Where do we find X Windows libraries?                   Standard dirs
    Does Emacs use -lXaw3d?                                 no
    Does Emacs use -lXpm?                                   no
    Does Emacs use -ljpeg?                                  no
    Does Emacs use -ltiff?                                  no
    Does Emacs use a gif library?                           no 
    Does Emacs use -lpng?                                   no
    Does Emacs use -lrsvg-2?                                no
    Does Emacs use imagemagick?                             no
    Does Emacs use -lgpm?                                   yes
    Does Emacs use -ldbus?                                  yes
    Does Emacs use -lgconf?                                 no
    Does Emacs use GSettings?                               no
    Does Emacs use -lselinux?                               no
    Does Emacs use -lgnutls?                                no
    Does Emacs use -lxml2?                                  no
    Does Emacs use -lfreetype?                              yes
    Does Emacs use -lm17n-flt?                              no
    Does Emacs use -lotf?                                   yes
    Does Emacs use -lxft?                                   yes
    Does Emacs use toolkit scroll bars?                     yes
  

Clean and to the bare minimum!
