*This repository is a mirror of the [component](http://component.io) module [wejendorp/local-collection-sync](http://github.com/wejendorp/local-collection-sync). It has been modified to work with NPM+Browserify. You can install it using the command `npm install npmcomponent/wejendorp-local-collection-sync`. Please do not open issues or send pull requests against this repo. If you have issues with this repo, report it to [npmcomponent](https://github.com/airportyh/npmcomponent).*
# local-collection-sync
Note: Work in progress!

Plugin for [local-collection](https://github.com/wejendorp/local-collection) to
sync localStorage collection with the server.

Idea: Progressive enhancement for collections. Return available models quickly,
and update the fields when (if) more data becomes available.

This will make collections work in offline mode, or just present cached data
until the server gets around to responding.

## Future
Keep track of unsaved models and sync when possible.

## Installation

    $ component install wejendorp/local-collection-sync

## Example
Load a list of names with progressive enhancement. The list will be populated
from cache on refresh, and new items will pop up when server responds to fetch.

```js
var userCollection = require('usercollection');

var users = userCollection.fetch('active');
users.each(show); // Show cached results

users.on('change', function() {
  this.each(show); // Show server results
});

var body = document.body;
var domNodes = {};
var tpl = '<p data-text="name"></p>';
function show(model) {
  var node = domNodes[model.id()] || reactive(domify(tpl), model);
  body.appendChild(node.el);
};
```

## API
This plugin introduces the methods

### fetch(url, fn)
Fetch the models at model.url() or at `options.url`, returning a new collection
instance with the result. Caches the response as a list of ids.

```js
var cacheCollection = collection.fetch('/users/active', function(err, updated) {
  // updated is the same collection after it has been fetched
});
// cacheCollection is a cached version of the model, or just an empty model with the id
```

### get(id, fn)
Fetch the model by id, returns model instance and updates it async. Use fn to
wait for server, or no callback to use cached.

```js
var cached = collection.get('id', function(err, updated) {
  // updated is a reference to model after it has been updated
});
// cached is a cached version of the model, or just an empty model with the id
```

### url
TODO

### route
TODO

## Dependencies
- [visionmedia/superagent](https://github.com/visionmedia/superagent)

## License
MIT
