# THStorage

This is a generic class. It is available for use with the following types:

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

### THStorage\* THStorage_new()

Creates and returns an empty THStorage object.

### THStorage\* THStorage_newWithSize(long size)

Creates and returns a THStorage object with capacity `size`.

<h3>
THStorage* THStorage_newWithAllocator(<br/>
&nbsp;&nbsp;&nbsp;&nbsp; long size<br/>
&nbsp;&nbsp;&nbsp;&nbsp; THAllocator *allocator,<br/>
&nbsp;&nbsp;&nbsp;&nbsp; void *allocatorContext)
</h3>

Uses an allocator to create a THStorage object with
capacity `size`.

<h3>
THStorage* THStorage_newWithMapping(<br/>
&nbsp;&nbsp;&nbsp;&nbsp; const char *filename,<br/>
&nbsp;&nbsp;&nbsp;&nbsp; long size,<br/>
&nbsp;&nbsp;&nbsp;&nbsp; int shared)
</h3>

Maps `filename` to memory, checks if it's at least `size` bytes, and uses it's
content as new THStorage data. Size smaller or equal to 0 means using all
available space in `filename` as data storage.

Please not that this method can be used not only to map files, but shared
memory too.

For possible `shared` values please refer to [THAllocator](/thallocator.md)
documentation.

**

### THStorage\* THStorage_newWithSize1(real data0)

Creates a THStorage object with capacity for one `real` variable and assigns
`data0` to it.

### THStorage\* THStorage_newWithSize2(real data0, real data1)

Creates a THStorage object with capacity for two `real` variables and fills it with arguments.

<h3>
THStorage* THStorage_newWithSize3(real data0,<br/>
&nbsp;&nbsp;&nbsp;&nbsp; real data1, real data2)
</h3>

Creates a THStorage object with capacity for three `real` variables and fills it with arguments.

<h3>
THStorage* THStorage_newWithSize4(real data0,<br/>
&nbsp;&nbsp;&nbsp;&nbsp; real data1, real data2, real data3)
</h3>

Creates a THStorage object with capacity for four `real` variables and fills it with arguments.

<h3>
void THStorage_(setFlag)(THStorage *storage, const char flag)
</h3>


void THStorage_(clearFlag)(THStorage *storage, const char flag)


void THStorage_(retain)(THStorage *storage)


void THStorage_(free)(THStorage *storage)


## Getter methods

### THStorage_data(storage)
Arguments:
* `storage` - a THStorage object

Returns: `real*` pointer to data held by storage.

### THStorage_size

### THStorage_elementSize
