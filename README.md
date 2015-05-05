noddity-view-model
=========

# example

```js
var setCurrent = (function () {
	var level = require('level-mem')
	var Sublevel = require('level-sublevel')
	var Retrieval = require('noddity-fs-retrieval')
	var Butler = require('noddity-butler')
	var Renderer = require('noddity-renderer')
	var ViewModel = require('noddity-view-model')
	var renderData = require('./renderData.json')
	var renderTemplate = require('fs').readFileSync('index.html', { encoding:'utf8' })

	var butlerOpts = {
		refreshEvery: 600000,
		cacheCheckIntervalMs: 10000
	}

	var db = new level('./database')
	var retrieve = new Retrieval(renderData.noddityRoot)
	var butler = new Butler(retrieve, db, butlerOpts)
	var renderer = new Renderer(butler, String)
	return new ViewModel(butler, renderer, renderTemplate, renderData)
})()

setCurrent('whatever-page.md', function (err, html) {
	document.getElementById('page').innerHTML = html
})
```

# api

```js
var ViewModel = require('noddity-view-model')
```

# `var setCurrent = ViewModel(butler, renderer, ractiveTemplate[, ractiveData])`

- `butler` is an instantiated noddity butler.
- `renderer` is an instantiated noddity renderer.
- `ractiveTemplate` is a ractive-flavored string template. (Like a handlebars template.)
- `ractiveData` is the initial data that the internal ractive instance starts with. Optional, defaults to `{}`.
- *Returns* `setCurrent`.

# `setCurrent(page, cb)`

- `page` is the markdown page that should be rendered.
- `cb` is a function that has the following arguments:
	- `err` is an error, or `null`.
	- `html` is the compiled html.

# notes on templates

Here is a list of things you can use in your template:

- `{{html}}`
- `{{current}}`
- `{{date}}`
- `{{page}}`
- `{{postList}}`
- Properties on the `ractiveData` object:

```js
ractiveTemplate = '{{html}}\n{{current}}\n{{date}}\n{{page}}\n{{postList}}\n{{thingy}}'
ractiveData = { thingy: 3 }
```


# install

With [npm](http://nodejs.org/download) do:

	npm install noddity-view-model

# license

[VOL](http://veryopenlicense.com)
