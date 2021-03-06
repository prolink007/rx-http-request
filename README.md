# Rx-Http-Request

<div style="overflow:hidden;margin-bottom:20px;">
<div style="float:left;line-height:60px">
    <a href="https://travis-ci.org/njl07/rx-http-request.svg?branch=master">
        <img src="https://travis-ci.org/njl07/rx-http-request.svg?branch=master" alt="build" />
    </a>
    <a href="https://coveralls.io/github/njl07/rx-http-request?branch=master">
        <img src="https://coveralls.io/repos/github/njl07/rx-http-request/badge.svg?branch=master" alt="coveralls" />
    </a>
    <a href="https://david-dm.org/njl07/rx-http-request">
        <img src="https://david-dm.org/njl07/rx-http-request.svg" alt="dependencies" />
    </a>
    <a href="https://david-dm.org/njl07/rx-http-request?type=dev">
        <img src="https://david-dm.org/njl07/rx-http-request/dev-status.svg" alt="devDependencies" />
    </a>
</div>
<div style="float:right;">
    <a href="https://www.typescriptlang.org/docs/tutorial.html">
        <img src="https://cdn-images-1.medium.com/max/800/1*8lKzkDJVWuVbqumysxMRYw.png"
             align="right" alt="Typescript logo" width="50" height="50"/>
    </a>
    <a href="http://reactivex.io/rxjs">
        <img src="http://reactivex.io/assets/Rx_Logo_S.png"
             align="right" alt="ReactiveX logo" width="50" height="50"/>
    </a>
</div>
</div>

