# Copyright (c) 2012 Peter Kümmel
# All rights reserved. Use of this source code is governed by a
# BSD-style license that can be found in the LICENSE file.


cmake_minimum_required(VERSION 2.8)
set(CMAKE_MODULE_PATH ${CMAKE_SOURCE_DIR}/cmake)


option(release "Release build" OFF)


project(dart-sqlite)


set(EXECUTABLE_OUTPUT_PATH  ${CMAKE_BINARY_DIR}/bin)
set(LIBRARY_OUTPUT_PATH     ${CMAKE_BINARY_DIR}/lib)


if(release)
    set(CMAKE_BUILD_TYPE Release)
else()
    set(CMAKE_BUILD_TYPE Debug)
endif()


add_subdirectory(src/dartruntime)

include_directories(${dart_includes})
include_directories(src/sqlite)

add_definitions(-DDART_SHARED_LIB)

add_library(dart_sqlite SHARED 
    src/dart_sqlite.cc 
    src/sqlite/sqlite3.c
    lib/sqlite.dart
    test/test.dart)

set_target_properties(dart_sqlite PROPERTIES LIBRARY_OUTPUT_DIRECTORY         ${CMAKE_SOURCE_DIR}/lib)
set_target_properties(dart_sqlite PROPERTIES RUNTIME_OUTPUT_DIRECTORY         ${CMAKE_SOURCE_DIR}/lib)
set_target_properties(dart_sqlite PROPERTIES RUNTIME_OUTPUT_DIRECTORY_DEBUG   ${CMAKE_SOURCE_DIR}/lib)
set_target_properties(dart_sqlite PROPERTIES RUNTIME_OUTPUT_DIRECTORY_RELEASE ${CMAKE_SOURCE_DIR}/lib)

set_source_files_properties(test/test.dart lib/sqlite.dart PROPERTIES HEADER_FILE_ONLY TRUE)
     
target_link_libraries(dart_sqlite ${dart_libraries})
add_dependencies(dart_sqlite dart)

