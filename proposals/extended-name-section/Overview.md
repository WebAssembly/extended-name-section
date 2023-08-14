## Overview

This proposal is to extend the WebAssembly binary format's name section to assign
names to all entities in the text format that can be named. The WebAssembly spec
this proposal modifies includes only a module name, function names, and local
names.

#### Data and elem segment names

All of the entities this proposal adds names to the binary format for are
part of the WebAssembly MVP. However, the MVP text format does not include
names for `elem` and `data` segments. The
[bulk memory ops extension](https://github.com/WebAssembly/bulk-memory-operations)
adds names to the text format for `elem` and `data` segments, and defines index
spaces for them, but does not define a binary format for those names.

This proposal adds names to the binary format for segments, and so is dependent
on the bulk-memory-ops extension's definition of an index space for those
segments. Other than the elem and data segment name subsections, this proposal
is independent of the bulk-memory-ops extension.

#### Function label indices

This proposal defines a "function label index" space in addition to the scoped
label index space already defined in the spec. This function label index space
assigns indices to the labels in a function in the order their corresponding
structured control instruction occurs in the function body.

Other proposals that deal with references to instructions seem to use binary
format offsets:
1. [DWARF for WebAssembly](https://github.com/WebAssembly/debugging/issues/1)
1. [The display conventions for WebAssembly locations](https://github.com/WebAssembly/design/pull/1053)
1. [Source maps applied to WebAssembly binaries](https://github.com/WebAssembly/design/pull/1051)

The reasons for using the function label index space over a binary offset in
this proposal are:
1. Any in-memory representation of the WebAssembly abstract syntax must already
be able to encode the order of the structured control instructions, and that
order implicitly defines the function label index space. Using binary offsets
would require an implementation to keep track of the binary offset a structured
control instruction was read from.
1. The function label index space also densely maps integers to names, while
binary offsets would sparsely map integers to names. That makes it more
practical to use a simple data structure to store the names in memory.
1. Finally, a binary offset would mean that a far greater proportion of
WebAssembly module transforms would need to also transform the binary offsets
in the name section. The function label index space is not changed by any
transform that doesn't change the structured control instructions.

#### Name subsections

The following new name subsections are defined:

| Name Type                           | Code | Description                          |
| ----------------------------------- | ---- | ------------------------------------ |
| [Labels](#label-names)              | `3`  | Assigns names to labels in functions |
| [Table](#table-names)               | `5`  | Assigns names to tables              |
| [Memory](#memory-names)             | `6`  | Assigns names to memories            |
| [Global](#global-names)             | `7`  | Assigns names to globals             |
| [Elem segment](#elem-segment-names) | `8`  | Assigns names to element segments    |
| [Data segment](#data-segment-names) | `9`  | Assigns names to data segments       |

**Note:** Code `4` was intentionally skipped as it represents the Type subsection, which is a part of the [Garbage Collection](https://github.com/WebAssembly/gc) proposal. 

#### Label names

The label names subsection assigns `namemap`(s) to a subset of functions in the
[function index space](Modules.md#function-index-space). This may include both
module-defined or imported functions, but is only meaningful for module-defined
functions. The `namemap` for a function assigns names to label indices, with
label indices assigned sequentially to the labels in the order they are introduced
by control structure operators (i.e. `block`, `loop`, or `if`) in the function's
code.

| Field | Type | Description |
| ----- | ---- | ----------- |
| count | `varuint32` | count of `label_names` in funcs |
| funcs | `label_names*` | sequence of `label_names` sorted by index |

where a `label_name` is encoded as:

| Field | Type | Description |
| ----- | ---- | ----------- |
| index | `varuint32` | the index of the function whose labels are being named |
| label_map | `namemap` | assignment of names to labeling operator |

#### Table names

The table names subsection is a `namemap` which assigns names to a subset
of tables in the [table index space](https://github.com/WebAssembly/design/blob/master/Modules.md#table-index-space).

#### Memory names

The memory names subsection is a `namemap` which assigns names to a subset
of memories in the [linear memory index space](https://github.com/WebAssembly/design/blob/master/Modules.md#linear-memory-index-space).

#### Global names

The global names subsection is a `namemap` which assigns names to a subset
of globals in the [global index space](https://github.com/WebAssembly/design/blob/master/Modules.md#global-index-space).

#### Elem segment names

The element segment names subsection is a `namemap` which assigns names to a
subset of element segments in the [element index space](https://github.com/WebAssembly/bulk-memory-operations/blob/master/document/core/syntax/modules.rst#indices).

#### Data segment names

The data segment names subsection is a `namemap` which assigns names to a
subset of data segments in the [data index space](https://github.com/WebAssembly/bulk-memory-operations/blob/master/document/core/syntax/modules.rst#indices).