The world-famous HTTP client [Request](https://github.com/request/request) now [RxJS](http://reactivex.io/rxjs) compliant, wrote in full [Typescript](https://www.typescriptlang.org/docs/tutorial.html) | [ES6](https://babeljs.io/docs/learn-es2015/) for client and server side.

## Table of contents

* [Installation](#installation)
* [Super simple to use](#super-simple-to-use)
* [Browser compatibility](#browser-compatibility)
* [API in Detail](#api-in-detail)
    * [.request](#request)
    * [.defaults(options)](#defaultsoptions)
    * [.get(uri[,options])](#geturi-options)
        * [Crawl a webpage](#crawl-a-webpage)
        * [GET something from a JSON REST API](#get-something-from-a-json-rest-api)
    * [.post(uri[,options])](#posturi-options)
        * [POST data to a JSON REST API](#post-data-to-a-json-rest-api)
        * [POST like HTML forms do](#post-like-html-forms-do)
    * [.put(uri[,options])](#puturi-options)
    * [.patch(uri[,options])](#patchuri-options)
    * [.delete(uri[,options])](#deleteuri-options)
    * [.head(uri[,options])](#headuri-options)
    * [.jar()](#jar)
    * [.cookie(str)](#cookiestr)
* [Contributing](#contributing)
* [Change History](#change-history)
* [License](#license)

## Installation

```sh
$ npm install @akanass/rx-http-request
```

## Super simple to use

**Rx-Http-Request** is designed to be the simplest way possible to make http calls.

It's fully `Typescript` | `ES6` wrotten so you can import it :

```javascript
import {RxHR} from "@akanass/rx-http-request";
```

or use `CommonJS`:

```javascript
const RxHR = require('@akanass/rx-http-request').RxHR;
```

Now, it's easy to perform a `HTTP` request:

```javascript
RxHR.get('http://www.google.fr').subscribe(
    (data) => {

        if (data.response.statusCode === 200) {
            console.log(data.body); // Show the HTML for the Google homepage.
        }
    },
    (err) => console.error(err) // Show error in console
);
```

## Browser compatibility

**Rx-Http-Request** can be used in your favorite browser to have all features in your own front application.

Just import `browser.js` script and enjoy:

```javascript
<script src="node_modules/@akanass/rx-http-request/browser.js" type="application/javascript"></script>
<script type="application/javascript">
    const RxHR = rhr.RxHR;
    
    RxHR.get('http://www.google.fr').subscribe(
        function(data){
    
            if (data.response.statusCode === 200) {
                console.log(data.body); // Show the HTML for the Google homepage.
            }
        },
        function(err){
            console.error(err) // Show error in console
        }
    );
</script>
```

## API in Detail

**Rx-Http-Request** uses [Request](https://github.com/request/request) **API** to perform calls and returns [RxJS.Observable](http://reactivex.io/rxjs/class/es6/Observable.js~Observable.html).

All **options** to pass to **API** **methods** can be found [here](https://github.com/request/request#requestoptions-callback).

All **methods** to execute on **response object** can be found [here](https://github.com/Reactive-Extensions/RxJS/blob/master/doc/api/core/observable.md#observable-instance-methods).

--------


### `.request`

Returns the original [Request](https://github.com/request/request#requestoptions-callback) **API** to perform calls without `RxJS.Observable` response but with a **callback method**.

```javascript
import {RxHR} from '@akanass/rx-http-request';

RxHR.request({uri: 'http://www.google.fr'}, (error, response, body) => {

    if (!error && response.statusCode == 200) {
        console.log(body); // Show the HTML for the Google homepage.
    }
});
```

[Back to top](#table-of-contents)

### `.defaults(options)`

This method **returns a wrapper** around the normal **Rx-Http-Request API**  that defaults to whatever options you pass to it.

**Parameters:**
> ***options*** *(required): Original [Request](https://github.com/request/request#requestoptions-callback) `options` object with default values foreach next requests*

**Response:**
> ***new*** *`RxHttpRequest` instance*

**Note:** `RxHR.defaults()` **does not** modify the global API; instead, it returns a wrapper that has your default settings applied to it.

**Note:** You can call `.defaults()` on the wrapper that is returned from `RxHR.defaults()` to add/override defaults that were previously defaulted.

For example:
 
```javascript
// requests using baseRequest will set the 'x-token' header
const baseRequest = RxHR.defaults({
    headers: {'x-token': 'my-token'}
});

// requests using specialRequest will include the 'x-token' header set in
// baseRequest and will also include the 'special' header
const specialRequest = baseRequest.defaults({
    headers: {special: 'special value'}
});
```

[Back to top](#table-of-contents)

### `.get(uri[, options])`

Performs a request with `get` http method.

**Parameters:**
> - ***uri*** *(required): The `uri` where request will be performed*
> - ***options*** *(optional): Original [Request](https://github.com/request/request#requestoptions-callback) `options` object*

**Response:**
> *[RxJS.Observable](https://github.com/Reactive-Extensions/RxJS/blob/master/doc/api/core/observable.md) instance*

#### Crawl a webpage

```javascript
import {RxHR} from '@akanass/rx-http-request';

RxHR.get('http://www.google.fr').subscribe(
    (data) => {

        if (data.response.statusCode === 200) {
            console.log(data.body); // Show the HTML for the Google homepage.
        }
    },
    (err) => console.error(err) // Show error in console
);
```

#### GET something from a JSON REST API
     
```javascript
import {RxHR} from '@akanass/rx-http-request';

const options = {
    qs: {
        access_token: 'xxxxx xxxxx' // -> uri + '?access_token=xxxxx%20xxxxx'
    },
    headers: {
        'User-Agent': 'Rx-Http-Request'
    },
    json: true // Automatically parses the JSON string in the response
};

RxHR.get('https://api.github.com/user/repos', options).subscribe(
    (data) => {

        if (data.response.statusCode === 200) {
            console.log(data.body); // Show the JSON response object.
        }
    },
    (err) => console.error(err) // Show error in console
);
```

[Back to top](#table-of-contents)

### `.post(uri[, options])`

Performs a request with `post` http method.

**Parameters:**
> - ***uri*** *(required): The `uri` where request will be performed*
> - ***options*** *(optional): Original [Request](https://github.com/request/request#requestoptions-callback) `options` object*

**Response:**
> *[RxJS.Observable](https://github.com/Reactive-Extensions/RxJS/blob/master/doc/api/core/observable.md) instance*

#### POST data to a JSON REST API

```javascript
import {RxHR} from '@akanass/rx-http-request';

const options = {
    body: {
        some: 'payload'
    },
    json: true // Automatically stringifies the body to JSON
};

RxHR.post('http://posttestserver.com/posts', options).subscribe(
    (data) => {

        if (data.response.statusCode === 201) {
            console.log(data.body); // Show the JSON response object.
        }
    },
    (err) => console.error(err) // Show error in console
);
```

#### POST like HTML forms do

```javascript
import {RxHR} from '@akanass/rx-http-request';

const options = {
    form: {
        some: 'payload' // Will be urlencoded
    },
    headers: {
        /* 'content-type': 'application/x-www-form-urlencoded' */ // Set automatically
    }
};

RxHR.post('http://posttestserver.com/posts', options).subscribe(
    (data) => {

        if (data.response.statusCode === 201) {
            console.log(data.body); // POST succeeded...
        }
    },
    (err) => console.error(err) // Show error in console
);
```

[Back to top](#table-of-contents)

### `.put(uri[, options])`

Performs a request with `put` http method.

**Parameters:**
> - ***uri*** *(required): The `uri` where request will be performed*
> - ***options*** *(optional): Original [Request](https://github.com/request/request#requestoptions-callback) `options` object*

**Response:**
> *[RxJS.Observable](https://github.com/Reactive-Extensions/RxJS/blob/master/doc/api/core/observable.md) instance*

```javascript
import {RxHR} from '@akanass/rx-http-request';

RxHR.put(uri).subscribe(...);
```

[Back to top](#table-of-contents)

### `.patch(uri[, options])`

Performs a request with `patch` http method.

**Parameters:**
> - ***uri*** *(required): The `uri` where request will be performed*
> - ***options*** *(optional): Original [Request](https://github.com/request/request#requestoptions-callback) `options` object*

**Response:**
> *[RxJS.Observable](https://github.com/Reactive-Extensions/RxJS/blob/master/doc/api/core/observable.md) instance*

```javascript
import {RxHR} from '@akanass/rx-http-request';

RxHR.patch(uri).subscribe(...);
```

[Back to top](#table-of-contents)

### `.delete(uri[, options])`

Performs a request with `delete` http method.

**Parameters:**
> - ***uri*** *(required): The `uri` where request will be performed*
> - ***options*** *(optional): Original [Request](https://github.com/request/request#requestoptions-callback) `options` object*

**Response:**
> *[RxJS.Observable](https://github.com/Reactive-Extensions/RxJS/blob/master/doc/api/core/observable.md) instance*

```javascript
import {RxHR} from '@akanass/rx-http-request';

RxHR.delete(uri).subscribe(...);
```

[Back to top](#table-of-contents)

### `.head(uri[, options])`

**Parameters:**
> - ***uri*** *(required): The `uri` where request will be performed*
> - ***options*** *(optional): Original [Request](https://github.com/request/request#requestoptions-callback) `options` object*

**Response:**
> *[RxJS.Observable](https://github.com/Reactive-Extensions/RxJS/blob/master/doc/api/core/observable.md) instance*

Performs a request with `head` http method.

```javascript
import {RxHR} from '@akanass/rx-http-request';

RxHR.head(uri).subscribe(...);
```

[Back to top](#table-of-contents)

### `.jar()`

Creates a new `RxCookieJar` instance

**Response:**
> *[RxJS.Observable](https://github.com/Reactive-Extensions/RxJS/blob/master/doc/api/core/observable.md) instance*


```javascript
import {RxHR} from '@akanass/rx-http-request';

RxHR.jar().subscribe(...);
```

[Back to top](#table-of-contents)

### `.cookie(str)`

Creates a new cookie

**Parameters:**
> - ***str*** *(required): The `string` representation of the cookie*

**Response:**
> *[RxJS.Observable](https://github.com/Reactive-Extensions/RxJS/blob/master/doc/api/core/observable.md) instance*

 
```javascript
import {RxHR} from '@akanass/rx-http-request';

RxHR.cookie('key1=value1').subscribe(...);
```

[Back to top](#table-of-contents)

## Contributing

To set up your development environment:

1. clone the repo to your workspace,
2. in the shell `cd` to the main folder,
3. hit `npm or yarn install`,
4. run `npm or yarn run test`.
    * It will lint the code and execute all tests. 
    * The test coverage report can be viewed from `./coverage/lcov-report/index.html`.

[Back to top](#table-of-contents)

## Change History

* v2.1.0 (2017-03-09)
    * Upgrade `Request` version to `v2.80.0`
* v2.0.0 (2017-02-28)
    * New package version for `RxJS` and `Request`
    * Don't import all of `RxJS`, only `Observable`
    * Rewritten all **library and test files in `Typescript`**
    * Add `typings` support
    * Add **scope** to library and move to **`@akanass/rx-http-request`**
* v1.2.0 (2016-09-29)
    * New package version for `RxJS` and `Request`
    * New `ES6` features
    * [Issue 1](https://github.com/njl07/rx-http-request/issues/1)
    * [Issue 2](https://github.com/njl07/rx-http-request/issues/2)
* v1.1.0 (2016-03-28)
    * Browserify to have browser compatibility
* v1.0.0 (2016-03-27)
    * Carefully rewritten from scratch to make Rx-Http-Request a drop-in replacement for Request
    
[Back to top](#table-of-contents)

## License

Copyright (c) 2017 **Nicolas Jessel** Licensed under the [MIT license](https://github.com/njl07/rx-http-request/tree/master/LICENSE.md).

[Back to top](#table-of-contents)
