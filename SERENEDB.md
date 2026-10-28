# SereneDB Snowball compiler

This fork contains an opt-in C/C++ generator tuned for SereneDB. The regular
Snowball output is unchanged unless `SNOWBALL_OPTIMIZE_GENERATED=1` is set
while compiling a grammar.

## Build

The compiler can be built by GNU make as usual, or as a target in a CMake
build:

```sh
cmake -S . -B build
cmake --build build --target serenedb-snowball-compiler
```

The CMake target intentionally builds only the host-side grammar compiler. It
does not build or install a Snowball runtime.

## Generate an optimized UTF-8 C stemmer

The command below uses the same naming and directory layout as the generated
`libstemmer_c` distribution:

```sh
SNOWBALL_OPTIMIZE_GENERATED=1 build/snowball \
  algorithms/english.sbl \
  -o /path/to/libstemmer_c/src_c/stem_UTF_8_english.c \
  -eprefix english_UTF_8_ \
  -r ../runtime \
  -u
```

The compiler writes both the `.c` file and its matching `.h` file. SereneDB's
build invokes this command for every UTF-8 algorithm and writes the generated
files into the `third_party/libstemmer_c` checkout. Generated sources belong
to that submodule working tree and must not be committed to the main SereneDB
repository.

The automatic optimized policy selects code shapes from the grammar rather
than from a language allow-list. It currently enables direct fixed-size
`among` comparisons, selective trie/ART dispatch for large tables, inlined
UTF-8 grouping and skip fast paths with runtime fallbacks, short literal
comparisons, a suffix-deletion fast path, and capacity-preserving C++ input
copies.

