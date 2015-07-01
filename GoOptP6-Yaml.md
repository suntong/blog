# Go Yaml parsing example

[category Tech][tags go,programming]

This is the follow up article of the [mini series](https://sfxpt.wordpress.com/2015/06/16/providing-options-for-go-applications/) on how to provide options for Go applications. The series itself has finished, but I still want to extend it, to talk something beyond configuration. Today's focus is on [Yaml](www.yaml.org/refcard.html).

<!--more-->

The reason is that, the two (configuration and Yaml) are somewhat related. The `viper` package can even support configuration file in Yaml format right out of the box. [Yaml](www.yaml.org/refcard.html) can go far beyond providing configuration, it is best to express data, any data, structured, or unstructured. Period.

The best illustration of Yaml's power I can think of is the [symfony](http://symfony.com/) project, a PHP clone of the Ruby On Rail project. With it, you can scaffold -- automatically define your database, generate an entire CRUD web interface for it -- from only a single Yaml file. The database can contain several tables with interrelationships, yet the Yaml file describing it can be plain simple, [like this](http://symfony.com/legacy/doc/jobeet/1_4/en/03?orm=Doctrine). That's truly amazing. So I told myself, if you ever need to describe your data, describe it in Yaml.

Let's look at how to use Go to  parse Yaml. The Yaml we are going to parse today is rather simple:

```
 - foo: 1
   bar:
    - one
    - two
    - three

 - foo: 2
   bar:
    - one1
    - two2
    - three3
```

But it already contains all we want to demo about Yaml: array, hash, hash nested under array, and array nested under hash.

Here is how to map it into Go:

```
type Config struct {
	Foo string
	Bar []string
}

type Configs struct {
	Cfgs []Config `foobars`
}
```

And to get it working, we do need to provide an extra top level identification, so that the array can be mapped into something in Go, and also give a hint, in the `Configs`, the `foobars`, to Go. Here it is, data expressed in Go (note the top level `foobars` and the corresponding hint in the `Configs` struct):


```
var data = `
foobars:
 - foo: 1
   bar:
    - one
    - two
    - three

 - foo: 2
   bar:
    - one1
    - two2
    - three3
`
```

Parsing then is super easy:

```
func main() {

  var config Configs
  
	/*
	   filename := os.Args[1]
	   source, err := ioutil.ReadFile(filename)
	   if err != nil {
	       panic(err)
	   }
	*/

	source := []byte(data)

	err := yaml.Unmarshal(source, &config)
	if err != nil {
		log.Fatalf("error: %v", err)
	}

	fmt.Printf("--- config:\n%v\n\n", config)
}
```

From the commented out code, you can also see how to do if you want to input from a file. Either way, it parses just fine:

```
$ go run Yaml-Example.go
--- config:
{[{1 [one two three]} {2 [one1 two2 three3]}]}
```

That's all we're going to cover for today, the only thing missing from the listing is the import of `gopkg.in/yaml.v2`. The full source code of Yaml parsing demo is available [here](https://github.com/suntong001/lang/blob/master/lang/Go/src/text/Yaml-Example.go). The credit goes to http://sweetohm.net/html/go-yaml-parsers.en.html (I should have list it at the very top of my code, but it was a quick test that I forgot to put any of my "stationary" there). All this might look dull and uninteresting, but wait until the next article, which will be much more exciting -- code auto generation. It will not be as far-fetched as the [symfony](http://symfony.com/)'s scaffolding, but a full source code of `viper` and `cobra` working together to deal with configuration from command line can be automatically generated from a single, simple Yaml file. 
