# TH Library

This library is a fundamental building block of Torch. It provides an
implementation for basic data structures (e.g. Tensor), that is later wrapped
with Lua for convenience.

## Directory structure
TH keeps code in two directories: in it's root and in `generic`. The reason why
it's organized this way is simple - C has no generic types or templates and it
would be a terrible practice to repeat the same implementation over and over
with only a couple of types changed. Code contained in `generic` will be
regenerated with different types during compilation. There are two types that
will be replaced:
* real - a main type
* accreal - a type that can accumulate a number of `real` variables (e.g. if
  real is `int`, then `accreal` should be `long`)

#### How it works?

If you open up `THStorage.c` from the library root you will see this repeated:
```C
#include "generic/<file>.c"
#include "THGenerateAllTypes.h"
```

THGenerateAllTypes detects that it was defined after a generic file and uses
several snippets like this (with different definitions of real and accreal):

```C
#define real char
#define accreal long
#define Real Char
#define TH_REAL_IS_CHAR
#line 1 TH_GENERIC_FILE
#include TH_GENERIC_FILE
#undef real
#undef accreal
#undef Real
#undef TH_REAL_IS_CHAR
```

to generate implementations of a generic file with different types.

#### How can I use it in my files?

It's easy.
Write your generic implementation using `real` and `accreal` in places where
the generated types should go and put it in `generic` directory. Remember to
place this snippet in the file (replace FILENAME with actual name):
```
#ifndef TH_GENERIC_FILE
#define TH_GENERIC_FILE "generic/FILENAME"
#else
...
#endif
```
Then, create a file in the root folder and include all the generic interleaving
it with one of the three headers:

* `THGenerateFloatTypes.h` - generates implementation for float and double
* `THGenerateIntTypes.h` - generates implementation for integer types of
                           sizes from `char` to `long`
* `THGenerateAllTypes.h` - generates implementation for all of the above
                           types
