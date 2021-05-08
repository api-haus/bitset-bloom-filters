# BitSet-Bloom-Filters
[![Build Status](https://travis-ci.com/yurihq/bitset-bloom-filters.svg?branch=master)](https://travis-ci.com/yurihq/bitset-bloom-filters)

This is a fork of [Callidon/bloom-filters](https://github.com/Callidon/bloom-filters) using [lemire/FastBitSet.js](https://github.com/lemire/FastBitSet.js/) as a data structure.
Original project uses plain JS arrays filled with numbers which could be quite heavy on memory.

---

JavaScript/TypeScript implementation of probabilistic data structures: Bloom Filter (and its derived), HyperLogLog, Count-Min Sketch, Top-K and MinHash.
**This package rely on [non-cryptographic hash functions](https://cyan4973.github.io/xxHash/)**.

📕[Online documentation](https://yurihq.github.io/bitset-bloom-filters/)

**Keywords:** *bloom filter, cuckoo filter, KyperLogLog, MinHash, Top-K, probabilistic data-structures.*

# Table of contents

* [Installation](#installation)
* [Data structures](#data-structures)
	* [Classic Bloom Filter](#classic-bloom-filter)
* [Documentation](#documentation)
* [Tests](#tests)
* [References](#references)
* [Changelog](#changelog)
* [License](#license)

## Installation

```bash
npm install bitset-bloom-filters --save
```

**Supported platforms**
* [Node.js](https://nodejs.org): *v4.0.0* or higher
* [Google Chrome](https://www.google.com/intl/en/chrome/): *v41* or higher
* [Mozilla Firefox](https://www.mozilla.org/en-US/firefox/new/): *v34* or higher
* [Microsoft Edge](https://www.microsoft.com/en-US/edge): *v12* or higher

## Data structures

### Classic Bloom Filter

A Bloom filter is a space-efficient probabilistic data structure, conceived by Burton Howard Bloom in 1970,
that is used to test whether an element is a member of a set. False positive matches are possible, but false negatives are not.

**Reference:** Bloom, B. H. (1970). *Space/time trade-offs in hash coding with allowable errors*. Communications of the ACM, 13(7), 422-426.
([Full text article](http://crystal.uta.edu/~mcguigan/cse6350/papers/Bloom.pdf))

#### Methods

* `add(element: string) -> void`: add an element into the filter.
* `has(element: string) -> boolean`: Test an element for membership, returning False if the element is definitively not in the filter and True is the element might be in the filter.
* `equals(other: BloomFilter) -> boolean`: Test if two filters are equals.
* `rate() -> number`: compute the filter's false positive rate (or error rate).

```javascript
const { BloomFilter } = require('bloom-filters')
// create a Bloom Filter with a size of 10 and 4 hash functions
let filter = new BloomFilter(10, 4)
// insert data
filter.add('alice')
filter.add('bob')

// lookup for some data
console.log(filter.has('bob')) // output: true
console.log(filter.has('daniel')) // output: false

// print the error rate
console.log(filter.rate())

// alternatively, create a bloom filter optimal for a number of items and a desired error rate
const items = ['alice', 'bob']
const errorRate = 0.04 // 4 % error rate
filter = BloomFilter.create(items.length, errorRate)

// or create a bloom filter optimal for a collections of items and a desired error rate
filter = BloomFilter.from(items, errorRate)
```

## Every hash function is seeded

By default every hash function is seeded with an internal seed which is equal to `0x1234567890`. If you want to change it:

```javascript
const { BloomFilter } = require('bloom-filter')
const bl = new BloomFilter(...)
console.log(bl.seed) // 78187493520
bl.seed = 0xABCD
console.log(bl.seed) // 43981
```

## Documentation

See [documentation online](https://yurihq.github.io/bitset-bloom-filters/) or generate it in directory `doc/` with: `npm run doc`

## Tests

Running with Mocha + Chai
```bash
# run tests
npm test
```

## References

* [Classic Bloom Filter](http://crystal.uta.edu/~mcguigan/cse6350/papers/Bloom.pdf): Bloom, B. H. (1970). *Space/time trade-offs in hash coding with allowable errors.* Communications of the ACM, 13(7), 422-426.

## Changelog

| **Version** | **Release date** | **Major changes** |
|---|---|---|
| `v0.1.0` | 08/05/2021 | Classic only implementation with FastBitSet |

## License
[MIT License](https://github.com/yurihq/bitset-bloom-filters/blob/master/LICENSE)
