# TH Library

This library is a fundamental building block of Torch. It provides an
implementation for basic data structures (e.g. Tensor), that is later wrapped
with Lua for convenience.

## Generic classes
Many TH classes come with support for different types. Because C doesn't support
function overloading and templates they need to have different names. The
convention is simple - generic types are marked as `real` and their respective
class names contain type name right after TH with the first letter capitalized.
`accreal` is a type that can accumulate multiple `real` values.

So `THTensor` can be used as `THFloatTensor` or `THIntTensor` for example.

If the documentation page describes a generic class it will mark it in the very
beginning, and list all available implementations, but will use class names
without types and `real` and `accreal` as argument types.

## API Docs
* [THTensor](/api/thtensor.md)
* [THStorage](/api/thstorage.md)

## Developer documentation
* [Generic files implementation](/developer/generic.md)
