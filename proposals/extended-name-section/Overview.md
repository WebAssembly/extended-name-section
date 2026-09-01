# Overview

The WebAssembly binary format has a well-known custom name section, with subsections for naming the module, functions, locals, types, struct fields, and tags. These names are frequently used for debugging and tooling.

However, there are now several places where identifiers are supported in the text format but no corresponding name subsection exists. This proposal extends the binary format's custom name section to close these gaps.

In addition, the text format defines custom `@name` annotations as a textual analogue to the binary format's custom name section. However, these annotations are currently out of sync with the binary format, and should also be extended to cover the newly-introduced binary subsections.

## New Name Subsections

The following new subsections are defined for the custom name section:

| Subsection                                      | Id   |
| ----------------------------------------------- | ---- |
| [label names](#label-names)                     | `3`  |
| [table names](#table-names)                     | `5`  |
| [memory names](#memory-names)                   | `6`  |
| [global names](#global-names)                   | `7`  |
| [element segment names](#element-segment-names) | `8`  |
| [data segment names](#data-segment-names)       | `9`  |
| [parameter names](#parameter-names)             | `12` |
| [tag parameter names](#tag-parameter-names)     | `13` |

### Label Names

The *label name subsection* has the id 3. It consists of an [indirect name map](https://webassembly.github.io/spec/core/appendix/custom.html#binary-indirectnamemap) assigning label names to label indices grouped by [function indices](https://webassembly.github.io/spec/core/syntax/modules.html#syntax-funcidx).

Labels are indexed in the order they appear in the function body. This is different from how labels are normally indexed (i.e. the indices used by branch instructions, which use the [scoped label index space](https://webassembly.github.io/spec/core/syntax/modules.html#syntax-labelidx)). Therefore, this proposal introduces a new *function-wide label index space* that assigns indices to labels in source code order.

### Table Names

The *table name subsection* has the id 5. It consists of a [name map](https://webassembly.github.io/spec/core/appendix/custom.html#binary-namemap) assigning table names to [table indices](https://webassembly.github.io/spec/core/binary/modules.html#binary-tableidx).

### Memory Names

The *memory name subsection* has the id 6. It consists of a [name map](https://webassembly.github.io/spec/core/appendix/custom.html#binary-namemap) assigning memory names to [memory indices](https://webassembly.github.io/spec/core/binary/modules.html#binary-memoryidx).

### Global Names

The *global name subsection* has the id 7. It consists of a [name map](https://webassembly.github.io/spec/core/appendix/custom.html#binary-namemap) assigning global names to [global indices](https://webassembly.github.io/spec/core/binary/modules.html#binary-globalidx).

### Element Segment names

The *element segment name subsection* has the id 8. It consists of a [name map](https://webassembly.github.io/spec/core/appendix/custom.html#binary-namemap) assigning element segment names to [element segment indices](https://webassembly.github.io/spec/core/binary/modules.html#binary-elemidx).

### Data Segment Names

The *data segment name subsection* has the id 9. It consists of a [name map](https://webassembly.github.io/spec/core/appendix/custom.html#binary-namemap) assigning data segment names to [data segment indices](https://webassembly.github.io/spec/core/binary/modules.html#binary-dataidx).

### Parameter Names

The *parameter name subsection* has the id 12. It consists of an [indirect name map](https://webassembly.github.io/spec/core/appendix/custom.html#binary-indirectnamemap) assigning parameter names to parameter indices grouped by [type indices](https://webassembly.github.io/spec/core/syntax/modules.html#syntax-typeidx) for function types.

Note that this section assigns names to parameters in *function types*, not type uses (i.e. parameters written inline in function definitions or imports). Such parameters are already covered by the local name subsection.

### Tag Parameter Names

The *tag parameter name subsection* has the id 13. It consists of an [indirect name map](https://webassembly.github.io/spec/core/appendix/custom.html#binary-indirectnamemap) assigning parameter names to parameter indices grouped by [tag indices](https://webassembly.github.io/spec/core/syntax/modules.html#syntax-tagidx).

This subsection exists because the names assigned to tag parameters belong to a type use, not a type definition, but because the type use is not associated with a function, the local name subsection cannot be used.
