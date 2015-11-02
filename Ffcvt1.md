# Showcasing the power of easygen with ffcvt

[category Tech][tags Debian,Ubuntu,Linux,go,programming,audio,video,tool,playing,CLI]

The [`easygen`](https://github.com/suntong001/easygen) is an easy to use [universal code/text generator](https://github.com/suntong001/blog/blob/master/GoOptP7-easygen.md).
On top of that, it can [provide options](https://sfxpt.wordpress.com/2015/06/18/passing-options-to-go-from-command-line/) for Go applications from the [command line](https://sfxpt.wordpress.com/2015/06/17/accessing-go-command-line-parameters/) as well. As matter of fact, it is using its own commandline flag code auto-generation capability to [code itself](https://sfxpt.wordpress.com/2015/07/04/easygen-is-now-coding-itself/). Such [commandline flag code auto-generation](https://sfxpt.wordpress.com/2015/07/04/easygen-is-now-coding-itself/) functionality has been improved sine then. 

<!--more-->

## Readability improvement

If you take a closer look at the [`config.go`](https://github.com/suntong001/ffcvt/blob/master/config.go) file, you will notice that the code block are now broken down into logical chunks. The extra line breaks are added so that related option-handling are grouped together now:

https://gist.github.com/b6ea4ccee2dabe084237

- It almost looks like being manually written, not by the [commandline-flag code auto-generation](https://sfxpt.wordpress.com/2015/07/04/easygen-is-now-coding-itself/).
- Note also that in the `struct` definition, there is an anonymous field for default values population. This feature is not part of the standard `easygen` commandline flag code auto-generation, but that doesn't prevent the code from being automatically generated either. The method is documented clearly at the top of the driving [ffcvt.yaml](https://github.com/suntong001/ffcvt/blob/master/ffcvt.yaml) file. Check the [commit history](https://github.com/suntong001/ffcvt/commits/master) to see how such file is driving the whole code auto-generation.

## Usage help improvement

The problem of using the Go `flag`'s `flag.PrintDefaults()` to show usage help is that, the usage help print out is not in the order that the options are defined/added, but according to the flag string instead. I.e., the logical order/grouping of the flags are broken up, and presented in an entirely different way. One solution is to strategically name the commandline flag strings so that similar options are grouped near each other, like this:

```
$ easygen 

Usage:
 easygen [flags] YamlFileName

Flags:

  -debug=0: debugging level
  -et=".tmpl": extension of template file
  -ey=".yaml": extension of yaml file
  -html=false: treat the template file as html instead of text
  -rf="": replace from, the from string used for the replace function
  -rt="": replace to, the to string used for the replace function
  -tf="": .tmpl template file name (default: same as .yaml file)
  -ts="": template string (in text)

YamlFileName: The name for the .yaml data and .tmpl template file
 Only the name part, without extension. Can include the path as well.
```

For `easygen` I can still use that method to make use of `flag.PrintDefaults()`, but for  [`ffcvt`](https://github.com/suntong001/ffcvt), it fails miserably because I'm providing options for both audio/video encoding. I want to group not by audio or video parameters, but by operations instead. This is why the `USAGE_SUMMARY` was introduced. Take a look:

https://gist.github.com/841d82361405e924ed55

- It is the quick usage help output produced when `ffcvt` is invoked without any parameters, produced by the automatically generated code `config.go`. Everything from this output is automatically generated.
- Grouping operations together make much more sense for this case. The output from `USAGE_SUMMARY` (the section after the "`Flags:`") is much more organized, and appealing to human eyes too, comparing to the default help (the section after the "`Details:`"). Moreover, that in turn, makes linking/documenting environment variables that corresponding to the command line parameters in usage help possible as well. 

