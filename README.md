apicache
========

An ultra-simplified API/JSON response caching middleware for Express/Node using plain-english durations.

## Why?

Because caching of simple data/responses should ALSO be simple.

This day and age, with less and less heavy lifting done on the server,
the most common thing we find ourselves doing is
letting the server power the API.  Whether the data is stored in Mongo, SQL,
CouchDB, or whatever - you get a request, you fetch the data, and you return
the data.  Sometimes these fetches are costly and you want to cache the response
so the next hit doesn't hammer your server.  This is why caching exists.

The problem is, with so many cache options, people are still left to fend for themselves
when it comes to implementation.  It often boils down to a manual process similar to the following:

1. Get the request
2. Check your cache for the key/url.
3. If found, intercept and output the cached version.
4. If not, do the work, cache it, and output.

You're still left wrapping the content of each request with this cache-checking mechanism.

Now it can be as simple as telling the request that you want to use a cache, and for
how long results should be cached (*in plain English*, not milliseconds, because
who really wants to calculate that each time?).

## Installation

```
npm install apicache
```

## API

- `apicache.middleware([duration])` - the actual middleware that will be used in your routes.  `duration` is in the following format "[length] [unit]", as in `"10 minutes"` or `"1 day"`.
- `apicache.options([options])` - getter/setter for options.  If used as a setter, this function is chainable, allowing you to do things such as... say... return the middleware.

## Usage

To use, simply inject the middleware (example: `apicache('5 minutes')`) into your routes.  Everything else is automagic.

```js
var apicache = require('apicache').middleware;

...

// an example route
app.get('/api/v1/myroute', apicache('5 minutes'), function(req, res, next) {
  res.send({ foo: bar });
});

```

## Limitations

- `apicache` is currently an in-memory cache, built upon [memory-cache](https://github.com/ptarjan/node-cache).  It may later be expanded to allow other cache-layers.
- This should only be used for JSON responses (as from an API) - if for no other reason, because it will return the cached response as `application/json`.  There's a reason it's called `apicache`.

