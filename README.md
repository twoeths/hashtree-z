# hashtree-z
Zig binding for hashtree

# How to build
- clone hashtree to root: `git clone --recurse-submodules https://github.com/prysmaticlabs/hashtree.git`
- `zig build`
- locate `zig-out/lib/libhashtree-z.dylib` (could be diffrerent name in other OSs) and continue the test below

# How to consume at javascript side using Bun FFI:

```typescript
import {dlopen} from "bun:ffi";

// Link to shared library path
const path = `.${your_path}/libhashtree-z.dylib`;

// Load the compiled Zig shared library
const lib = dlopen(path, {
  init: {
      args: [],
      returns: "void"
  },
  hash: {
      args: ["ptr", "ptr", "u64"],
      returns: "void"
  },
});

const chunk = new Uint8Array(64).fill(0xAB);
const out = new Uint8Array(32);

lib.symbols.init();

lib.symbols.hash(out, chunk, 1);
console.log("out", out);

// Close the library when done
lib.close();
```