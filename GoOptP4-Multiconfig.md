# Beyond TOML, the Go's De Facto config file

[category Tech][tags go,programming,CLI]

This is the forth article of the [mini series](https://sfxpt.wordpress.com/2015/06/16/providing-options-for-go-applications/) on how to provide options for Go applications. Today's focus is overcoming the limitation of TOML, the Go's de facto config file. This is the first of the two articles on this topic.

<!--more-->

We've moved from how to [provide options](https://sfxpt.wordpress.com/2015/06/18/passing-options-to-go-from-command-line/) for Go applications from the [command line](https://sfxpt.wordpress.com/2015/06/17/accessing-go-command-line-parameters/) to [TOML](https://sfxpt.wordpress.com/2015/06/18/toml-go-de-facto-config-file/), and have touched upon TOML's [limitation](https://sfxpt.wordpress.com/2015/06/18/toml-go-de-facto-config-file/#disadvantages) if you are not only to get configurations from the configuration file. Of course, the solution is more third party packages, and every one is shouting, "Viper, Viper!". But before going directly to the heavy weight champion, let me introduce you something that is much less well-known -- the package called `multiconfig`, https://github.com/koding/multiconfig.

Some people always thinks bigger is better, but I believe what fits better is the better choice. Because,

- Viper is huge, you need to pulling [20 packages](https://godoc.org/github.com/spf13/viper?imports) before you can even start using it.
- Moreover, in fact, viper alone is not enough -- to get the full benefit of viper, you need yet another package that goes along with it, which depends on 13 other packages. 
- Furthermore, comparing to `multiconfig`, for `viper` you've got some heavy lifting to do before you can use it, whereas `multiconfig` is much more easier.

Ok, once again, the problem at hand is, it is not uncommon that we need to, 

 - Define default values within the program so that you don’t need to define them again in TOML file
 - Override values defined in the program from TOML config file
 - Override values defined in config file from env var or command line 

This article will show the code and the results. Alright, without further ado, here is how to use `multiconfig`:

```
func main() {
  m := multiconfig.NewWithPath("config.toml") // supports TOML and JSON

  // Get an empty struct for your configuration
  serverConf := new(Server)

  // Populated the serverConf struct
  m.MustLoad(serverConf) // Check for error

  fmt.Println("After Loading: ")
  fmt.Printf("%+v\n", serverConf)

  if serverConf.Enabled {
    fmt.Println("Enabled field is set to true")
  } else {
    fmt.Println("Enabled field is set to false")
  }
}
```

Pretty straightforward. Here is how the `config.toml` file looks like:

```
Name              = "koding"
Enabled           = false
Port              = 6066
Users             = ["ankara", "istanbul"]

[Postgres]
Enabled           = true
Port              = 5432
Hosts             = ["192.168.2.1", "192.168.2.2", "192.168.2.3"]
AvailabilityRatio = 8.23
```

And here is how that `config.toml` file maps into Go structure:

```
type (
  // Server holds supported types by the multiconfig package
  Server struct {
    Name     string
    Port     int `default:"6060"`
    Enabled  bool
    Users    []string
    Postgres Postgres
  }

  // Postgres is here for embedded struct feature
  Postgres struct {
    Enabled           bool
    Port              int
    Hosts             []string
    DBName            string
    AvailabilityRatio float64
  }
)
```

That's it! The only things have not listed from the [full source](https://github.com/suntong001/lang/blob/master/lang/Go/src/sys/CommandLineMulticonfig.go) are the `package` and `import` lines. I.e., it is super simple to use `multiconfig`, and you will agree with me later when you see the next article. According to Fatih Arslan @arslan.io, multiconfig is menant to *"override config in each layer and provide multiple config parsing"*:

- It has support for defaults as well for explicit requirement of fields you wish.
- It supports toml files, json files, struct tags, flags and environment
variables.
- You can even define your own source (for example a remote server) if you wish.
- It's highly extensible (via the loader interface) and you can create your
own loaders.

Here is how it runs. Again, usage help first:

```
$ go run CommandLineMulticonfig.go -help
Usage of /tmp/go-build067833251/command-line-arguments/_obj/exe/CommandLineMulticonfig:
  -enabled=false: Change value of Enabled.
  -name=koding: Change value of Name.
  -port=6060: Change value of Port.
  -postgres-availabilityratio=8.23: Change value of Postgres-AvailabilityRatio.
  -postgres-dbname=: Change value of Postgres-DBName.
  -postgres-enabled=true: Change value of Postgres-Enabled.
  -postgres-hosts=[192.168.2.1 192.168.2.2 192.168.2.3]: Change value of Postgres-Hosts.
  -postgres-port=5432: Change value of Postgres-Port.
  -users=[ankara istanbul]: Change value of Users.

Generated environment variables:
   SERVER_NAME
   SERVER_PORT
   SERVER_ENABLED
   SERVER_USERS
   SERVER_POSTGRES_ENABLED
   SERVER_POSTGRES_PORT
   SERVER_POSTGRES_HOSTS
   SERVER_POSTGRES_DBNAME
   SERVER_POSTGRES_AVAILABILITYRATIO


$ go run CommandLineMulticonfig.go
After Loading: 
&{Name:koding Port:6060 Enabled:false Users:[ankara istanbul] Postgres:{Enabled:true Port:5432 Hosts:[192.168.2.1 192.168.2.2 192.168.2.3] DBName: AvailabilityRatio:8.23}}
Enabled field is set to false

```

The `config.toml` file is properly loaded and interpreted.


```
$ go run CommandLineMulticonfig.go -enabled=true
After Loading: 
&{Name:koding Port:6060 Enabled:true Users:[ankara istanbul] Postgres:{Enabled:true Port:5432 Hosts:[192.168.2.1 192.168.2.2 192.168.2.3] DBName: AvailabilityRatio:8.23}}
Enabled field is set to true

$ SERVER_ENABLED=true go run CommandLineMulticonfig.go -port=8080
After Loading: 
&{Name:koding Port:8080 Enabled:true Users:[ankara istanbul] Postgres:{Enabled:true Port:5432 Hosts:[192.168.2.1 192.168.2.2 192.168.2.3] DBName: AvailabilityRatio:8.23}}
Enabled field is set to true

```

Check out the printed out results when different options are provided. Everything is working as expected.

That's the end of today's topic, the full source code is available [here](https://github.com/suntong001/lang/blob/master/lang/Go/src/sys/CommandLineMulticonfig.go).

