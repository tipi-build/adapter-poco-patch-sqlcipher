cmake_minimum_required(VERSION 3.17.0)
project(adapter_poco_patch_sqlcipher VERSION "0.0.1")

include(FetchContent)
set(FETCHCONTENT_QUIET OFF)
set(FETCHCONTENT_UPDATES_DISCONNECTED ON)



set(POCO_SOURCE_DIR ${CMAKE_CURRENT_BINARY_DIR}/poco-src)
set(POCO_BUILD_DIR ${CMAKE_CURRENT_BINARY_DIR}/poco-build)
set(POCO_INSTALL_DIR ${CMAKE_INSTALL_PREFIX})
set(POCO_INCLUDE_DIR ${POCO_INSTALL_DIR}/include)


add_compile_definitions(
    _GNU_SOURCE=1
    SQLITE_HAS_CODEC=1
    SQLITE_TEMP_STORE=3
    SQLCIPHER_CRYPTO_OPENSSL
)

include(FetchContent)
FetchContent_Declare(poco
    GIT_REPOSITORY    https://github.com/pocoproject/poco
    GIT_TAG           "poco-1.11.0-release"
    SOURCE_DIR        ${POCO_SOURCE_DIR}
    SUBBUILD_DIR      ${POCO_BUILD_DIR}
    PATCH_COMMAND     cp ${CMAKE_CURRENT_LIST_DIR}/sqlcipher/sqlite3.c ${CMAKE_CURRENT_LIST_DIR}/sqlcipher/sqlite3.h ${POCO_SOURCE_DIR}/Data/SQLite/src/
)


FetchContent_MakeAvailable(poco)

file(MAKE_DIRECTORY ${POCO_INSTALL_DIR}/bin/)

install(DIRECTORY DESTINATION ${POCO_INSTALL_DIR}/adapter-poco-patch-sqlcipher.done)