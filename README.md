# ES6 Shim
Provides compatibility shims so that legacy JavaScript engines behave as
closely as possible to ECMAScript 6 (Harmony).

[![Build Status][1]][2] [![dependency status][3]][4] [![dev dependency status][5]][6]

[![browser support](https://ci.testling.com/paulmillr/es6-shim.png)](https://ci.testling.com/paulmillr/es6-shim)

[![Sauce Test Status](https://saucelabs.com/browser-matrix/es6-shim.svg)](https://saucelabs.com/u/es6-shim)

[HTML version of the final ECMAScript 6 spec][spec-html-url]

## Installation
If you want to use it in browser:

* Just include es6-shim before your scripts.
* Include [es5-shim][es5-shim-url] especially if your browser doesn't support ECMAScript 5 - but every JS engine requires the `es5-shim` to correct broken implementations, so it's strongly recommended to always include it.

For `node.js`, `io.js`, or any `npm`-managed workflow (this is the recommended method):

    npm install es6-shim

Alternative methods:
* `component install paulmillr/es6-shim` if you’re using [component(1)](https://github.com/component/component).
* `bower install es6-shim` if you’re using [Bower](http://bower.io/).

In both browser and node you may also want to include `unorm`; see the [`String.prototype.normalize`](#stringprototypenormalize) section for details.

## Safe shims

* `Map`, `Set` (requires ES5 property descriptor support)
* `Promise`
* `String`:
    * `fromCodePoint()` ([a standalone shim is also available](http://mths.be/fromcodepoint))
    * `raw()`
* `String.prototype`:
    * `codePointAt()` ([a standalone shim is also available](http://mths.be/codepointat))
    * `endsWith()` ([a standalone shim is also available](http://mths.be/endswith))
    * `includes()` ([a standalone shim is also available](http://mths.be/includes))
    * `repeat()` ([a standalone shim is also available](http://mths.be/repeat))
    * `startsWith()` ([a standalone shim is also available](http://mths.be/startswith))
* `RegExp`:
    * `new RegExp`, when given a RegExp as the pattern, will no longer throw when given a "flags" string argument. (requires ES5)
* `RegExp.prototype`:
    * `flags` (requires ES5) ([a standalone shim is also available](https://github.com/es-shims/RegExp.prototype.flags))
* `Number`:
    * `EPSILON`
    * `MAX_SAFE_INTEGER`
    * `MIN_SAFE_INTEGER`
    * `isNaN()`([a standalone shim is also available](https://npmjs.org/package/is-nan))
    * `isInteger()`
    * `isSafeInteger()`
    * `isFinite()`
    * `parseInt()`
    * `parseFloat()`
* `Array`:
    * `from()` ([a standalone shim is also available](https://npmjs.org/package/array.from))
    * `of()` ([a standalone shim is also available](https://npmjs.org/package/array.of))
* `Array.prototype`:
    * `copyWithin()`
    * `entries()`
    * `fill()`
    * `find()` ([a standalone shim is also available](https://github.com/paulmillr/Array.prototype.find))
    * `findIndex()` ([a standalone shim is also available](https://github.com/paulmillr/Array.prototype.findIndex))
    * `keys()` (note: keys/values/entries return an `ArrayIterator` object)
    * `values()`
* `Object`:
    * `assign()` ([a standalone shim is also available](https://github.com/ljharb/object.assign))
    * `is()` ([a standalone shim is also available](https://github.com/ljharb/object-is))
    * `keys()` (in ES5, but no longer throws on non-object non-null/undefined values in ES6)
    * `setPrototypeOf()` (IE >= 11)
* `Math`:
    * `acosh()`
    * `asinh()`
    * `atanh()`
    * `cbrt()`
    * `clz32()`
    * `cosh()`
    * `expm1()`
    * `fround()`
    * `hypot()`
    * `imul()`
    * `log10()`
    * `log1p()`
    * `log2()`
    * `sign()`
    * `sinh()`
    * `tanh()`
    * `trunc()`

Math functions’ accuracy is 1e-11.

* `Reflect`
    * `apply()`
    * `construct()`
    * `defineProperty()`
    * `deleteProperty()`
    * `enumerate()`
    * `get()`
    * `getOwnPropertyDescriptor()`
    * `getPrototypeOf()`
    * `has()`
    * `isExtensible()`
    * `ownKeys()`
    * `preventExtensions()`
    * `set()`
    * `setPrototypeOf()`

* `String.prototype` Annex B HTML methods
These methods are part of "Annex B", which means that although they are a defacto standard, you shouldn't use them. None the less, the `es6-shim` provides them:
    * `anchor()`
    * `big()`
    * `blink()`
    * `bold()`
    * `fixed()`
    * `fontcolor()`
    * `fontsize()`
    * `italics()`
    * `link()`
    * `small()`
    * `strike()`
    * `sub()`
    * `sup()`

## Subclassing
The `Map`, `Set`, and `Promise` implementations are subclassable.
You should use the following pattern to create a subclass in ES5 which will continue to work in ES6:
```javascript
function MyPromise(exec) {
  var promise = new Promise(exec);
  Object.setPrototypeOf(promise, MyPromise.prototype);
  // ...
  return promise;
}
Object.setPrototypeOf(MyPromise, Promise);
MyPromise.prototype = Object.create(Promise.prototype, {
  constructor: { value: MyPromise }
});
```

## String.prototype.normalize
Including a proper shim for `String.prototype.normalize` would
increase the size of this library by a factor of more than 4.
So instead we recommend that you install the
[`unorm`](https://github.com/walling/unorm)
package alongside `es6-shim` if you need `String.prototype.normalize`.
See https://github.com/paulmillr/es6-shim/issues/134 for more
discussion.


## WeakMap shim
It is not possible to implement WeakMap in pure javascript.
The [es6-collections](https://github.com/WebReflection/es6-collections)
implementation doesn't hold values strongly, which is critical
for the collection. es6-shim decided to not include an incorrect shim.

WeakMap has a very unusual use-case so you probably won't need it at all
(use simple `Map` instead).

## Getting started

```javascript
'abc'.startsWith('a') // true
'abc'.endsWith('a') // false
'john alice'.includes('john') // true
'123'.repeat(2)     // '123123'

Object.is(NaN, NaN) // Fixes ===. 0 isnt -0, NaN is NaN
Object.assign({a: 1}, {b: 2}) // {a: 1, b: 2}

Number.isNaN('123') // false. Global isNaN('123') will give true.
Number.isFinite('asd') // false. Global isFinite() will give true.
// Tests if value is a number, finite,
// >= -9007199254740992 && <= 9007199254740992 and floor(value) === value
Number.isInteger(2.4) // false.

Math.sign(400) // 1, 0 or -1 depending on sign. In this case 1.

[5, 10, 15, 10].find(function (item) {return item / 2 === 5;}) // 10
[5, 10, 15, 10].findIndex(function (item) {return item / 2 === 5;}) // 1

// Replacement for `{}` key-value storage.
// Keys can be anything.
var map = new Map();
map.set('John', 25);
map.set('Alice', 400);
map.set(['meh'], 555);
assert(map.get(['meh']) === undefined); // undefined because you need to use exactly the same object.
map.delete('Alice');
map.keys();
map.values();
assert(map.size === 2);

// Useful for storing unique items.
var set = new Set();
set.add(1);
set.add(5);
assert(set.has(1) === true);
assert(set.has(4) === false);
set.delete(5);

// Promises, see
// http://www.slideshare.net/domenicdenicola/callbacks-promises-and-coroutines-oh-my-the-evolution-of-asynchronicity-in-javascript
// https://github.com/petkaantonov/bluebird/#what-are-promises-and-why-should-i-use-them
Promise.resolve(5).then(function (value) {
  if ( ... ) throw new Error("whoops!");
  // do some stuff
  return anotherPromise();
}).catch(function (e) {
  // any errors thrown asynchronously end up here
});
```

## [License][license-url]

[1]: https://travis-ci.org/paulmillr/es6-shim.svg
[2]: https://travis-ci.org/paulmillr/es6-shim
[3]: https://david-dm.org/paulmillr/es6-shim.svg
[4]: https://david-dm.org/paulmillr/es6-shim
[5]: https://david-dm.org/paulmillr/es6-shim/dev-status.svg
[6]: https://david-dm.org/paulmillr/es6-shim#info=devDependencies
[license-url]: https://github.com/paulmillr/es6-shim/blob/master/LICENSE
[spec-html-url]: http://www.ecma-international.org/ecma-262/6.0/
[es5-shim-url]: https://github.com/es-shims/es5-shim
