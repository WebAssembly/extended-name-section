# Overview

The WebAssembly spec has a custom name section, with subsections for naming the module, functions, locals, types, fields, and tags. This proposal extends the binary format's custom name section, adding subsections for all entities that can be named in the text format. These entities include labels, tables, memories, globals, element segments, and data segments.

## New Subsections

The following new subsections are defined for the custom name section:

| Subsection                    | Id   |
| ----------------------------- | ---- |
| [label names](#label-names)   | `3`  |
| [table names](#table-names)   | `5`  |
| [memory names](#memory-names) | `6`  |
| [global names](#global-names) | `7`  |
| [elem names](#elem-names)     | `8`  |
| [data names](#data-names)     | `9`  |

### Label Names

The *label name subsection* has the id 3. It consists of an [indirect name map](https://webassembly.github.io/spec/core/appendix/custom.html#binary-indirectnamemap) assigning label names to label indices grouped by [function indices](https://webassembly.github.io/spec/core/syntax/modules.html#syntax-funcidx).

The label indices referred to above are not to be confused with the [scoped label index space](https://webassembly.github.io/spec/core/syntax/modules.html#syntax-labelidx) already defined in the spec. This proposal introduces a new *function-wide label index space* that assigns indices to the labels in a function in the order their corresponding structured control instruction occurs in the function body.

### Table Names

The *table name subsection* has the id 5. It consists of a [name map](https://webassembly.github.io/spec/core/appendix/custom.html#binary-namemap) assigning table names to [table indices](https://webassembly.github.io/spec/core/binary/modules.html#binary-tableidx).

### Memory Names

The *memory name subsection* has the id 6. It consists of a [name map](https://webassembly.github.io/spec/core/appendix/custom.html#binary-namemap) assigning memory names to [memory indices](https://webassembly.github.io/spec/core/binary/modules.html#binary-memoryidx).

### Global Names

The *global name subsection* has the id 7. It consists of a [name map](https://webassembly.github.io/spec/core/appendix/custom.html#binary-namemap) assigning global names to [global indices](https://webassembly.github.io/spec/core/binary/modules.html#binary-globalidx).

### Elem Names

The *elem name subsection* has the id 8. It consists of a [name map](https://webassembly.github.io/spec/core/appendix/custom.html#binary-namemap) assigning element segment names to [elem indices](https://webassembly.github.io/spec/core/binary/modules.html#binary-elemidx).

### Data Names

The *data names subsection* has the id 9. It consists of a [name map](https://webassembly.github.io/spec/core/appendix/custom.html#binary-namemap) assigning data segment names to [data indices](https://webassembly.github.io/spec/core/binary/modules.html#binary-dataidx).
