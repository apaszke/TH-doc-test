# THAllocator

There are two allocators provided by TH:
* THDefaultAllocator - allocates regular memory
* THMapAllocator - maps files and shared memory

They are singletons and can be used right after including  `THAllocator.h`.

## Allocator API

Allocators are actually virtual method tables with three function pointers (called `alloc`, `realloc` and `free`).
Their semantics are nearly the same to that of their stdlib counterparts. The
only difference is that THAllocator functions always take an additional context
as first argument. It's content can change the allocator behaviour.

If you want to use `alloc()` of `THDefaultAllocator` then just write
`THDefaultAllocator->alloc()`.

## THDefaultAllocator

Context is unused (can be NULL). Uses `THAlloc`, `THRealloc` and `THFree` (defined in [THGeneral](THGeneral.md)).

<h3 id="alloc">
  void* alloc(void *ctx, long size)
</h3>

Allocates a memory block with `size` bytes and returns a pointer to it.

<h3 id="realloc">
  void* realloc(void *ctx, void* ptr, long size)
</h3>

Attempts to resize block that `ptr` points to if `ptr` is not NULL and `size`
is greater than 0.

If `ptr` is NULL it's equivalent to `malloc(ctx, size)`.

If `size` is less or equal to 0 it's equivalent to `free(ptr)`.

<h3 id="free">
  void* free(void *ctx, void* ptr)
</h3>

Frees memory pointed to by `ptr`.

## THMapAllocator

<h3 id="alloc">
  void* alloc(void *ctx, long size)
</h3>

Maps a file to memory and returns a pointer to it. For possible behaviours
please see [`THMapAllocatorContext_new`](#THMapAllocatorContext_new)
documentation. Will error if it cannot allocate at least `size` bytes.

`ctx` should be a THMapAllocatorContext object.

<h3 id="realloc">
  void* realloc(void *ctx, void* ptr, long size)
</h3>

Mapped memory can't be reallocated.

**Should never be used.** Will always create an error.

<h3 id="free">
  void* free(void *ctx, void* ptr)
</h3>

Unmaps `filename`.

**Warning!** This will free the context object.

## THMapAllocatorContext

Provides additional information for THMapAllocator.

<h3 id="THMapAllocatorContext_new">
  THMapAllocatorContext *THMapAllocatorContext_new(<br/>
    &nbsp;&nbsp;&nbsp;&nbsp; const char *filename, int shared)
</h3>

Creates a new THMapAllocatorContext. It should be later freed with a call to
`THMapAllocatorContext_free`. `shared` can take one of the following values:

* **0** - Mappings will use a `filename` file in read-only mode. Any modifications **won't be saved to disk**!
* **TH_ALLOCATOR_MAPPED_SHARED** - Mappings will use a `filename` file, but writes to mapped memory are carried on to the underlying file.
* **TH_ALLOCATOR_MAPPED_SHAREDMEM** - Mappings will use a shared memory object called `filename`.

<h3>
  long THMapAllocatorContext_size(THMapAllocatorContext *ctx)
</h3>

Returns the size of a memory block that was mapped using `ctx` context.

<h3>
  void THMapAllocatorContext_free(THMapAllocatorContext *ctx)
</h3>

Frees the THMapAllocatorContext object.
