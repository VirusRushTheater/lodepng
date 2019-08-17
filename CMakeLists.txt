# Copyright 2019 VirusRushTheater
#
# Permission is hereby granted, free of charge, to any person obtaining a copy
# of this software and associated documentation files (the "Software"), to deal
# in the Software without restriction, including without limitation the rights
# \to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
# copies of the Software, and to permit persons to whom the Software is
# furnished to do so, subject to the following conditions:
#
# The above copyright notice and this permission notice shall be included in
# all copies or substantial portions of the Software.
#
# THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
# IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
# FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
# AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
# LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
# OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN
# THE SOFTWARE.

############################################################################
# OPTIONS:
#
# * LODEPNG_BUILD_C_LIBRARY: False (default) for C++ linkage. True if you want
#   C linkage. 
# * LODEPNG_BUILD_STATIC_LIBRARY: True (default) to generate a static library
#   (.lib/.a). False to generate a dynamic library (.dll/.so)

project(lodepng)
cmake_minimum_required(VERSION 3.6)

if (POLICY CMP0048)
    cmake_policy(SET CMP0048 NEW)
endif (POLICY CMP0048)

# The difference here is that, if you build it as a C++ library, it mangles the
# function names so the methods can be overloaded with the same names.
option(LODEPNG_BUILD_C_LIBRARY "Build Lodepng as a C library instead of a C++ library" False)
option(LODEPNG_BUILD_STATIC_LIBRARY "Build Lodepng as a static library" True)
#option(LODEPNG_BUILD_EXAMPLES "Build example programs (Requires SDL2)" False)

set(SOURCES
    lodepng.h
    lodepng.cpp
)

# Sets the compiler macro "LODEPNG_NO_COMPILE_CPP" if the option LODEPNG_BUILD_C_LIBRARY
# is set.
if(LODEPNG_BUILD_C_LIBRARY)
    SET_SOURCE_FILES_PROPERTIES( lodepng.cpp PROPERTIES LANGUAGE C )
    add_definitions(-DLODEPNG_NO_COMPILE_CPP)

    message(STATUS "Building Lodepng with C linkage (LODEPNG_BUILD_C_LIBRARY = True)")
else()
    message(STATUS "Building Lodepng with C++ linkage (LODEPNG_BUILD_C_LIBRARY = False)")
endif()

# if "LODEPNG_BUILD_STATIC_LIBRARY" is set, builds a static library.
if(LODEPNG_BUILD_STATIC_LIBRARY)
    set(BUILD_STATIC_OR_DYNAMIC STATIC)
    message(STATUS "Building Lodepng as a static library (LODEPNG_BUILD_STATIC_LIBRARY = True)")
else()
    set(BUILD_STATIC_OR_DYNAMIC SHARED)
    message(STATUS "Building Lodepng as a shared library (LODEPNG_BUILD_STATIC_LIBRARY = False)")
endif()

add_library( ${CMAKE_PROJECT_NAME} ${BUILD_STATIC_OR_DYNAMIC} ${SOURCES} )
target_include_directories (lodepng PUBLIC ${CMAKE_CURRENT_SOURCE_DIR})
