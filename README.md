# http-client.js

* <a href='#introduction'>Introduction</a>
* <a href='#features'>Features</a>
* <a href='#examples'>Examples</a>
* <a href='#install'>Install</a>
* <a href='#license'>License</a>

## <a id='introduction'>Introduction</a>

A promise-based API around `XMLHttpRequest`.  

## <a id='features'>Features</a>

* Supports making requests using all available HTTP methods.

* Supports adding HTTP header(s) to a request.

* Supports adding a request body.

* Supports request time outs.

* Supports providing query parameters as a object literal.

* **light**: `dist/http-client.min.js` is transpiled ES5 and weighs around 2.3kb.   
  There's no dependencies besides what's provided by the browser.

## window.fetch or http-client.js ?

Probably `window.fetch`.

This library was written before it existed, and for most cases it's the better
option because it is built into the browser. If you're curious about an
alternative API built on `XMLHttpRequest` and Promise's though, read on.

## <a id='examples'>Examples</a>

**#1**

This example makes a GET request to `/blog?id=10` with the Accept header 
set to `application/json`.

```javascript
import HttpClient from 'http-client.js';
new HttpClient()
    .get('/blog', {params: {id: 10}, headers: {'Accept': 'application/json'}})
    .then((xhr) => console.log(xhr))
    .catch((xhr) => console.log(xhr));
```

**#2**

The reason a request failed can be found at `xhr.httpClient.cause` and it
returns one of the following strings: `abort`, `timeout`, `error`, `status`:

```javascript
new HttpClient().get('/index.html').catch((xhr) => {
  switch(xhr.httpClient.cause) {
  case 'abort':
    return console.log('request aborted')
  case 'timeout':
    return console.log('request timed out')
  case 'error':
    return console.log('network error (including cross-origin errors)')
  case 'status':
    return console.log(`response status code is ${xhr.status}, not 2XX`)
  default:
    throw new Error("Shouldn't happen");
  }
});
```

**#3**

An instance of `HttpClient` can operate under a timeout (measured in ms)
by providing a `timeout` option as this example shows:

```javascript
import HttpClient from 'http-client.js';
const client = new HttpClient(location.origin, {timeout: 1000});
client.get('/1.html').then(..).catch(..);
client.get('/2.html').then(..).catch(..);
```

The timeout can be overridden on a per-request basis by passing a timeout option
to a verb method such as `get`:

```javascript
import HttpClient from 'http-client.js';
const client = new HttpClient(location.origin, {timeout: 500});
client.get('/1.html').then(..).catch(..);
client.get('/veryslow.html', {timeout: 5000}).then(..).catch(..);
```

## <a id='install'>Install</a>

__NPM environment__

If you're in a NPM environment, there's an NPM package to use.  
The package should be required or imported as `@rg-3/http-client.js`.

    # npm users
    $ npm i --save @rg-3/http-client.js

    # yarn users
    $ yarn add @rg-3/http-client.js

__Old school method__

If you're in a browser environment without NPM, you can save [dist/http-client.min.js](https://github.com/rg-3/http-client.js/blob/master/dist/http-client.min.js) to your project and link to it from a `<script>` tag. It has been transpiled to ES5,
and adds `window.HttpClient`.


## <a id='license'>License</a>

This project uses the MIT license, see [LICENSE.txt](./LICENSE.txt) for details.
