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
// index.cjs

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

-----

### C learning

Was looking this morning into deciding whether I want to learn C, C++, or Rust. Eventually it would be cool to have experience in all of them, first I want to choose one to work with though. I know Rust is the most secure option, but I think I tend to like taking the less safe and sound way of learning new things, because I learn by what things I can do wrong, then I get better with what I choose to account for those concerns with. So I want to learn why it is frequent to band your head against the wall with C errors, to understand why Rust would be the alternative to that. I also think I am leaning towards using C before C++ as well, because of the functional aspect of things.

- [Reddit - Dive into anything](https://www.reddit.com/r/rust/comments/10ibc7y/c_or_rust_for_learning_systems_programming/)
- [C data types - Wikipedia](https://en.wikipedia.org/wiki/C_data_types)
- [data structures - Implementing a HashMap in C - Stack Overflow](https://stackoverflow.com/questions/838404/implementing-a-hashmap-in-c)
- [libnbt/nbt.h at master · Celisium/libnbt](https://github.com/Celisium/libnbt/blob/master/nbt.h)
- [Why do we need C Unions? - Stack Overflow](https://stackoverflow.com/questions/252552/why-do-we-need-c-unions)
- [c++ - Variable name after struct definition - Stack Overflow](https://stackoverflow.com/questions/60138785/variable-name-after-struct-definition)
- [Simple Code Highlight of the Week (C++) — typedef | by <Blake†Codez /> | Medium](https://blake-wood-bst.medium.com/simple-code-highlight-of-the-week-c-typedef-d8ddde5b3db)
- [Should I learn C before learning C++? - Stack Overflow](https://stackoverflow.com/questions/598552/should-i-learn-c-before-learning-c)
- [C Pointers - GeeksforGeeks](https://www.geeksforgeeks.org/c-pointers/)
- [read eval print loop - Is there a REPL for C programming? - Stack Overflow](https://stackoverflow.com/questions/10766900/is-there-a-repl-for-c-programming)
- [Regex for HTML title? - Stack Overflow](https://stackoverflow.com/questions/12030599/regex-for-html-title)

I'm also curious about the concept of writing a TS to C transpiler, then you can "write native code in TS", and compile it to a real native binary. It would kind of be like AssemblyScript, where it's not a full version of the language. That's kind of an odd idea, and I should probably just learn C itself, but I think my interest in having that is also making me want to learn C more, so I can contrast it against the language specifics that I already am familiar with.

I think the concept for doing that transpiler is from when I saw the definitions for C structs and unions. They look essentially just like TS ones, just with a different shape in how you declare them. So if I write custom C-focused library types for TS, then you can write C-specific code that is in the syntax shape of TS. I like the idea of that. Depending on how far that can go as well, if you used simple TS features (the ones that crossover with C), you could maybe even write a heavily bare-bones program that is valid in both languages. Say like this:

```ts
type hi = int | char;

function main(): int {
  console.log("hi\n");
  return 0;
}
```

With custom TS types, you could compile that down to regular JS as well. And it would look like this in C:

```c
union {
  int i;
  char str;
} hi;

int main() {
  printf("hi\n");
  return 0;
}
```

And if anything, maybe it could just be a new syntax over C itself, and it doesn't have to be TS-compatible. This is just a goofy brainstorm, I know that's not a real viable thing to build. It makes more sense to use existing languages. The concept of writing a low-level langauge in the syntax of a higher-level language seems interesting to me though. Maybe that's just Rust?