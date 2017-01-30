cmake_minimum_required(VERSION 3.5)

project(dwf-toolkit)

option(BUILD_SHARED_LIBS "build shared libraries" ON)

file(GLOB SOURCE_FILES RELATIVE "${CMAKE_CURRENT_SOURCE_DIR}" dwfcore/*.cpp dwfcore/*.h)
file(GLOB WIN32_SOURCE_FILES RELATIVE "${CMAKE_CURRENT_SOURCE_DIR}" dwfcore/win32/*.cpp dwfcore/win32/*.h)
file(GLOB CRYPTO_SOURCE_FILES RELATIVE "${CMAKE_CURRENT_SOURCE_DIR}" dwfcore/crypto/*.cpp dwfcore/crypto/*.h)
file(GLOB ANSI_SOURCE_FILES RELATIVE "${CMAKE_CURRENT_SOURCE_DIR}" dwfcore/ansi/*.cpp dwfcore/ansi/*.h)
file(GLOB ZIP_SOURCE_FILES RELATIVE "${CMAKE_CURRENT_SOURCE_DIR}" dwfcore/zip/*.cpp dwfcore/zip/*.h)


if(NOT BUILD_SHARED_LIBS)
  add_definitions(-DDWFCORE_STATIC)
else()
  add_definitions(-DDWFCORE_EXPORT_API)
endif(NOT BUILD_SHARED_LIBS)

add_definitions(-DWFCORE_BUILD_ZLIB)

add_definitions(-D_CRT_SECURE_NO_DEPRECATE)
add_definitions(-DDWFCORE_EXPORT_API)
add_definitions(-DDWFCORE_BUILD_ZLIB)
add_definitions(-DDWFCORE_ENABLE_MEMORY_BASE)
add_definitions(-DDWFCORE_USE_DEFAULT_MEMORY_BASE_IMPL)
add_definitions(-DDWFCORE_BUILD_EXPAT)
add_definitions(-DEXPAT_COMPILED_FROM_DSP)

#file(GLOB_RECURSE ZLIB_SOURCE_FILES RELATIVE "${CMAKE_CURRENT_SOURCE_DIR}" dwfcore/zlib/*.c)
set(ZLIB_SOURCE_FILES
  "dwfcore/zlib/adler32.c"
  "dwfcore/zlib/compress.c"
  "dwfcore/zlib/crc32.c"
  "dwfcore/zlib/deflate.c"
  "dwfcore/zlib/inflate.c"
  "dwfcore/zlib/inftrees.c"
  "dwfcore/zlib/infutil.c"
  "dwfcore/zlib/trees.c"
  "dwfcore/zlib/uncompr.c"
  "dwfcore/zlib/zutil.c"
  "dwfcore/zlib/gzio.c"
  "dwfcore/zlib/infblock.c"
  "dwfcore/zlib/infcodes.c"
  "dwfcore/zlib/inffast.c"
)

set(EXPAT_SOURCE_FILES 
  "dwfcore/expat/xmlparse.c"
  "dwfcore/expat/xmlrole.c"
  "dwfcore/expat/xmltok.c"
)

set(DIGEST_SOURCE_FILES
  "dwfcore/digest/md5.c"
  "dwfcore/digest/sha1.c"
)

set(CORE_SOURCE_FILES)
if(${CMAKE_SIZEOF_VOID_P} EQUAL 4)
  file(GLOB_RECURSE CORE_SOURCE_FILES RELATIVE "${CMAKE_CURRENT_SOURCE_DIR}" dwfcore/x86/*.cpp dwfcore/x86/*.h )
else()
  file(GLOB_RECURSE CORE_SOURCE_FILES RELATIVE "${CMAKE_CURRENT_SOURCE_DIR}" dwfcore/x64/*.cpp dwfcore/x64/*.h )
endif(${CMAKE_SIZEOF_VOID_P} EQUAL 4)

include_directories(${CMAKE_CURRENT_SOURCE_DIR})
include_directories(./dwfcore/zlib)

add_library(dwfcore 
  ${SOURCE_FILES}
  ${CORE_SOURCE_FILES}
  ${WIN32_SOURCE_FILES}
  ${CRYPTO_SOURCE_FILES}
  ${ANSI_SOURCE_FILES}
  ${ZIP_SOURCE_FILES}
  ${ZLIB_SOURCE_FILES}
  ${EXPAT_SOURCE_FILES}
  ${DIGEST_SOURCE_FILES}
  "dwfcore/sqlite/sqlite3.c"
)

target_link_libraries(dwfcore rpcrt4.lib)

#add_subdirectory(dwfcore)