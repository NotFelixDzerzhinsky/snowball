cmake_minimum_required(VERSION 3.25)

if(CMAKE_SOURCE_DIR STREQUAL CMAKE_CURRENT_SOURCE_DIR)
  project(serenedb_snowball LANGUAGES C)
endif()

add_executable(
  serenedb-snowball-compiler
  compiler/analyser.c
  compiler/driver.c
  compiler/generator.c
  compiler/generator_ada.c
  compiler/generator_c.c
  compiler/generator_csharp.c
  compiler/generator_dart.c
  compiler/generator_go.c
  compiler/generator_java.c
  compiler/generator_js.c
  compiler/generator_pascal.c
  compiler/generator_php.c
  compiler/generator_python.c
  compiler/generator_rust.c
  compiler/generator_zig.c
  compiler/space.c
  compiler/tokeniser.c
)

target_compile_features(serenedb-snowball-compiler PRIVATE c_std_99)
set_target_properties(
  serenedb-snowball-compiler
  PROPERTIES
    C_EXTENSIONS OFF
    OUTPUT_NAME snowball
)

