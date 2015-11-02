# Easygen for mock-up

[category Tech][tags go,programming,CLI]

Back to my recent favorite again, [easygen](https://github.com/suntong001/easygen), an easy to use universal code/text generator, this time, on mock-up generation, moving into an new territory we haven't [covered](https://github.com/suntong001/blog/blob/master/GoOptP7-easygen.md) [before](https://sfxpt.wordpress.com/2015/07/04/easygen-is-now-coding-itself/).

<!--more-->

<a name="we" />
## Why easygen?

[ ](https://sfxpt.wordpress.com/)

Why [easygen](https://github.com/suntong001/easygen) is born?

- When I'm not sure if something works or not, or don't know how it works, I'd like to first try with a small example; as opposed to incorporate it directly into my big project and hope it will works out perfectly at first attempt.
- Manually generating such mock-up is tedious, and error prone. 
- Most importantly, when things don't work out yet, I most often need to make dramatic changes, and many times, such changes are so dramatic, that you can't use any existing tools to do the "replace-all" for you, for the already-handcrafted mock-up. 

## Problem at hand

There is this [_Multiple Checkbox Select/Deselect Using JQuery_](http://viralpatel.net/blogs/multiple-checkbox-select-deselect-jquery-tutorial-example/) which has an [Online Demo here](http://viralpatel.net/blogs/demo/jquery/multiple-checkbox-select-deselect/jquery-select-multiple-checkbox.html), it demonstrates the functionality of selecting multiple items from a list to process them.

I want to adapt the algorithm so that it can not only handle merely a single group of items, but can also handle several groups as well. The problems are,

- to have a better coverage of test cases, it is better to have at least three groups to start with.
- however, due to the fact that the processing is done by the jQuery operating on the html elements, a dumb copy-and-paste won't cut it because the duplicated groups need to be different from the original, otherwise it won't work.
- most importantly, I don't want a directly copy even at the first place, because I don't quite understand how the html elements and the jQuery inter-operate with each others, so making my own version is very important for the jQuery script to be "detached" from the original working example.
- furthermore, I need to simplify the table structure as well, from two columns to single column. 
- overall, I don't know Javascript and I don't know jQuery, so the only way for me to get things right is through countless of trials and errors, until I hit the jackpot. 

On all these accounts, this seems to be an arduous job, so I put it aside and developed [easygen](https://github.com/suntong001/easygen) first instead.

<a name="we" />
## How easygen helps?

[ ](https://sfxpt.wordpress.com/)

The [easygen](https://github.com/suntong001/easygen) brings a new ways of thinking. I didn't realize it until I've experienced it.

- If we have to change mock-up manually, and there isn't any tools to assists, we'd tend to avoid dramatic changes but to try with easier alternatives first. But it might be that the dramatic change that you put off to the last minutes might be the one that really work. 
- Thus the trials will be arranged according to how rewarding it could be, instead of easier ones comes first.
- One thing that comes as unexpected surprise is, since making changes are now almost zero cost, I was much more willing to try out new thoughts.
- And because of the changes and responds are instantaneous, you trains of thought won't be lost in the arduous side jobs of making manual changes. 
- Thus you get your test done in a very short amount of time.
- Above of all, [easygen](https://github.com/suntong001/easygen) makes complicated mock-up easy. Different groups, each groups having their own entries, that's a double loop right there. The more complicated the situation is, the clearly and more expressive you will find `easygen` to be. 

## The result

Well, the journey, i.e., the process of explorations, and making progress bit by bit, is much more fun than the actual results, but listed here anyway, as a result of many people's help, especially [ibrahimyilmaz](http://stackoverflow.com/users/2696626/ibrahimyilmaz) from stackoverflow.

So for the sake of showing off how easy it is to use `easygen`, let's look at some files. First, please take a look at the [finished HTML file](https://github.com/suntong001/simplicity/blob/master/www/test-Checkbox-Multiple2.html):

```html
  ...
  <table border="1">
	<tbody>

	  <tr>
	    <th>
	      <input type="checkbox" name="caseall" id="Android" data-type="android">
	    </th>
	    <th>Android Cell phones</th>
	  </tr>
	  <tr>
	    <td align="center">
	      <input type="checkbox" name="case" data-type="android" value="Samsung Galaxy">
	    </td>
	    <td>Samsung Galaxy</td>
	  </tr>
	  <tr>
	    <td align="center">
	      <input type="checkbox" name="case" data-type="android" value="Droid X">
	    </td>
	    <td>Droid X</td>
	  </tr>
	...
```

Take a good look at the [rendered effect](http://fiddle.jshell.net/ybrkteqp/show/), and think of what kind of data is necessary to drive such output, then think for a second or two how best those data should be arranged. Now take a look how actually the data is expressed:

```
phone:

  - Type: Android
    Modules: 
      - Samsung Galaxy
      - Droid X
      - HTC Desire

  - Type: iPhone
    Modules: 
      - Apple iPhone 4
      - Apple iPhone 5
      - Apple iPhone 6

  - Type: BlackBerry
    Modules: 
      - BlackBerry Bold 9650
      - BlackBerry 10
```

That's it. Can't be simpler. That's the beauty of yaml, expressing complicated things in a very clear and easy way.

For those people who had payed close attention to the above two files, they might have noticed that the string for `data-type=` in the above html file actually cannot be found in the driving data. Where do they come from? The trick is in the template file.

For your convenient, I'll list the [template file](https://github.com/suntong001/easygen/blob/master/test/html-Checkbox-Group.tmpl) here as well:

```html
      ...
      <table border="1">
	<tbody>
{{range .phone}}{{$phoneType := ck2ls .Type}}
	  <tr>
	    <th>
	      <input type="checkbox" name="caseall" id="{{.Type}}" data-type="{{$phoneType}}">
	    </th>
	    <th>{{.Type}} Cell phones</th>
	  </tr>{{range .Modules}}
	  <tr>
	    <td align="center">
	      <input type="checkbox" name="case" data-type="{{$phoneType}}" value="{{.}}">
	    </td>
	    <td>{{.}}</td>
	  </tr>{{end}}
{{end}}	
	</tbody>
      </table>
      ...
```

Very intuitive. Hopefully I've shown a tip of the iceberg how powerful the [Go Template](https://goo.gl/dz7yih) is, the Hercules behind [easygen](https://github.com/suntong001/easygen).

That's the end of the first example. Right after it, I need to generate some nice looking modern HTML tables, and the only thing to make sure the [stunning effect](http://codepen.io/geoffyuen/pen/FCBEg) will still be working after duplicating to my site is to try it first, with a small example. And which tools is best for it again? :-) This time, I can borrow the previous existing working template, which has that double-loop already in place. So before I even started, I've already half-way done, and I don't even need to write driving yaml data at all, just reusing the old one is enough:

	easygen -tf test/html-Table-RWD test/html-Checkbox-Group

Again, as I am not a CSS programmer, [this small demo](http://htmlpreview.github.io/?https://github.com/suntong001/easygen/blob/master/test/html-Table-RWD.html) now ensures me that I've successfully "borrowed"/"tore-away" [the original stunning effect](http://codepen.io/geoffyuen/pen/FCBEg) into my own.


In conclusion, the [easygen](https://github.com/suntong001/easygen) is all about getting arduous things done quite easily and lighting fast.

Thanks for reading thus far. Happy hacking.
