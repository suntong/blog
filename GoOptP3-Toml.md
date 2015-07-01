# TOML - Go De Facto config file

[category Tech][tags go,programming,CLI]

This is the third article of the [mini series](https://sfxpt.wordpress.com/2015/06/16/providing-options-for-go-applications/) on how to provide options for Go applications. Today's focus is shifting from the built-in options to third-party ones. TOML is Go's de facto config file, which is also the foundation of my next two articles. 

<!--more-->

We've covered how to [provide options](https://sfxpt.wordpress.com/2015/06/18/passing-options-to-go-from-command-line/) for Go applications from the [command line](https://sfxpt.wordpress.com/2015/06/17/accessing-go-command-line-parameters/). But there are many cases, especially large projects, when settings are more static. Like a port number for a service. We don't need it to change it all the time, but we do need a way to change it from the default if we want to, and the change should better be saved. More or less like Windows `.ini` files. This is where TOML shines. It in fact looks very similar like Windows `.ini` file, but much more powerful.

The following samples are form the following sites,


**Reading config files the Go way**  
http://blog.gopheracademy.com/advent-2014/reading-config-files-the-go-way/

**TOML parser and encoder for Go with reflection**  
https://github.com/BurntSushi/toml

Reposted again just for the sake of convenient.

## TOML Advantages

As said before, TOML files looks very similar like Windows `.ini` file, but it can be much more powerful, like this:


```
	# This is a TOML document. Boom.

	title = "TOML Example"

	[owner]
	name = "Lance Uppercut"
	dob = 1979-05-27T07:32:00-08:00 # First class dates? Why not?

	[database]
	server = "192.168.1.1"
	ports = [ 8001, 8001, 8002 ]
	connection_max = 5000
	enabled = true

	[servers]

	  # You can indent as you please. Tabs or spaces. TOML don't care.
	  [servers.alpha]
	  ip = "10.0.0.1"
	  dc = "eqdc10"

	  [servers.beta]
	  ip = "10.0.0.2"
	  dc = "eqdc10"

	[clients]
	data = [ ["gamma", "delta"], [1, 2] ]

	# Line breaks are OK when inside arrays
	hosts = [
	  "alpha",
	  "omega"
	]
```

One may think that parsing and handling such beast would be very arduous, but on the contrary, it is very simple. Take this TOML file for example:


```
Age = 25
Cats = [ "Cauchy", "Plato" ]
Pi = 3.14
Perfection = [ 6, 28, 496, 8128 ]
DOB = 1987-07-05T05:45:00Z
```

It could be mapped into Go as:

```
type Config struct {
  Age int
  Cats []string
  Pi float64
  Perfection []int
  DOB time.Time // requires `import time`
}
```

And then decoded with:

```
var conf Config
if _, err := toml.Decode(tomlData, &conf); err != nil {
  // handle error
 }
```

That's it. Couldn't be simpler.

<a name="limits"/>
<a name="disadvantages"/>

## TOML Disadvantages

It's very easy if you are only to get configurations from the configuration file. However, what if I do need to override those options from the command line? In fact, having the following three-tiered approach is not uncommon:

- Have a set of default values defined in the program
- Variables defined in the config file will override them
- Variables passed from the command line takes the highest priority

However, with TOML, which maps from config file into a go struct, the problem is that go struct don't take initial values. Moreover, for command line parameters, the standard go flag package requires a bunch of loose variables, instead of organizing them in a structure. It allows to set default, but I saw no way to get their defined default values before the flag.Parse() call. 

My next two articles will talk about how to tackle it via yet more third party tools. But if you want no more third party packages, and want to solve it yourself, it is still possible, but awkward. For example, like the following:


```
func ConfigGet() *Config {
  var err error
  var cf *Config = NewConfig()

  // set default values defined in the program
  cf.ConfigFromFlag()
  //log.Printf("P: %d, B: '%s', F: '%s'\n", cf.MaxProcs, cf.Webapp.Path)

  // Load config file, from flag or env (if specified)
  _, err = cf.ConfigFromFile(*configFile, os.Getenv("APPCONFIG"))
  if err != nil {
    log.Fatal(err)
  }
  //log.Printf("P: %d, B: '%s', F: '%s'\n", cf.MaxProcs, cf.Webapp.Path)

  // Override values from command line flags
  cf.ConfigToFlag()
  flag.Usage = usage
  flag.Parse()
  cf.ConfigFromFlag()
  //log.Printf("P: %d, B: '%s', F: '%s'\n", cf.MaxProcs, cf.Webapp.Path)

  cf.ConfigApply()

  return cf
}

func (cf *Config) ConfigFromFlag() {
  cf.MaxProcs = *maxProcs
}

func (cf *Config) ConfigToFlag() {
  *maxProcs = cf.MaxProcs
}

```

I.e., you need to

0. set default values defined in the program from flag defaults first, then
0. load config file
0. then override values from command line to flags defaults
0. parse command line flags
0. pass parsed flag results back to configuration settings

Missing one step, you won't get the above three-tiered capability. 


That's the end of today's topic, the full source code with the three-tiered capability is available [here](https://github.com/suntong001/simplicity/blob/master/src/config.go).
