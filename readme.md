<img src='http://aralbalkan.com/images/tally-label-printer.png'>

Tally: Hello badge example (part 1 of 4)
===

[In part 1](https://github.com/aral/tally-hello-badge-1-text-and-attribute), we used the [Tally template engine](http://tally.jit.su) ([fork Tally on GitHub](https://github.com/aral/tally)) to create a name badge that displays the user’s name and links it to her web site.

In this example, we’re going to use that to make lots of labels.

Usage
---

1. Clone the repository.
2. Run server.coffee (I use ```nodemon server.coffee``` while developing. Or you can just run ```coffee server.coffee```). Once the server is running, go to ```http://localhost:3000``` to see the example and ```http://localhost:3000/hello-badge.html``` to see the template source.
3. Read the notes below and take a peek at the source code.

How it works
---

Templates in Tally are pure HTML 5. Tally uses ```data-``` attributes to populate templates either on the server or on the client (or both).

One of those attributes, ```data-tally-repeat``` allows you to repeat a DOM element multiple times by looping over an array in the data.

The template
---

Let’s modify the ```hello-badge.html``` template so that our name badge is wrapped up in a list. Each list item in the list will be repeated and become a new badge.

```html
<ul>
	<li data-tally-repeat='person people'>
		<a data-tally-attribute='href person.homepage'><p>Hello, my name is <span data-tally-text='person.name'>Inigo Montoya</span></p></a>
	</li>
</ul>
```

With the attribute ```data-tally-repeat='person people'``` we are saying ‘repeat this node for each person in the people array’.

That’s the only change we need to make in the HTML.

The server
---

The only thing we need to change in the server is the data object that we’re passing to Tally. We create an array of people in it, where each person has name and homepage properties.

```javascript
express = require 'express'
tally = require 'tally'

app = express()
app.engine 'html', tally.__express
app.set 'view engine', 'html'
app.use express.static('views')

data = {
	people: [
			{ name: 'Aral', homepage: 'http://aralbalkan.com' }
		,	{ name: 'Laura', homepage: 'http://laurakalbag.com' }
		, 	{ name: 'Natalie', homepage: 'http://ndkane.com' }
		, 	{ name: 'Seb', homepage: 'http://seb.ly' }
		,	{ name: 'Osky', homepage: 'http://twitter.com/gigapup' }
	]
}

app.get '/', (request, response) ->
	response.render 'hello-badge', data

app.listen 3000
```

That’s it!

When you run the example, five badges will be created.

Continue learning about Tally in [Part 3: Conditionals](https://github.com/aral/tally-hello-badge-3-conditionals).

Table of Contents
---

* Part 1: [Text and Attribute](https://github.com/aral/tally-hello-badge-1-text-and-attribute)
* Part 2: [Repetition](https://github.com/aral/tally-hello-badge-2-repetition)
* Part 3: [Conditionals](https://github.com/aral/tally-hello-badge-3-conditionals)
* Part 4: [Dummy Data](https://github.com/aral/tally-hello-badge-4-dummy-data)


This is just a very simple example. [Check out the Tally web site](http://tally.jit.su) for more complicated ones.