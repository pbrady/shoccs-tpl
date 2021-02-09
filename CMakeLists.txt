cmake_minimum_required(VERSION 3.16)

project(shoccs-tpl LANGUAGES C CXX)
include(ExternalProject)

set(CMAKE_EXPORT_COMPILE_COMMANDS ON)
set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

find_package(Lua QUIET)
find_package(sol2 QUIET)
find_package(Catch2 3 QUIET)
find_package(range-v3 QUIET)
find_package(fmt QUIET)
find_package(pugixml 1.10 QUIET)
find_package(spdlog QUIET)
find_package(cxxopts QUIET)

set(lua_platform "generic")
if (CMAKE_SYSTEM_NAME STREQUAL "Windows")
    set (lua_platform "mingw")
elseif (CMAKE_SYSTEM_NAME STREQUAL "Darwin")
    set (lua_platform "macosx")
elseif (CMAKE_SYSTEM_NAME STREQUAL "Linux")
    set (lua_platform "linux")
elseif (CMAKE_SYSTEM_NAME STREQUAL "FreeBSD")
    set (lua_platform "freebsd")
endif()

find_program(MAKE_EXECUTABLE NAMES nmake gmake make)

if (NOT Lua_FOUND)
   
    ExternalProject_Add(external_lua
        URL https://www.lua.org/ftp/lua-5.3.6.tar.gz
        URL_HASH MD5=83f23dbd5230140a3770d5f54076948d
        INSTALL_DIR ${CMAKE_INSTALL_PREFIX}
        CONFIGURE_COMMAND ""
        BUILD_IN_SOURCE ON
        BUILD_COMMAND CC=${CMAKE_C_COMPILER} ${MAKE_EXECUTABLE} ${lua_platform}
        INSTALL_COMMAND ${MAKE_EXECUTABLE} install INSTALL_TOP=<INSTALL_DIR>
    )
endif()

if (NOT sol2_FOUND)
    ExternalProject_Add(external_sol2
        GIT_REPOSITORY git@github.com:ThePhD/sol2.git
        GIT_TAG develop
        INSTALL_DIR ${CMAKE_INSTALL_PREFIX}
        CMAKE_ARGS -D CMAKE_INSTALL_PREFIX:PATH=<INSTALL_DIR>
                    -D CMAKE_C_COMPILER=${CMAKE_C_COMPILER}
                    -D CMAKE_CXX_COMPILER=${CMAKE_CXX_COMPILER}
                    -D CMAKE_BUILD_TYPE=${CMAKE_BUILD_TYPE})
endif()

if (NOT Catch2_FOUND)
    ExternalProject_Add(external_catch2
        GIT_REPOSITORY git@github.com:catchorg/Catch2.git
        GIT_TAG devel
        INSTALL_DIR ${CMAKE_INSTALL_PREFIX}
        CMAKE_ARGS -D CMAKE_INSTALL_PREFIX:PATH=<INSTALL_DIR>
                    -D CMAKE_CXX_COMPILER=${CMAKE_CXX_COMPILER}
                    -D CMAKE_BUILD_TYPE=${CMAKE_BUILD_TYPE}
                    -D CATCH_BUILD_TESTING=OFF)
endif()

if (NOT range-v3_FOUND)
    ExternalProject_Add(external_ranges
        GIT_REPOSITORY git@github.com:ericniebler/range-v3.git
        GIT_SUBMODULES_RECURSE OFF
        INSTALL_DIR ${CMAKE_INSTALL_PREFIX}
        CMAKE_ARGS -D CMAKE_INSTALL_PREFIX:PATH=<INSTALL_DIR>
            -D CMAKE_CXX_COMPILER=${CMAKE_CXX_COMPILER}
            -D CMAKE_BUILD_TYPE=${CMAKE_BUILD_TYPE}
            -D RANGES_BUILD_CALENDAR_EXAMPLE=OFF
            -D RANGE_V3_TESTS=OFF
            -D RANGE_V3_EXAMPLES=OFF
            -D RANGE_V3_DOCS=OFF
            -D RANGE_V3_PERF=OFF)
endif()

if (NOT fmt_FOUND)
    ExternalProject_Add(external_fmt
        GIT_REPOSITORY git@github.com:fmtlib/fmt.git
        INSTALL_DIR ${CMAKE_INSTALL_PREFIX}
        CMAKE_ARGS  -D CMAKE_INSTALL_PREFIX:PATH=<INSTALL_DIR>
                    -D CMAKE_CXX_COMPILER=${CMAKE_CXX_COMPILER}
                    -D FMT_DOC=OFF
                    -D FMT_INSTALL=ON
                    -D FMT_TEST=OFF)
endif()

if (NOT pugixml_FOUND)
    ExternalProject_ADD(external_pugixml
        GIT_REPOSITORY git@github.com:zeux/pugixml.git
        INSTALL_DIR ${CMAKE_INSTALL_PREFIX}
        CMAKE_ARGS  -D CMAKE_INSTALL_PREFIX:PATH=<INSTALL_DIR>
                    -D CMAKE_CXX_COMPILER=${CMAKE_CXX_COMPILER})
endif()

if (NOT spdlog_FOUND)
    ExternalProject_ADD(external_spdlog
        GIT_REPOSITORY git@github.com:gabime/spdlog.git
        GIT_TAG v1.8.2
        INSTALL_DIR ${CMAKE_INSTALL_PREFIX}
        CMAKE_ARGS -D CMAKE_INSTALL_PREFIX:PATH=<INSTALL_DIR>
                   -D CMAKE_CXX_COMPILER=${CMAKE_CXX_COMPILER}
    )
endif()

if (NOT cxxopts_FOUND)
    ExternalProject_ADD(external_cxxopts
        GIT_REPOSITORY git@github.com:jarro2783/cxxopts.git
        GIT_TAG v2.2.1
        INSTALL_DIR ${CMAKE_INSTALL_PREFIX}
        CMAKE_ARGS -D CMAKE_INSTALL_PREFIX:PATH=<INSTALL_DIR>
                   -D CMAKE_CXX_COMPILER=${CMAKE_CXX_COMPILER}
                   -D CXXOPTS_BUILD_EXAMPLES=OFF
                   -D CXXOPTS_BUILD_TESTS=OFF                   
    )
endif()
