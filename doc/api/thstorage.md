# THStorage

This is a generic class. It is available for use with following types:

| Type   | Class name      |
|--------|-----------------|
| byte   | THByteStorage   |
| char   | THCharStorage   |
| short  | THShortStorage  |
| int    | THIntStorage    |
| long   | THLongStorage   |
| float  | THFloatStorage  |
| double | THDoubleStorage |

## Constructors

<h3 id="THStorage_new">
  THStorage* THStorage_new()
</h3>

Creates and returns an empty THStorage object.

<h3 id="THStorage_newWithSize">
  THStorage* THStorage_newWithSize(long size)
</h3>

Creates and returns a THStorage object with capacity `size`.

<h3 id="THStorage_newWithSize1">
  THStorage* THStorage_newWithSize1(real data0)
</h3>

Creates a THStorage object with capacity for one `real` variable and assigns
`data0` to it.

<h3 id="THStorage_newWithSize2">
  THStorage* THStorage_newWithSize2(real data0, real data1)
</h3>

Creates a THStorage object with capacity for two `real` variables and fills it with arguments.

<h3 id="THStorage_newWithSize3">
  THStorage* THStorage_newWithSize3(real data0,<br/>
    &nbsp;&nbsp;&nbsp;&nbsp; real data1, real data2)
</h3>

Creates a THStorage object with capacity for three `real` variables and fills it with arguments.

<h3 id="THStorage_newWithSize4">
  THStorage* THStorage_newWithSize4(real data0,<br/>
    &nbsp;&nbsp;&nbsp;&nbsp; real data1, real data2, real data3)
</h3>

Creates a THStorage object with capacity for four `real` variables and fills it with arguments.

<h3 id="THStorage_newWithData">
  THStorage* THStorage_newWithData(real* data, long size)
</h3>

Creates a new THStorage object that uses given data.

This object takes ownership of the data - it will be freed when refcount will
equal 0. It can be changed by clearing **TH_STORAGE_FREEMEM** flag.

<h3 id="THStorage_newWithDataAndAllocator">
  THStorage* THStorage_newWithDataAndAllocator(<br/>
    &nbsp;&nbsp;&nbsp;&nbsp; real* data, long size,<br/>
    &nbsp;&nbsp;&nbsp;&nbsp; THAllocator* allocator,<br/>
    &nbsp;&nbsp;&nbsp;&nbsp; void* allocatorContext)
</h3>

Creates a new THStorage that uses given data (it should be an array of length
`size`). When resizing or freeing this object appropriate `allocator` methods
will be used.

This object takes ownership of the data - it will be freed when refcount will
equal 0. It can be changed by clearing **TH_STORAGE_FREEMEM** flag.

<h3 id="THStorage_newWithAllocator">
  THStorage* THStorage_newWithAllocator(<br/>
    &nbsp;&nbsp;&nbsp;&nbsp; long size<br/>
    &nbsp;&nbsp;&nbsp;&nbsp; THAllocator *allocator,<br/>
    &nbsp;&nbsp;&nbsp;&nbsp; void *allocatorContext)
</h3>

Uses an allocator to create a THStorage object with
capacity `size`.

<h3 id="THStorage_newWithMapping">
  THStorage* THStorage_newWithMapping(<br/>
    &nbsp;&nbsp;&nbsp;&nbsp; const char *filename,<br/>
    &nbsp;&nbsp;&nbsp;&nbsp; long size,<br/>
    &nbsp;&nbsp;&nbsp;&nbsp; int shared)
</h3>

Maps `filename` to memory (using THMapAllocator), checks if it's at least `size` bytes, and uses it's
content as new THStorage data. Size smaller or equal to 0 means that all
available space in `filename` will be used as data storage.

Please not that this method can be used not only to map files, but shared
memory too.

For possible `shared` values please refer to [THAllocator](/thallocator.md)
documentation.

## Element access

<h3 id="THStorage_set">
  void THStorage_set(THStorage *storage, long idx, real value);
</h3>

Sets value at index `idx` to `value`.

This function checks if `idx` is valid.

<h3 id="THStorage_get">
  real THStorage_get(const THStorage *storage, long idx);
</h3>

Returns value from index `idx`.

This function checks if `idx` is valid.

<h3 id="THStorage_fill">
  void THStorage_fill(THStorage *storage, real value)
</h3>

Fills the storage with `value`.


<h3 id="THStorage_resize">
  void THStorage_resize(THStorage *storage, long size)
</h3>

Attempts to resize storage to `size`. Will result in an error if
**TH_STORAGE_RESIZABLE** flag is clear.


## Memory management

<h3 id="THStorage_retain">
  void THStorage_retain(THStorage *storage)
</h3>

Increments refcount if given THStorage is refcounted.

<h3 id="THStorage_free">
  void THStorage_free(THStorage *storage)
</h3>

Decrements refcount if given THStorage is refcounted.

If it equals 0 after this operation, then the appropriate free method is
called.

## Getter methods

<h3 id="THStorage_data">
  real* THStorage_data(const THStorage *storage)
</h3>

Returns a pointer to given THStorage data.

<h3 id="THStorage_size">
  long THStorage_size(const THStorage *storage)
</h3>

Returns capacity of given THStorage object.

<h3 id="THStorage_elementSize">
  int THStorage_elementSize()
</h3>

Returns size of one THStorage element in bytes.

## Flags

THStorage objects contain a flags field with a bit-mask. There are four flags available:
* **TH_STORAGE_REFCOUNTED** - Object is refcounted
* **TH_STORAGE_RESIZABLE** - Object can be resized
* **TH_STORAGE_FREEMEM** - When this object will be freed, then the data that it points to will be freed as well
* **TH_STORAGE_VIEW** - This object is only a view onto another THStorage

<h3 id="THStorage_setFlag">
  void THStorage_setFlag(THStorage *storage,<br/>
    &nbsp;&nbsp;&nbsp;&nbsp; const char flag)
</h3>

Sets given flag in a THStorage object.

<h3 id="">
  void THStorage_clearFlag(THStorage *storage,<br/>
    &nbsp;&nbsp;&nbsp;&nbsp; const char flag)
</h3>

Clears given flag in a THStorage object.
