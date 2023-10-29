# WASM-C

Learning how to use WASM with C!

First you have to install Emscripten locally on your system. I'm doing this on macOS, so I'm using Homebrew.

```bash
brew install emscripten
```

Then you build the C entry point using `emcc`.

```bash
emcc ./demo.c
```

That will build the files `a.out.js` and `a.out.wasm` for you, which can then be run in the browser and Node.

```js
// index.js

require("./a.out.js");
```

This demo is just a runtime-effect kind of script, it only logs `"hi"` to the console.

```c
#include <stdio.h>
#include <emscripten.h>

int main() {
  printf("hi\n");
  return 0;
}
```

You can also make 'function exports' in a way, making use of the `EMSCRIPTEN_KEEPALIVE` function name. I'm going to try looking into that next also.

These were some of the inspirations in figuring out how to make this work. I want to further figure out how to use the WASM builds more standalone from the `a.out.js` file, I'm curious how that sets things up to work for you. Maybe you aren't supposed to handle that yourself though? It does seem to have a lot of boilerplate just to get things set up properly though.

- [Compiling a New C/C++ Module to WebAssembly](https://developer.mozilla.org/en-US/docs/WebAssembly/C_to_Wasm)
- [Compiling an Existing C Module to WebAssembly](https://developer.mozilla.org/en-US/docs/WebAssembly/existing_C_to_Wasm)
  - I want to use this page to further understand how to make use of existing libraries, as I want to get `leveldb-zlib` working in the browser somehow, and I'm wondering if I can do that with WASM and a virtual file system implementation? Even the Origin-Private File System could be great for that, if it's possible to set that up accordingly.
- [Emscripten VSCode IntelliSense](https://gist.github.com/wayou/59f3a8e4fbab050fbb32e94dd9582660)