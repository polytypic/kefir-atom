[![npm version](https://badge.fury.io/js/kefir.atom.svg)](http://badge.fury.io/js/kefir.atom) [![Build Status](https://travis-ci.org/calmm-js/kefir.atom.svg?branch=master)](https://travis-ci.org/calmm-js/kefir.atom) [![](https://david-dm.org/calmm-js/kefir.atom.svg)](https://david-dm.org/calmm-js/kefir.atom) [![](https://david-dm.org/calmm-js/kefir.atom/dev-status.svg)](https://david-dm.org/calmm-js/kefir.atom#info=devDependencies) [![Gitter](https://img.shields.io/gitter/room/calmm-js/chat.js.svg?style=flat-square)](https://gitter.im/calmm-js/chat)

Modifiable models for [Kefir](http://rpominov.github.io/kefir/).

Note: This implementation is **optimized for space** at a fairly low level.  The
good news is that you can **use atoms and lenses with impunity**.  The bad news
is that the implementation is tightly bound to the internals of Kefir.  Should
the internals change, this library will need to be updated as well.

## Reference

```js
import Atom from "kefir.atom"
```

### [`Atom(initialValue)`](#atominitialvalue "Atom :: a -> Atom a")

Creates a new atom with the given initial value.  An atom is a modifiable Kefir
[property](http://rpominov.github.io/kefir/#about-observables).  Atoms (and
lensed atoms) implicitly skip duplicates using Ramda's
[equals](http://ramdajs.com/0.20.0/docs/#equals) function.

### [`atom.get()`](#atomget "get :: Atom a -> a")

A slow operation to synchronously get the current value of the atom.  Use of
`get` is discouraged: prefer to depend on an atom as you would with ordinary
Kefir properties.

### [`atom.lens(l, ...ls)`](#atomlensl-ls "lens :: Atom a -> (PLens a a1, ...PLens aN b) -> LensedAtom b")

Creates a new lensed atom with the given path from the original atom.
Modifications to the lensed atom are reflected in the original atom and vice
versa.

The lens can be any Ramda compatible
[lens](http://ramdajs.com/0.20.0/docs/#lens), but the lenses on the given path
are implicitly composed and lifted as
[partial lenses](https://github.com/calmm-js/partial.lenses/).

### [`atom.modify(currentValue => newValue)`](#atommodifycurrentvalue--newvalue "modify :: Atom a -> (a -> a) -> ()")

Applies the given function to the current value of the atom and replaces the
value of the atom with the new value returned by the function.

### [`atom.set(value)`](#atomsetvalue "set :: Atom a -> a -> ()")

`atom.set(value)` is equivalent to `atom.modify(() => value)` and is provided
for convenience.

### [`atom.view(l, ...ls)`](#atomviewl-ls "view :: Atom a -> (PLens a a1, ...Plens aN b) -> Property b")

Creates a new view with the given path from the original atom.  Changes to the
original atom are reflected in the view.
