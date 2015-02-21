# Contributing To Emacs MELPA

[category Tech][tags Debian,Ubuntu,Linux,Emacs,Package,preseed]

I've been using the [emacs-traditional](http://sfxpt.wordpress.com/2014/12/13/bring-back-the-traditional-emacs/) for a while, just the bare-bone of it, without any site-level elisp modules, because it is based on the Ubuntu Emacs Daily Snapshot Recipe, which offers "little/no support for installing debian packages of elisp modules". 

Now I need to use the elisp modules. This leads to my rediscovery of GNU Emacs, thanks to the Emacs 24's package system, because my .emacs file is so mal-maintained, with a single humongous file containing all kinds of hacks and fixes, that it is beyond maintainable. I.e., I've declared [Dot Emacs Bankruptcy](http://www.emacswiki.org/emacs/DotEmacsBankruptcy) for my .emacs file long time ago.

I completely restructured [my Emacs init file](https://github.com/suntong001/emacs.d), which is now based on the built-in `package-install` method. While at it, I found that I need an Emacs  mode to edit Debian preseed files. So here is how I add [my Debian preseed Emacs mode](https://github.com/suntong001/preseed-generic-mode) To Emacs MELPA. It is super easy. 

<!--more-->

## Steps 1, Building and Testing

0. Fork the MELPA repository.
0. Add your new file under the default `recipes/` directory where package-build was loaded. 
0. Open the recipe in Emacs and press `C-c C-c` in the recipe buffer. This will also prompt you to install the freshly-built package.

```bash
git clone --depth 1 https://github.com/milkypostman/melpa.git

cd melpa/
echo '(preseed-generic-mode :fetcher github :repo "suntong001/preseed-generic-mode")' | tee recipes/preseed-generic-mode

emacs recipes/preseed-generic-mode
```

Then press `C-c C-c` in the recipe buffer. That should be it, if all went smoothly. Here is my total log FYI:


```
;;; preseed-generic-mode

Fetcher: github
Source: suntong001/preseed-generic-mode

Cloning git://github.com/suntong001/preseed-generic-mode.git to /export/repo/gitother/melpa/working/preseed-generic-mode/
Saving file /export/repo/gitother/melpa/packages/preseed-generic-mode-20150119.1541.el...
Wrote /export/repo/gitother/melpa/packages/preseed-generic-mode-20150119.1541.el
Wrote /export/repo/gitother/melpa/packages/preseed-generic-mode-readme.txt
File: /export/repo/gitother/melpa/packages/preseed-generic-mode-20150119.1541.entry
Built in 0.175s, finished at Sun Feb  1 10:41:50 2015
File: /export/repo/gitother/melpa/packages/archive-contents
Install new package? (y or n) y
Making version-control local to preseed-generic-mode-autoloads.el while let-bound!
Generating autoloads for preseed-generic-mode.el...done
Saving file /home/tong/.emacs.d/elpa/preseed-generic-mode-20150119.1541/preseed-generic-mode-autoloads.el...
Wrote /home/tong/.emacs.d/elpa/preseed-generic-mode-20150119.1541/preseed-generic-mode-autoloads.el
(No files need saving)
Checking /home/tong/.emacs.d/elpa/preseed-generic-mode-20150119.1541... [3 times]
Compiling /home/tong/.emacs.d/elpa/preseed-generic-mode-20150119.1541/preseed-generic-mode.el...done
Wrote /home/tong/.emacs.d/elpa/preseed-generic-mode-20150119.1541/preseed-generic-mode.elc
Checking /home/tong/.emacs.d/elpa/preseed-generic-mode-20150119.1541...
Done (Total of 1 file compiled, 2 skipped)
```

## Contributing To MELPA

The rest are even simpler:

- Open a pull request on Github. One pull request per recipe. 
- The package name should match the name of the feature provided. 
- Include the following information in the pull request:
  * A brief summary of what the package does.
  * A direct link to the package repository.
  * Your association with the package (e.g., are you the maintainer? have you contributed? do you just like the package a lot?).
- After verifying the entry works properly, submit the pull request.

That's it. Check out https://github.com/milkypostman/melpa#contributing-new-recipes for more details.
