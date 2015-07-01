# Providing Options for Go Applications

[category Tech][tags go,programming,CLI]

I'll be starting a mini series on how to provide options for Go applications, whether through command line parameters/options/flags, or through configuration files. This is going to be the index page to list them all. 

<!--more-->

Go syntax might be simple but its available packages are rich and abundant. This mini series covers the whole spectrum on how Go applications can take parameters, and configuration values, touching upon methods from the simplest, the built-in `Args`, all the way to third party packages like viper, which provides the richest set of features available today.

I'm new to Go and I can't claim that all the methods/packages are covered, but I tried my best, leaving no stone not turned, except for [gonfig](https://github.com/Nomon/gonfig), which I don't feel that it belongs here, because it is more for two-way configuration, saving configuration from the running application back, apart from the focus here, which is merely loading/overriding configurations. 

The advantages of having all available options displayed here is that, it is then easier to pick and choose. If the only thing I want is to get the first command line argument, then `Args` will surely be enough, and no need to go all the whole nine yards to get `viper` to do that.

Being new to Go, I appreciate more on the existing and working sample code. It will be a much easier start than to devour into the man page and trying to piece all nitty-gritty details together to come up with a total solution. So here they are, most of them are not from me, but reputable sources, with best-practices built-in. For example, it is nearly impossible for me to figure out how to make `viper` and `cobra` work together, but anyway, without further ado,

0. [**Accessing Go command line parameters**](https://sfxpt.wordpress.com/2015/06/17/accessing-go-command-line-parameters/)  
   which starts with `Args` 
0. [**Passing options to Go from command line**](https://sfxpt.wordpress.com/2015/06/18/passing-options-to-go-from-command-line/)  
   which focuses on the built-in command line parameters handling option, the built-in `flag` package.
0. [**TOML - Go De Facto config file**](https://sfxpt.wordpress.com/2015/06/18/toml-go-de-facto-config-file/)  
   which covers TOML, and also on overcoming its drawbacks
0. [**Beyond TOML, the Go's De Facto config file**](https://sfxpt.wordpress.com/2015/06/19/beyond-toml-the-gos-de-facto-config-file/)  
   which introduce a package less well-known but get the job done nice and easily
0. [**Viper, Go configuration with fangs**](https://sfxpt.wordpress.com/2015/06/25/viper-go-configuration-with-fangs/)  
   which, of course, touches upon `viper` and with a working example that works with `cobra` together, which you can't find elsewhere easily

<!---
0. [****]()
0. [****]()
-->

