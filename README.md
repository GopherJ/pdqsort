# Pattern-defeating quicksort

[![Build Status](https://travis-ci.org/stjepang/pdqsort.svg?branch=master)](https://travis-ci.org/stjepang/pdqsort)
[![License](https://img.shields.io/badge/license-Apache--2.0%2FMIT-blue.svg)](https://github.com/stjepang/pdqsort)
[![Cargo](https://img.shields.io/crates/v/pdqsort.svg)](https://crates.io/crates/pdqsort)
[![Documentation](https://docs.rs/pdqsort/badge.svg)](https://docs.rs/pdqsort)

This sort is significantly faster than the standard sort in Rust. In particular, it sorts
random arrays of integers approximately 45% faster. The key drawback is that it is an unstable
sort (i.e. may reorder equal elements). However, in most cases stability doesn't matter anyway.

The algorithm is based on pdqsort by Orson Peters, published at: https://github.com/orlp/pdqsort

#### Now in nightly Rust

If you're using nightly Rust, you don't need this crate. The sort is as of recently
[implemented](https://github.com/rust-lang/rust/pull/40601) in libcore.

Use it like this:

```rust
#![feature(sort_unstable)]

let mut v = [-5, 4, 1, -3, 2];

v.sort_unstable();
assert!(v == [-5, -3, 1, 2, 4]);
```

See [The Unstable Book](https://manishearth.github.io/rust-internals-docs/unstable-book/sort-unstable.html)
for more information.

#### Properties

* Best-case running time is `O(n)`.
* Worst-case running time is `O(n log n)`.
* Unstable, i.e. may reorder equal elements.
* Does not allocate additional memory.
* Uses `#![no_std]`.

#### Examples

```rust
extern crate pdqsort;

let mut v = [-5i32, 4, 1, -3, 2];

pdqsort::sort(&mut v);
assert!(v == [-5, -3, 1, 2, 4]);

pdqsort::sort_by(&mut v, |a, b| b.cmp(a));
assert!(v == [4, 2, 1, -3, -5]);

pdqsort::sort_by_key(&mut v, |k| k.abs());
assert!(v == [1, 2, -3, 4, -5]);
```

#### A simple benchmark

Sorting 10 million random numbers of type `u64`:

| Sort              | Time       |
|-------------------|-----------:|
| pdqsort           | **370 ms** |
| slice::sort       |     668 ms |
| [quickersort][qs] |     777 ms |
| [rdxsort][rs]     |     602 ms |

[qs]: https://github.com/notriddle/quickersort
[rs]: https://github.com/crepererum/rdxsort-rs
