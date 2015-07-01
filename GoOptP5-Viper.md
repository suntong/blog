# Viper, Go configuration with fangs

[category Tech][tags go,programming,CLI]

This is the fifth article of the [mini series](https://sfxpt.wordpress.com/2015/06/16/providing-options-for-go-applications/) on how to provide options for Go applications. Today's focus is, finally, the heavy weight champion, Viper -- https://github.com/spf13/viper.

<!--more-->

Everyone is recommending `viper`, but I haven't found anyone blogging about a sample code showing how to use `viper`, so I decided to write one.

Viper is powerful, no doubt about that. But if you want to override predefined values from command line, viper alone is not enough. In order to get the full benefit of viper, you need yet another package that goes along with it, cobra -- https://github.com/spf13/cobra. 

Moreover, unlike `multiconfig`, which emphasizes simplify your configuration handling, `viper` give you the full control. Unfortunately that also means that you've got some heavy lifting to do before you can use it.

Once again, let's first take a look how to use `multiconfig`:

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

That's pretty much everything you need to do, besides [providing the TOML file and mapping it into Go](https://sfxpt.wordpress.com/2015/06/19/beyond-toml-the-gos-de-facto-config-file/). Now let's look how to use `viper` and `cobra` together to handle options:

```
  mainCmd.AddCommand(versionCmd)

  viper.SetEnvPrefix("DISPATCH")
  viper.AutomaticEnv()

  /*

    When AutomaticEnv called, Viper will check for an environment variable any
    time a viper.Get request is made. It will apply the following rules. It
    will check for a environment variable with a name matching the key
    uppercased and prefixed with the EnvPrefix if set.

  */

  flags := mainCmd.Flags()

  flags.Bool("debug", false, "Turn on debugging.")
  flags.String("addr", "localhost:5002", "Address of the service")
  flags.String("smtp-addr", "localhost:25", "Address of the SMTP server")
  flags.String("smtp-user", "", "User to authenticate with the SMTP server")
  flags.String("smtp-password", "", "Password to authenticate with the SMTP server")
  flags.String("email-from", "noreply@example.com", "The from email address.")

  viper.BindPFlag("debug", flags.Lookup("debug"))
  viper.BindPFlag("addr", flags.Lookup("addr"))
  viper.BindPFlag("smtp_addr", flags.Lookup("smtp-addr"))
  viper.BindPFlag("smtp_user", flags.Lookup("smtp-user"))
  viper.BindPFlag("smtp_password", flags.Lookup("smtp-password"))
  viper.BindPFlag("email_from", flags.Lookup("email-from"))

  // Viper supports reading from yaml, toml and/or json files. Viper can
  // search multiple paths. Paths will be searched in the order they are
  // provided. Searches stopped once Config File found.
  
  viper.SetConfigName("CommandLineCV") // name of config file (without extension)
  viper.AddConfigPath("/tmp")          // path to look for the config file in
  viper.AddConfigPath(".")             // more path to look for the config files

  err := viper.ReadInConfig()
  if err != nil {
    println("No config file found. Using built-in defaults.")
  }
```

I.e., you need that extra `BindPFlag` step for `viper` and `cobra` to work together. But that's not too bad. 

The true strength of `cobra` is that it provides subcommand capability, just like `go`, or `git` or any modern CLI commands, like `apt-get`, `apptitude`, etc.
 Also, `cobra` provides *"fully posix compliant flags (including short & long versions), nesting commands, and the ability to define your own help and usage for any or all commands"*.

Here is how to define subcommands:

```
// The main command describes the service and defaults to printing the
// help message.
var mainCmd = &cobra.Command{
  Use:   "dispatch",
  Short: "Event dispatch service.",
  Long:  `HTTP service that consumes events and dispatches them to subscribers.`,
  Run: func(cmd *cobra.Command, args []string) {
    serve()
  },
}

// The version command prints this service.
var versionCmd = &cobra.Command{
  Use:   "version",
  Short: "Print the version.",
  Long:  "The version of the dispatch service.",
  Run: func(cmd *cobra.Command, args []string) {
    fmt.Println(version)
  },
}
```

Together with flags definition above, it can give a help message like this:

```
Usage:
  dispatch [flags]
  dispatch [command]

Available Commands:
  version     Print the version.
  help        Help about any command

Flags:
      --addr="localhost:5002": Address of the service
      --debug=false: Turn on debugging.
      --email-from="noreply@example.com": The from email address.
  -h, --help=false: help for dispatch
      --smtp-addr="localhost:25": Address of the SMTP server
      --smtp-password="": Password to authenticate with the SMTP server
      --smtp-user="": User to authenticate with the SMTP server


Use "dispatch help [command]" for more information about a command.

```

That's all we're going to cover for today, the rest are trivial. The full source code is available [here](https://github.com/suntong001/lang/blob/master/lang/Go/src/sys/CommandLineCV.go), which gives the credit to its based source on the very top of the file. Check out the printed out results at the end of the program when different options are provided. All usual cases are covered and everything is working as expected.

