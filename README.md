# jsdom-global

> Enables DOM in Node.js

jsdom-global will inject `document`, `window` and other DOM API into your Node.js environment. Useful for tests.

[![Status](https://travis-ci.org/rstacruz/jsdom-global.svg?branch=master)](https://travis-ci.org/rstacruz/jsdom-global "See test builds")

## Install

Requires [jsdom][].

```
npm install --save-dev --save-exact jsdom jsdom-global
```

[jsdom]: https://github.com/tmpvar/jsdom

## Usage

Just invoke it to turn your Node.js environment into a DOM environment.

```js
var jsdom = require('jsdom-global')
jsdom()

// you can now use the DOM
document.body.innerHTML = 'hello'
```

To clean up after itself, just invoke the function it returns.

```js
var jsdom = require('jsdom-global')
var cleanup = jsdom()

// do things

cleanup()
```

You can also invoke it with a function block so it'll clean up afterwards.

```js
jsdom(function () {
  var $ = require('jquery')
  $('body').html('hello')
})
```

## Tape

In [tape][], run it before your other tests.

```js
var test = require('tape')

// example: run your tests with `env JSDOM=1` to enable jsdom in an otherwise
// browser-only test
if (process.env.JSDOM) {
  test('jsdom', (t) => {
    require('jsdom-global')()
    t.pass('jsdom enabled')
    t.end()
  })
}
```

Or even use the tape helper, which will accomplish the same thing:

```js
var test = require('tape')
test('jsdom', require('jsdom-global/tape')())
```

## Mocha

Just add it to [mocha]'s `before` and `after` hooks.

```js
before(function () {
  this.jsdom = require('jsdom-global')()
})

after(function () {
  this.jsdom()
})
```

[tape]: https://github.com/substack/tape
[mocha]: https://mochajs.org/

## Browserify

If you use Browserify on your tests (eg: [tape-run], [budo], [hihat], [zuul], and so on), doing `require('jsdom-global')()` is a noop. In practice, this means you can use jsdom-global even if your tests are powered by browserify, and your test will now work in both the browser and Node.

[zuul]: https://www.npmjs.com/package/zuul
[tape-run]: https://www.npmjs.com/package/tape-run
[budo]: https://github.com/mattdesl/budo
[hihat]: https://www.npmjs.com/package/hihat

```js
// test.js - use jsdom-global
require('jsdom-global')()

// do your tests here
```

```sh
browserify test.js | tape-run   # run in a browser
browserify test.js | node       # run in node
```

## Thanks

**jsdom-global** © 2016+, Rico Sta. Cruz. Released under the [MIT] License.<br>
Authored and maintained by Rico Sta. Cruz with help from contributors ([list][contributors]).

> [ricostacruz.com](http://ricostacruz.com) &nbsp;&middot;&nbsp;
> GitHub [@rstacruz](https://github.com/rstacruz) &nbsp;&middot;&nbsp;
> Twitter [@rstacruz](https://twitter.com/rstacruz)

[MIT]: http://mit-license.org/
[contributors]: http://github.com/rstacruz/jsdom-global/contributors
