# stream-intersect

  intersections of sorted streams

## Installation

    npm install stream-intersect

## Example

```js
var cmp = require('typewise-cmp')
function max(a, b) { return 0 <= cmp(a, b) ? a : b }

function getKey(item) { return item.key }
function maxValue(a, b) { return max(a.value, b.value) }

var intersect = require('stream-intersect')
    .using({ toKey: getKey, cmp: cmp, merge: maxValue })

…
```

## API
### intersect(aStream :: ReadableStream, bStream :: ReadableStream) -> ReadableStream

  Intersect two streams.

### intersect.using({ toKey, cmp, merge }) -> function intersect(a, b) -> ReadableStream

  Customise the behaviour of `intersect`. Returns a customised `intersect` function.

#### toKey :: function(item) -> key

  the key function is applied to the items before comparison. it defaults to the identity function (a no-op).

#### cmp :: function(a, b) -> Number

  a comparison function.
  return `-1` for `a < b`, `1` for `a > b` and `0` for `a = b`.
  the default comparator considers values equal if they aren't less than or greater than each other.

#### merge :: function(a, b) -> item

  the merge function is called with every equivalent pair of input items, and its return value is output on the intersection stream.
  if it returns `undefined`, nothing will be output. if it returns `null`, the intersection stream ends.

