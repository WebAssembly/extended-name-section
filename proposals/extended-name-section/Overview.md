## Overview

This proposal is to extend the WebAssembly binary format's name section to assign
names to all entities in the text format that can be named. The WebAssembly spec
this proposal modifies includes only a module name, function names, and local
names.

#### Name subsections

The following new name subsections are defined:

| Name Type                   | Code | Description                          |
| --------------------------- | ---- | ------------------------------------ |
| [Labels](#label-names)      | `3`  | Assigns names to labels in functions |
| [Type](#type-names)         | `4`  | Assigns names to types               |
| [Table](#table-names)       | `5`  | Assigns names to tables              |
| [Memory](#memory-names)     | `6`  | Assigns names to memories            |
| [Global](#global-names)     | `7`  | Assigns names to globals             |

#### Label names

The label names subsection assigns `name_map`s to a subset of functions in the
[function index space](Modules.md#function-index-space). This may include both
module-defined or imported functions, but is only meaningful for module-defined
functions. The `name_map` for a function assigns names to label indices, with
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
| label_map | `name_map` | assignment of names to labeling operator |

#### Type names

The type names subsection is a `name_map` which assigns names to a subset
of types in the module's [type section](https://github.com/WebAssembly/design/blob/master/BinaryEncoding.md#type-section).

#### Table names

The table names subsection is a `name_map` which assigns names to a subset
of tables in the [table index space](https://github.com/WebAssembly/design/blob/master/Modules.md#table-index-space).

#### Memory names

The memory names subsection is a `name_map` which assigns names to a subset
of memories in the [linear memory index space](https://github.com/WebAssembly/design/blob/master/Modules.md#linear-memory-index-space).

#### Global names

The global names subsection is a `name_map` which assigns names to a subset
of globals in the [global index space](https://github.com/WebAssembly/design/blob/master/Modules.md#global-index-space).
