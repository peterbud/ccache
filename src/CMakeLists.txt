add_library(
  ccache_lib STATIC
  Args.cpp
  AtomicFile.cpp
  CacheEntryReader.cpp
  CacheEntryWriter.cpp
  CacheFile.cpp
  Compression.cpp
  Compressor.cpp
  Config.cpp
  Context.cpp
  Counters.cpp
  Decompressor.cpp
  InodeCache.cpp
  Lockfile.cpp
  MiniTrace.cpp
  NullCompressor.cpp
  NullDecompressor.cpp
  ProgressBar.cpp
  SignalHandler.cpp
  Stat.cpp
  ThreadPool.cpp
  Util.cpp
  ZstdCompressor.cpp
  ZstdDecompressor.cpp
  argprocessing.cpp
  ccache.cpp
  cleanup.cpp
  compopt.cpp
  compress.cpp
  execute.cpp
  hash.cpp
  hashutil.cpp
  language.cpp
  legacy_util.cpp
  logging.cpp
  manifest.cpp
  result.cpp
  stats.cpp
  win32compat.cpp
  Version.cpp)

if(WIN32)
  target_link_libraries(ccache_lib PRIVATE ws2_32 "psapi")

  if(CMAKE_CXX_COMPILER_ID MATCHES "GNU")
    target_link_libraries(ccache_lib PRIVATE -static gcc stdc++ winpthread
                                             -dynamic)
  else()
    target_link_libraries(ccache_lib PRIVATE -static c++ -dynamic)
  endif()
endif()

find_package(Threads REQUIRED)
target_link_libraries(
  ccache_lib PRIVATE standard_settings standard_warnings ZSTD::ZSTD
                     libb2::libb2 Threads::Threads third_party_lib)

# There is currently no separate interface directory.
target_include_directories(ccache_lib PRIVATE ${CMAKE_BINARY_DIR} .)

add_subdirectory(third_party)
