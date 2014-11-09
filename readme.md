To manage webapp states so that you don't have to deal with url paths or anything.

[ui-router](https://github.com/angular-ui/ui-router/wiki) is fantastic, and I would use it in all of my projects if it wasn't tied to AngularJS.  Thus, this library!  Written to work with [browserify](https://github.com/substack/node-browserify).

addState(stateName, route, data, resolveFunction, callback)
========

`stateName` is parsed in the same way as ui-router's [dot notation](https://github.com/angular-ui/ui-router/wiki/Nested-States-%26-Nested-Views#dot-notation), so 'contacts.list' is a child state of 'contacts'.

`route` is an express-style url string that is parsed with a fork of [path-to-regexp](https://github.com/pillarjs/path-to-regexp).  If the state is a child state, this route string will be concatenated to the route string of its parent (e.g. if 'contacts' state has route ':user/contacts' and 'contacts.list' has a route of '/list', you could visit the child state by browsing to '/tehshrike/contacts/list').

`data` is an object that can hold whatever you want - it will be passed in to the resolve and callback functions.

`resolveFunction` is called when the selected state begins to be transitioned to, allowing you to accomplish the same objective as you would with ui-router's [resolve](https://github.com/angular-ui/ui-router/wiki#resolve).

## resolveFunction(data, parameters, callback(err, content), redirectCallback(stateName, params))

The first argument is the data object you passed to the addState call.  The second argument is an object containing the parameters that were parsed out of the route params and the query string.

If you call `callback(err, content)` with a truthy err value, the state change will be cancelled and the previous state will remain active.

If you call `redirectCallback(stateName, params)`, the state router will begin transitioning to that state instead.  The current destination will never become active, and will not show up in the browser history.

## callback(data, parameters, content)

The callback function is called when the state becomes active.  It is passed the data object from the addState call, the route/querystring parameters, and the content object passed into the resolveFunction's callback.

This is the point where you display the view for the current state!

go(stateName, parameters, [options])
=========

Browses to the given state, with the current parameters.  Changes the url to match.

The options object currently supports just one option "replace" - if it is truthy, the current state is replaced in the url history.

License
======

[WTFPL](http://wtfpl2.com)
